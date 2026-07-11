# ----Exporting as CSV and PDF

### Exporting as CSV

```javascript
import Product from "../models/product.model.js"
import { Parser } from "json2csv"   // install: npm i json2csv

export const exportProducts = async (req, res, next) => {
  try {
    // Fetch all products (or filter based on query if needed)
    const products = await Product.find().lean()

    if (!products || products.length === 0) {
      return next(errorHandler(404, "No products found"))
    }

    // Fields to include in CSV
    const fields = [
      { label: "Product ID", value: "_id" },
      { label: "Name", value: "name" },
      { label: "Category", value: "category" },
      { label: "Price", value: "price" },
      { label: "Stock", value: "stock" },
      { label: "Created At", value: "createdAt" },
    ]

    // Convert JSON → CSV
    const json2csv = new Parser({ fields })
    const csv = json2csv.parse(products)

    // Set headers for file download
    res.header("Content-Type", "text/csv")
    res.attachment("products.csv")
    return res.send(csv)
  } catch (error) {
    console.error("Error exporting products:", error.message)
    next(error)
  }
}

```

### Exporting as Pdf

```javascript
import Product from "../models/product.model.js"
import PDFDocument from "pdfkit"
import path from "path"

export const exportProductsPDF = async (req, res, next) => {
  try {
    const products = await Product.find().lean()

    if (!products || products.length === 0) {
      return next(errorHandler(404, "No products found"))
    }

    const doc = new PDFDocument()
    res.setHeader("Content-Type", "application/pdf")
    res.setHeader("Content-Disposition", "attachment; filename=products.pdf")

    doc.pipe(res)

    // Title
    doc.fontSize(18).text("Product List", { align: "center" })
    doc.moveDown()

    // Table header
    doc.fontSize(12).text("ID", 50, doc.y, { continued: true })
    doc.text("Name", 150, doc.y, { continued: true })
    doc.text("Category", 300, doc.y, { continued: true })
    doc.text("Price", 400, doc.y, { continued: true })
    doc.text("Stock", 470, doc.y)

    doc.moveDown()

    // Loop through products
    products.forEach((p) => {
      doc.text(p._id.toString(), 50, doc.y, { continued: true })
      doc.text(p.name, 150, doc.y, { continued: true })
      doc.text(p.category, 300, doc.y, { continued: true })
      doc.text(p.price.toString(), 400, doc.y, { continued: true })
      doc.text(p.stock.toString(), 470, doc.y)
    })

    doc.end()
  } catch (error) {
    console.error("Error exporting products to PDF:", error.message)
    next(error)
  }
}

```

> ### **Step by Step**
>
> 1. **Fetch Products**
>    * It queries all products from your MongoDB using Mongoose (`Product.find().lean()`).
>    * `.lean()` makes the results plain JS objects instead of full Mongoose documents (lighter & faster).
> 2. **Check if Products Exist**
>    * If no products found, it triggers an error handler.
> 3. **Create PDF Document**
>    * `const doc = new PDFDocument()` initializes a new PDF file.
> 4. **Set Response Headers**
>    * Tells the browser:
>      * `"Content-Type: application/pdf"` → the response is a PDF file.
>      * `"Content-Disposition: attachment; filename=products.pdf"` → force download as `products.pdf`.
> 5. **Pipe Output to Response**
>    * `doc.pipe(res)` means everything written into the PDF is streamed directly to the HTTP response, so the browser downloads it in real time.
> 6. **Write Title & Header**
>    * Writes `"Product List"` centered at the top.
>    * Then creates table headers (`ID`, `Name`, `Category`, etc.) at specific `x` positions.
> 7. **Write Table Rows**
>    * Loops through each product and writes values aligned under their headers using fixed positions:
>      * ID starts at `x = 50`
>      * Name at `x = 150`
>      * Category at `x = 300`
>      * Price at `x = 400`
>      * Stock at `x = 470`
> 8. **Finalize PDF**
>    * `doc.end()` signals the PDF stream is complete, and the browser finishes downloading.
>
> #### 👉 So in short:
>
> This controller **pulls products from DB → generates a structured PDF → streams it to the admin for download.**
>
> #### `doc.y `, `{continue: true}`, `doc.moveDown`
>
> ### 🔹 `doc.y`
>
> * `doc.y` is the **current vertical cursor position** (Y-coordinate) in the PDF.
> * Every time you write text, PDFKit keeps track of the cursor’s vertical position.
> * Using `doc.y` means: *“place this text at whatever the current line’s Y position is.”*
> * That’s why all your header cells (`ID`, `Name`, `Category`, etc.) appear  **on the same row** , because you’re explicitly fixing the Y coordinate to `doc.y` for each column.
>
> Example:
>
> ```js
> doc.text("Hello", 50, doc.y)   // writes "Hello" at X=50, current Y
> doc.text("World", 150, doc.y)  // writes "World" at X=150, same Y
> ```
>
> ### 🔹 `{ continued: true }`
>
> * By default, when you call `.text()`, PDFKit **moves the cursor to the next line** after finishing the text.
> * `{ continued: true }` tells PDFKit:
>   > “Don’t move down yet — I’m going to add more text on this same line.”
>   >
>
> So in your header row:
>
> ```js
> doc.text("ID", 50, doc.y, { continued: true })
> doc.text("Name", 150, doc.y, { continued: true })
> doc.text("Category", 300, doc.y, { continued: true })
> doc.text("Price", 400, doc.y, { continued: true })
> doc.text("Stock", 470, doc.y)  // no continued → now cursor can move down
> ```
>
> This way, all the columns appear side-by-side on the same row.
>
> Finally, you call:
>
> ```js
> doc.moveDown()
> ```
>
> which tells PDFKit to move the Y position down to the next line (so the product rows can start).
>
> ✅ **In short:**
>
> * `doc.y` = current row’s vertical position.
> * `{ continued: true }` = keep writing on the same line without moving down.

### Now in the Frontend--

Right now, you’re using `axios.post` and expecting `response.data` (JSON). But your backend routes for CSV/PDF won't be sending JSON  — they’re sending  **binary files with `Content-Disposition: attachment`** . Browsers only auto-download if you **hit the route directly** (via `<a href>` or direct URL). When you use `axios`, it just gives you raw binary data in memory, not a download.

✅ Fix: Handle file download in React

You need to tell Axios to expect a **blob** and then create a download link manually. Example:

```javascript
const exportFile = async (type) => {
      try {
        const response = await axios.post(`${baseApiUrl}/products/export/${type}`, {products}, {withCredentials: true, responseType: 'blob'})

        const fileBlob = new Blob([response.data], { type: type === 'csv' ? 'text/csv' : 'application/pdf'})

        const url = window.URL.createObjectURL(fileBlob)
        const link = document.createElement("a");
        link.href = url;
        link.download = type === "csv" ? "products.csv" : "products.pdf"
        document.body.appendChild(link);
        link.click()

        link.remove()
        window.URL.revokeObjectURL(url);

      }
      catch (error) {
        console.error("Error exporting file:", error.message)
      }
    }
```

**🔑 Note**

1. `responseType: 'blob'` → so axios treats it as binary, not JSON.
2. We use `Blob` + `URL.createObjectURL` to create a downloadable file.
3. Instead of `navigate`, we programmatically trigger the download.

---

# ----Node.js & its core characteristics

### 🟢 What is  **Node.js** ?

**Node.js** is a **runtime environment** that lets you run JavaScript **outside the browser** (mainly on servers).

### ⚙️ Key Features of Node.js

##### 1. 🧵 Asynchronous & Non-Blocking I/O

* Node.js uses **non-blocking operations**
* It doesn’t wait for tasks like file read, DB call, API call

👉 Example:

```js
fs.readFile('file.txt', () => {
  console.log('done');
});
console.log('next');
```

Output:

```
next
done
```

✔️ This makes it **very fast and scalable**

##### 2. 🔁 Event-Driven Architecture

* Uses an **event loop**
* Tasks are handled via **callbacks, promises, async/await**

👉 Ideal for:

* Real-time apps (chat apps, notifications)
* APIs

##### 3. ⚡ Single-Threaded (But Not Weak!)

* Node.js runs on **one main thread**
* But handles thousands of requests using:
  * Event loop
  * Background worker threads (internally via libuv)

👉 So:

* ❌ Not traditionally multi-threaded
* ✅ But **concurrent**

##### 4. 📦 Huge Ecosystem (NPM)

* Comes with **npm**
* Millions of packages

👉 Example:

* Express (backend framework)
* Mongoose (MongoDB ORM)

##### 5. 🌐 Perfect for Network Apps

* Built-in modules:
  * `http`
  * `fs`
  * `path`
* Easy to build APIs and servers

##### 6. 🧩 Cross-Platform

* Runs on:
  * Windows
  * Linux
  * macOS

##### 7. ⚙️ Fast Execution (V8 Engine)

* Uses **Google V8**
* Compiles JS → machine code

### 🔄 Is Node.js Synchronous or Asynchronous?

👉  **Both** , but primarily:

* ✅ **Asynchronous (default & preferred)**
* ❌ Synchronous APIs exist but rarely used in production

👉 Example:

```js
fs.readFileSync() // blocking ❌
fs.readFile()     // non-blocking ✅
```

### 🧵 Is Node.js Multi-threaded?

👉 Official answer:

* ❌ Single-threaded (main thread)
* ✅ Uses **thread pool internally** (libuv)

👉 So:

* You write **single-threaded code**
* Node handles concurrency behind the scenes

### ⚔️ Node.js vs JavaScript (Browser)

| Feature            | Node.js               | JavaScript (Browser)                     |
| ------------------ | --------------------- | ---------------------------------------- |
| Environment        | Server-side           | Client-side (browser)                    |
| Engine             | V8                    | V8 (Chrome), SpiderMonkey (Firefox), etc |
| File System Access | ✅ Yes (`fs`)       | ❌ No                                    |
| DOM Access         | ❌ No                 | ✅ Yes                                   |
| Modules            | CommonJS / ES Modules | ES Modules                               |
| Networking         | Direct (http module)  | Via browser APIs (fetch, XHR)            |
| Use Case           | Backend, APIs, tools  | UI, frontend logic                       |
| Event Loop         | Yes                   | Yes                                      |

### 🧩 Simple Analogy

* **JavaScript** = Language (like English 📝)
* **Node.js** = Environment where you use it (like office 🏢)
* **Browser JS** = Another environment (like classroom 🎓)

### ⚡ When to Use Node.js

✅ Good for:

* APIs (REST, GraphQL)
* Real-time apps (chat, sockets)
* Microservices
* Streaming apps

❌ Not ideal for:

* Heavy CPU tasks (image processing, ML)
  * (Better use worker threads or other languages)

### 🔚 Final Summary

* Node.js is:
  * ⚡ Fast
  * 🔁 Event-driven
  * 🧵 Single-threaded but concurrent
  * 📦 Huge ecosystem
* Mostly **asynchronous**
* Extends JavaScript beyond the browser

---

# ----Important Folders of Node js

🧱 3 Important Folders in Node.js Source Code

### 1. 📦 **deps/** (Dependencies)

This folder contains  **external libraries that Node.js depends on** .

🔑 Key components inside:

##### ⚡ Google V8

* The **JavaScript engine**
* Converts JS → machine code
* Handles:
  * Execution of JS
  * Memory management (garbage collection)
  * JIT compilation

👉 Without V8:

* Node.js cannot run JavaScript

##### 🧵 libuv

