# ----Aria

`aria-*` in HTML refers to  **ARIA (Accessible Rich Internet Applications) attributes** —a set of special attributes that help **assistive technologies (like screen readers)** understand your UI better ♿✨

### 🔷 1. What exactly is `aria-*`?

👉 They are attributes added to HTML elements like:

```html
<button aria-expanded="false">Menu</button>
```

👉 Here:

* `aria-expanded` tells whether something (like a dropdown) is open or closed

### 🧠 Why do we need ARIA?

HTML alone sometimes  **doesn’t fully describe UI behavior** , especially for:

* custom components (dropdowns, modals, tabs)
* dynamic content (JS-driven UI)

👉 ARIA fills that gap.

### 🔷 What ARIA actually does

👉 **ARIA does NOT “do” anything visually or functionally**

It **only communicates information** to:

* screen readers 🗣️
* assistive technologies ♿

**🧠 Think of ARIA like this**

**ARIA = a narrator describing your UI**

### **🔥 Question:**

> “Does it just say when a button is pressed?”

👉 ✅ **Yes—but only as information, not behavior**

🧪 Example

```html
<button aria-pressed="true">Like</button>
```

What happens?

* 👁️ Visually → NOTHING changes
* 🖱️ Functionally → NOTHING happens
* 🗣️ Screen reader →
  👉 “Like button, pressed”

### 🔷 Important distinction

❌ ARIA does NOT:

* toggle states
* open dropdowns
* disable buttons
* add click behavior

✅ ARIA DOES:

* describe state (`expanded`, `checked`, etc.)
* describe role (`button`, `dialog`, etc.)
* describe relationships (`controls`, `labelledby`)

### 🔥 Real Example (Dropdown)

```html
<button aria-expanded="false">Menu</button>
```

👉 This means:

* Screen reader: “Menu button, collapsed”

**When user clicks:**

You must do BOTH:

```js
button.setAttribute("aria-expanded", "true");
menu.hidden = false;
```

**🔑 Key point**

👉 **JavaScript changes the UI**
👉 **ARIA explains the UI**

### 🔷 Analogy (very important)

**Without ARIA**

👉 UI works visually
👉 But blind users are lost 😵

**With ARIA**

👉 UI works visually ✅
👉 AND is understandable via screen reader ✅

### 🔷 Another Example

```html
<div role="checkbox" aria-checked="true"></div>
```

👉 Screen reader says:

> “Checkbox checked”

But:

* It won’t toggle
* It won’t behave like a real checkbox

### 🔷 When should you use ARIA?

✅ Use it when:

* building custom components
* using `div` instead of semantic elements
* managing dynamic UI (modals, tabs, etc.)

❌ Avoid when:

You already have native HTML:

```html
<button></button>   ✅ (no need role="button")
<input type="checkbox" /> ✅ (no need aria-checked)
```

### 🔷 2. Common `aria-*` attributes

##### 🟢 `aria-label`

```html
<button aria-label="Close menu">❌</button>
```

👉 Gives a **text label** when no visible text exists

##### 🔵 `aria-hidden`

```html
<div aria-hidden="true"></div>
```

👉 Hides element from screen readers (but still visible visually)

##### 🟣 `aria-expanded`

```html
<button aria-expanded="true">Menu</button>
```

👉 Indicates:

* `true` → open
* `false` → closed

##### 🟡 `aria-checked`

```html
<div role="checkbox" aria-checked="true"></div>
```

👉 Used in custom checkboxes

##### 🔴 `aria-disabled`

```html
<button aria-disabled="true">Submit</button>
```

👉 Means “disabled” (but doesn’t actually disable like `disabled` does)

##### 🟠 `aria-selected`

```html
<li aria-selected="true">Item</li>
```

👉 Used in tabs, lists

### 🔷 3. ARIA vs Normal HTML Attributes

**❌ Don’t replace native HTML**

```html
<button disabled>Submit</button>   ✅ better
```

instead of:

```html
<button aria-disabled="true">Submit</button> ❌ weaker
```

**✅ Use ARIA when needed**

Example:

```html
<div role="button" aria-pressed="true"></div>
```

👉 When using non-semantic elements

### 🔷 4. ARIA + Roles

ARIA often works with `role`

