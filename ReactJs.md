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

Let me know if you’d like to refactor this to a context-based or event-driven solution!
