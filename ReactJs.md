# Making Array with empty elements--

```javascript
{[...Array(2)].map((_, idx) => (
  <div key={idx} className="skeleton h-6 w-48 mb-3" />
))}

```

###### ✅ `Array(2)`

Creates an  **array with 2 empty slots** :

`Array(2) // → [empty × 2]`

But this is not directly iterable with `.map()`.

###### ✅ `[...Array(2)]`

Spreads that array into a **new array** that's iterable:

`[...Array(2)] // → [undefined, undefined]`

Now you can use `.map()` on it.

###### ✅ `.map((_, idx) => ( ... ))`

Loops over the array:

* `_` is the value (which is `undefined` here, and not used).
* `idx` is the index (0 and 1).
* For each item, it returns a JSX `<div>`.

# useCallBack()- More Details with Examples

⚠️ Every time a React component  **re-renders** , all the functions inside it are **redefined** (new instances are created).

Example (without `useCallback`):

```javascript
function MyComponent() {
  const sayHi = () => {
    console.log("Hi");
  };

  return <button onClick={sayHi}>Click Me</button>;
}

```

Even though the logic of `sayHi` doesn't change, a **new copy of that function** is created every time `MyComponent` re-renders.

This is what we mean by "the function is recreated."

**✅ `useCallback` prevents unnecessary recreation**

When you wrap a jfunction with `useCallback`:

```
const sayHi = useCallback(() => {
  console.log("Hi");
}, []);

```

React will:

* ✅ **Return the same function instance** on every render,
* ❌ **Unless** something in the dependency array (`[]` here) changes.

So it **doesn’t "recreate"** the function unless necessary.

### 💡 Why does this matter?

Because  **function identity matters in React** .

Some child components or hooks (like `useEffect`) may behave differently if they receive a "new" function every render.

### TL;DR

* **“Recreated”** means a new copy of the function is made during a render.
* React **normally recreates** functions every time a component renders.
* `useCallback` tells React: "Only recreate this function  **if dependencies change** ."

### Example--

```javascript
import React, { useState, useCallback, useEffect } from "react";
import { useNavigate } from "react-router-dom";
import { useSocket } from "../context/SocketProvider";

const LobbyScreen = () => {
  const [email, setEmail] = useState("");
  const [room, setRoom] = useState("");

  const socket = useSocket();
  const navigate = useNavigate();

  const handleSubmitForm = useCallback(
    (e) => {
      e.preventDefault();
      socket.emit("room:join", { email, room });
    },
    [email, room, socket]
  );

  const handleJoinRoom = useCallback(
    (data) => {
      const { email, room } = data;
      navigate(`/room/${room}`);
    },
    [navigate]
  );

  useEffect(() => {
    socket.on("room:join", handleJoinRoom);
    return () => {
      socket.off("room:join", handleJoinRoom);
    };
  }, [socket, handleJoinRoom]);

  return (
    <div>
      <h1>Lobby</h1>
      <form onSubmit={handleSubmitForm}>
        <label htmlFor="email">Email ID</label>
        <input
          type="email"
          id="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        <br />
        <label htmlFor="room">Room Number</label>
        <input
          type="text"
          id="room"
          value={room}
          onChange={(e) => setRoom(e.target.value)}
        />
        <br />
        <button>Join</button>
      </form>
    </div>
  );
};

export default LobbyScreen

```

Here ----

`useCallback(fn, deps)` is a React hook that returns a  **memoized version of the callback function** , which means:

* The function is *only recreated* if one of the dependencies changes.
* It helps prevent unnecessary re-renders or re-subscriptions in child components or hooks that depend on function identity (like `useEffect`, `useMemo`, etc.).

You pass `handleJoinRoom` to the `socket.on()` inside a `useEffect`. Without `useCallback`, `handleJoinRoom` would be  **recreated on every render** , which would:

* Cause the `useEffect` cleanup to run and re-subscribe every time.
* Result in  **unnecessary `socket.off` and `socket.on` calls** , potentially causing bugs like multiple listeners being attached.

So here, `useCallback` is **very important** to keep the reference stable between renders and make the effect work correctly.

However for handleSubmitform()

* `handleSubmitForm` is only used as an `onSubmit` handler for the form.
* You **don’t need it to be memoized** unless:
  * You were passing it to a child component that depended on reference equality for optimization (`React.memo`, `useEffect`, etc.).

So, in this case, `useCallback` is  **not strictly necessary** , but it doesn't hurt performance either. It might be used here for consistency or future-proofing if you later refactor or optimize.

---

# Globally Registering a function using useEffect() and Window Object

🔍 Code in focus:

```js
useEffect(() => {
  // Make the tooltip function globally accessible
  window.showChatTooltip = () => {
    setShowTooltip(true)
    // Auto-hide after 4 seconds
    setTimeout(() => {
      setShowTooltip(false)
    }, 4000)
  }

  return () => {
    delete window.showChatTooltip
  }
}, [])
```

✅ What does it do?

#### 1. **Registers a global function on `window`**

```js
window.showChatTooltip = () => { ... }
```