```html
<div role="dialog" aria-modal="true"></div>
```

👉 Defines what the element *is*

### 🔷 5. Important Rules ⚠️

**🚫 Rule 1: Don’t overuse ARIA**

👉 Native HTML is always better

**🚫 Rule 2: ARIA doesn’t add behavior**

```html
<button aria-expanded="true">
```

❌ This does NOT open anything
✔️ You still need JS

**🚫 Rule 3: Keep values accurate**

If UI changes:

* update ARIA too

### 🔷 6. Real-world Example

**Dropdown**

```html
<button aria-expanded="false" aria-controls="menu">
  Menu
</button>

<ul id="menu" hidden>
  <li>Item</li>
</ul>
```

👉 When opened:

* `aria-expanded="true"`
* remove `hidden`

---

# ----Tag format and Attribute types

This is about  **how attributes are structured in HTML tags** , including single values, multiple values, and attributes that contain structured/sub-values. Let’s break it cleanly 👇

### 🧱 1. Basic Tag Format

```html
<tag attribute="value">Content</tag>
```

Or for void elements:

```html
<tag attribute="value">
```

### 🔹 2. Single-Value Attributes

Most attributes take  **one value** :

```html
<input type="text">
<img src="image.jpg" alt="Profile">
```

👉 Here:

* `type="text"` → one value
* `src="image.jpg"` → one value

### 🔹 3. Multiple Values in One Attribute

Some attributes accept  **space-separated values** :

**✅ `class` (most common)**

```html
<div class="card primary shadow-lg"></div>
```

👉 Internally:

```js
["card", "primary", "shadow-lg"]
```

**✅ `rel`**

```html
<link rel="stylesheet preload">
```

**✅ `sandbox` (iframe)**

```html
<iframe sandbox="allow-scripts allow-same-origin"></iframe>
```

### 🔹 4. Boolean Attributes (No Value Needed)

Presence = `true`, absence = `false`

```html
<input type="checkbox" checked>
<button disabled>Click</button>
```

Equivalent (but not necessary):

```html
<input checked="checked">
```

### 🔹 5. Attributes with Structured / Sub-Values

These look like they have “sub-attributes”, but technically it's just a  **single string with a defined format** .

##### 🎨 a) `style` (CSS inside attribute)

```html
<div style="color: red; font-size: 16px;"></div>
```

👉 Format:

```css
property: value; property: value;
```

##### 📡 b) `data-*` (custom data)

```html
<div data-user-id="123" data-role="admin"></div>
```

👉 Access in JS:

```js
element.dataset.userId
```

##### ♿ c) `aria-*` (accessibility)

```html
<button aria-label="Close" aria-hidden="true"></button>
```

These are standardized accessibility attributes.

##### 🔗 d) `srcset` (responsive images)

```html
<img srcset="small.jpg 500w, large.jpg 1000w">
```

👉 Format:

```
URL widthDescriptor, URL widthDescriptor
```

##### 📏 e) `sizes`

```html
<img sizes="(max-width: 600px) 100vw, 50vw">
```

##### 🧭 f) `meta viewport`

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

👉 Inside `content`:

```
key=value, key=value
```

### 🔹 6. Comma-Separated Values

Some attributes use commas:

```html
<img srcset="img1.jpg 1x, img2.jpg 2x">
```

### 🔹 7. Attributes Without Quotes (Allowed but Risky)

```html
<input type=text>
```

✅ Works
❌ Avoid if:

* value has spaces
* special characters

### 🔹 8. Mixing Everything Together

```html
<img 
  src="image.jpg"
  class="card shadow-lg"
  style="border-radius: 10px; width: 100px;"
  data-id="101"
  alt="Profile image"
>
```

---

# ----Global attributes

**Global attributes** in HTML are attributes that can be applied to  **almost every HTML element** , regardless of its type. They’re part of the core HTML spec and give you universal control over behavior, styling, accessibility, and metadata 🌍

### 🆔 Identification & Classification

* **`id`** → Unique identifier for an element
* **`class`** → Used for grouping elements (CSS / JS targeting)

```html
<div id="header" class="container main"></div>
```

### 🎨 Styling

* **`style`** → Inline CSS styles

```html
<p style="color: red;">Hello</p>
```

### 🌐 Language & Direction

