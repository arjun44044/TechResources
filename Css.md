# Shimmering Effect for Loaders

```css
/* shimmer.css */
.skeleton {
  background: linear-gradient(
    90deg,
    #f0f0f0 25%,
    #e0e0e0 37%,
    #f0f0f0 63%
  );
  background-size: 400% 100%;
  animation: shimmer 1.2s ease-in-out infinite;
  border-radius: 4px;
}

@keyframes shimmer {
  0% {
    background-position: -400px 0;
  }
  100% {
    background-position: 400px 0;
  }
}

```

> This is a CSS class that can be applied to any `div` or element you want to show a shimmering loading effect on.

##### 🟦 `background: linear-gradient(...)`

Creates a **horizontal gradient** that moves from light gray → slightly darker gray → light gray again.

* `90deg` → Horizontal direction (left to right).
* `#f0f0f0 25%` → Light gray until 25%.
* `#e0e0e0 37%` → Darker gray at 37%.
* `#f0f0f0 63%` → Light gray again at 63%.

This gradient creates the "shine" that moves across the element.

##### 🟦 `background-size: 400% 100%`

This enlarges the background gradient **horizontally** (4x wider than the element itself), so that the animated "shine" effect has room to scroll through.

##### 🟦 `animation: shimmer 1.2s ease-in-out infinite`

This runs the animation:

* `shimmer` → Uses the `@keyframes shimmer` defined below.
* `1.2s` → Each cycle lasts 1.2 seconds.
* `ease-in-out` → Smooth acceleration and deceleration.
* `infinite` → Loops forever.

##### 🟦 `border-radius: 4px`

Rounds the corners slightly to make the placeholder look better (like loading text or buttons).

##### 🔁 `@keyframes shimmer`

This defines how the background moves during the animation:

##### ⏮ `0% { background-position: -400px 0; }`

Start the background  **off to the left** , way outside the element.

##### ⏭ `100% { background-position: 400px 0; }`

End the background  **off to the right** , also outside the element.

👉 This movement creates the  **illusion of a light shimmer sweeping across** .

##### 🎯 Visual Summary

Imagine a div with a glowing band of light sliding over it from left to right. This is often used as a **loading placeholder** (before real content appears), simulating what you'd see in modern apps like Facebook, LinkedIn, or dashboards.
