# **----Types of variable**

SCSS variables are one of the **biggest productivity boosters** in styling 🔥

They let you store values (colors, spacing, fonts, etc.) and reuse them cleanly.

Let’s go step by step—but importantly:
👉 In SCSS, variables don’t have “types” like JavaScript
👉 Instead, they hold **different kinds of values**

**🔷 Basic Syntax**

```scss
$variable-name: value;
```

```scss
$primary-color: blue;
```

### 🔥 Types of Values SCSS Variables Can Hold

### 🟢 1. Numbers

**🧠 Includes:**

* unitless numbers
* numbers with units

🧪 Example

```scss
$padding: 10;
$margin: 20px;
$width: 50%;
```

```scss
.box {
  padding: $padding + 5;   // 15
  margin: $margin;
  width: $width;
}
```

### 🔵 2. Strings

**🧠 Text values**

🧪 Example

```scss
$font-stack: "Arial, sans-serif";
$direction: left;
```

```scss
body {
  font-family: $font-stack;
  text-align: $direction;
}
```

### 🟡 3. Colors

**🧠 SCSS fully understands colors**

🧪 Example

```scss
$primary: #3498db;
$secondary: red;
```

```scss
.button {
  background: $primary;
  border-color: $secondary;
}
```

**🔥 Bonus (Color functions)**

```scss
$primary: #3498db;

.button {
  background: darken($primary, 10%);
}
```

### 🟣 4. Booleans

🧪 Example

```scss
$is-dark: true;
```

```scss
body {
  @if $is-dark {
    background: black;
  } @else {
    background: white;
  }
}
```

### 🟠 5. Null

🧪 Example

```scss
$margin: null;

.box {
  margin: $margin;
}
```

**👉 Property won’t be output if value is `null`**

### 🔴 6. Lists

**🧠 Multiple values in one variable**

🧪 Example

```scss
$spacing: 10px 20px 30px;
```

```scss
.box {
  margin: $spacing;
}
```

**🔥 Advanced**

```scss
$colors: red, green, blue;
```

### 🟤 7. Maps (VERY IMPORTANT 🔥)

**🧠 Key-value pairs (like objects)**

🧪 Example

```scss
$theme: (
  primary: #3498db,
  secondary: #2ecc71
);
```

**🔥 Accessing values**

```scss
.button {
  background: map-get($theme, primary);
}
```

**🔥 Real-world Example**

```scss
$colors: (
  primary: #3498db,
  danger: #e74c3c
);

$spacing: 16px;

.button {
  padding: $spacing;
  background: map-get($colors, primary);
}
```

### 🔥 Variable Scope (IMPORTANT)

🧪 Example

```scss
$color: red;

.box {
  $color: blue;
  color: $color; // blue
}

.text {
  color: $color; // red
}
```

👉 Inner variables override locally

### 🔥 Global Variables

```scss
$color: red !global;
```

👉 Makes variable available everywhere

### 🧠 Mental Model

👉 SCSS variables are:

* simple containers
* flexible (can hold anything)
* resolved at compile time

---

# ----Types of Strings

**In SCSS, strings *can* be written without quotes** , but there’s an important nuance 🔥

### 🔷 Two Types of Strings in SCSS

**🟢 1. Quoted strings**

```scss
$font: "Arial";
$message: 'Hello World';
```

👉 Treated explicitly as strings

**🔵 2. Unquoted strings**

```scss
$direction: left;
$display: flex;
$color-name: red;
```

👉 These are also  **strings** , just written without quotes

### 🧠 So… what’s the difference?

**✅ They behave mostly the same**

```scss
$val1: "center";
$val2: center;

.box {
  text-align: $val1;
  text-align: $val2;
}
```

👉 Output:

```css
text-align: center;
text-align: center;
```

✔️ No difference in most CSS use cases

### 🔥 BUT — Important Differences

**⚠️ 1. Quoted strings stay quoted in output**

```scss
$content: "Hello";
```

```scss
.box::before {
  content: $content;
}
```

👉 Output:

```css
content: "Hello";
```

**⚠️ 2. Unquoted strings may behave like identifiers**

```scss
$font: Arial;
```

👉 This is fine, but:

```scss
$content: Hello;
```

```scss
.box::before {
  content: $content;
}
```

👉 Output:

```css
content: Hello;
```

❌ Invalid in CSS (needs quotes)