* **`lang`** → Language of content
* **`dir`** → Text direction (`ltr`, `rtl`)

```html
<p lang="en" dir="ltr">Hello</p>
```

### 🎯 Accessibility (Very Important)

* **`title`** → Tooltip text
* **`tabindex`** → Controls keyboard navigation
* **`accesskey`** → Keyboard shortcut

```html
<button title="Submit form" tabindex="1">Submit</button>
```

### 🧠 Editing & Behavior

* **`contenteditable`** → Makes element editable
* **`draggable`** → Allows drag & drop
* **`hidden`** → Hides the element

```html
<div contenteditable="true">Edit me</div>
```

### 📡 Data & Custom Attributes

* **`data-*`** → Store custom data

```html
<div data-user-id="123"></div>
```

Access in JS:

```js
element.dataset.userId
```

### ⚡ Event Handlers (Technically Global)

You can attach events to almost any element:

* `onclick`, `oninput`, `onchange`, etc.

```html
<button onclick="alert('Clicked!')">Click</button>
```

### ♿ ARIA Attributes (Accessibility Boost)

* `aria-*` attributes improve screen reader support

```html
<button aria-label="Close">X</button>
```

### 🧾 Summary

Global attributes:

* Work on **almost all HTML elements**
* Help with:
  * Styling (`class`, `style`)
  * Identification (`id`)
  * Accessibility (`aria-*`, `title`)
  * Behavior (`hidden`, `draggable`)
  * Custom data (`data-*`)

---

# ----Global attributes in detail

### ✅ MUST-KNOW GLOBAL ATTRIBUTES

These are used  **daily in real-world projects** .

##### 🆔 1. `id`

* Unique identifier (only one per page ideally)

```html
<div id="navbar"></div>
```

##### 🏷️ 2. `class`

* Multiple values allowed (space-separated)

```html
<div class="card shadow-lg active"></div>
```

##### 🎨 3. `style`

* Inline CSS (not preferred, but important to know)

```html
<p style="color: red;"></p>
```

##### 🌐 4. `title`

* Tooltip on hover

```html
<button title="Delete item">Delete</button>
```

##### 🎯 5. `tabindex`

* Controls keyboard navigation order

```html
<input tabindex="1">
```

> ###### ✅ What it does:
>
> Controls **how elements receive focus when you press `Tab`**
>
> ###### 🔢 Values:
>
> **🔹 `tabindex="0"`**
>
> * Element becomes **focusable**
> * Follows **natural DOM order**
>
> ```html
> <div tabindex="0">Focusable div</div>
> ```
>
> 👉 Use this when:
>
> * Making non-interactive elements (like `<div>`) keyboard accessible
>
> **🔹 `tabindex="1"` (or any positive number)**
>
> * Custom tab order (priority-based)
>
> ```html
> <input tabindex="2">
> <input tabindex="1">
> ```
>
> 👉 Tab order:
> 1 → 2
>
> ⚠️ Avoid in real projects — **breaks natural accessibility flow**
>
> **🔹 `tabindex="-1"`**
>
> * Not reachable via Tab
> * Can still be focused via JS
>
> ```html
> <div tabindex="-1" id="modal"></div>
> ```
>
> ```js
> document.getElementById("modal").focus();
> ```
>
> 👉 Used for:
>
> * Modals
> * Error messages
> * Focus management

##### 🙈 6. `hidden`

* Completely hides element

```html
<div hidden></div>
```

> ###### ✅ What it does:
>
> Completely **hides the element from the page**
>
> ```html
> <div hidden>This is hidden</div>
> ```
>
> **🔍 Behavior:**
>
> * Not visible
> * Not interactive
> * Removed from **accessibility tree**
>
> **⚖️ Equivalent to:**
>
> ```css
> display: none;
> ```
>
> ###### 🔁 Toggle with JS:
>
> ```js
> element.hidden = false; // show
> element.hidden = true;  // hide
> ```
>
> ###### ⚠️ Difference vs CSS:
>
> | Method                | Visible | Accessible |
> | --------------------- | ------- | ---------- |
> | `hidden`            | ❌      | ❌         |
> | `visibility:hidden` | ❌      | ✅         |
> | `opacity:0`         | ❌      | ✅         |
>
> ###### 🧠 Key Insight:
>
> * `hidden` = **completely removed from UI + accessibility**

