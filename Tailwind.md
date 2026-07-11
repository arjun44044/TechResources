# ----Increasing gap between rows in the table

To increase the  **gap between rows in a `<table>`** , you can’t directly use `gap` like in Flex or Grid — but you *can* simulate row spacing using a few techniques:

Tailwind class: `border-separate` with `border-spacing-y-*`

Example---

```html
<table class="border-separate border-spacing-y-4 w-full">
  <thead>
    <tr>
      <th>Header 1</th>
      <th>Header 2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Row 1</td>
      <td>Row 1</td>
    </tr>
    <tr>
      <td>Row 2</td>
      <td>Row 2</td>
    </tr>
  </tbody>
</table>

```

This will add vertical space between rows.

##### 🟨 Notes:

* You **must** use `border-separate` (not `border-collapse`) for `border-spacing` to work.
* You can adjust the spacing with Tailwind classes like `border-spacing-y-2`, `border-spacing-y-4`, etc.

### In CSS

To do the same with css, apply class "dashed-table" in `<table>' then, `

```css
.dashed-table {
  border-collapse: separate;
  border-spacing: 0 12px; /* spacing between rows */
  width: 100%;
}
```

### For Dashed Horizontal line

`<hr class="border-t border-dashed border-gray-400 my-4" />`

* `border-t` → top border (used by `<hr>`)
* `border-dashed` → makes it dashed
* `border-gray-400` → color
* `my-4` → margin top and bottom

You can change the color, thickness, or margins as needed.

---

# ----Grouping

The `group` utility in Tailwind CSS can be  **confusing at first** , but it's a  **powerful way to style child elements based on the parent’s state** , especially on hover or focus.

#### 🧠 What is `group` in Tailwind?

In Tailwind, `group` is used to allow **child elements to respond to the state (like hover/focus/active)** of a parent element.

Think of it like saying:

> *“When the parent is hovered, change the style of the child.”*

#### 🛠️ How It Works

✅ Step 1: Add `group` to the parent

```html
<div class="group hover:bg-gray-200">
```

✅ Step 2: Add `group-*` variant to child

```html
  <span class="group-hover:text-red-500">
```

#### 🧪 Example

```html
<div class="group p-4 border hover:bg-gray-100 rounded-lg">
  <p class="text-sm group-hover:text-blue-600 transition">Hover over the box</p>
</div>
```

* ✅ When the  **parent `<div>` is hovered** , the  **child `<p>` text color changes** .

#### 🎯 Common Use-Cases

| Use Case                | Example                                                 |
| ----------------------- | ------------------------------------------------------- |
| Tooltip on hover        | `group-hover:visible group-hover:opacity-100`         |
| Icon animation on hover | `group-hover:rotate-180`                              |
| Button child effects    | `group-hover:scale-105`,`group-hover:translate-x-1` |
| Nested menus            | `group-hover:block`to show dropdowns                  |

#### 💡 Why Use It?

* Allows  **clean, scalable hover/focus behavior** .
* Avoids JS for simple interactive UI.
* Works **declaratively** and is perfect for  **components** .

#### 🔁 Bonus: Works with other states too!

* `group-focus:*`
* `group-active:*`
* `group-disabled:*`

#### 🚫 Without `group`, This Won’t Work

```html
<div class="hover:bg-gray-200">
  <span class="hover:text-red-500"> ← Only triggers when **span** is hovered </span>
</div>
```

To trigger styles based on the parent,  **you must use `group`** .

#### ✅ Example: Tooltip + Icon Colxor Change on Hover

Here's a **complete working example** showing how `group` is used in Tailwind to trigger **tooltip visibility** and **child styling** when hovering over a parent:

```html
<div class="group relative inline-block cursor-pointer">
  <button class="bg-blue-500 text-white px-4 py-2 rounded">
    Hover me
  </button>

  <!-- Tooltip -->
  <div class="absolute bottom-full left-1/2 -translate-x-1/2 mb-2 px-3 py-1 rounded bg-gray-800 text-white text-xs
              opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all duration-300">
    Tooltip shown on hover
  </div>

  <!-- Icon with hover effect -->
  <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5 mt-2 text-gray-500 transition-colors group-hover:text-blue-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
  </svg>
</div>
```

🔍 What’s Happening?

| Element            | Behavior                                                        |
| ------------------ | --------------------------------------------------------------- |
| `.group`         | Added to the parent `div`to act as a hover group              |
| `.group-hover:*` | Child tooltip and icon styles change when the parent is hovered |
| `tooltip`        | Appears with fade-in effect                                     |
| `svg icon`       | Changes color on hover of parent button                         |

---

# ----Peer based styling

It is a **power features in Tailwind** that let you style elements based on **state coming from somewhere else** 🔥

Once you get them, your UI logic becomes much cleaner.

### 🔷 1. `peer-*` (Styling based on a sibling)

**🧠 Core Idea**

Style an element based on the state of a **previous sibling**

**⚠️ Rule (VERY important)**

* The element being watched must have class **`peer`**
* The element you style must come **after it** in the DOM

**🧪 Basic Example**