**⚠️ 3. Spaces REQUIRE quotes**

```scss
$bad: Hello World;   // ❌ ERROR
```

```scss
$good: "Hello World"; // ✅
```

**⚠️ 4. Special characters need quotes**

```scss
$url: url(image.png);   // fine
$text: Hello@world;     // ❌ risky
```

👉 Safer:

```scss
$text: "Hello@world";
```

**⚠️ 5. Functions & keywords confusion**

```scss
$val: calc;   // might conflict with CSS function
```

👉 Better:

```scss
$val: "calc";
```

### 🔥 When to Use What

**✅ Use unquoted strings when:**

* CSS keywords
* Simple identifiers

```scss
display: flex;
text-align: center;
position: absolute;
```

**✅ Use quoted strings when:**

* Text content
* Fonts with spaces
* Anything with spaces/special chars

```scss
font-family: "Open Sans";
content: "Hello";
```

### 🧠 Mental Model

👉

* Unquoted = **CSS keyword / identifier-like**
* Quoted = **actual text string**

---

# ----List and its build-in functions

SCSS **lists** are super powerful once you get them 🔥

They look simple, but they unlock loops, dynamic styles, and reusable patterns.

### 🔷 What is a List in SCSS?

👉 A **list** is a collection of values stored in a single variable

```scss
$my-list: value1 value2 value3;
```

### 🔥 Types of Lists

SCSS has  **two main kinds based on separators** :

**🟢 1. Space-separated list (default)**

```scss
$spacing: 10px 20px 30px;
```

👉 Equivalent to:

```css
margin: 10px 20px 30px;
```

**🔵 2. Comma-separated list**

```scss
$colors: red, green, blue;
```

👉 Useful for:

```css
box-shadow, font-family, gradients
```

**🔥 3. Bracketed Lists (LESS common)**

```scss
$items: [one, two, three];
```

👉 Keeps structure intact (used in advanced cases)

> #### 🔷 The Two Forms of Bracketed Lists
>
> **🟢 1. Parentheses list → `(1, 2, 3)`**
>
> ```scss
> $list: (1, 2, 3);
> ```
>
> 👉 This is a **normal SCSS list**
>
> **🔵 2. Bracketed list → `[1, 2, 3]`**
>
> ```scss
> $list: [1, 2, 3];
> ```
>
> 👉 This is a **bracketed list (special type of list)**
>
> #### 🔥 Key Differences
>
> ###### **🧠 1. Output Behavior**
>
> ✅ Parentheses
>
> ```scss
> $list: (1, 2, 3);
>
> .box {
>   values: $list;
> }
> ```
>
> 👉 Output:
>
> ```css
> values: 1, 2, 3;
> ```
>
> ✅ Brackets
>
> ```scss
> $list: [1, 2, 3];
>
> .box {
>   values: $list;
> }
> ```
>
> 👉 Output:
>
> ```css
> values: [1, 2, 3];
> ```
>
> 🔥 Brackets are **preserved in CSS**
>
> ###### 🧠 2. Use Case Difference
>
> ✅ `(1, 2, 3)` → Regular list
>
> Used for:
>
> * margins
> * padding
> * colors
> * loops
>
> ```scss
> margin: (10px, 20px);
> ```
>
> 👉 becomes:
>
> ```css
> margin: 10px, 20px;
> ```
>
> ✅ `[1, 2, 3]` → Structured / literal output
>
> Used when you want:
>
> * exact structure preserved
> * modern CSS features (like grid, custom syntax)
>
> ```scss
> grid-template-columns: [col1] 1fr [col2] 1fr;
> ```
>
> #### 🔥 3. Internal Behavior
>
> Both support list functions:
>
> ```scss
> @use "sass:list";
>
> $list: [1, 2, 3];
>
> length($list);        // 3
> list.nth($list, 2);   // 2
> ```
>
> ✔️ SCSS treats both as lists internally
>
> #### 🔥 4. Separator Still Matters
>
> ```scss
> $list1: (1 2 3);     // space-separated
> $list2: (1, 2, 3);   // comma-separated
> $list3: [1 2 3];     // bracket + space
> ```
>
> #### 🔥 When to Use What
>
> ✅ Use `( )` when:
>
> * General styling
> * Loops
> * Data structures
>
> 👉 Most common choice
>
> ✅ Use `[ ]` when:
>
> * You need **literal brackets in output**
> * Working with advanced CSS syntax (like grid lines)

