# Increasing gap between rows in the table

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

# For Dashed Horizontal line

`<hr class="border-t border-dashed border-gray-400 my-4" />`

* `border-t` → top border (used by `<hr>`)
* `border-dashed` → makes it dashed
* `border-gray-400` → color
* `my-4` → margin top and bottom

You can change the color, thickness, or margins as needed.

---

# Grouping

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