##### ✏️ 7. `contenteditable`

* Makes element editable

```html
<div contenteditable="true">Edit me</div>
```

> ###### ✅ What it does:
>
> Turns any element into an **editable text area**
>
> ```html
> <div contenteditable="true">Edit this text</div>
> ```
>
> **🔢 Values:**
>
> 🔹 `true`
>
> * Editable
>
> 🔹 `false`
>
> * Not editable (override parent)
>
> 🧪 Example:
>
> ```html
> <div contenteditable="true">
>   Parent editable
>   <p contenteditable="false">Not editable</p>
> </div>
> ```
>
> ###### 🔍 Behavior:
>
> * User can type, delete, format text
> * Acts like a mini text editor
>
> ###### ⚠️ Important Notes:
>
> * No built-in validation
> * Can introduce **XSS risks** if not sanitized
> * Used in:
>   * Rich text editors
>   * Chat inputs
>   * CMS tools

##### 📦 8. `data-*`

* Custom data storage (VERY important for JS)

```html
<div data-user-id="42"></div>
```

JS:

```js
element.dataset.userId
```

##### ♿ 9. `aria-*`

* Accessibility (huge in modern dev)

```html
<button aria-label="Close menu"></button>
```

##### 🌍 10. `lang`, `dir`

```html
<html lang="en" dir="ltr"></html>
```

### 💡 RARE BUT POWERFUL GLOBAL ATTRIBUTES

These make you stand out in interviews 😄

##### 🧲 1. `draggable`

* Enables drag & drop

```html
<div draggable="true"></div>
```

##### 🧭 2. `spellcheck`

* Enable/disable spell checking

```html
<input spellcheck="false">
```

##### 🔍 3. `translate`

* Prevent auto translation

```html
<p translate="no">BrandName</p>
```

##### 🧬 4. `is` (Custom Elements)

* Extend built-in elements

```html
<button is="custom-button"></button>
```

---

# ----Tags- `embed vs noembed` and `object & iframe`

The comparison between `<embed>` and `<noembed>` is mostly about  **modern vs legacy HTML** —one is still used, the other is basically obsolete

### 🔹 embed tag (Modern, still used)

**✅ What it does:**

Embeds **external content** into a webpage (media, PDFs, plugins, etc.)

**🧱 Syntax:**

```html
<embed src="file.pdf" type="application/pdf" width="600" height="400">
```

**📌 Common uses:**

* PDF viewer
* Audio/video (less common now due to `<audio>` / `<video>`)
* External content like Flash (historically)

**⚠️ Notes:**

* It’s a **void element** → no closing tag
* Works natively in modern browsers
* Often replaced by more semantic tags like:
  * `<iframe>`
  * `<video>`
  * `<audio>`
  * `<object>`

### 🔹 noembed tag (Deprecated ❌)

**❌ What it was:**

Fallback content for browsers that **didn’t support `<embed>`**

🧱 Example:

```html
<embed src="video.swf">
<noembed>Your browser does not support embedded content.</noembed>
```

**⚠️ Why it’s obsolete:**

* Modern browsers **all support `<embed>`**
* `<noembed>` is **not part of modern HTML5 standards**
* Better alternatives exist

### 🔁 Modern Replacement Pattern for noembed tag

Instead of `<noembed>`, use:

##### ✅ object tag with fallback:

```html
<object data="file.pdf" type="application/pdf">
  <p>Your browser can't display PDFs. <a href="file.pdf">Download instead</a>.</p>
</object>
```

##### ✅ Or iframe tag:

```html
<iframe src="file.pdf" width="600" height="400"></iframe>
```

### 🧾 Summary

| Feature      | `<embed>`✅ | `<noembed>`❌          |
| ------------ | ------------- | ------------------------ |
| Purpose      | Embed content | Fallback for `<embed>` |
| Status       | Supported     | Deprecated               |
| Closing tag  | No (void)     | Yes                      |
| Modern usage | Yes           | No                       |

---

# ----Video & Audio tag using source tag

You **can** use `src` directly on `<video>`, but `<source>` exists for flexibility and fallback.

### ✅ 1. Using `src` directly on video tag