### 🔥 Accessing List Values

🧪 Example

```scss
$colors: red, green, blue;
```

```scss
color: nth($colors, 2);
```

👉 Output:

```css
color: green;
```

### ⚠️ Index starts at 1 (not 0!)

### 🔥 Useful List Functions

##### 🟢 1. `length()`

```scss
length($colors);
```

👉 Output: `3`

##### 🔵 2. `nth(list, index)`

```scss
nth($colors, 1);
```

👉 `red`

##### 🟡 3. `index(list, value)`

```scss
index($colors, green);
```

👉 `2`

##### 🔴 4. `append()`

```scss
$colors: append($colors, yellow);
```

👉 `red, green, blue, yellow`

##### 🟣 5. `join()`

```scss
join((red, green), (blue, yellow));
```

👉 `red, green, blue, yellow`

> #### ---`append()` and `join()` look similar, but they behave quite differently.
>
> 🔷 Core Difference (Quick View)
>
> | Function     | What it does                          |
> | ------------ | ------------------------------------- |
> | `append()` | Adds a**single value**to a list |
> | `join()`   | Combines**two lists**together   |
>
> #### 🟢 `append()`
>
> 👉 Adds **one item** to an existing list
>
> 🧪 Example
>
> ```scss
> $list: red, green;
>
> $list: append($list, blue);
> ```
>
> 👉 Result:
>
> ```scss
> red, green, blue
> ```
>
> **🔥 With separator control**
>
> ```scss
> $list: append($list, yellow, space);
> ```
>
> 👉 Result:
>
> ```scss
> red green blue yellow
> ```
>
> ###### 🧠 Key Points
>
> * Adds **only one value**
> * Treats the new value as a **single item**
> * Even if it's a list, it becomes **nested**
>
> ###### ⚠️ Important Behavior
>
> ```scss
> $list1: red, green;
> $list2: blue, yellow;
>
> $result: append($list1, $list2);
> ```
>
> 👉 Result:
>
> ```scss
> red, green, (blue, yellow)
> ```
>
> 🔥 Notice:
>
> * `$list2` becomes a  **nested list** , NOT merged
>
> #### 🔵 `join()`
>
> 👉 Combines **two lists into one**
>
> 🧪 Example
>
> ```scss
> $list1: red, green;
> $list2: blue, yellow;
>
> $result: join($list1, $list2);
> ```
>
> 👉 Result:
>
> ```scss
> red, green, blue, yellow
> ```
>
> **🔥 With separator control**
>
> ```scss
> join($list1, $list2, space);
> ```
>
> 👉 Result:
>
> ```scss
> red green blue yellow
> ```
>
> ###### 🧠 Key Points
>
> * Merges lists **flat**
> * No nesting
> * Used when combining datasets
>
> #### 🔥 Side-by-Side Comparison
>
> 🎯 Input
>
> ```scss
> $list1: red, green;
> $list2: blue, yellow;
> ```
>
> **🧪 append()**
>
> ```scss
> append($list1, $list2);
> ```
>
> 👉 Output:
>
> ```scss
> red, green, (blue, yellow)
> ```
>
> **🧪 join()**
>
> ```scss
> join($list1, $list2);
> ```
>
> 👉 Output:
>
> ```scss
> red, green, blue, yellow
> ```
>
> #### 🔥 Real-world Use Cases
>
> **✅ Use `append()` when:**
>
> * Adding one value dynamically
> * Building list step-by-step
>
> ```scss
> $spacing: 10px;
> $spacing: append($spacing, 20px);
> ```
>
> **✅ Use `join()` when:**
>
> * Combining two existing lists
> * Merging configs
>
> ```scss
> $primary-colors: red, blue;
> $secondary-colors: green, yellow;
>
> $all-colors: join($primary-colors, $secondary-colors);
> ```

##### 🟠 6. `separator()`

```scss
separator($colors);
```

👉 Returns:

* `comma` or `space`

### 🔥 Real-world Example

**🎯 Dynamic Spacing**

```scss
$spacing: 5px 10px 15px 20px;

.box {
  margin: $spacing;
}
```

**🎯 Looping through list**

```scss
$colors: red, green, blue;

@each $color in $colors {
  .text-#{$color} {
    color: $color;
  }
}
```