```html
<input type="checkbox" class="peer" />

<div class="hidden peer-checked:block">
  Visible when checked ✅
</div>
```

🔍 What’s happening

```css
.peer:checked ~ div {
  display: block;
}
```

### 🔥 Common Variants

**🟢 peer-focus**

```html
<input class="peer" />

<p class="peer-focus:text-blue-500">
  Changes when input is focused
</p>
```

**🔵 peer-checked**

```html
<input type="checkbox" class="peer" />

<div class="peer-checked:bg-green-500"></div>
```

**🟣 peer-invalid**

```html
<input type="email" class="peer" />

<p class="peer-invalid:text-red-500">
  Invalid email
</p>
```

### 🎯 Real Use Case

👉 Floating labels:

```html
<div class="relative">
  <input class="peer border" placeholder=" " />
  
  <label class="absolute left-2 top-2 
    peer-focus:-top-3 peer-focus:text-sm">
    Email
  </label>
</div>
```

---

# ----Aria and styling based on accessibility state

### 🧠 Core Idea

👉 Style element based on **ARIA attributes**

**🧪 Example**

```html
<button aria-expanded="true"
  class="aria-expanded:bg-blue-500">
  Toggle
</button>
```

🔍 What’s happening

```css
button[aria-expanded="true"] {
  background: blue;
}
```

### 🔥 Common ARIA Variants

🟢 aria-checked

```html
<div aria-checked="true"
  class="aria-checked:bg-green-500">
</div>
```

🔵 aria-disabled

```html
button aria-disabled="true"
  class="aria-disabled:opacity-50">
</button>
```

🟣 aria-selected

```html
<li aria-selected="true"
  class="aria-selected:bg-blue-200">
</li>
```

### 🔷 Combining `peer` + `aria` 🔥

```html
<button class="peer" aria-expanded="true"></button>

<div class="peer-aria-expanded:block hidden">
  Menu
</div>
```

👉 Super powerful for dropdowns, accordions, etc.

---

# ----Styling based on external conditions

This is where Tailwind really shines 🔥

You already know `group` and `peer`. Now let’s expand your mental model to **all “state-sharing / contextual” utilities** like them.

# 🧠 Big Picture

These are all about:

👉 **“Style this element based on some external condition”**

### 🔷 1. `group-*` (Parent → Child)

**🧠 Concept**

Style children based on **parent state**

🧪 Example

```html
<div class="group">
  <p class="group-hover:text-red-500">
    Hover me
  </p>
</div>
```

👉 When parent is hovered → child changes

**🔥 Variants**

```html
group-hover:
group-focus:
group-active:
group-disabled:
```

### 🔷 2. `peer-*` (Sibling → Sibling)

**🧠 Concept**

Style element based on **previous sibling**

🧪 Example

```html
<input type="checkbox" class="peer" />

<div class="peer-checked:bg-green-500">
  Changes
</div>
```

### 🔷 3. `aria-*` (Attribute-based)

**🧠 Concept**

Style based on **ARIA attributes**

🧪 Example

```html
<button aria-expanded="true"
  class="aria-expanded:bg-blue-500">
</button>
```

### 🔷 4. `data-*` (Custom attribute state) 🔥

**🧠 Concept**

Style based on **data attributes**

🧪 Example

```html
<div data-state="open"
  class="data-[state=open]:bg-green-500">
</div>
```

**🔥 Very useful for JS frameworks**

* React
* Headless UI
* Radix UI

### 🔷 5. `has-*` (Parent reacts to children) 🧠🔥

**🧠 Concept**

Style parent based on **child condition**

🧪 Example

```html
<div class="has-[input:checked]:bg-green-200">
  <input type="checkbox" />
</div>
```

👉 Parent changes if child is checked

**⚠️ Uses CSS `:has()` (modern browsers only)**

### 🔷 6. `not-*` (Negation)

🧠 Concept

Apply style when condition is NOT met

🧪 Example

```html
<div class="not-hover:bg-gray-200">
</div>
```

🔥 Combined

```html
<div class="hover:bg-blue-500 not-hover:bg-gray-200"></div>
```

### 🔷 7. `supports-*` (Feature queries)

**🧠 Concept**

👉 Apply style if browser supports something

🧪 Example

```html
<div class="supports-[display:grid]:grid">
</div>
```

### 🔷 8. `motion-*` (User preference)

**🧠 Concept**

Respect reduced motion settings

🧪 Example

```html
<div class="motion-safe:animate-bounce 
            motion-reduce:animate-none">
</div>
```

### 🔷 9. `dark` (Theme-based)

**🧠 Concept**

Apply styles in dark mode

🧪 Example

```html
<div class="bg-white dark:bg-black"></div>
```

### 🔷 10. `rtl` / `ltr` (Direction)

**🧠 Concept**

👉 Based on text direction

🧪 Example

```html
<div class="rtl:text-right ltr:text-left"></div>
```

### 🔷 11. `first`, `last`, `odd`, `even` (Structural)

🧪 Example

```html
<li class="first:text-red-500"></li>
<li class="odd:bg-gray-100"></li>
```

### 🔷 12. `focus-within` 🔥

**🧠 Concept**

Parent reacts when child is focused