This creates a **globally accessible function** called `showChatTooltip`. It becomes available from anywhere in your app that can access the `window` object (such as another component's click handler, modals, overlays, etc.).

#### 2. **Function behavior: Show + auto-hide tooltip**

```js
setShowTooltip(true)
setTimeout(() => {
  setShowTooltip(false)
}, 4000)
```

When `window.showChatTooltip()` is called:

* It  **sets `showTooltip` to `true`** , which makes the `<ChatTooltip />` component visible.
* Then,  **after 4 seconds** , it automatically sets `showTooltip` back to `false`, hiding the tooltip again.

#### 3. **Cleanup on unmount**

```js
return () => {
  delete window.showChatTooltip
}
```

This ensures that when the `CustomerSupportPage` component is  **unmounted** , the global reference to `showChatTooltip` is removed to prevent memory leaks or stale references.

### 💡 Why use this approach?

Because the tooltip logic is  **local to `CustomerSupportPage`** , but you want to **trigger it from other components** (like a modal that closes somewhere else), using `window` is a simple way to **bridge communication** without prop drilling or context.

So when this is used:

```js
onClick={() => {
  onClose()
  setTimeout(() => {
    if (window.showChatTooltip) {
      window.showChatTooltip()
    }
  }, 300)
}}
```

It ensures:

* The modal closes first (`onClose()`),
* Then after 300ms, the tooltip is shown via `window.showChatTooltip()`,
* The tooltip appears for 4 seconds and hides automatically.

### 🛡️ Caveat

Using `window` to share state/functionality is okay for **quick glue code** or  **demo-level features** , but for a more robust app, it’s better to use:

* React **Context** for global state or shared functions
* An **event emitter** (like mitt) for decoupled communication
* **State management libraries** (Redux, Zustand, etc.)

---

# ----IntersectionObserver

This is *one of those core browser APIs* that, once you really understand it, opens up a ton of front-end possibilities (lazy loading, animation triggers, analytics, etc.).

Let’s go through it **step-by-step** so you understand exactly how it works, what’s going on under the hood, and when to use it.

### 🧩 What Is `IntersectionObserver`?

`IntersectionObserver` is a **browser API** that lets you **asynchronously detect when an element enters or leaves the viewport (or another container)** — *without constantly listening to scroll events.*

In simple words:

> It tells you, “Hey, this element is now visible (or not visible) on the screen.”

### ⚙️ Why It Exists

Before this API, developers had to:

* Attach `scroll` listeners.
* Compute visibility manually with `getBoundingClientRect()`.
* Constantly recalculate things — hurting performance.

`IntersectionObserver` replaces all that by running in a  **low-level browser thread** , optimized for performance.

It doesn’t trigger layout thrashing or jank because it doesn’t run on the main thread constantly.

### 🧠 Basic Concept

You create an **observer** that watches one or more  **target elements** .

When any of those targets **intersect** (appear or disappear) with a **root** (like the viewport or another container), the browser notifies you via a callback.

**✨ Basic Example**

```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      console.log('Element is visible!');
    } else {
      console.log('Element is hidden!');
    }
  });
});

const target = document.querySelector('.my-element');
observer.observe(target);
```

✅ When `.my-element` scrolls into view → logs “Element is visible!”

❌ When it scrolls out → logs “Element is hidden!”

### 🧮 Key Terms

| Term                  | Meaning                                                             |
| --------------------- | ------------------------------------------------------------------- |
| **Target**      | The element(s) being watched.                                       |
| **Root**        | The container being used as a viewport (default: browser viewport). |
| **Root margin** | Offsets the root’s bounds (like CSS margin).                       |
| **Threshold**   | How much of the element must be visible (0–1 range).               |
| **Entry**       | Each observation result — includes visibility info, ratio, etc.    |

**Example With Custom Options**

```js
const options = {
  root: null, // null = use the viewport
  rootMargin: '0px 0px -10% 0px', // triggers a bit early
  threshold: 0.3 // 30% of element must be visible
};

const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      console.log('Now visible!');
    }
  });
}, options);

observer.observe(document.querySelector('.section'));
```

### 🧾 Anatomy of an `entry`

Each `entry` object has:

```js
{
  target,           // the observed element
  isIntersecting,   // true if visible
  intersectionRatio, // how much is visible (0–1)
  intersectionRect, // visible part of target
  boundingClientRect, // full target box
  rootBounds,        // visible root area
  time               // timestamp
}
```

You’ll mostly use:

* `entry.isIntersecting`
* `entry.intersectionRatio`
* `entry.target`

### 🚀 Common Use Cases

1. **Lazy-load images or videos**

   → Only load media when user scrolls near them.
2. **Trigger animations when section appears**

   → Like your `StatCounter` rolling numbers or fade-in text.
3. **Infinite scrolling / pagination**

   → Detect when user reaches bottom to fetch more items.
4. **Analytics / tracking**

   → Log when users actually *view* certain sections.

### 🧰 Example: Animation on Scroll

```js
const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate');
      observer.unobserve(entry.target); // run once
    }
  });
}, { threshold: 0.2 });

document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));
```

This adds the `.animate` class when each `.fade-in` element is 20% visible, triggering a CSS animation.

### 🧠 React Integration Pattern

React components often use a **ref** to attach the observer.

```jsx
import { useRef, useEffect, useState } from 'react'

function LazySection() {
  const ref = useRef(null)
  const [isVisible, setIsVisible] = useState(false)

  useEffect(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            setIsVisible(true)
            observer.disconnect()
          }
        })
      },
      { threshold: 0.3 }
    )

    if (ref.current) observer.observe(ref.current)
    return () => observer.disconnect()
  }, [])

  return (
    <div ref={ref}>
      {isVisible && <MyComponent />}
    </div>
  )
}
```

👉 This is exactly what we used for your `FitlabHighlights`.

### ⚡ Performance Tips

* **Disconnect** once you’re done observing (`observer.disconnect()` or `unobserve(target)`).
* Avoid watching *hundreds* of small elements; group or throttle updates.
* Adjust `threshold` and `rootMargin` to trigger visibility earlier or later.
* Works best for scroll-based effects — not continuous animations.

### 🧩 Summary

| Concept          | Meaning                                           |
| ---------------- | ------------------------------------------------- |
| What it does     | Detects when an element enters/leaves viewport    |
| Key options      | `root`,`rootMargin`,`threshold`             |
| Main method      | `observe(target)`                               |
| Returns info via | Callback with `entries`array                    |
| Performance      | Very efficient — runs outside main thread        |
| Best for         | Lazy load, scroll-triggered animations, analytics |

---

# ----PropTypes

In React, **PropTypes** are used for **type-checking props** passed to components. They help catch bugs during development by warning if a component receives props of the wrong type. 🛠️

They are especially useful in large projects or when multiple developers use the same components.

### 1. Installing PropTypes

In modern React, PropTypes are provided by a separate package:

```bash
npm install prop-types
```

Import it:

```js
import PropTypes from "prop-types"
```

### 2. Basic Example

```jsx
import React from "react"
import PropTypes from "prop-types"

function UserCard(props) {
  return (
    <div>
      <h2>{props.name}</h2>
      <p>Age: {props.age}</p>
    </div>
  )
}

UserCard.propTypes = {
  name: PropTypes.string,
  age: PropTypes.number
}

export default UserCard
```

Usage:

```jsx
<UserCard name="Arjun" age={22} />
```

### 3. What Happens If Wrong Type Is Passed?

```jsx
<UserCard name="Arjun" age="22" />
```

You’ll get a warning in console:

```txt
Warning: Failed prop type:
Invalid prop `age` of type `string`
supplied to `UserCard`, expected `number`.
```

⚠️ Only in development mode, not production.

### 4. Common PropTypes

String

```js
name: PropTypes.string
```

Number

```js
age: PropTypes.number
```

Boolean

```js
isAdmin: PropTypes.bool
```

Array

```js
skills: PropTypes.array
```

Object

```js
user: PropTypes.object
```

Function

```js
onClick: PropTypes.func
```

Symbol

```js
id: PropTypes.symbol
```

### 5. `.isRequired`

Makes a prop mandatory.

```jsx
UserCard.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number
}
```

If `name` is missing:

```jsx
<UserCard age={20} />
```

React warning:

```txt
The prop `name` is marked as required
```

### 6. Default Props

Used when prop is not passed.

```jsx
UserCard.defaultProps = {
  age: 18
}
```

Now:

```jsx
<UserCard name="Arjun" />
```

renders:

```txt
Age: 18
```

### 7. `arrayOf()`

Checks array element types.

```jsx
skills: PropTypes.arrayOf(PropTypes.string)
```

Example:

```jsx
<User skills={["React", "Node"]} />
```

Wrong:

```jsx
<User skills={[1, 2]} />
```

### 8. `objectOf()`

Checks object values.

```jsx
scores: PropTypes.objectOf(PropTypes.number)
```

Example:

```js
{
  math: 90,
  english: 85
}
```

### 9. `shape()`

Defines structure of an object.

```jsx
user: PropTypes.shape({
  name: PropTypes.string,
  age: PropTypes.number
})
```

Example:

```jsx
<User
  user={{
    name: "Arjun",
    age: 22
  }}
/>
```

### 10. `exact()`

Like `shape()`, but prevents extra properties.

```jsx
user: PropTypes.exact({
  name: PropTypes.string,
  age: PropTypes.number
})
```

Extra fields cause warnings.

### 11. `oneOf()`

Restricts to specific values.

```jsx
status: PropTypes.oneOf(["loading", "success", "error"])
```

### 12. `oneOfType()`

Allows multiple possible types.

```jsx
id: PropTypes.oneOfType([
  PropTypes.string,
  PropTypes.number
])
```

### 13. `instanceOf()`

Checks class instance.

```jsx
date: PropTypes.instanceOf(Date)
```

### 14. `node`

Anything React can render.

```jsx
children: PropTypes.node
```

Can be:

* string
* JSX
* number
* fragment
* array of JSX

### 15. `element`

Must be a single React element.

```jsx
header: PropTypes.element
```

Example:

```jsx
<MyComp header={<h1>Hello</h1>} />
```

### 16. `elementType`

Checks component type itself.

```jsx
component: PropTypes.elementType
```

Example:

```jsx
<MyComp component={Button} />
```

### 17. Full Example

```jsx
import React from "react"
import PropTypes from "prop-types"

function ProductCard({ title, price, tags, user }) {
  return (
    <div>
      <h2>{title}</h2>
      <p>${price}</p>

      <ul>
        {tags.map(tag => (
          <li key={tag}>{tag}</li>
        ))}
      </ul>

      <h4>{user.name}</h4>
    </div>
  )
}

ProductCard.propTypes = {
  title: PropTypes.string.isRequired,

  price: PropTypes.number.isRequired,

  tags: PropTypes.arrayOf(
    PropTypes.string
  ),

  user: PropTypes.shape({
    name: PropTypes.string,
    age: PropTypes.number
  })
}

export default ProductCard
```

### 18. Custom Validator

You can create your own validation.

```jsx
age: function(props, propName, componentName) {
  if (props[propName] < 18) {
    return new Error(
      `${propName} must be above 18 in ${componentName}`
    )
  }
}
```

### 19. Why Use PropTypes?

Benefits ✅

* Detects bugs early
* Makes components self-documented
* Improves maintainability
* Useful in reusable component libraries

### 20. PropTypes vs TypeScript

| PropTypes           | TypeScript                |
| ------------------- | ------------------------- |
| Runtime checking    | Compile-time checking     |
| Simpler             | More powerful             |
| JS-based            | TS-based                  |
| Warnings in console | Errors during development |
| Good for small apps | Preferred for large apps  |

Today many React projects prefer TypeScript, but PropTypes are still common in plain JavaScript React projects.

### 21. Important Notes ⚠️

* PropTypes only validate in development mode.
* They do not stop execution.
* They only give warnings.
* Usually added after component definition:

```js
Component.propTypes = {}
```

### 22. Functional vs Class Components

Works with both.

**Functional Component**

```jsx
function Test() {}
Test.propTypes = {}
```

**Class Component**

```jsx
class Test extends React.Component {}

Test.propTypes = {}
```

### 23. Modern Alternative

Nowadays many developers use:

* TypeScript
* interfaces
* type aliases

instead of PropTypes for stronger type safety.

But PropTypes are still excellent for:

* JavaScript-only projects
* quick validation
* reusable UI libraries 📦

---

# ----PureComponent vs `shouldComponentUpdate()` & Functional Component Equivalent

In React class components, you’re comparing:

* `PureComponent`
  vs
* `shouldComponentUpdate()`

Both are related to **preventing unnecessary re-renders** ⚡

But `PureComponent` exists because writing `shouldComponentUpdate()` manually again and again is repetitive and error-prone.

### 1. Problem: Normal Components Re-render Too Much

Example:

```jsx
class Child extends React.Component {
  render() {
    console.log("Child rendered")

    return <h1>{this.props.name}</h1>
  }
}
```

Even if prop values are same:

```jsx
<Child name="Arjun" />
```

React still re-renders the component whenever parent renders.

That can hurt performance in large apps.

### 2. `shouldComponentUpdate()`

React gives us lifecycle method:

```jsx
shouldComponentUpdate(nextProps, nextState) {
  return true
}
```

You manually decide whether re-render should happen.

Example:

```jsx
class Child extends React.Component {

  shouldComponentUpdate(nextProps) {
    if (nextProps.name !== this.props.name) {
      return true
    }

    return false
  }

  render() {
    console.log("Rendered")
    return <h1>{this.props.name}</h1>
  }
}
```

Now component only re-renders if `name` changes ✅

### 3. Then Why `PureComponent`? 🤔

Because writing this manually everywhere is annoying:

```js
if (nextProps.x !== this.props.x)
if (nextProps.y !== this.props.y)
if (nextState.a !== this.state.a)
if (nextState.b !== this.state.b)
```

For every component.

So React provides:

```jsx
class Child extends React.PureComponent
```

It automatically implements:

```js
shouldComponentUpdate()
```

with a built-in shallow comparison.

### 4. What `PureComponent` Actually Does

Internally it roughly behaves like:

```jsx
shouldComponentUpdate(nextProps, nextState) {

  return (
    shallowCompare(this.props, nextProps) ||
    shallowCompare(this.state, nextState)
  )
}
```

If nothing changed → no re-render.

### 5. Example

```jsx
class Child extends React.PureComponent {
  render() {
    console.log("Child rendered")

    return <h1>{this.props.name}</h1>
  }
}
```

Now if parent re-renders but `name` stays same:

```jsx
<Child name="Arjun" />
```

Child will NOT re-render ✅

without manually writing `shouldComponentUpdate()`.

### 6. Main Difference

| `shouldComponentUpdate()` | `PureComponent`       |
| --------------------------- | ----------------------- |
| Manual control              | Automatic optimization  |
| You write logic             | React writes logic      |
| Flexible                    | Simple                  |
| Can do deep/custom checks   | Only shallow comparison |
| More verbose                | Cleaner                 |

### 7. Shallow Comparison Important ⚠️

`PureComponent` only does shallow comparison.

Meaning:

```js
1 === 1 // true
"hi" === "hi" // true
```

But for objects/arrays:

```js
{} === {} // false
[] === [] // false
```

It compares references, not contents.

### 8. Common Pitfall 🚨

```jsx
this.state = {
  user: {
    name: "Arjun"
  }
}
```

Then:

```js
this.state.user.name = "Rahul"
```

Reference didn't change.

So `PureComponent` may NOT detect update.

Correct way:

```js
this.setState({
  user: {
    ...this.state.user,
    name: "Rahul"
  }
})
```

New object reference ✅

### 9. When `shouldComponentUpdate()` Is Better

Use it when you need custom logic.

Example:

```jsx
shouldComponentUpdate(nextProps) {

  if (nextProps.user.id !== this.props.user.id) {
    return true
  }

  return false
}
```

Or:

* deep comparison
* selective comparison
* complex performance tuning

### 10. When `PureComponent` Is Better

Best when:

✅ Props/state are simple
✅ Immutable updates are used
✅ You want quick optimization
✅ No custom comparison needed

### 11. Functional Component Equivalent

In modern React:

```jsx
React.memo(Component)
```

acts similar to `PureComponent`.

Example:

```jsx
const Child = React.memo(function Child(props) {
  return <h1>{props.name}</h1>
})
```

### 12. Important Interview Point ⭐

`PureComponent` is basically:

```txt
Component + built-in shallow shouldComponentUpdate
```

That’s the core idea.

---

# ----Error handling & Error Boundary and its Functional Component Equivalent

### 1. What Is Error Handling in React? ⚠️

Error handling means preventing your entire React app from crashing when some component throws an error.

Example:

```jsx
function User() {
  throw new Error("Something broke!")

  return <h1>User</h1>
}
```

Without proper handling, the whole React component tree may crash.

### 2. What Is an Error Boundary?

An **Error Boundary** is a special React component that catches JavaScript errors in child components during:

* rendering
* lifecycle methods
* constructors

and shows fallback UI instead of crashing entire app.

### 3. Important Point 🚨

Error Boundaries can ONLY be created using:

```txt
Class Components
```

Not functional components directly.

### 4. Basic Error Boundary Example

```jsx
import React from "react"

class ErrorBoundary extends React.Component {

  constructor(props) {
    super(props)

    this.state = {
      hasError: false
    }
  }

  static getDerivedStateFromError(error) {
    return {
      hasError: true
    }
  }

  componentDidCatch(error, info) {
    console.log("Error:", error)
    console.log("Info:", info)
  }

  render() {

    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>
    }

    return this.props.children
  }
}

export default ErrorBoundary
```

Usage:

```jsx
<ErrorBoundary>
  <App />
</ErrorBoundary>
```

### 5. How It Works

Step 1

Child crashes:

```jsx
throw new Error("Crash")
```

Step 2

`getDerivedStateFromError()` runs

```jsx
static getDerivedStateFromError(error)
```

Updates state:

```js
hasError: true
```

Step 3

Fallback UI renders:

```jsx
<h1>Something went wrong</h1>
```

### 6. `componentDidCatch()`

Used for logging/reporting.

```jsx
componentDidCatch(error, info) {
  console.log(error)
}
```

Usually used with:

* logging services
* monitoring tools
* analytics

Like:

* Sentry
* LogRocket

### 7. What Error Boundaries Catch ✅

They catch errors in:

✅ render methods
✅ lifecycle methods
✅ constructors
✅ child components

### 8. What They DO NOT Catch ❌

**A. Event Handlers**

```jsx
<button onClick={() => {
  throw new Error("Boom")
}}>
```

NOT caught.

Need normal `try/catch`.

**B. Async Code**

```js
setTimeout(() => {
  throw new Error()
}, 1000)
```

NOT caught.

**C. API Calls**

```js
fetch(...)
```

NOT caught automatically.

**D. Server Side Rendering**

Not caught there either.

### 9. Error Handling in Class Components

Class components use:

| Technique                      | Purpose                  |
| ------------------------------ | ------------------------ |
| `try/catch`                  | event handlers           |
| `componentDidCatch()`        | boundary logging         |
| `getDerivedStateFromError()` | fallback UI              |
| Error Boundary                 | subtree crash protection |

### 10. Example of Event Handler Error Handling

```jsx
class App extends React.Component {

  handleClick = () => {

    try {
      throw new Error("Button Error")
    }

    catch(err) {
      console.log(err.message)
    }
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click
      </button>
    )
  }
}
```

### 11. Functional Components and Error Handling

Functional components themselves CANNOT become error boundaries directly 🚨

This does NOT work:

```jsx
function ErrorBoundary() {

}
```

because functional components do not support:

* `componentDidCatch`
* `getDerivedStateFromError`

### 12. Then How Do Functional Components Handle Errors?

Using:

**A. `try/catch`  **

For async/event logic.

**B. External Error Boundary**

Wrap functional components inside class-based boundary.

### 13. Functional Component Example

```jsx
function User() {

  const handleClick = () => {

    try {
      throw new Error("Failed")
    }

    catch(err) {
      console.log(err.message)
    }
  }

  return (
    <button onClick={handleClick}>
      Click
    </button>
  )
}
```

#### 14. Functional Components + Error Boundary

```jsx
<ErrorBoundary>
  <User />
</ErrorBoundary>
```

Even though `User` is functional,
the class boundary catches rendering errors.

### 15. Example of Render Error

```jsx
function User() {

  throw new Error("Crash")

  return <h1>User</h1>
}
```

Wrapped:

```jsx
<ErrorBoundary>
  <User />
</ErrorBoundary>
```

Fallback UI appears ✅

### 16. Modern Functional Approach 🆕

Many apps now use libraries like:

* react-error-boundary

It allows easier functional-style handling.

### 17. Example Using `react-error-boundary`

Install:

```bash
npm install react-error-boundary
```

Usage:

```jsx
import { ErrorBoundary } from "react-error-boundary"

function Fallback() {
  return <h1>Something failed</h1>
}

<ErrorBoundary FallbackComponent={Fallback}>
  <App />
</ErrorBoundary>
```

Internally it still uses class boundary logic.

### 18. Class vs Functional Error Handling

| Feature                      | Class Components | Functional Components |
| ---------------------------- | ---------------- | --------------------- |
| Can create Error Boundary    | ✅ Yes           | ❌ No                 |
| `componentDidCatch`        | ✅               | ❌                    |
| `getDerivedStateFromError` | ✅               | ❌                    |
| `try/catch`                | ✅               | ✅                    |
| Event handler errors         | Manual           | Manual                |
| Async errors                 | Manual           | Manual                |

### 19. Best Practice 🚀

Modern React apps usually:

✅ Use functional components
✅ Use class-based Error Boundary at top level
✅ Use `try/catch` for API/events
✅ Use monitoring tools like Sentry

### 20. Typical Real App Structure

```jsx
<ErrorBoundary>
  <Navbar />

  <Dashboard />

  <Profile />
</ErrorBoundary>
```

or even smaller boundaries:

```jsx
<ErrorBoundary>
  <Dashboard />
</ErrorBoundary>
```

so only one section fails instead of full app.

### 21. Important Interview Points ⭐

**Q: Why only class components can be Error Boundaries?**

Because React error boundary system depends on lifecycle methods:

```txt
componentDidCatch()
getDerivedStateFromError()
```

which functional components don't have.

**Q: Do Error Boundaries catch async errors?**

❌ No.

Need manual handling.

**Q: Do Error Boundaries catch event handler errors?**

❌ No.

Need `try/catch`.

### 22. Summary 🎯

**Class Components**

Can:

* create Error Boundaries
* catch render/lifecycle errors
* show fallback UI

Using:

* `componentDidCatch`
* `getDerivedStateFromError`

### Functional Components

Cannot directly become Error Boundaries.

Use:

* `try/catch`
* external Error Boundary wrappers
* libraries like react-error-boundary

### Core Idea

```txt
Error Boundary = Special class component
that catches rendering errors
in child component tree
```

---

# ---- `getDeriveStatesFromProps()` Functional Component Equivalent

`getDerivedStateFromProps()` in class components is used to **update state based on incoming props** before rendering.

But in modern React, this pattern is often avoided because it can create duplicated state and bugs 😅

Still, understanding its functional equivalent is important for interviews and legacy code.

### 1. `getDerivedStateFromProps()` in Class Components

Syntax:

```jsx
static getDerivedStateFromProps(props, state) {

  return updatedStateObject

  // or return null
}
```

It runs:

* on initial render
* on every re-render
* before `render()`

**Example**

```jsx
class User extends React.Component {

  constructor(props) {
    super(props)

    this.state = {
      name: props.name
    }
  }

  static getDerivedStateFromProps(props, state) {

    if (props.name !== state.name) {
      return {
        name: props.name
      }
    }

    return null
  }

  render() {
    return <h1>{this.state.name}</h1>
  }
}
```

### 2. Functional Component Equivalent ✅

Usually done using:

```txt
useEffect() + useState()
```

**Equivalent Functional Version**

```jsx
import React, { useState, useEffect } from "react"

function User(props) {

  const [name, setName] = useState(props.name)

  useEffect(() => {
    setName(props.name)
  }, [props.name])

  return <h1>{name}</h1>
}
```

### 3. Mapping Comparison

| Class Component                | Functional Component |
| ------------------------------ | -------------------- |
| `getDerivedStateFromProps()` | `useEffect()`      |
| `this.state`                 | `useState()`       |
| lifecycle method               | Hook                 |

### 4. But Usually You DON’T Need This 🚨

This is a very important modern React concept.

Most of the time:

❌ Don’t copy props into state.

Bad pattern:

```jsx
const [name, setName] = useState(props.name)
```

because now:

* prop exists
* duplicate state exists

Two sources of truth 😬

**Better Approach ✅**

Directly use props:

```jsx
function User(props) {
  return <h1>{props.name}</h1>
}
```

Simpler and safer.

### 5. Then When Is Derived State Needed? 🤔

Only in special cases like:

✅ syncing external data
✅ animations
✅ controlled/uncontrolled transitions
✅ caching previous prop values
✅ expensive computations

**Example Where It Makes Sense**

Suppose:

* prop changes from server
* local editable form state needed

```jsx
function Profile({ user }) {

  const [formData, setFormData] = useState(user)

  useEffect(() => {
    setFormData(user)
  }, [user])

  return (
    <input
      value={formData.name}
      onChange={(e) =>
        setFormData({
          ...formData,
          name: e.target.value
        })
      }
    />
  )
}
```

Now local edits + prop syncing both needed.

### 6. Another Functional Equivalent Pattern

Sometimes you don't even need state.

Use memoization instead.

Example:

```jsx
const filtered = useMemo(() => {
  return items.filter(item => item.active)
}, [items])
```

instead of deriving state.

### 7. Important Difference ⚠️

`getDerivedStateFromProps()` runs:

```txt
BEFORE render
```

But `useEffect()` runs:

```txt
AFTER render
```

So they are not perfectly identical internally.

### **8. Closer Equivalent?**

If you truly need pre-render syncing, sometimes:

```txt
useMemo()
```

or direct calculations during render are better.

### 9. Example Using Direct Calculation

Instead of:

```jsx
const [fullName, setFullName] = useState("")
```

Do:

```jsx
const fullName = `${first} ${last}`
```

No derived state needed 🚀

---

# ----Higher Order Components

A **Higher Order Component (HOC)** is an advanced React pattern where:

```txt
A function takes a component
and returns a new enhanced component
```

It is used for:

* reusing logic
* adding features
* sharing behavior between components

**🧠 Simple Definition**

```txt
HOC = Component Transformer
```

Like:

```js
EnhancedComponent = HOC(OriginalComponent)
```

**🔧 Basic Syntax**

```jsx
function withSomething(WrappedComponent) {

  return function EnhancedComponent(props) {

    return (
      <WrappedComponent {...props} />
    )
  }
}
```

### 🚀 Basic Example

Suppose we want multiple components to show loading state.

Instead of repeating code everywhere, we create HOC.

📦 HOC Example

```jsx
function withLoading(Component) {

  return function EnhancedComponent(props) {

    if (props.loading) {
      return <h1>Loading...</h1>
    }

    return <Component {...props} />
  }
}
```

🎯 Original Component

```jsx
function User(props) {
  return <h1>{props.name}</h1>
}
```

Enhance it:

```jsx
const UserWithLoading =
  withLoading(User)
```

Usage:

```jsx
<UserWithLoading
  loading={false}
  name="Arjun"
/>
```

**⚡ Flow of HOC**

```txt
User Component
      ↓
withLoading(User)
      ↓
Enhanced Component
```

### 🧩 Why HOCs Exist?

Before Hooks existed, HOCs were heavily used for:

✅ code reuse
✅ authentication
✅ loading logic
✅ permissions
✅ data fetching
✅ logging
✅ Redux connection

### 🏛️ HOC in Class Components

HOCs were extremely popular in class-based React.

Example:

```jsx
class User extends React.Component {

  render() {
    return <h1>{this.props.name}</h1>
  }
}
```

Wrap with HOC:

```jsx
const EnhancedUser =
  withLoading(User)
```

Works perfectly ✅

**⚙️ Example: Authentication HOC**

```jsx
function withAuth(Component) {

  return function(props) {

    const isLoggedIn = true

    if (!isLoggedIn) {
      return <h1>Please Login</h1>
    }

    return <Component {...props} />
  }
}
```

Usage:

```jsx
class Dashboard extends React.Component {

  render() {
    return <h1>Dashboard</h1>
  }
}

export default withAuth(Dashboard)
```

### 🪝 HOC in Functional Components

HOCs also work with functional components.

```jsx
function Profile(props) {
  return <h1>Profile</h1>
}

export default withAuth(Profile)
```

### 🔥 Important Point

HOCs are NOT:

* hooks
* components
* special React APIs

They are simply:

```txt
JavaScript functions
that work with components
```

### 🧬 Real Internal Structure

```jsx
function HOC(Component) {

  return function(props) {

    return (
      <Component {...props} />
    )
  }
}
```

### 📌 Why `...props` Is Important

Without:

```jsx
<Component {...props} />
```

original component won’t receive props.

### 🚨 Common Mistake

Wrong:

```jsx
return <Component />
```

Correct:

```jsx
return <Component {...props} />
```

# 🧠 HOC Naming Convention

Usually starts with:

```txt
with
```

Examples:

* `withAuth`
* `withLoading`
* `withRouter`

### 🔄 HOC vs Normal Component

**Normal Component**

```jsx
function User() {}
```

renders UI.

**HOC**

```jsx
function withAuth(Component) {}
```

enhances components.

### ⚡ Real-World Example — Redux

In older Redux:

```jsx
connect(mapState)(Component)
```

`connect()` is a famous HOC from Redux.

Example:

```jsx
export default connect(mapState)(User)
```

### 🛡️ Another Example — Logging HOC7

```jsx
function withLogger(Component) {

  return function(props) {

    console.log("Rendering:", Component.name)

    return <Component {...props} />
  }
}
```

### 🎨 HOC Composition

Multiple HOCs can wrap together.

```jsx
export default withAuth(
  withLoading(User)
)
```

Flow:

```txt
User
 ↓
withLoading
 ↓
withAuth
 ↓
Enhanced Component
```

### ⚠️ Problems With HOCs

**❌ Wrapper Hell**

Too many nested HOCs:

```jsx
withA(withB(withC(Component)))
```

Hard to read 😵

**❌ Prop Collisions**

HOC may overwrite existing props.

**❌ Debugging Difficulty**

Component tree becomes complex.

**❌ Naming Issues**

React DevTools may show:

```txt
Anonymous
```

instead of useful names.

### 🪝 HOCs vs Hooks

Modern React prefers Hooks.

Before Hooks:

```txt
HOCs were primary logic reuse method
```

Now:

* `useAuth()`
* `useFetch()`
* custom hooks

are more common.

### ⚔️ HOC vs Hook

| HOC                       | Hook                  |
| ------------------------- | --------------------- |
| Wraps component           | Reuses logic directly |
| Creates extra component   | No wrapper            |
| Older pattern             | Modern pattern        |
| Works with classes        | Functional only       |
| Can cause wrapper nesting | Cleaner               |

### 🚀 Modern Equivalent

Instead of:

```jsx
withAuth(Component)
```

Modern React often uses:

```jsx
const user = useAuth()
```

inside component.

### 🎯 Important Interview Points

✅ HOC Definition

```txt
A function that takes a component
and returns a new enhanced component
```

✅ Are HOCs React APIs?

❌ No.

They are JavaScript patterns.

✅ Can HOCs work with both class and functional components?

Yes.

✅ Why were HOCs popular?

Because Hooks didn’t exist yet.

### 🏁 Final Full Example

```jsx
import React from "react"

function withCounter(Component) {

  return class extends React.Component {

    state = {
      count: 0
    }

    increment = () => {
      this.setState({
        count: this.state.count + 1
      })
    }

    render() {

      return (
        <Component
          count={this.state.count}
          increment={this.increment}
          {...this.props}
        />
      )
    }
  }
}
```

Usage:

```jsx
function ClickCounter(props) {

  return (
    <button onClick={props.increment}>
      {props.count}
    </button>
  )
}

export default withCounter(ClickCounter)
```

---

# Hooks - `useCallback() and useMemo()`

### ⚡ Why `useMemo` and `useCallback` Exist

In React, components re-render frequently.

During every render:

* functions recreated
* calculations rerun
* objects recreated

Most of the time that’s fine ✅

But sometimes:

* calculations are expensive
* child components re-render unnecessarily
* performance drops

That’s where:

* `useMemo`
* `useCallback`

help 🚀

### 🧠 Core Difference

| Hook            | Memoizes |
| --------------- | -------- |
| `useMemo`     | VALUE    |
| `useCallback` | FUNCTION |

This is the MOST important line 🎯

### 🔥 `useMemo`

Used to memoize (cache) a  **computed value** .

**📌 Syntax**

```jsx
const memoizedValue = useMemo(() => {

  return expensiveCalculation()

}, [dependencies])
```

⚡ Why Use It?

Without `useMemo`, expensive calculations run on EVERY render.

**❌ Without `useMemo`**

```jsx
function App() {

  const [count, setCount] = useState(0)
  const [theme, setTheme] = useState(false)

  function slowFunction() {

    console.log("Running slow function")

    let total = 0

    for(let i = 0; i < 1000000000; i++) {
      total += i
    }

    return total
  }

  const result = slowFunction()

  return (
    <>
      <h1>{result}</h1>

      <button onClick={() => setCount(count + 1)}>
        Count
      </button>

      <button onClick={() => setTheme(!theme)}>
        Theme
      </button>
    </>
  )
}
```

Problem 🚨

Even changing:

* theme
* unrelated state

reruns huge calculation.

Slow 😵

**✅ With `useMemo`**

```jsx
import { useMemo } from "react"

function App() {

  const [count, setCount] = useState(0)
  const [theme, setTheme] = useState(false)

  const result = useMemo(() => {

    console.log("Running slow function")

    let total = 0

    for(let i = 0; i < 1000000000; i++) {
      total += i
    }

    return total

  }, [count])

  return (
    <>
      <h1>{result}</h1>

      <button onClick={() => setCount(count + 1)}>
        Count
      </button>

      <button onClick={() => setTheme(!theme)}>
        Theme
      </button>
    </>
  )
}
```

Now expensive calculation runs ONLY when:

```txt
count changes
```

🚀 Huge optimization.

**🧠 Real-Life Analogy For `useMemo`**

Imagine calculating company payroll 💰

Very expensive.

If office theme changes:

* dark mode
* light mode

Would you recalculate payroll again? 😵

No.

You cache previous result.

That’s `useMemo`.

### ⚡ `useCallback`

Used to memoize a  **function itself** .

**📌 Syntax**

```jsx
const memoizedFunction = useCallback(() => {

}, [dependencies])
```

##### ❓ Why Does This Matter?

Because in React:

```jsx
const fn = () => {}
```

creates NEW function every render.

Even if code looks same.

**🧠 Important JavaScript Concept**

```js
() => {}
=== 
() => {}
```

is:

```txt
false
```

Different references.

##### 🚨 Problem Scenario

Suppose parent passes function to child.

👨 Parent Component

```jsx
function Parent() {

  const [count, setCount] = useState(0)

  const handleClick = () => {
    console.log("Clicked")
  }

  return (
    <>
      <Child onClick={handleClick} />

      <button onClick={() => setCount(count + 1)}>
        Count
      </button>
    </>
  )
}
```

👶 Child Component

```jsx
const Child = React.memo(({ onClick }) => {

  console.log("Child Rendered")

  return <button onClick={onClick}>Child</button>
})
```

🚨 Problem

Even when only `count` changes:

* Parent re-renders
* `handleClick` recreated
* new function reference
* child thinks prop changed
* child re-renders 😵

**✅ Fix Using `useCallback`**

```jsx
import { useCallback } from "react"

function Parent() {

  const [count, setCount] = useState(0)

  const handleClick = useCallback(() => {

    console.log("Clicked")

  }, [])

  return (
    <>
      <Child onClick={handleClick} />

      <button onClick={() => setCount(count + 1)}>
        Count
      </button>
    </>
  )
}
```

Now:

* same function reference reused
* child doesn’t re-render unnecessarily 🚀

##### 🧠 Real-Life Analogy For `useCallback`

Suppose office manager sends employee contact sheet 📄

Without memoization:

* prints new copy every minute

Even if unchanged 😵

With `useCallback`:

* reuse same sheet unless data changes

##### 🔥 `React.memo` Connection

`useCallback` becomes useful mainly with:

```jsx
React.memo()
```

because memoized child compares prop references.

### ⚠️ Important Truth

Many beginners OVERUSE:

* `useMemo`
* `useCallback`

This can actually worsen performance 😅

Because memoization itself has cost.

**🚀 Use Them ONLY When**

✅ `useMemo`

Use when:

* expensive calculations
* large filtering/sorting
* derived heavy computations

✅ `useCallback`

Use when:

* passing callbacks to memoized children
* preventing unnecessary re-renders
* stable function references needed

❌ When NOT Needed

Don’t do this 😅

```jsx
const add = useCallback(() => {
  return 2 + 2
}, [])
```

Unnecessary optimization.

### ⚡ `useMemo` vs `useCallback`

| Feature  | `useMemo`            | `useCallback`     |
| -------- | ---------------------- | ------------------- |
| Memoizes | VALUE                  | FUNCTION            |
| Returns  | computed result        | function            |
| Used for | expensive calculations | stable callbacks    |
| Prevents | recalculation          | function recreation |

### 🧠 Interview-Level Understanding

🔹 `useMemo`

```txt
Cache calculation result
```

🔹 `useCallback`

```txt
Cache function reference
```

### ⚠️ Dependency Array Importance

Wrong dependencies can cause:

* stale values
* bugs
* unexpected behavior

Example:

```jsx
useCallback(() => {
  console.log(count)
}, [])
```

Problem:

* count becomes stale

Should be:

```jsx
useCallback(() => {
  console.log(count)
}, [count])
```

### 🔥 Common Startup Interview Questions

**❓ Why does child re-render even with `React.memo`?**

Because:

* new function/object references created

**❓ Can `useMemo` improve every app?**

❌ No.

Only specific performance bottlenecks.

---