👉 Output:

```css
.text-red { color: red; }
.text-green { color: green; }
.text-blue { color: blue; }
```

🔥 This is where lists become powerful

### 🔥 Nested Lists

```scss
$grid: (10px 20px) (30px 40px);
```

👉 Each item is itself a list

### 🧠 Mental Model

👉
**List = array-like structure in SCSS**

* Ordered
* Index-based
* Can loop through

---

# ----SASS Modules

SCSS built-in modules are how you access **powerful functions (math, colors, lists, maps, etc.) in a modern, structured way** 🔥

Earlier SCSS dumped everything globally, but now we use the  **module system (`@use`)** .

### 🔷 1. What are Built-in Modules?

👉 Predefined SCSS libraries like:

* `sass:math`
* `sass:color`
* `sass:list`
* `sass:map`
* `sass:string`

They provide functions like:

* `math.div()`
* `color.adjust()`
* `list.append()`
* `map.get()`

### 🔥 2. How to Use Them

**✅ Step 1: Import using `@use`**

```scss
@use "sass:math";
```

**✅ Step 2: Use with namespace**

```scss
width: math.div(100px, 2);
```

👉 Output:

```css
width: 50px;
```

### 🔥 3. Example with Different Modules

**🟢 Math Module**

```scss
@use "sass:math";

.box {
  width: math.div(100px, 4);
}
```

**🔵 Color Module**

```scss
@use "sass:color";

.button {
  background: color.adjust(#3498db, $lightness: -10%);
}
```

**🟡 List Module**

```scss
@use "sass:list";

$list: red, green;

$new: list.append($list, blue);
```

**🟣 Map Module**

```scss
@use "sass:map";

$theme: (
  primary: #3498db
);

color: map.get($theme, primary);
```

**🟠 String Module**

```scss
@use "sass:string";

$name: string.to-upper-case("hello");
```

### 🔥 4. Aliasing (Shorter Names)

```scss
@use "sass:math" as m;

.box {
  width: m.div(100px, 2);
}
```

👉 Cleaner code 👍

### 🔥 5. Using Without Namespace (NOT recommended)

```scss
@use "sass:math" as *;

width: div(100px, 2);
```

👉 Works, but:
❌ Pollutes global scope
❌ Can cause conflicts

### 🔥 6. Old Way (Deprecated ⚠️)

```scss
width: 100px / 2;
```

👉 ❌ Deprecated → use:

```scss
math.div(100px, 2);
```

**Use `@use "sass:module"` and call functions with `module.function()` to access built-in SCSS features**

---

# ----Using `@at-root`

`@at-root` is one of those SCSS features that feels confusing at first—but once you get it, it becomes **super powerful for controlling output structure** 🔥

### 🔷 What is `@at-root`?

👉 It tells SCSS:

> **“Don’t nest this rule here—move it to the root (top level) of the CSS output.”**

**🧠 Why do we need it?**

SCSS nesting is great:

```scss
.card {
  .title {
    color: red;
  }
}
```

👉 Output:

```css
.card .title {
  color: red;
}
```

**❌ Problem**

Sometimes you  **don’t want nesting** , even though you're inside it.

##### 🔥 Basic Example

🧪 Without `@at-root`

```scss
.card {
  .title {
    color: red;
  }
}
```

🧪 With `@at-root`

```scss
.card {
  @at-root .title {
    color: red;
  }
}
```

👉 Output:

```css
.title {
  color: red;
}
```

🔥 `.title` is now **completely independent**

### 🔥 Example with `&` (VERY IMPORTANT)

```scss
.button {
  @at-root #{&}--primary {
    background: blue;
  }
}
```

👉 Output:

```css
.button--primary {
  background: blue;
}
```

**🧠 What’s happening?**

* `&` = `.button`
* `#{}` = interpolation
* `@at-root` = removes nesting

### 🔥 Real Use Case #1 — BEM Modifiers

```scss
.card {
  @at-root #{&}--active {
    border: 2px solid red;
  }
}
```

👉 Output:

```css
.card--active {
  border: 2px solid red;
}
```

✔️ Clean BEM structure
✔️ No `.card .card--active` mistake

### 🔥 Real Use Case #2 — Utility Classes

```scss
.container {
  @at-root .hidden {
    display: none;
  }
}
```

👉 Output:

```css
.hidden {
  display: none;
}
```