🧪 Example

```html
<div class="focus-within:border-blue-500">
  <input />
</div>
```

### 🔷 13. `target` (URL-based)

🧪 Example

```html
<div id="section"
  class="target:bg-yellow-200">
</div>
```

👉 When URL = `#section`

### 🔷 14. `open` (Details/summary)

```html
<details class="open:bg-gray-100">
```

### 🔷 15. `selection` (Text selection)

```html
<p class="selection:bg-yellow-300">
```

### 🔥 Final Comparison Table

| Utility          | Depends On | Direction            |
| ---------------- | ---------- | -------------------- |
| `group-*`      | parent     | parent → child      |
| `peer-*`       | sibling    | prev sibling → next |
| `aria-*`       | attributes | self                 |
| `data-*`       | attributes | self                 |
| `has-*`        | children   | child → parent      |
| `focus-within` | children   | child → parent      |

### 🧠 Ultimate Mental Model

Tailwind state system = **“who affects whom”**

* Parent → child → `group`
* Sibling → sibling → `peer`
* Self attributes → `aria`, `data`
* Child → parent → `has`, `focus-within`

### 🚀 One-line takeaway

**Tailwind’s advanced utilities let you style elements based on relationships (parent, sibling, child) or state (ARIA, data, media)—often eliminating the need for JavaScript**

---

# ----Abstract Utilities

Now we are stepping into **Tailwind’s “abstract utility layer”** (things that  *don’t directly exist as CSS properties* ) 😄

These are utilities that:
👉 either combine multiple CSS properties
👉 or use CSS features in a hidden way
👉 or are just naming abstractions

### 🔷 1. `ring-*` (Focus ring system)

🧪 Example

```html
<button class="ring-2 ring-blue-500"></button>
```

**🔍 What it actually does**

```css
box-shadow: 0 0 0 2px blue;
```

👉 It’s just a  **box-shadow trick** , not a real “ring” property

### 🔷 2. `shadow-*`

```html
shadow-lg
```

👉 Maps to:

```css
box-shadow: ...
```

### 🔷 3. `divide-*` 🔥

```html
<div class="divide-y divide-gray-300">
```

**🔍 What it does**

Adds borders **between children only**

👉 Not a real CSS concept

```css
> * + * {
  border-top: 1px solid;
}
```

### 🔷 4. `space-*` 🔥

```html
<div class="space-x-4">
```

**🔍 What it does**

Adds spacing between children:

```css
> * + * {
  margin-left: 1rem;
}
```

👉 Like `gap`, but works even without flex/grid

### 🔷 5. `container`

```html
<div class="container"></div>
```

👉 Applies:

* max-width at breakpoints
* auto margin

👉 No direct CSS equivalent keyword

### 🔷 6. `sr-only`

```html
<span class="sr-only">Hidden text</span>
```

**🔍 Expands to:**

```css
position: absolute;
width: 1px;
height: 1px;
overflow: hidden;
```

👉 For screen readers only

### 🔷 7. `not-sr-only`

👉 Reverses the above

### 🔷 8. `truncate` 🔥

```html
<p class="truncate"></p>
```

**🔍 Expands to:**

```css
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap;
```

### 🔷 9. `line-clamp-*`

```html
line-clamp-3
```

👉 Uses:

```css
display: -webkit-box;
-webkit-line-clamp: 3;
```

👉 Not a standard simple property

### 🔷 10. `aspect-*`

```html
aspect-square
aspect-video
```

👉 Uses:

```css
aspect-ratio: 1 / 1;
```

👉 But simplified naming

### 🔷 11. `columns-*`

```html
columns-3
```

👉 Maps to:

```css
column-count: 3;
```

👉 Abstract naming

### 🔷 12. `isolate`

```html
isolate
```

👉 Maps to:

```css
isolation: isolate;
```

👉 Rarely used CSS, abstracted nicely

### 🔷 13. `backdrop-*`

```html
backdrop-blur-md
```

👉 Maps to:

```css
backdrop-filter: blur(...)
```

👉 Not directly intuitive from name

### 🔷 14. `scroll-*` utilities

```html
scroll-mt-10
scroll-smooth
```

👉 Combine scroll-related behaviors

### 🔷 15. `appearance-none`

```html
appearance-none
```

👉 Removes default browser styling

### 🔷 16. `accent-*`

```html
accent-blue-500
```

👉 Styles checkbox/radio accent color

### 🔷 17. `caret-*`

```html
caret-red-500
```

👉 Text cursor color

### 🔷 18. `pointer-events-*`

```html
pointer-events-none
```

👉 Controls interaction

### 🔷 19. `select-*`

```html
select-none
```

👉 Prevents text selection

### 🔷 20. `will-change`

```html
will-change-transform
```

👉 Performance hint

### 🔥 Most Important Ones (Interview Focus)

👉 If you remember just these:

* `ring-*`
* `space-*`
* `divide-*`
* `truncate`
* `line-clamp-*`
* `sr-only`

You’re already ahead of most devs 🚀

### 🧠 Mental Model

👉 These utilities are:

> “Convenience abstractions over multiple CSS rules”

---