* Core library for **async behavior**
* Handles:
  * Event loop
  * Thread pool
  * Non-blocking I/O
  * File system operations
  * Networking

👉 This is why Node.js feels “asynchronous” even though your code is single-threaded.

##### 🧰 Other things in `deps/`

* OpenSSL → security (HTTPS, encryption)
* zlib → compression
* nghttp2 → HTTP/2 support

**🧠 Summary of `deps/`**

👉 External powerhouses:

* V8 → runs JS
* libuv → makes it async

### 2. ⚙️ **src/** (Core C++ Implementation)

This is the **heart of Node.js internals** written in C++.

**What happens here?**

* Glue between:
  * V8 (JS world)
  * libuv (system world)

**Responsibilities:**

* Binding JS → C++ functions
* Implementing core modules:
  * `fs`
  * `http`
  * `net`
* Managing event loop integration
* Handling low-level system calls

🧠 Example flow:

```js
fs.readFile('file.txt', cb)
```

👉 What happens internally:

1. JS call goes to **src/**
2. src calls **libuv**
3. libuv uses OS thread pool
4. Result comes back → callback executed

**🧠 Summary of `src/`**

👉 The **bridge layer**

* Connects JS (V8) ↔ system (libuv)

### 3. 📚 **lib/** (JavaScript Core Library)

This folder contains  **JavaScript code written by Node.js authors** .

### What’s inside?

* High-level modules:
  * `fs.js`
  * `http.js`
  * `events.js`
  * `stream.js`

### 🔑 Key point:

These are **not magic** — just JS built on top of C++ bindings.

👉 Example:

* `fs.readFile()`
  → defined in `lib/fs.js`
  → internally calls C++ binding in `src/`

### 🧠 Why this matters:

* You can actually read Node’s internal JS code
* Helps understand:
  * Streams
  * EventEmitter
  * Buffers

### 🔄 How All 3 Work Together

### Flow diagram:

```
Your JS Code
     ↓
lib/ (JS implementation)
     ↓
src/ (C++ bindings)
     ↓
deps/
   ├── V8 (executes JS)
   └── libuv (handles async + OS)
```

### ⚡ Interview-Level Understanding

**💡 Key Points:**

* Node.js is **not just JavaScript**
* It’s a combination of:
  * V8 (execution)
  * libuv (async system)
  * C++ bindings (src)
  * JS APIs (lib)

### 🎯 Simple Analogy

Think of Node.js like a restaurant 🍽️

* `lib/` → Waiter (takes your order in JS)
* `src/` → Kitchen manager (translates order)
* `deps/`
  * V8 → Chef (cooks JS)
  * libuv → Staff handling multiple orders simultaneously

---

# ----Types of Modules in Node.js

Node.js mainly has  **3 types of modules** :

### 1. 🧱 Core Modules (Built-in)

These come **pre-installed with Node.js** — no need to install anything.

Examples:

* `fs` → File system
* `http` → Create servers
* `path` → Handle file paths
* `events` → EventEmitter

👉 Usage:

```js
const fs = require('fs');
```

✔️ Fast
✔️ Highly optimized (written in C++ + JS internally)

### 2. 📁 Local Modules (Custom Modules)

These are  **your own files/modules** .

👉 Example:

**math.js**

```js
function add(a, b) {
  return a + b;
}

module.exports = add;
```

**app.js**

```js
const add = require('./math');
console.log(add(2, 3));
```

✔️ Helps in modular code
✔️ Reusability

### 3. 📦 Third-Party Modules

Installed via **npm**

👉 Example:

```bash
npm install express
```

```js
const express = require('express');
```

✔️ Saves time
✔️ Huge ecosystem

### 🧠 Quick Summary

| Type        | Source    | Example    |
| ----------- | --------- | ---------- |
| Core        | Built-in  | fs, http   |
| Local       | Your code | ./utils.js |
| Third-party | npm       | express    |

---

# Module Wrapper Function

Here’s where things get interesting 👀

👉 Node.js **wraps every module inside a function** before executing it.

### 🔍 Actual Wrapper (internally)

```js
(function (exports, require, module, __filename, __dirname) {
  // Your code here
});
```

👉 You don’t see this — Node adds it automatically.

### 🤔 Why this wrapper exists?

##### 1. 🛡️ Scope Isolation

* Variables are **NOT global**
* Each file has its own scope

```js
var x = 10;
```

👉 `x` is NOT global — thanks to wrapper

##### 2. 📦 Module System Support

* Enables:
  * `module.exports`
  * `require()`

##### 3. 🧭 Provides useful variables

You get:

* `__dirname`
* `__filename`

### 🧩 Parameters of Wrapper Function

Let’s break each one clearly.

##### 1. 📤 `exports`

👉 Shortcut to export values

```js
exports.add = (a, b) => a + b;
```

💡 Internally:

```js
exports === module.exports // true (initially)
```

##### 2. 📦 `module`

👉 Represents the current module

```js
module.exports = function() {};
```

✔️ This is the **actual export object**

**⚠️ Important Interview Point:**

```js
exports = {}           // ❌ breaks link
module.exports = {}   // ✅ correct
```

##### 3. 📥 `require`

👉 Function to import modules

```js
const fs = require('fs');
```

✔️ Resolves:

* Core modules
* Local files
* npm packages

##### 4. 📄 `__filename`

👉 Full path of current file

```js
console.log(__filename);
```

✔️ Includes file name

##### 5. 📁 `__dirname`

👉 Directory path of current file

```js
console.log(__dirname);
```

✔️ Excludes file name

### 🔄 How It All Works Together

```js
// your file
console.log('Hello');
```

👉 Node converts it internally to:

```js
(function (exports, require, module, __filename, __dirname) {
  console.log('Hello');
});
```

---

# ----Path Module- `.join() & .resolve()`

Let’s break down **`path.join()` vs `path.resolve()`** from the Node.js `path` module in a very clear way.

### 📦 About `path` module

* Built-in Node.js module for handling file paths
* No install needed

```js
const path = require('path');
```

### 🔗 `path.join()`

**👉 What it does:**

* Joins multiple path segments into **one normalized path**
* Handles:
  * `/` automatically
  * Removes redundant separators
  * Resolves `.` and `..`

### ✅ Examples

**1. Basic join**

```js
path.join('folder', 'file.txt');
```

👉 Output:

```
folder/file.txt
```

**2. Handles extra slashes**

```js
path.join('folder/', '/subfolder', 'file.txt');
```

👉 Output:

```
folder/subfolder/file.txt
```

**3. Resolves `..`**

```js
path.join('folder', 'sub', '..', 'file.txt');
```

👉 Output:

```
folder/file.txt
```

**4. Relative path stays relative**

```js
path.join('a', 'b');
```

👉 Output:

```
a/b
```

**🧠 Key Point:**

👉 `join()` does **NOT** make path absolute
👉 It just **concatenates + cleans**

### 📍 `path.resolve()`

**👉 What it does:**

* Resolves a sequence into an **absolute path**
* Starts from:
  * Right → Left
  * Stops when it finds an **absolute path**

### ✅ Examples

**1. Basic resolve**

```js
path.resolve('folder', 'file.txt');
```

👉 Output (example):

```
/current/working/directory/folder/file.txt
```

**2. Absolute path overrides everything**

```js
path.resolve('a', '/b', 'c');
```

👉 Output:

```
/b/c
```

✔️ Because `/b` is absolute → ignores `a`

**3. Works right-to-left**

```js
path.resolve('a', 'b', '..', 'c');
```

👉 Output:

```
/current/dir/a/c
```

**4. No args**

```js
path.resolve();
```

👉 Output:

```
current working directory
```

**🧠 Key Point:**

👉 Always returns **absolute path**

### ⚔️ `join()` vs `resolve()`

| Feature                | `path.join()`      | `path.resolve()`          |
| ---------------------- | -------------------- | --------------------------- |
| Output                 | Relative or absolute | Always absolute             |
| Starts from            | Left → Right        | Right → Left               |
| Handles absolute paths | No override          | Stops at first absolute     |
| Uses current directory | ❌ No                | ✅ Yes                      |
| Main purpose           | Combine paths        | Resolve final absolute path |

### 🔥 Side-by-Side Examples

**✅ Example 1**

```js
path.join('a', 'b');
```

👉 Output:

```
a/b
```

✔️ Just joined
❌ No absolute path

```js
path.resolve('a', 'b');
```

👉 Output:

```
/current/folder/a/b
```

✔️ Adds current directory automatically

**🔥 Example 2 (IMPORTANT)**

```js
path.join('/a', '/b', 'c');
```

👉 Output:

```
/a/b/c
```

✔️ It **keeps everything**

```js
path.resolve('/a', '/b', 'c');
```

👉 Output:

```
/b/c
```

⚠️ Why?

👉 Because:

* `/b` is **absolute**
* So `resolve()` **ignores everything before it**

💡 Rule:

👉 `resolve()` stops when it sees an absolute path

**🔥 Example 3 (with `..`)**

```js
path.join('a', '..', 'b');
```

👉 Output:

```
b
```

✔️ Go to `a`
✔️ `..` → go back
✔️ then `b`

```js
path.resolve('a', '..', 'b');
```

👉 Output:

```
/current/folder/b
```

✔️ Same logic
✔️ But gives full absolute path

### 🎯 When to Use What?

**✅ Use `join()` when:**

* You just want to **build a path**
* Don’t care if it’s absolute

**✅ Use `resolve()` when:**

* You need **absolute path**
* Working with file system operations
* Avoid path confusion

### 🧠 Simple Analogy

* `join()` → “Just combine these pieces 🧩”
* `resolve()` → “Give me the  **final exact location 📍** ”

---

# ----Fs Module- Flag & Mode options in `.readFile() & .writeFile()`

### 📦 `fs.readFile()` → **flag option**

**👉 What is `flag`?**

It tells Node.js **how to open the file** before reading it.

**🧠 Default:**

```js
fs.readFile(path, options, callback)
```

If you don’t pass anything:

```js
flag = 'r'
```

👉 `'r'` = **read-only**

### 🔑 Common Flags (for `readFile`)

##### 1. `'r'` (default)

* Open file for reading
* ❌ Error if file doesn’t exist

```js
fs.readFile('file.txt', { flag: 'r' }, cb);
```

##### 2. `'r+'` 🔄

* Read **and write**
* ❌ Error if file doesn’t exist

👉 Rare with `readFile`, but possible

##### 3. `'rs'` ⚡

* Read in **synchronous mode internally**
* Bypasses OS cache

👉 Used when:

* You want **fresh data from disk**
* (very rare)

##### 4. `'rs+'`

* Read + write + bypass cache

**🎯 Key Idea:**

👉 In `readFile()`, flags mostly control:

* Read vs read+write
* Cache behavior

### 📦 `fs.writeFile()` → **flag + mode**

#### 🔹 1. `flag` in `writeFile()`

👉 Controls **how writing happens**

##### 1. `'w'` (default) ✍️

* Write to file
* ✅ Creates file if not exists
* ⚠️ Overwrites existing content

```js
fs.writeFile('file.txt', 'Hello', { flag: 'w' }, cb);
```

##### 2. `'a'` ➕

* Append to file
* ✅ Creates if not exists
* ❌ Does NOT overwrite

```js
fs.writeFile('file.txt', 'Hello', { flag: 'a' }, cb);
```

##### 3. `'wx'` 🚫

* Write only if file **does NOT exist**
* ❌ Throws error if exists

👉 Useful for:

* Preventing accidental overwrite

##### 4. `'ax'`

* Append only if file doesn’t exist

##### 5. `'w+'`

* Read + write
* Overwrites file

**🎯 Key Idea:**

👉 In `writeFile()`:

* `'w'` → replace
* `'a'` → add
* `'x'` → safety (no overwrite)

### 🔹 2. `mode` in `writeFile()`

👉 Controls **file permissions** (who can read/write/execute)

**🧠 Default:**

```js
mode = 0o666
```

👉 Meaning:

* Owner: read + write
* Group: read + write
* Others: read + write

**🔑 Example:**

```js
fs.writeFile('file.txt', 'Hello', {
  mode: 0o444
}, cb);
```

👉 File becomes:

* Read-only for everyone 👀

##### 🧩 Permission Breakdown

| Value | Meaning |
| ----- | ------- |
| 4     | Read    |
| 2     | Write   |
| 1     | Execute |

Example:

```js
mode: 0o755
```

👉 Means:

* Owner: 7 → rwx
* Group: 5 → r-x
* Others: 5 → r-x

### ⚔️ `flag` vs `mode`

| Feature  | flag                        | mode                |
| -------- | --------------------------- | ------------------- |
| Purpose  | How file is opened          | File permissions    |
| Controls | Read/write/append/overwrite | Who can access      |
| Example  | `'w'`,`'a'`             | `0o666`,`0o755` |

### 🔥 Real-World Examples

**✅ Example 1: Safe Write (no overwrite)**

```js
fs.writeFile('file.txt', 'Hello', { flag: 'wx' }, (err) => {
  if (err) console.log('File already exists!');
});
```

**✅ Example 2: Append logs**

```js
fs.writeFile('log.txt', 'New log\n', { flag: 'a' }, cb);
```

**✅ Example 3: Secure file**

```js
fs.writeFile('secret.txt', 'Top Secret', {
  mode: 0o600
}, cb);
```

👉 Only owner can read/write 🔐

### 🎯 Final Takeaway

**📖 `readFile()`**

* `flag` → how to read (mostly `'r'`)

**✍️ `writeFile()`**

* `flag` → overwrite / append / safe write
* `mode` → permissions

---

# ----Fs vs Fs/promises Module

This is exactly where **modern Node.js vs old style** becomes clear 🔥

Both are from the same module, but they differ in  **how you handle async code** .

### 🧠 Core Difference

| Module               | Style                  |
| -------------------- | ---------------------- |
| `node:fs`          | Callback-based         |
| `node:fs/promises` | Promise-based (modern) |

### 🔹 1. Using `node:fs` (Callback Style)

```js
const fs = require('node:fs');

fs.readFile('file.txt', 'utf-8', (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});
```

**❌ Problems:**

* Callback nesting 😵
* Harder error handling
* Less readable

### 🔹 2. Using `node:fs/promises` (Modern Way)

```js
const fs = require('node:fs/promises');

async function read() {
  try {
    const data = await fs.readFile('file.txt', 'utf-8');
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}

read();
```

**✅ Benefits:**

* Clean with `async/await`
* Better error handling
* Easier to scale

### 🔥 Why `node:fs/promises` Exists

Because JavaScript evolved to use:

* Promises
* `async/await`

👉 So Node.js introduced a **promise-based API** instead of forcing callbacks.

### 🧩 Side-by-Side Comparison

**Read File**

**Callback (old):**

```js
fs.readFile('file.txt', cb);
```

**Promise (new):**

```js
await fs.readFile('file.txt');
```

**Write File**

**Callback:**

```js
fs.writeFile('file.txt', 'Hello', cb);
```

**Promise:**

```js
await fs.writeFile('file.txt', 'Hello');
```

### ⚡ Important Insight

👉 Internally:

* Both use same core system (libuv etc.)
* Only **API style differs**

### 🧠 When to Use What?

**✅ Use `node:fs/promises` (Recommended)**

* Modern apps
* Clean code
* Async/await usage

**⚠️ Use `node:fs` when:**

* Working with old codebase
* Need streams or special APIs (some still callback-based)

---

# ----Events Module

The **`events` module** is one of the most important core concepts in Node.js 🔥

It’s the foundation of how Node handles  **asynchronous, event-driven behavior** .

Let’s make it simple and intuitive.

### 📦 What is the `events` module?

The `events` module lets you:
👉 **Create, listen to, and respond to events**

At the center of it is:

👉 **EventEmitter**

### 🧠 Real-Life Analogy

Think of it like a **YouTube subscription system ▶️**

* You **subscribe** to a channel → `on()`
* Creator uploads a video → `emit()`
* You get notified → callback runs

### ⚙️ Basic Usage

```js
const EventEmitter = require('node:events');

const emitter = new EventEmitter();

// listen
emitter.on('greet', () => {
  console.log('Hello!');
});

// trigger
emitter.emit('greet');
```

👉 Output:

```
Hello!
```

### 🔑 Core Concepts

#### 1. 📡 `emit()` → Trigger event

```js
emitter.emit('eventName');
```

👉 “Hey, this event happened!”

#### 2. 👂 `on()` → Listen to event

```js
emitter.on('eventName', callback);
```

👉 “When this happens, run this”

#### 3. 📥 Passing Data

```js
emitter.on('userLogin', (username) => {
  console.log(`${username} logged in`);
});

emitter.emit('userLogin', 'Arjun');
```

👉 Output:

```
Arjun logged in
```

#### 4. 🔁 `once()` → Run only once

```js
emitter.once('start', () => {
  console.log('Started!');
});

emitter.emit('start');
emitter.emit('start');
```

👉 Output:

```
Started!
```

✔️ Runs only first time

#### 5. ❌ `off()` / `removeListener()`

```js
function greet() {
  console.log('Hi');
}

emitter.on('greet', greet);
emitter.off('greet', greet);
```

👉 Removes listener

### ⚠️ Important Rule

👉 **Listeners must be registered BEFORE emitting**

```js
emitter.emit('test'); // ❌ nothing happens

emitter.on('test', () => console.log('Hi'));
```

### 🧠 Why Events Are Important in Node.js

Node.js is:

* ⚡ Event-driven
* 🧵 Non-blocking

👉 Internally uses events for:

* HTTP requests
* Streams
* File system operations

### 🔥 Real Example (Practical)

```js
const EventEmitter = require('node:events');
const emitter = new EventEmitter();

// listener
emitter.on('orderPlaced', (item) => {
  console.log(`Order received: ${item}`);
});

// trigger
emitter.emit('orderPlaced', 'Laptop');
```

### 🧩 Custom EventEmitter (Advanced)

You can extend it:

```js
class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

myEmitter.on('ping', () => {
  console.log('pong');
});

myEmitter.emit('ping');
```

### ⚠️ Error Handling (VERY IMPORTANT)

👉 Special event: `'error'`

```js
emitter.emit('error', new Error('Something broke'));
```

❌ If no listener → app crashes

**✅ Proper way:**

```js
emitter.on('error', (err) => {
  console.error(err.message);
});
```

### 🧠 Behind the Scenes

* EventEmitter stores listeners in memory
* When `emit()` is called:
  * It loops through listeners
  * Executes them synchronously

👉 Yes — **listeners run synchronously** (important!)

### ⚡ Sync vs Async Insight

```js
emitter.on('test', () => console.log('A'));
emitter.on('test', () => console.log('B'));

emitter.emit('test');
```

👉 Output:

```
A
B
```

✔️ Runs in order
✔️ Synchronous execution

---

# ----Events Module- Use cases & real scenerios

Instead of theory, let’s walk through **real-world scenarios** you’d build as a developer.

### 🎯 Why EventEmitter Exists (Real Need)

In real apps, you often want:

👉 “When X happens → trigger multiple independent things”

Without tightly coupling everything.

### 🛒 Scenario 1: E-commerce Order System (VERY REAL)

**🧠 Problem:**

When a user places an order, multiple things should happen:

* Save order ✅
* Send email 📧
* Update inventory 📦
* Log analytics 📊

👉 You **don’t want all this logic in one function** ❌

**✅ Solution using events**

```js
const EventEmitter = require('node:events');
const emitter = new EventEmitter();

// listeners
emitter.on('orderPlaced', (order) => {
  console.log('📧 Email sent for', order.id);
});

emitter.on('orderPlaced', (order) => {
  console.log('📦 Inventory updated for', order.id);
});

emitter.on('orderPlaced', (order) => {
  console.log('📊 Analytics tracked for', order.id);
});

// main logic
function placeOrder(order) {
  console.log('✅ Order saved:', order.id);

  emitter.emit('orderPlaced', order);
}

// trigger
placeOrder({ id: 101 });
```

**💡 Why this is powerful:**

* Each feature is **independent**
* Easy to:
  * Add new listeners later
  * Remove features without breaking code

👉 This is called **decoupling**

### 🔔 Scenario 2: Notification System

**🧠 Problem:**

User performs an action → notify via:

* Email
* SMS
* Push notification

**✅ Event-based solution**

```js
emitter.on('userRegistered', (user) => {
  console.log('📧 Email sent to', user.email);
});

emitter.on('userRegistered', (user) => {
  console.log('📱 SMS sent to', user.phone);
});

function registerUser(user) {
  console.log('User created');

  emitter.emit('userRegistered', user);
}
```

### 🌊 Scenario 3: Streams (Internal Node.js Usage)

Node.js uses events heavily in streams:

```js
const fs = require('node:fs');

const stream = fs.createReadStream('file.txt');

stream.on('data', (chunk) => {
  console.log('Received chunk');
});

stream.on('end', () => {
  console.log('Finished reading');
});
```

**💡 What’s happening?**

* `'data'` → emitted when chunk arrives
* `'end'` → emitted when done

👉 This is pure **EventEmitter in action**

### 🌐 Scenario 4: HTTP Server (Behind the Scenes)

When a request hits a server:

```js
const http = require('node:http');

http.createServer((req, res) => {
  res.end('Hello');
});
```

**👉 Internally:**

* Node emits a **"request" event**
* Your callback is just a listener

### 🧩 Scenario 5: Logging System

**🧠 Problem:**

Whenever something happens → log it

```js
emitter.on('errorOccurred', (err) => {
  console.log('📝 Logged error:', err.message);
});

function doSomething() {
  try {
    throw new Error('Oops');
  } catch (err) {
    emitter.emit('errorOccurred', err);
  }
}
```

### ⚡ Why Not Just Call Functions Directly?

Good question 👇

**❌ Without events (tight coupling)**

```js
function placeOrder(order) {
  saveOrder(order);
  sendEmail(order);
  updateInventory(order);
  trackAnalytics(order);
}
```

👉 Problems:

* Hard to maintain 😵
* Hard to extend
* Everything is tightly linked

**✅ With events (loose coupling)**

```js
function placeOrder(order) {
  saveOrder(order);
  emitter.emit('orderPlaced', order);
}
```

👉 Benefits:

* Clean separation ✨
* Plug-and-play features 🔌
* Scalable architecture 📈

### 🧠 When Should YOU Use Events?

**✅ Use EventEmitter when:**

* One action triggers **multiple side effects**
* You want **loose coupling**
* Building:
  * Notifications 🔔
  * Logging systems 📝
  * Background tasks ⚙️
  * Real-time systems ⚡

**❌ Avoid when:**

* Simple function calls are enough
* Over-engineering small logic

### 🎯 Final Takeaway

👉 EventEmitter is about:

* Decoupling logic
* Handling async flows
* Making systems scalable

### 🧠 One-Line Real Meaning

👉 **“Something happened → whoever cares can react”**

---

# ----Buffer in detail

Buffers are one of those **core Node.js concepts** that suddenly make a lot of things (streams, files, networking) click 🔥

Let’s build it step-by-step so it actually  *makes sense* .

### 🧠 What is a Buffer?

👉 A **Buffer** is a way to store **raw binary data** (bytes) in Node.js.

```js
const buf = Buffer.from('Hello');
```

### 💡 Why do we need Buffers?

JavaScript normally works with:

* Strings
* Objects

But systems (files, network, images, videos) deal with:
👉 **binary data (0s and 1s)**

So Node.js provides:

👉 **Buffer**

to handle that.

### 🧩 Real-World Analogy

Think of Buffer like:
📦 **A box of bytes**

* Each slot = 1 byte (8 bits)
* Stores raw data exactly as system understands

### 🔢 Example

```js
const buf = Buffer.from('ABC');
console.log(buf);
```

👉 Output:

```
<Buffer 41 42 43>
```

✔️ These are **hex values**

* A → 41
* B → 42
* C → 43

### ⚙️ How Buffers Work

* Fixed-size memory allocation
* Stored outside V8 heap (important!)
* Faster for binary operations

### 📦 Ways to Create Buffers

##### 1. From string

```js
Buffer.from('Hello');
```

##### 2. From array

```js
Buffer.from([65, 66, 67]);
```

👉 Output → "ABC"

##### 3. Allocate empty buffer

```js
Buffer.alloc(10);
```

✔️ Safe (filled with zeros)

> ##### 🧠 `Buffer.alloc(10)` → What it really means
>
> ```js
> const buf = Buffer.alloc(10);
> ```
>
> 👉 This creates:
>
> ✔️ **10 slots**
> ✔️ Each slot = **1 byte (8 bits)**
>
> 👉 So total = **10 bytes**
>
> ##### 🔍 Visualizing it
>
> ```
> [ 00 ][ 00 ][ 00 ][ 00 ][ 00 ][ 00 ][ 00 ][ 00 ][ 00 ][ 00 ]
>    1     2     3     4     5     6     7     8     9    10
> ```
>
> ✔️ Each box = 1 byte
> ✔️ Filled with `0` (safe allocation)
>
> ##### ⚡ Important Clarification
>
> 👉 1 byte = can store:
>
> * A number (0–255)
> * OR part of a character (depending on encoding)

##### 4. Allocate uninitialized (faster but risky)

```js
Buffer.allocUnsafe(10);
```

⚠️ May contain old memory data

### 🔍 Reading & Writing

**Access bytes**

```js
const buf = Buffer.from('ABC');

console.log(buf[0]); // 65
```

**Modify**

```js
buf[0] = 97;
console.log(buf.toString()); // "aBC"
```

### 🔄 Encoding & Decoding

**String → Buffer**

```js
Buffer.from('Hello', 'utf-8');
```

**Buffer → String**

```js
buf.toString('utf-8');
```

### Other encodings:

* `utf-8` (default)
* `base64`
* `hex`

### 📡 Where Buffers Are Used (VERY IMPORTANT)

**1. 📁 File System**

```js
const fs = require('node:fs');

fs.readFile('file.txt', (err, data) => {
  console.log(data); // Buffer
});
```

👉 Why Buffer?

* File is stored as **binary on disk**

**2. 🌐 HTTP R**






























**equests**

```js
req.on('data', (chunk) => {
  console.log(chunk); // Buffer
});
```

👉 Data comes in **chunks (buffers)**
👉 Useful for large uploads

**3. 🌊 Streams (MOST IMPORTANT)**

```js
const fs = require('node:fs');

const stream = fs.createReadStream('bigfile.txt');

stream.on('data', (chunk) => {
  console.log(chunk); // Buffer
});
```

👉 Instead of loading full file ❌
👉 Node processes **piece by piece (buffers)** ✅

**4. 🖼️ Image Handling**

```js
const fs = require('node:fs');

const img = fs.readFileSync('image.png');
```

👉 This is pure binary → Buffer
👉 You don’t convert to string

**5. 🖼️ Binary Data**

* Images
* Videos
* PDFs

### ⚡ Buffers vs Strings

| Feature  | Buffer         | String   |
| -------- | -------------- | -------- |
| Type     | Binary         | Text     |
| Mutable  | ✅ Yes         | ❌ No    |
| Use case | Files, streams | UI, logs |

### ⚠️ Important Concepts

**1. Fixed Size**

```js
const buf = Buffer.alloc(3);
```

👉 Cannot grow dynamically

**2. Memory Efficient**

* Stored outside JS heap
* Avoids GC overhead

**3. Chunk Processing**

Large files are processed in **small buffers**

👉 Prevents memory crash

### 🔥 Example: Real Use Case

```js
const fs = require('node:fs');

fs.createReadStream('bigfile.txt')
  .on('data', (chunk) => {
    console.log('Chunk:', chunk);
  });
```

👉 Each `chunk` = Buffer

**🧠 Behind the Scenes**

* Node receives raw data → stores in Buffer
* Converts to string **only when needed**

### 🎯 Common Mistakes

**❌ Assuming data is string**

```js
console.log(data); // Buffer, not string
```

✔️ Fix:

```js
console.log(data.toString());
```

**❌ Using allocUnsafe blindly**

👉 Can expose old memory (security risk)

### 🧰 Important Buffer Built-in Methods

Let’s cover the ones that actually matter 👇

##### 🔹 1. `Buffer.from()`

👉 Create buffer from data

```js
Buffer.from('Hello');
Buffer.from([65, 66, 67]);
```

##### 🔹 2. `Buffer.alloc()`

👉 Safe allocation

```js
Buffer.alloc(5);
```

✔️ Filled with zeros

##### 🔹 3. `Buffer.allocUnsafe()`

👉 Fast but risky

```js
Buffer.allocUnsafe(5);
```

⚠️ May contain garbage data

##### 🔹 4. `.toString()`

👉 Convert buffer → string

```js
const buf = Buffer.from('Hello');
buf.toString(); // "Hello"
```

##### 🔹 5. `.write()`

👉 Write into buffer

```js
const buf = Buffer.alloc(10);
buf.write('Hi');
console.log(buf.toString());
```

##### 🔹 6. `.slice()`

👉 Get part of buffer

```js
const buf = Buffer.from('Hello');

const sub = buf.slice(0, 2);
console.log(sub.toString()); // "He"
```

##### 🔹 7. `.copy()`

👉 Copy data to another buffer

```js
const buf1 = Buffer.from('Hello');
const buf2 = Buffer.alloc(5);

buf1.copy(buf2);
console.log(buf2.toString());
```

##### 🔹 8. `.length`

👉 Size of buffer

```js
const buf = Buffer.from('Hello');
console.log(buf.length); // 5
```

##### 🔹 9. `.fill()`

👉 Fill buffer with value

```js
const buf = Buffer.alloc(5);
buf.fill(1);

console.log(buf); // <Buffer 01 01 01 01 01>
```

##### 🔹 10. `.equals()`

👉 Compare buffers

```js
buf1.equals(buf2);
```

##### 🔹 11. `.concat()`

👉 Combine multiple buffers

```js
const b1 = Buffer.from('Hello ');
const b2 = Buffer.from('World');

const result = Buffer.concat([b1, b2]);
console.log(result.toString());
```

**🔥 Mini Real Example (Combining Chunks)**

```js
let chunks = [];

req.on('data', (chunk) => {
  chunks.push(chunk);
});

req.on('end', () => {
  const data = Buffer.concat(chunks);
  console.log(data.toString());
});
```

👉 This is **real backend logic**

---

# ----Stream Module- Transform Stream

**Transform streams** are where streams become really powerful 🔥

If you understand this well, you’ll unlock a lot of real-world Node.js use cases.

### 🌊 What is a Transform Stream?

A **Transform stream** is a special type of stream that:

👉 **Reads data → modifies it → outputs new data**

### 🧠 In one line:

👉 **“Input comes in, gets transformed, and goes out”**

### 🔄 Stream Types Recap

| Type                | Can Read | Can Write                |
| ------------------- | -------- | ------------------------ |
| Readable            | ✅       | ❌                       |
| Writable            | ❌       | ✅                       |
| Duplex              | ✅       | ✅                       |
| **Transform** | ✅       | ✅ (with transformation) |

### ⚙️ Core Idea

Transform streams implement a method:

```js
_transform(chunk, encoding, callback)
```

👉 This runs **for every chunk**

### 🧩 Simple Example

##### Uppercase Transformer (usual way- without using class)

```js
const { Transform } = require('node:stream');

const upperCase = new Transform({
  transform(chunk, encoding, callback) {
    const result = chunk.toString().toUpperCase();
    callback(null, result);
  }
});

process.stdin.pipe(upperCase).pipe(process.stdout);
```

### 🧠 What happens?

```id=
input:  hello
output: HELLO
```

✔️ Reads input
✔️ Transforms it
✔️ Sends output

> #### ❓“Are we passing an object to a class?”
>
> Code:
>
> ```js
> const upperCase = new Transform({
>   transform(chunk, encoding, callback) {
>     const result = chunk.toString().toUpperCase();
>     callback(null, result);
>   }
> });
> ```
>
> ✅ What’s actually happening?
>
> 👉 **`Transform` is a class** (from `node:stream`)
> 👉 And yes, you are **passing an object**
>
> BUT 👇
>
> 👉 That object is  **configuration options** , not random data.
>
> ##### 🧠 How this works internally
>
> When you do:
>
> ```js
> new Transform({ transform() {} })
> ```
>
> 👉 Node internally does something like:
>
> ```js
> class Transform {
>   constructor(options) {
>     if (options.transform) {
>       this._transform = options.transform;
>     }
>   }
> }
> ```
>
> ##### 💡 So:
>
> * You are **instantiating the class** ✅
> * You are **passing an options object** ✅
> * That object contains the implementation of `_transform` indirectly
>
> #### ❓  `_transform` vs `transform`
>
> **🧠 Official Rule**
>
> 👉 Internally, Node expects:
>
> ```js
> _transform(chunk, encoding, callback)
> ```
>
> **❓ Then why are we writing `transform`?**
>
> 👉 Because Node provides a shortcut:
>
> ###### ✔️ Option 1 (Shortcut - what we used)
>
> ```js
> new Transform({
>   transform(chunk, enc, cb) {
>     cb(null, chunk.toString().toUpperCase());
>   }
> });
> ```
>
> 👉 Node internally maps:
>
> ```js
> transform → _transform
> ```
>
> ###### ✔️ Option 2 (Manual way - more explicit)
>
> ```js
> const { Transform } = require('node:stream');
>
> class UpperCase extends Transform {
>   _transform(chunk, enc, cb) {
>     cb(null, chunk.toString().toUpperCase());
>   }
> }
>
> const upperCase = new UpperCase();
> ```
>
> ###### ⚔️ Difference
>
> | Approach         | Method used    | Style         |
> | ---------------- | -------------- | ------------- |
> | Object (options) | `transform`  | Shortcut      |
> | Class extend     | `_transform` | Core/explicit |

##### Uppercase Transformer (using Class)----

```javascript
const { Transform } = require('node:stream');

class UpperCaseTransform extends Transform {
  _transform(chunk, encoding, callback) {
    const result = chunk.toString().toUpperCase();
    callback(null, result);
  }
}

const upperCase = new UpperCaseTransform();

process.stdin.pipe(upperCase).pipe(process.stdout);
```

**✅ When to Prefer Class Way?**

 Use class when:

* Complex transformations
* Maintaining state
* Reusable components
* Library/framework code

> #### 👉 Same but using `this.push()`
>
> ```js
> const { Transform } = require('node:stream');
>
> class UpperCaseTransform extends Transform {
>   _transform(chunk, encoding, callback) {
>     const result = chunk.toString().toUpperCase();
>     this.push(result);
>     callback();
>   }
> }
> ```
>
> ##### 🧠 What Changed Compared to Earlier?
>
> Earlier we did:
>
> ```js
> callback(null, result);
> ```
>
> Now you did:
>
> ```js
> this.push(result);
> callback();
> ```
>
> ##### ⚙️ Are Both Correct?
>
> 👉 **YES — both are correct ✅**
>
> But they work slightly differently.
>
> ##### 🔄 Two Ways to Send Data Forward
>
> **🔹 1. Using `callback(null, data)` (Shortcut way)**
>
> ```js
> callback(null, result);
> ```
>
> 👉 Internally does:
>
> ```js
> this.push(result);
> callback();
> ```
>
> ✔️ Simpler
> ✔️ Recommended for most cases
>
> **🔹 2. Using `this.push()` manually (Your way)**
>
> ```js
> this.push(result);
> callback();
> ```
>
> ✔️ More control
> ✔️ Can push multiple outputs per chunk
>
> #### 🔥 Why `this.push()` Exists?
>
> 👉 It allows **advanced transformations**
>
> 🧩 Example: Split one chunk into multiple outputs
>
> ```js
> class SplitWords extends Transform {
>   _transform(chunk, enc, cb) {
>     const words = chunk.toString().split(' ');
>
>     words.forEach(word => this.push(word + '\n'));
>
>     cb();
>   }
> }
> ```
>
> 👉 One chunk → multiple outputs
>
> #### 🧠 Internal Understanding
>
> Think of it like:
>
> ```js
> // shortcut
> callback(null, data)
>
> // expanded version
> this.push(data);
> callback();
> ```
>
> #### 🔥 When to Use Which?
>
> ✅ Use `callback(null, data)` when:
>
> * Simple 1:1 transformation
> * Cleaner code
>
> ✅ Use `this.push()` when:
>
> * Splitting data
> * Filtering data
> * Emitting multiple chunks
> * Advanced logic

### 🔥 Real Example 1: File Transformation

```js
const fs = require('node:fs');
const { Transform } = require('node:stream');

const upperCase = new Transform({
  transform(chunk, enc, cb) {
    cb(null, chunk.toString().toUpperCase());
  }
});

fs.createReadStream('input.txt')
  .pipe(upperCase)
  .pipe(fs.createWriteStream('output.txt'));
```

**💡 Use Case:**

* Modify file content **on the fly**
* No need to load full file into memory

### 🔥 Real Example 2: Data Compression (Built-in)

Node.js uses transform streams internally:

👉 via **zlib**

```js
const zlib = require('node:zlib');
const fs = require('node:fs');

fs.createReadStream('file.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('file.txt.gz'));
```

**🧠 Flow:**

```id=
file.txt → gzip transform → compressed file
```

✔️ Input transformed into compressed output

### 🔥 Real Example 3: JSON Stream Modifier

```js
const { Transform } = require('node:stream');

const addField = new Transform({
  transform(chunk, enc, cb) {
    const obj = JSON.parse(chunk.toString());
    obj.processed = true;

    cb(null, JSON.stringify(obj));
  }
});
```

**💡 Use Case:**

* Modify API data in streaming pipelines

### ⚙️ How `_transform()` Works

```js
transform(chunk, encoding, callback)
```

**Parameters:**

* `chunk` → piece of data (Buffer)
* `encoding` → encoding type
* `callback` → send result

**✅ Correct Usage:**

```js
callback(null, transformedData);
```

**❌ Wrong:**

```js
return transformedData; // won't work
```

### 🔁 Data Flow Visualization

```id=
Readable → Transform → Writable

   data      modify      output
```

### ⚡ Key Benefits

**1. 🚀 Memory Efficient**

* Works **chunk by chunk**
* No full data load

**2. 🔄 Real-Time Processing**

* Data processed while streaming

**3. 🔌 Pipe-Friendly**

* Easily chain multiple transforms

🔥 Chaining Multiple Transforms

```js
readStream
  .pipe(transform1)
  .pipe(transform2)
  .pipe(writeStream);
```

### ⚠️ Important Notes

**1. Works with Buffers**

👉 `chunk` is usually a Buffer

**2. Must call `callback`**

👉 Otherwise stream hangs 😬

**3. Can be async**

```js
transform(chunk, enc, cb) {
  setTimeout(() => {
    cb(null, chunk.toString().toUpperCase());
  }, 100);
}
```

### 🎯 When to Use Transform Streams?

**✅ Use when:**

* Modifying data in pipelines
* File processing
* Compression / encryption
* Streaming APIs

**❌ Avoid when:**

* Small data (just use normal functions)

---

# ----Stream Module- Duplex module

### 🌊 What is a Duplex Stream?

👉 A **Duplex stream** is a stream that can:

✔️ **Read data (Readable)**
✔️ **Write data (Writable)**

### 🧠 One-line definition

👉 **“Can read and write, but doesn’t have to transform data”**

### ⚠️ Important Difference

👉 Duplex ≠ Transform

* **Duplex** → read & write are **independent**
* **Transform** → output is **based on input**

### 🧠 Analogy

**Duplex stream = Phone call 📞**

* You can **talk (write)**
* You can **listen (read)**
* Both are independent

### ⚙️ Creating a Duplex Stream

You extend the Duplex class:

```js
const { Duplex } = require('node:stream');
```

### **🔧 Example 1: Simple Duplex Stream**

```js
const { Duplex } = require('node:stream');

class MyDuplex extends Duplex {
  constructor() {
    super();
    this.data = ['Hello', 'World'];
  }

  // READ side
  _read() {
    this.push(this.data.shift() || null);
  }

  // WRITE side
  _write(chunk, encoding, callback) {
    console.log('Received:', chunk.toString());
    callback();
  }
}

const stream = new MyDuplex();

stream.write('Hi');
stream.write('There');

stream.on('data', (chunk) => {
  console.log('Read:', chunk.toString());
});
```

🧠 Output:

```id=
Received: Hi
Received: There
Read: Hello
Read: World
```

### 🔍 What’s happening?

**WRITE side (`_write`)**

```js
stream.write('Hi');
```

👉 Goes to `_write()`

**READ side (`_read`)**

```js
this.push('Hello');
```

👉 Emits readable data

👉 Both are **separate flows**

### 🔥 Example 2: Echo Stream

```js
const { Duplex } = require('node:stream');

class EchoStream extends Duplex {
  _write(chunk, enc, cb) {
    this.push(chunk); // send back same data
    cb();
  }

  _read(size) {}
}

const echo = new EchoStream();

echo.on('data', (chunk) => {
  console.log('Echo:', chunk.toString());
});

echo.write('Hello');
echo.write('World');
```

🧠 Output:

```id=
Echo: Hello
Echo: World
```

👉 Here we made Duplex behave like Transform
(but manually)

### 🌐 Real-World Examples of Duplex Streams

**1. 🔌 TCP Sockets (MOST IMPORTANT)**

```js
const net = require('node:net');

const server = net.createServer((socket) => {
  socket.on('data', (data) => {
    console.log('Received:', data.toString());
  });

  socket.write('Hello client');
});
```

👉 `socket` is a Duplex stream

✔️ Read → incoming data
✔️ Write → outgoing data

**2. 📡 HTTP Requests (Internally)**

* Request = readable stream
* Response = writable stream
* Combined → behaves like duplex system

**3. 🖥️ stdin/stdout combo**

```js
process.stdin.pipe(process.stdout);
```

👉 Data flows both ways via streams

### ⚔️ Duplex vs Transform

| Feature      | Duplex                  | Transform               |
| ------------ | ----------------------- | ----------------------- |
| Read & Write | ✅                      | ✅                      |
| Relationship | Independent             | Output depends on input |
| Methods      | `_read`,`_write`    | `_transform`          |
| Use case     | Sockets, custom streams | Data processing         |

### 🧠 Key Insight

👉 In Duplex:

```js
_write() !== _read()
```

👉 They are NOT connected automatically

### ⚠️ Common Mistake

**❌ Thinking Duplex auto-transforms**

```js
_write(chunk) {
  // data written
}
```

👉 This does NOT go to `_read()` automatically

**✅ You must manually push**

```js
_write(chunk, enc, cb) {
  this.push(chunk); // manually connect
  cb();
}
```

### 🎯 When to Use Duplex?

✅ Use when:

* Two-way communication
* Sockets
* Custom protocols
* Independent read/write logic

❌ Avoid when:

* You just want to transform data
  👉 Use Transform instead

---

# ----Cluster Module

**`cluster` module** is a classic Node.js topic 🔥

It’s all about using **multiple CPU cores** (since Node is single-threaded by default).

Let’s build this clearly from scratch.

### 🧠 Why `cluster` module exists

👉 Node.js runs on **one thread (event loop)**

❌ Problem:

* Uses only **one CPU core**

👉 On a 4-core machine:

* 3 cores = wasted 😬

### 🚀 Solution: `cluster` module

👉 **cluster**

Lets you:
✔️ Create multiple processes (workers)
✔️ Each runs Node.js
✔️ All share same server port

### 🧩 How it works

```text
        Master Process
              │
   ┌──────────┼──────────┐
 Worker 1   Worker 2   Worker 3   Worker 4
```

👉 Master:

* Manages workers

👉 Workers:

* Handle requests

### ⚙️ Basic Example

```js
const cluster = require('node:cluster');
const http = require('node:http');
const os = require('node:os');

const numCPUs = os.cpus().length;

if (cluster.isPrimary) {
  console.log(`Master ${process.pid} is running`);

  // create workers
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

} else {
  // worker code
  http.createServer((req, res) => {
    res.end(`Handled by ${process.pid}`);
  }).listen(3000);

  console.log(`Worker ${process.pid} started`);
}
```

### 🧠 What happens?

* Master creates workers
* Each worker:
  * Runs server
  * Handles requests

👉 Requests are **load balanced**

> #### ❓ 1. How does Node know Master vs Worker?
>
> 👉 You **don’t manually assign it**
> 👉 Node.js decides it internally
>
> **🧠 What actually happens**
>
> When you run:
>
> ```bash
> node app.js
> ```
>
> 👉 This first process becomes:
>
> ✔️ **Primary (Master) process**
>
> Then inside your code:
>
> ```js
> cluster.fork();
> ```
>
> 👉 Node:
>
> * Creates a **new child process**
> * Runs the **same file again**
>
> 🔥 Important Insight
>
> 👉 Workers run the **same file again from top**
>
> BUT:
>
> ```js
> cluster.isPrimary
> ```
>
> will now be:
>
> * `true` → original process
> * `false` → forked processes
>
> 🎯 So:
>
> 👉 Master vs Worker is decided by:
>
> ```js
> cluster.isPrimary
> ```
>
> #### ❓ 3. How exactly does fork work?
>
> Step-by-step flow:
>
> ```text
> 1. node app.js starts
> 2. This becomes PRIMARY
> 3. PRIMARY calls cluster.fork()
> 4. New process created
> 5. Same file runs again
> 6. Now cluster.isPrimary = false
> 7. So worker code runs
> ```
>
> #### ❓ 4. Should everything be in one file?
>
> 👉 ✅ YES — most of the time
>
> 🧠 Why?
>
> Because:
>
> * Workers run the **same file**
> * Logic is split using:
>
> ```js
> if (cluster.isPrimary) {
>   // master code
> } else {
>   // worker code
> }
> ```
>
> #### 🧠 How to identify master vs worker?
>
> ✅ 1. Using `cluster.isPrimary`
>
> ```js
> if (cluster.isPrimary) {
>   console.log('I am master');
> } else {
>   console.log('I am worker');
> }
> ```
>
> ✅ 2. Using process ID
>
> ```js
> console.log(process.pid);
> ```
>
> 👉 Each worker has different PID
>
> ✅ 3. Using `cluster.worker`
>
> Inside worker:
>
> ```js
> console.log(cluster.worker.id);
> ```
>
> 👉 Gives worker ID (1, 2, 3...)
>
> #### 🔥 Important Insight (VERY IMPORTANT)
>
> 👉 This line:
>
> ```js
> cluster.fork();
> ```
>
> 👉 Does NOT run a function
>
> 👉 It:
>
> * Starts a **new Node process**
> * Re-runs your file
>
> ⚠️ Common Confusion
>
> #### ❌ Thinking fork runs worker code directly
>
> 👉 WRONG
>
> It actually:
>
> ```text
> fork → new process → re-run file → check isPrimary → run worker block
> ```

### 🔑 Important Properties

##### 1. `cluster.isPrimary`

```js
if (cluster.isPrimary) { ... }
```

✔️ True → master process
✔️ False → worker process

> ##### ❓ 2. `isPrimary` vs `isMaster`
>
> 👉 Both exist, but:
>
> * `cluster.isMaster` ❌ (deprecated)
> * `cluster.isPrimary` ✅ (modern, recommended)
>
> **🧠 Why change?**
>
> Node renamed it to be:
>
> * More inclusive
> * Future-proof

##### 2. `cluster.workers`

👉 Object of all workers

```js
console.log(cluster.workers);
```

### 🔧 Important Methods

##### 1. `cluster.fork()`

👉 Creates a new worker

```js
cluster.fork();
```

✔️ Spawns new process

##### 2. `worker.kill()`

👉 Kill a worker

```js
cluster.workers[id].kill();
```

##### 3. `cluster.on()`

👉 Listen to events

### 🔔 Important Events

##### 1. `fork`

```js
cluster.on('fork', (worker) => {
  console.log(`Worker ${worker.process.pid} created`);
});
```

##### 2. `online`

```js
cluster.on('online', (worker) => {
  console.log(`Worker ${worker.process.pid} is online`);
});
```

##### 3. `exit`

```js
cluster.on('exit', (worker, code, signal) => {
  console.log(`Worker ${worker.process.pid} died`);

  // restart worker
  cluster.fork();
});
```

👉 Very important for **auto-restart**

### 🔥 Example with Restart Logic

```js
if (cluster.isPrimary) {
  for (let i = 0; i < 2; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker) => {
    console.log(`Restarting worker...`);
    cluster.fork();
  });

} else {
  console.log(`Worker ${process.pid} running`);

  setTimeout(() => {
    process.exit(); // simulate crash
  }, 3000);
}
```

### 📡 Communication (Advanced)

Workers can talk to master:

**Send message**

```js
process.send({ msg: 'hello from worker' });
```

**Receive message**

```js
process.on('message', (msg) => {
  console.log(msg);
});
```

> #### ❓ 2. cluster.on() vs process.send() / process.on()
>
> 🔥 This is a **very important distinction**
>
> **🧠 Short Answer**
>
> 👉 They serve **different purposes**
>
> | API                                 | Purpose                                |
> | ----------------------------------- | -------------------------------------- |
> | `cluster.on()`                    | Listen to**lifecycle events**    |
> | `process.send()`/`process.on()` | **Send/receive custom messages** |
>
> ##### **🔔 1. `cluster.on()` → System Events**
>
> Used in **primary (master)** process
>
> ```js
> cluster.on('fork', (worker) => {
>   console.log('Worker created');
> });
>
> cluster.on('exit', (worker) => {
>   console.log('Worker died');
> });
> ```
>
> **🧠 These are:**
>
> 👉 **Built-in lifecycle events**
>
> * fork
> * online
> * exit
> * disconnect
>
> 👉 You are NOT sending these manually
>
> ##### 📡 2. `process.send()` / `process.on()` → Custom Messaging
>
> Used for **communication between primary & workers**
>
> **✅ Worker → Primary**
>
> ```js
> // worker
> process.send({ msg: 'Hello from worker' });
> ```
>
> **✅ Primary receives**
>
> ```js
> cluster.on('message', (worker, message) => {
>   console.log(message);
> });
> ```
>
> **✅ Primary → Worker**
>
> ```js
> cluster.workers[id].send({ msg: 'Hello worker' });
> ```
>
> **✅ Worker receives**
>
> ```js
> process.on('message', (msg) => {
>   console.log(msg);
> });
> ```
>
> ##### 🧠 Analogy
>
> **cluster.on()**
>
> 👉 Like OS notifications 🔔
>
>> “Worker crashed”
>>
>
> **process.send()**
>
> 👉 Like WhatsApp message 💬
>
>> “Hey worker, do this task”
>>
>
> #### 🎯 When to Use What?
>
> **✅ Use `cluster.on()` when:**
>
> * Tracking workers
> * Restarting workers
> * Debugging lifecycle
>
> **✅ Use `process.send()` when:**
>
> * Sharing data
> * Coordinating tasks
> * Custom communication
>
> ##### ⚡ Final Combined Example
>
> ```js
> const cluster = require('node:cluster');
>
> if (cluster.isPrimary) {
>   const worker = cluster.fork();
>
>   // lifecycle event
>   cluster.on('exit', () => {
>     console.log('Worker died');
>   });
>
>   // receive message
>   cluster.on('message', (worker, msg) => {
>     console.log('From worker:', msg);
>   });
>
>   // send message
>   worker.send({ task: 'start' });
>
> } else {
>   // receive message
>   process.on('message', (msg) => {
>     console.log('From master:', msg);
>   });
>
>   // send message
>   process.send({ status: 'ready' });
> }
> ```

### ⚡ Real-World Use Case

**🌐 Web Server Scaling**

```text
Incoming Requests
        ↓
     Master
        ↓
Load balanced across workers
```

✔️ Better CPU usage
✔️ Higher throughput

### ⚠️ Important Limitations

**1. Not true multithreading**

👉 Each worker = separate process
👉 Memory is NOT shared

**2. State issues**

❌ In-memory data not shared

👉 Solution:

* Redis
* Database

**3. Overhead**

* More memory usage

### ⚔️ Cluster vs Worker Threads

| Feature  | Cluster   | Worker Threads |
| -------- | --------- | -------------- |
| Type     | Processes | Threads        |
| Memory   | Separate  | Shared         |
| Use case | Servers   | CPU tasks      |

### 🎯 When to Use Cluster?

**✅ Use when:**

* Building HTTP servers
* Need to utilize all CPU cores
* Handling many requests

**❌ Avoid when:**

* CPU-heavy tasks → use worker_threads

### 🧠 Final Mental Model

👉 **Cluster = multiple Node processes sharing load ⚖️**

---

# ----CPU, Processes and Threads | Cluster vs Worker threads

### ❌ CPU ≠ Process ≠ Thread

They are different layers:

| Concept  | Meaning                                     |
| -------- | ------------------------------------------- |
| CPU Core | Physical hardware unit 🧠                   |
| Process  | Independent program instance                |
| Thread   | Lightweight execution unit inside a process |

### 🔥 Worker Threads (Correct Understanding)

👉 Worker Threads (via `worker_threads`) are:

* Multiple **threads inside ONE process**
* Share **same memory space**
* Can run on **different CPU cores**

**🧠 So:**

👉 The statement:

> “worker threads use single CPU”

❌ Incorrect

👉 Correct:
✔️ Worker threads can use **multiple CPU cores**
✔️ But they stay inside **one process**

### 🔄 Comparison (Clear Now)

| Feature   | Cluster        | Worker Threads |
| --------- | -------------- | -------------- |
| Processes | Multiple       | Single         |
| Threads   | 1 per process  | Multiple       |
| CPU usage | Multiple cores | Multiple cores |
| Memory    | Separate       | Shared         |

### ❌ The statement:

> “Cluster uses multithread (1 per process) and 1 core per process”

👉 This is **partly wrong**

### 🧠 Correct Understanding

##### **1. Does Cluster use multithreading?**

👉 ❌ **No**

👉 **cluster** uses:

✔️ **Multiple processes**
✔️ Each process has **its own single thread (event loop)**

**🧠 So:**

👉 Cluster =  **multi-process** , NOT multi-thread

##### **🔥 2. “1 core per process” — also not exactly**

👉 OS decides CPU scheduling

✔️ A process is **not fixed to one core**
✔️ OS can:

* Move it between cores
* Run multiple processes on same core
* Run one process across time slices

**🧠 Correct statement:**

👉 Cluster allows Node to **utilize multiple CPU cores effectively**
(not strictly one process = one core)

##### ⚡ 3. Is it heavy overhead?

👉 ✅ YES — compared to worker threads

Because:

**Each worker process has:**

* Separate memory 🧠
* Separate V8 instance
* Separate event loop

### 🧠 So overhead:

| Feature      | Cluster   |
| ------------ | --------- |
| Memory       | High ❌   |
| Startup time | Higher ❌ |
| Isolation    | Strong ✅ |

### 🔄 Worker Threads vs Cluster (Now crystal clear)

| Feature       | Cluster      | Worker Threads |
| ------------- | ------------ | -------------- |
| Type          | Processes    | Threads        |
| Memory        | Separate ❌  | Shared ✅      |
| Overhead      | High ❌      | Low ✅         |
| Communication | Slower (IPC) | Faster         |
| Isolation     | Strong       | Weak           |

### 🧠 Real Insight (VERY IMPORTANT)

👉 Cluster trades:

❌ More memory
❌ More overhead

FOR:

✔️ Stability (crash isolation)
✔️ Better scaling for servers

### 🔥 Why Cluster is still used

Even with overhead, it’s useful for:

**🌐 Web servers**

* Each worker handles requests independently
* If one crashes → others still run ✅

### ⚠️ When NOT to use Cluster

👉 Avoid when:

* CPU-heavy tasks → use worker threads
* Memory is limited

---

# ----Cluster module- `setUpMaster(), now renamed to- setUpPrimary()`

This is a slightly lesser-used but **important internal control API** of the **cluster** 🔥

### 🧠 What is `setupMaster()`?

👉 `cluster.setupMaster()` (now renamed to  **`setupPrimary()`** ) is used to:

✔️ **Configure how worker processes are created**
✔️ Define what file they should run
✔️ Customize execution behavior

### ⚠️ Modern Name

👉 Old:

```js
cluster.setupMaster()
```

👉 New (recommended):

```js
cluster.setupPrimary()
```

### 🧠 Default Behavior (Without setup)

Normally:

```js
cluster.fork();
```

👉 Workers:

* Run **the same file** (`app.js`)
* Same arguments

#### 🔥 What `setupPrimary()` changes

👉 It lets you override that behavior

**✅ Example**

```js
const cluster = require('node:cluster');

if (cluster.isPrimary) {

  cluster.setupPrimary({
    exec: 'worker.js',   // different file
    args: ['--test'],    // arguments
  });

  cluster.fork();

} else {
  console.log('Worker running:', process.argv);
}
```

### 🔄 What actually happens

**🟢 Step 1: You run**

```bash
node app.js
```

👉 This starts:

* **Primary process**
* Runs `app.js`

**🟢 Step 2: Primary executes**

```js
cluster.setupPrimary({
  exec: 'worker.js',
  args: ['--test'],
});
```

👉 Configures:

> “Future workers should run `worker.js`”

**🟢 Step 3: Primary calls**

```js
cluster.fork();
```

👉 Now Node spawns a **new process**

**🟢 Step 4: Worker process starts**

👉 Worker runs:

```bash
node worker.js --test
```

✔️ NOT `app.js`
✔️ NOT re-running the same file

### ❗ Important Correction

👉 Because of `setupPrimary({ exec: 'worker.js' })`:

❌ Worker does NOT run `app.js`
❌ So it never reaches that `else` block in `app.js`

**🧠 So what happens to `else`?**

👉 This block:

```js
else {
  console.log('Worker running:', process.argv);
}
```

❌ **Will NOT run at all**

Why?

👉 Because worker is running  **`worker.js`** , not `app.js`

### 🔥 Key Insight

**Without `setupPrimary()`**

```js
cluster.fork();
```

👉 Worker runs:

```bash
node app.js
```

✔️ Then `else` block runs ✅

**With `setupPrimary()`**

```js
cluster.setupPrimary({ exec: 'worker.js' });
cluster.fork();
```

👉 Worker runs:

```bash
node worker.js
```

❌ `app.js` worker code is ignored

> ##### The name `setUpPrimary` **does sound like creating a particular file as master(primay) in plain English** , but in Node it means something slightly different.
>
> 🧠 What one may think
>
>> “setupPrimary sets which file is the primary (master)”
>>
>
> 👉 ❌ Not correct
>
> #### ✅ What it actually means
>
> 👉 **`setupPrimary()` configures how *workers* are created**
>
> NOT the primary process.
>
> 🔥 Key Idea
>
> 👉 The **primary process is ALWAYS the one you start manually**
>
> ```bash
> node app.js
> ```
>
> ✔️ That file = primary
> ✔️ You cannot change that using `setupPrimary()`
>
> #### 🧠 So what does `setupPrimary()` really do?
>
> 👉 Think of it as:
>
>> “Primary, when you create workers, use THIS configuration” or “Future workers should run `worker.js` (from previous example)”
>>
>
> #### 🎯 Better Name (Mentally)
>
> Instead of:
>
> 👉 `setupPrimary()`
>
> Think:
>
> 👉 **“configureWorkerCreation()”** 🧠

### 🔧 Options in `setupPrimary()`

**1. `exec`**

```js
exec: 'worker.js'
```

👉 File to run in workers

**2. `args`**

```js
args: ['--env=prod']
```

👉 Command-line arguments

**3. `execArgv`**

```js
execArgv: ['--inspect']
```

👉 Node flags (debugging etc.)

**4. `cwd`**

```js
cwd: '/app'
```

👉 Working directory

**5. `silent`**

```js
silent: true
```

👉 Redirect stdout/stderr

### 🔥 Real Use Case

**🧩 Separate Worker File**

Instead of mixing:

```js
if (cluster.isPrimary) { ... }
else { ... }
```

👉 You can split files:

primary.js

```js
cluster.setupPrimary({ exec: 'worker.js' });

cluster.fork();
```

worker.js

```js
console.log('Worker running');
```

✔️ Cleaner architecture
✔️ Easier to maintain

# ⚠️ Important Notes

**1. Call before `fork()`**

```js
cluster.setupPrimary({...});
cluster.fork();
```

**2. Only affects future workers**

👉 Already created workers ❌ not affected

### ⚔️ Without vs With setupPrimary

**❌ Without**

```js
cluster.fork();
```

👉 Same file runs again

**✅ With**

```js
cluster.setupPrimary({ exec: 'worker.js' });
cluster.fork();
```

👉 Different file runs

### 🎯 When to Use It?

**✅ Use when:**

* You want separate worker file
* Custom arguments needed
* Debugging workers differently

**❌ Avoid when:**

* Simple apps (single file is enough)

### 🧠 Final Mental Model

👉 **setupPrimary = configure how workers are spawned ⚙️**

---

# ----Child Process Module

**🧠 What is `child_process`?**

👉 It lets Node.js **create new processes manually**

✔️ Run another Node script
✔️ Run system commands (like `ls`, `python`, etc.)
✔️ Do work in parallel

### ⚡ Core Idea

👉 **Main process (parent) → spawns child processes**

```text
Parent Process
     ├── Child Process 1
     ├── Child Process 2
     └── Child Process 3
```

### 🔥 Important: Process Model

👉 Just like cluster:

✔️ Each child = **separate process**
✔️ Own memory
✔️ Own event loop

❌ No shared memory (by default)

### ⚔️ Compare with Others

| Feature       | child_process        | cluster                  | worker_threads |
| ------------- | -------------------- | ------------------------ | -------------- |
| Control       | Manual               | Automatic (fork workers) | Manual         |
| Use case      | Run tasks / commands | Scale servers            | CPU work       |
| Memory        | Separate             | Separate                 | Shared         |
| Process count | Multiple             | Multiple                 | Single         |

### 🧰 Main Methods in `child_process`

##### 🔹 1. `exec()`

👉 Runs a command in a shell

```js
const { exec } = require('node:child_process');

exec('ls', (err, stdout, stderr) => {
  console.log(stdout);
});
```

🧠 **Characteristics**:

* Uses shell (`/bin/sh`)
* Buffers full output
* Simple but not efficient for large data

##### 🔹 2. `spawn()` (MOST IMPORTANT 🔥)

👉 Runs command as stream 

```js
const { spawn } = require('node:child_process');

const child = spawn('ls');

child.stdout.on('data', (data) => {
  console.log(data.toString());
});
```

**🧠 Characteristics:**

✔️ Streaming (no full buffering)
✔️ Better for large data
✔️ More control

##### 🔹 3. `fork()`

👉 Special method to run **Node.js files**

```js
const { fork } = require('node:child_process');

const child = fork('./worker.js');

child.on('message', (msg) => {
  console.log('From child:', msg);
});

child.send({ hello: 'world' });
```

**🧠 Key Feature:**

✔️ Built-in IPC (message passing)
✔️ Like mini-cluster

##### 🔹 4. `execFile()`

👉 Like `exec()` but without shell

```js
const { execFile } = require('node:child_process');

execFile('node', ['app.js'], (err, stdout) => {
  console.log(stdout);
});
```

✔️ More secure
✔️ Faster than exec

### 🔁 Communication (VERY IMPORTANT)

**Parent → Child**

```js
child.send({ msg: 'Hello child' });
```

**Child → Parent**

```js
process.send({ msg: 'Hello parent' });
```

**Listen**

```js
process.on('message', (msg) => {
  console.log(msg);
});
```

### 🔥 Real Examples

**📁 Example 1: Run Shell Command**

```js
exec('node -v', (err, stdout) => {
  console.log(stdout);
});
```

**⚙️ Example 2: Background Worker**

**parent.js**

```js
const { fork } = require('node:child_process');

const child = fork('./worker.js');

child.send({ num: 5 });

child.on('message', (msg) => {
  console.log('Result:', msg);
});
```

**worker.js**

```js
process.on('message', (msg) => {
  const result = msg.num * 2;
  process.send(result);
});
```

### ⚠️ Important Differences

**child_process vs cluster**

👉 Cluster internally uses:

```js
child_process.fork()
```

**🧠 So:**

👉 cluster = **specialized wrapper over child_process**

### 🎯 When to Use child_process?

**✅ Use when:**

* Running OS commands
* Running other programs (Python, etc.)
* Custom process control
* Background jobs

**❌ Avoid when:**

* CPU-heavy JS → use worker threads
* HTTP scaling → use cluster

### 🧠 Final Mental Model

👉 **child_process = raw power (manual processes)**
👉 **cluster = structured scaling**
👉 **worker_threads = lightweight parallelism**

---

# Child_process vs Cluster | sub-process in detail

### 🧠 1. Cluster vs child_process — are they the same?

👉 **Yes at a low level**

* **cluster** internally uses
* **child_process** (specifically `fork()`)

✅ So the statement:

> “Both create separate processes with their own V8, memory etc.”

👉 ✔️ **Absolutely correct**

Each process has:

* Its own V8 engine
* Its own memory
* Its own event loop

### 🧠 2. “Child process means sub-process → extra overhead?”

👉 This is where the confusion is ❗

**❌ Misconception**

> “One process now has 2 processes”

👉 Sounds like:

* One process *contains* another ❌

**✅ Reality**

👉 OS-level view:

```text
Parent Process
Child Process
```

✔️ They are **independent processes**
✔️ Just linked logically (parent-child relationship)
✔️ Neither “contains” the other

**🔥 Important Insight**

👉 Parent ≠ container
👉 It’s just the **creator + controller**

**🧠 Example**

```js
const { fork } = require('node:child_process');

fork('./worker.js');
```

👉 OS now has:

```text
Process A (parent)
Process B (child)
```

✔️ Both run independently
✔️ OS schedules them separately

### ⚡ 3. Is there overhead?

👉 ✅ YES — but not because of “sub-process”

👉 Because:

Each process has:

* Separate memory 🧠
* Separate V8 instance
* Separate event loop

### 🧠 So overhead comes from:

| Reason       | Why                             |
| ------------ | ------------------------------- |
| Memory       | Each process duplicates runtime |
| Startup time | New V8 instance                 |
| IPC          | Communication cost              |

### 🔄 Cluster vs child_process (Now Clear)

**🔹 child_process**

👉 General-purpose

```js
fork('task.js');
exec('python script.py');
```

✔️ You control everything manually

**🔹 cluster**

👉 Specialized for servers

```js
cluster.fork();
```

✔️ Automatically:

* Load balances
* Manages workers
* Shares port

### 🧠 Key Difference

| Feature        | child_process | cluster      |
| -------------- | ------------- | ------------ |
| Purpose        | General       | HTTP scaling |
| Control        | Manual        | Structured   |
| Load balancing | ❌            | ✅           |

### 🔥 4. Core doubt (final answer)

> “Does child process create overhead because it becomes 2 processes?”

👉 ❌ Not because it's “sub-process”
👉 ✅ Because **you now have 2 independent processes**

### 🎯 Final Mental Model

👉 **Cluster = organized multi-process system ⚖️**
👉 **child_process = manual multi-process control 🛠️**

---

# Worker Threads Module

### 🧠 What is Worker Threads?

👉 **worker_threads** allows Node.js to:

✔️ Run JavaScript in **parallel threads**
✔️ Inside a **single process**
✔️ Share memory between threads

### ❗ Why Worker Threads Exist

**🧠 Problem in Node.js**

👉 Node is **single-threaded (event loop)**

So:

```js
while(true) {}
```

❌ Blocks everything
❌ No other request can be handled

**✅ Solution**

👉 Offload heavy work to **worker threads**

### 🔄 Basic Architecture

```text
Main Thread
   ├── Worker Thread 1
   ├── Worker Thread 2
   └── Worker Thread 3
```

✔️ Same process
✔️ Separate threads
✔️ Parallel execution

### ⚙️ Creating a Worker

**📁 main.js**

```js
const { Worker } = require('node:worker_threads');

const worker = new Worker('./worker.js');

worker.on('message', (msg) => {
  console.log('From worker:', msg);
});

worker.postMessage(10);
```

**📁 worker.js**

```js
const { parentPort } = require('node:worker_threads');

parentPort.on('message', (num) => {
  const result = num * 2;
  parentPort.postMessage(result);
});
```

**🧠 Flow**

```text
Main → postMessage → Worker
Worker → process → postMessage → Main
```

### 🔑 Core Concepts

##### 1️⃣ `isMainThread`

```js
const { isMainThread } = require('node:worker_threads');
```

👉 Helps decide:

```js
if (isMainThread) {
  // main code
} else {
  // worker code
}
```

##### **2️⃣ `parentPort`**

👉 Communication channel

```js
parentPort.on('message', handler);
parentPort.postMessage(data);
```

##### **3️⃣ `workerData`**

👉 Pass initial data

```js
const worker = new Worker('./worker.js', {
  workerData: 5
});
```

In worker:

```js
const { workerData } = require('node:worker_threads');

console.log(workerData);
```

##### 🔥 4️⃣ `Worker` Class

Constructor

```js
new Worker(filename, options);
```

### Important Methods

##### 🔹 `.postMessage()`

```js
worker.postMessage(data);
```

##### 🔹 `.terminate()`

```js
worker.terminate();
```

👉 Stops worker

##### 🔹 `.on()`

```js
worker.on('message', cb);
worker.on('error', cb);
worker.on('exit', cb);
```

### 🔄 Communication Model

**🧠 Default: Message Passing**

👉 Uses **structured clone algorithm**

✔️ Sends objects
✔️ Not shared directly

### ⚡ 5️⃣ Shared Memory (ADVANCED 🔥)

**Using `SharedArrayBuffer`**

```js
const shared = new SharedArrayBuffer(4);
const arr = new Int32Array(shared);
```

👉 Both threads can access same memory

🧠 Why?

✔️ Faster than messaging
✔️ No copying

### ⚡ 6️⃣ `Atomics` (VERY IMPORTANT)

👉 Prevent race conditions

```js
Atomics.add(arr, 0, 1);
```

👉 Ensures safe concurrent updates

### 🔥 Real Example: CPU-heavy task

**❌ Without worker**

```js
function heavy() {
  for (let i = 0; i < 1e9; i++) {}
}
```

👉 Blocks server

**✅ With worker**

```js
new Worker('./heavyTask.js');
```

👉 Runs in parallel
👉 Main thread stays responsive

### ⚔️ Worker Threads vs Others

**vs Cluster**

| Feature  | Worker Threads | Cluster        |
| -------- | -------------- | -------------- |
| Type     | Threads        | Processes      |
| Memory   | Shared         | Separate       |
| Overhead | Low            | High           |
| Use case | CPU tasks      | Server scaling |

**vs child_process**

| Feature       | Worker Threads | child_process |
| ------------- | -------------- | ------------- |
| Communication | Fast           | Slower (IPC)  |
| Memory        | Shared         | Separate      |
| Control       | JS only        | OS-level      |

### ⚠️ Limitations

**❌ 1. Not for I/O tasks**

👉 Node already handles I/O efficiently

**❌ 2. Complexity**

👉 Shared memory → bugs if not careful

**❌ 3. Crash risk**

👉 Thread crash can affect process

### 🎯 When to Use Worker Threads

**✅ Use when:**

* CPU-heavy tasks
* Image processing
* Encryption
* Data parsing

**❌ Avoid when:**

* API requests
* DB queries
* File I/O

---

# ----Worker Threads- Atomics & Shared Memory

### 🧠 Why do we need Shared Memory & Atomics?

**❌ Problem with normal messaging**

```js
worker.postMessage(data);
```

👉 Data is:

* **copied** , not shared
* Slower for large data

**🚀 Solution**

👉 Use **shared memory**

### 🧠 1. Shared Memory

**📌 What is it?**

👉 Memory that is **accessible by multiple threads simultaneously**

In Node.js

👉 Using:

```js
SharedArrayBuffer
```

**🔧 Example**

```js
const shared = new SharedArrayBuffer(4); // 4 bytes
const arr = new Int32Array(shared);
```

**🧠 What’s happening?**

```text
Memory (4 bytes)
   ↑
Shared between:
Main thread + Worker thread
```

##### 🔄 Both can read/write

```js
arr[0] = 10;
```

👉 Worker sees same value instantly

##### ⚠️ Problem: Race Condition

**❌ Example**

Two threads:

```js
arr[0] = arr[0] + 1;
```

👉 Both read same value → overwrite each other 😬

🧠 This is called:

👉 **Race condition**

### 🚀 2. Atomics

👉 **Atomics** solves this

##### 📌 What is Atomics?

👉 A set of operations that are:

✔️ **Atomic (indivisible)**
✔️ Thread-safe

**🧠 Meaning of “atomic”**

👉 Operation happens:

* Fully
* Without interruption

##### 🔧 Example

❌ Without Atomics

```js
arr[0] = arr[0] + 1;
```

👉 Unsafe

✅ With Atomics

```js
Atomics.add(arr, 0, 1);
```

👉 Safe increment

##### 🧠 What happens internally?

👉 Atomics ensures:

```text
Read → Modify → Write
```

is done as **one single operation**

##### 🔑 Important Atomics Methods

🔹 1. `Atomics.add()`

```js
Atomics.add(arr, index, value);
```

👉 Adds safely

🔹 2. `Atomics.sub()`

```js
Atomics.sub(arr, 0, 1);
```

🔹 3. `Atomics.load()`

```js
Atomics.load(arr, 0);
```

👉 Safe read

🔹 4. `Atomics.store()`

```js
Atomics.store(arr, 0, 100);
```

👉 Safe write

🔹 5. `Atomics.compareExchange()` 🔥

```js
Atomics.compareExchange(arr, 0, expected, newValue);
```

👉 Update only if value matches

🔹 6. `Atomics.wait()` (blocking)

```js
Atomics.wait(arr, 0, 0);
```

👉 Thread waits until value changes

🔹 7. `Atomics.notify()`

```js
Atomics.notify(arr, 0, 1);
```

👉 Wake waiting thread

### 🔥 Real Example (Important)

📁 main.js

```js
const { Worker } = require('node:worker_threads');

const shared = new SharedArrayBuffer(4);
const arr = new Int32Array(shared);

const worker = new Worker('./worker.js', {
  workerData: shared
});

setInterval(() => {
  console.log('Counter:', arr[0]);
}, 1000);
```

📁 worker.js

```js
const { workerData } = require('node:worker_threads');

const arr = new Int32Array(workerData);

setInterval(() => {
  Atomics.add(arr, 0, 1);
}, 500);
```

🧠 Output

```text
Counter: 1
Counter: 2
Counter: 3
...
```

👉 Shared memory updated safely across threads

### ⚔️ Shared Memory vs Message Passing

| Feature    | Shared Memory      | Message Passing |
| ---------- | ------------------ | --------------- |
| Speed      | Fast 🚀            | Slower          |
| Safety     | Needs Atomics ⚠️ | Safe            |
| Complexity | High ❌            | Low ✅          |
| Use case   | High-performance   | General use     |

### ⚠️ Important Rules

❌ Never do this

```js
arr[0]++;
```

👉 Not safe

✅ Always use

```js
Atomics.add(arr, 0, 1);
```

### 🎯 When to Use

**✅ Use shared memory when:**

* High performance needed
* Frequent updates
* Large data

**❌ Avoid when:**

* Simple apps
* No concurrency issues

### 🚀 Pro Insight (Interview Gold)

👉 Without Atomics:

* Shared memory = **bug factory** 😬

👉 With Atomics:

* Safe parallel programming

---

# ----JWT

Tthis is a **core backend/auth topic** 🔥

I’ll make it crystal clear and practical.

### 🔐 What is JWT?

👉 **JSON Web Token (JWT)** is a way to:

✔️ **Authenticate users**
✔️ **Share data securely between client & server**

**🧠 Simple Idea**

👉 Instead of storing session on server:

👉 You give the client a **token**
👉 Client sends it back on every request

### 📦 Structure of JWT

A JWT has  **3 pa****rts** :

```text
HEADER.PAYLOAD.SIGNATURE
```

**🔹 1. Header**

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

👉 Algorithm used for signing

**🔹 2. Payload**

```json
{
  "userId": "123",
  "role": "admin"
}
```

👉 Contains data (claims)

⚠️ Not encrypted → just encoded

**🔹 3. Signature**

```text
HMACSHA256(
  base64(header + payload),
  secret
)
```

👉 Ensures token is **not tampered**

### 🔄 Flow of JWT Authentication

**🧾 1. Login**

```text
Client → Server (email/password)
```

**🔐 2. Server creates JWT**

```js
jwt.sign({ userId: 1 }, SECRET, { expiresIn: '1h' });
```

**📦 3. Client stores token**

* localStorage OR cookie

**📡 4. Client sends token**

```text
Authorization: Bearer <token>
```

**🔍 5. Server verifies**

```js
jwt.verify(token, SECRET);
```

**🎯 If valid:**

✔️ User authenticated

### 🔥 Why JWT is Used

**✅ 1. Stateless**

👉 Server does NOT store session

✔️ Scales easily
✔️ No memory usage

**✅ 2. Works across services**

👉 Useful in:

* Microservices
* APIs
* Mobile apps

**✅ 3. Portable**

👉 Token contains data itself

### ⚠️ Important: JWT is NOT Encryption

👉 Anyone can decode payload:

```js
atob(tokenPart)
```

❌ Don’t store sensitive data

### 🍪 What is express-session?

👉 **express-session**

Uses:

✔️ Server-side session storage
✔️ Client stores only session ID (cookie)

**🔄 express-session Flow**

```text
Client → login
Server → creates session
Server → stores data in memory/db
Client → gets session ID (cookie)
Client → sends cookie on each request
```

### ⚔️ JWT vs express-session

🧠 Core Difference

| Feature      | JWT       | express-session |
| ------------ | --------- | --------------- |
| Storage      | Client    | Server          |
| State        | Stateless | Stateful        |
| Scalability  | High ✅   | Harder ❌       |
| Memory usage | Low ✅    | High ❌         |
| Revocation   | Hard ❌   | Easy ✅         |

### ❓ Why NOT use express-session?

**❌ 1. Scaling Problem**

👉 Sessions stored on server

👉 With multiple servers:

* Need Redis / DB

**❌ 2. Memory Usage**

👉 Each user = session stored

**❌ 3. Not ideal for APIs**

👉 Mobile / SPA prefer tokens

### ❓ Why NOT always JWT?

**❌ 1. Cannot easily logout**

👉 Token is valid until expiry

**❌ 2. Security risks**

* XSS (localStorage)
* Token theft

❌ 3. Large payload

👉 Sent in every request

### 🔥 When to Use What?

**✅ Use JWT when:**

* REST APIs
* Microservices
* Mobile apps
* Scalable systems

**✅ Use express-session when:**

* Traditional web apps
* Server-rendered apps
* Need easy logout / session control

### ⚡ Real-World Practice

👉 Many apps use:

✔️ JWT + Refresh Tokens
✔️ HTTP-only cookies (for safety)

---