✔️ Avoids accidental nesting

### 🔥 Real Use Case #3 — Media Queries Control

```scss
.card {
  color: black;

  @media (max-width: 600px) {
    @at-root {
      .card {
        color: red;
      }
    }
  }
}
```

👉 Output:

```css
.card {
  color: black;
}

@media (max-width: 600px) {
  .card {
    color: red;
  }
}
```

🔥 Keeps media queries clean and flat

### 🔥 Advanced: Selective Rooting

**🧪 Ignore only certain rules**

```scss
@media (max-width: 600px) {
  .card {
    @at-root (without: media) {
      color: red;
    }
  }
}
```

👉 Output:

```css
.card {
  color: red;
}
```

✔️ Removes only `@media`, keeps selector

**🧪 Keep only media**

```scss
.card {
  @at-root (with: media) {
    @media (max-width: 600px) {
      color: red;
    }
  }
}
```

### 🔥 Another Powerful Pattern

**Generate global classes inside components**

```scss
.button {
  @at-root .is-disabled {
    opacity: 0.5;
  }
}
```

👉 Output:

```css
.is-disabled {
  opacity: 0.5;
}
```

---

# ----Understanding `@use` vs `@import`

This is a  **very important SCSS topic** —and also a common interview question 🔥

Short answer:  **`@import` is old (deprecated), `@use` is the modern standard** .

Let’s break it down clearly 👇

### 🔷 Core Difference

| Feature            | `@import`❌ (old) | `@use`✅ (modern) |
| ------------------ | ------------------- | ------------------- |
| Scope              | Global              | Scoped (namespaced) |
| Name conflicts     | Common 😬           | Avoided 👍          |
| Re-import behavior | Duplicates CSS      | Loaded once         |
| Maintainability    | Poor                | Excellent           |
| Status             | Deprecated ⚠️     | Recommended ✅      |

### 🔥 1. `@import` (Old Way)

🧪 Example

```scss
@import "variables";

.button {
  color: $primary;
}
```

#### 😬 Problems

**❌ 1. Global pollution**

```scss
$color: red;
```

👉 Available everywhere (even unintentionally)

❌ 2. Name conflicts

```scss
// file1.scss
$color: red;

// file2.scss
$color: blue;
```

👉 Which one wins? 😵 Confusing

**❌ 3. Duplicate CSS**

If imported multiple times:

```scss
@import "buttons";
@import "buttons";
```

👉 CSS gets duplicated ❌

### 🔥 2. `@use` (Modern Way)

🧪 Example

```scss
@use "variables";

.button {
  color: variables.$primary;
}
```

#### ✅ Key Features

**🟢 1. Namespacing**

```scss
@use "colors";

.button {
  color: colors.$primary;
}
```

✔️ No conflicts
✔️ Clear origin of variables

**🔵 2. Loaded only once**

Even if used multiple times:

```scss
@use "buttons";
@use "buttons";
```

👉 Included only once ✅

**🟡 3. Private variables support**

```scss
$_secret-color: red;
```

👉 `_` → private
👉 Not accessible outside module 🔒

**🟣 4. Safer and predictable**

No accidental overrides 👍

**🟠 5. Aliasing (Cleaner Code)**

```scss
@use "colors" as c;

.button {
  color: c.$primary;
}
```

### 🔥 3. Using without namespace (Not recommended)

```scss
@use "colors" as *;

.button {
  color: $primary;
}
```

👉 Works, but:
❌ Loses benefits of namespacing

### 🔥 4. File Structure Example

**📁 `_variables.scss`**

```scss
$primary: blue;
```

**📁 `main.scss`**

```scss
@use "variables";

body {
  color: variables.$primary;
}
```

### 🔥 5. Important Difference in Behavior

**🧪 With `@import`**

```scss
// variables.scss
$color: red;

// main.scss
@import "variables";

$color: blue;

.box {
  color: $color;
}
```

👉 Output:

```css
color: blue;
```

**🧪 With `@use`**

```scss
@use "variables";

.box {
  color: variables.$color;
}
```

👉 Cannot override directly ❌
👉 More controlled ✅

### 🔥 6. When to Use What

**✅ Use `@use` ALWAYS**

* New projects
* Clean architecture
* Avoid conflicts

**❌ Avoid `@import`**

* Deprecated
* Will be removed in future Sass versions

---