```html
<video src="movie.mp4" controls></video>
```

✔️ Works fine when:

* You have **only one video format**
* You’re okay with **no fallback**

👉 Simple, but limited.

### 🔥 2. Using source tag inside video tag

```html
<video controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.webm" type="video/webm">
</video>
```

### 🎯 Why source tag exists

##### 🔹 1. Multiple format support (BIGGEST reason)

Different browsers support different formats:

| Format | Support            |
| ------ | ------------------ |
| MP4    | ✅ Most browsers   |
| WebM   | ✅ Chrome, Firefox |
| OGG    | ⚠️ Limited       |

👉 Browser will:

* Try first `<source>`
* If unsupported → try next one

##### 🔹 2. Automatic fallback handling

```html
<video controls>
  <source src="movie.webm" type="video/webm">
  <source src="movie.mp4" type="video/mp4">
  Your browser doesn't support video.
</video>
```

👉 Browser decides what to play — no JS needed.

##### 🔹 3. Better control with `type`

```html
<source src="movie.mp4" type="video/mp4">
```

👉 Browser can **skip downloading unsupported formats** 🚀

### 🎧 Audio tag with source tag (recommended)

```html
<audio controls>
  <source src="song.mp3" type="audio/mpeg">
  <source src="song.ogg" type="audio/ogg">
  Your browser does not support the audio element.
</audio>
```

### ⚖️ Direct Comparison

| Approach              | Pros             | Cons               |
| --------------------- | ---------------- | ------------------ |
| `src`on `<video>` | Simple           | No fallback        |
| `<source>`          | Flexible, robust | Slightly more code |

### 🧠 Important Behavior

👉 If both are present:

```html
<video src="movie.mp4" controls>
  <source src="movie.webm" type="video/webm">
</video>
```

➡️ `<source>` is used, and `src` on `<video>` is **ignored**

### 💡 When to use what?

✅ Use `src` directly:

* Quick demo
* One guaranteed format

✅ Use `<source>` (recommended in real apps):

* Production apps
* Cross-browser support
* Performance optimization

---

# ----Video tag vs embed of type video

This is a **classic modern vs legacy comparison** — and interviewers love it because it tests whether you understand *semantics + browser behavior* 🎯

### ✅ 1. Video tag (Modern, recommended)

**✔️ Purpose:**

Specifically designed for **playing videos in HTML5**

🧱 Example:

```html
<video controls width="600">
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.webm" type="video/webm">
  Your browser does not support the video tag.
</video>
```

**🌟 Features:**

* Built-in controls (play, pause, volume) 🎮
* Supports multiple formats via `<source>`
* Supports:
  * `autoplay`
  * `loop`
  * `muted`
  * `controls`
* Subtitles via `<track>` 📝
* Fully **accessible & semantic**

##### 🧠 Key Insight:

👉 `<video>` = **native video player built into browser**

### ⚙️ 2. Embed tag of type video (Generic, older approach)

**✔️ Purpose:**

Generic embedding of  **external resources** , including video

🧱 Example:

```html
<embed src="movie.mp4" type="video/mp4" width="600" height="400">
```

**⚠️ Limitations:**

* ❌ No built-in controls
* ❌ No multiple source fallback
* ❌ No subtitles support
* ❌ Poor accessibility
* ❌ Less semantic (browser doesn’t “know” it's a video player)

##### 🧠 Key Insight:

👉 `<embed>` = **just dumps content, no intelligence**

### ⚔️ Direct Comparison

| Feature                 | `<video>`✅      | `<embed>`❌ |
| ----------------------- | ------------------ | ------------- |
| Semantic meaning        | Yes                | No            |
| Built-in controls       | Yes                | No            |
| Multiple formats        | Yes (`<source>`) | No            |
| Subtitles (`<track>`) | Yes                | No            |
| Accessibility           | Good               | Poor          |
| Modern usage            | Standard           | Rare          |

### 🔥 When would you EVER use `<embed>`?

Almost never for video.

Only if:

* Embedding **unknown media/plugin content**
* Legacy systems

### ✅ Modern Alternatives

Instead of `<embed>`:

* Use `<video>` → for videos 🎬
* Use `<audio>` → for audio 🎧
* Use `<iframe>` → for YouTube, Vimeo

---
