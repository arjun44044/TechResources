# ----TO-LEARN

#### 🧩 **1. Core React Three Fiber Fundamentals**

Learn the basic way R3F works compared to vanilla Three.js.

🔹 Topics

* What is React Three Fiber? (Declarative rendering)
* How R3F maps to Three.js (JSX ≈ scene graph)
* Setting up a `<Canvas />`
* `useThree()`, `useFrame()`, and the render loop
* Adding basic objects (`<mesh>`, `<boxGeometry>`, `<meshStandardMaterial>`)
* Camera basics (`<PerspectiveCamera>`, `makeDefault`)
* Lights (`<ambientLight>`, `<directionalLight>`, etc.)
* Shadows (`castShadow`, `receiveShadow`)
* Orbit controls (`<OrbitControls />` from `@react-three/drei`)
* Working with colors and fog (`<fog attach="fog" args={[...]} />`)

#### 🎨 **2. Materials, Textures & Lighting**

Bring realism and variety to your scene.

🔹 Topics

* Built-in materials (`meshStandardMaterial`, `meshPhysicalMaterial`, etc.)
* Applying textures (using `useLoader(TextureLoader, url)`)
* Environment maps (HDRi lighting)
* Emissive and normal maps
* AO maps (and `uv2`)
* Light types and positioning
* Tone mapping and color encoding

#### 🧱 **3. Geometry & Models**

Handle complex geometries and imported 3D models.

🔹 Topics

* Primitive geometries (`<sphereGeometry>`, `<planeGeometry>`, etc.)
* Custom geometries (`bufferGeometry`, custom vertices)
* Loading GLTF / GLB models (`useGLTF` from drei)
* Using Draco compression
* Accessing model parts (`nodes`, `materials`)
* Animations from GLTF (`useAnimations`)
* Instancing (`<Instances />`, `<Instance />`)

#### ⚙️ **4. Hooks & Animation**

React hooks are at the heart of R3F’s interactivity.

🔹 Topics

* `useFrame()` for per-frame updates
* Managing state with React + R3F
* Animating with [Framer Motion 3D](https://www.framer.com/motion/three/)
* React Spring (`@react-spring/three`) for smooth transitions
* Gestures (rotation, dragging, etc.)
* Updating properties over time (`ref.current.position.x += …`)

#### 🎛️ **5. Drei (Helper Library)**

`@react-three/drei` is like a toolkit of ready-to-use 3D components.

🔹 Important Drei Components

* `<OrbitControls />`, `<TransformControls />`, `<FlyControls />`
* `<Environment />`, `<Sky />`, `<Stars />`
* `<Text />`, `<Html />` (for UI in 3D space)
* `<ContactShadows />`, `<AccumulativeShadows />`
* `<Instances />` and `<Merged />` (for performance)
* `<useGLTF />`, `<useTexture />`, `<useAnimations />`
* `<MeshTransmissionMaterial />`, `<Reflector />`, `<CameraControls />`

#### 🧠 **6. Reactivity & State Management**

Connect your 3D world with React’s reactivity.

🔹 Topics

* Using React state and props to control 3D objects
* Using Zustand or Jotai for global state
* Syncing UI controls (e.g., Leva or Tweakpane) with 3D parameters
* Updating materials, colors, and camera on UI input

#### ⚡ **7. Performance Optimization**

Make sure your scenes stay smooth and efficient.

🔹 Topics

* Instancing and batching
* `useMemo()` for geometry/materials
* Dynamic resolution scaling
* Baking lighting (lightmaps)
* Suspense and lazy-loading assets
* Postprocessing and effects composer
* Using `<Performance />` from drei

#### 🌍 **8. Postprocessing & Effects**

Make scenes cinematic and professional-looking.

🔹 Topics

* Using `@react-three/postprocessing`
* Bloom, SSAO, Depth of Field, God Rays, etc.
* Custom shaders via `<shaderMaterial />`
* Mixing R3F with GLSL shaders
* ShaderToy → React Three Fiber adaptation

#### 🕹️ **9. Interactivity & Physics**

Add interaction and realism.

🔹 Topics

* Raycasting and `onPointerOver`, `onClick`, `onPointerMove`
* Dragging and manipulating objects
* Using physics engines:
  * `@react-three/rapier` (modern physics engine)
  * Cannon.js (`use-cannon`)
* Collision detection
* Gravity and constraints

#### 🧩 **10. Advanced Topics**

Where you move beyond the basics.

🔹 Topics

* Creating custom materials (`shaderMaterial`)
* Combining multiple scenes and cameras
* Performance profiling
* Portals (`<Portal />` from drei)
* Mixing HTML UI with 3D scenes
* Integrating Three.js controls manually
* Creating 3D product configurators or 3D viewers

#### 🧠 **11. Complementary Tools**

Learn libraries often used with R3F:

| Purpose        | Library                         |
| -------------- | ------------------------------- |
| Controls UI    | Leva or Tweakpane               |
| Model loading  | Blender (to export GLTF/GLB)    |
| Physics        | @react-three/rapier             |
| Postprocessing | @react-three/postprocessing     |
| Animations     | Framer Motion 3D / React Spring |
| State          | Zustand / Jotai                 |
| 3D text        | drei’s `<Text />`            |
| GUI overlays   | drei’s `<Html />`            |

#### 💡 **12. Practice Projects**

To truly learn, build.

🔹 Project Ideas

* 3D Gym Product Viewer (for your e-commerce site 🏋️)
* 3D Product Configurator (choose color, lighting, angle)
* Interactive Gym Environment / Room Scene
* Rotating dumbbell animation with reflections
* 3D Logo animation for FitLab
* Gym equipment gallery with lighting & fog effects
* Physics-based gym ball / rope simulation

---

# ----Introduction

### ⚛️ What is React Three Fiber (R3F)?

**React Three Fiber (R3F)** is a **React renderer for Three.js** —

just like React DOM renders HTML, R3F renders  **Three.js scenes** .

✅ It lets you **write Three.js in JSX**

✅ Uses **React’s component model** for 3D scenes

✅ Handles **render loops, cameras, resizing, and updates** automatically

✅ Makes it easy to **combine 3D with UI (via React)**

### 🧠 1. Analogy: “React DOM vs React Three Fiber”

| Concept      | React DOM              | React Three Fiber                 |
| ------------ | ---------------------- | --------------------------------- |
| Renderer     | `react-dom`          | `@react-three/fiber`            |
| Scene Graph  | HTML elements          | Three.js objects                  |
| Root element | `<div>`              | `<Canvas>`                      |
| Elements     | `<div>`,`<span>`   | `<mesh>`,`<directionalLight>` |
| Styling      | CSS                    | Three.js materials, props         |
| Events       | onClick, onMouseOver   | onPointerDown, onPointerMove      |
| Lifecycle    | useEffect, state, refs | Same (it’s React!)               |

### 🧩 2. Installing R3F

```bash
npm install three @react-three/fiber
```

(Optional but recommended):

```bash
npm install @react-three/drei
```

> Drei provides helpful prebuilt components like `<OrbitControls>`, `<Environment>`, `<Text>`, etc.

### 🪄 3. The Core: `<Canvas>` is your scene

In R3F, you **don’t manually create** renderer, scene, or camera.

You wrap your scene inside `<Canvas>` — and R3F does the rest.

👉 Three.js Version

```js
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, aspect, 0.1, 100);
const renderer = new THREE.WebGLRenderer();
renderer.render(scene, camera);
```

⚛️ React Three Fiber Version

```jsx
import { Canvas } from '@react-three/fiber'

function App() {
  return (
    <Canvas camera={{ position: [0, 0, 5], fov: 75 }}>
      <mesh>
        <boxGeometry args={[1, 1, 1]} />
        <meshStandardMaterial color="orange" />
      </mesh>
      <ambientLight intensity={0.3} />
      <directionalLight position={[5, 5, 5]} />
    </Canvas>
  )
}
```

🟢 That’s it — no render loop, no manual setup — just JSX!

### 🧱 4. JSX ≈ Scene Graph

In R3F,  **each JSX tag corresponds to a Three.js class** .

| JSX Element                        | Three.js Equivalent                  |
| ---------------------------------- | ------------------------------------ |
| `<mesh>`                         | `new THREE.Mesh()`                 |
| `<boxGeometry args={[1,1,1]} />` | `new THREE.BoxGeometry(1,1,1)`     |
| `<meshStandardMaterial />`       | `new THREE.MeshStandardMaterial()` |
| `<ambientLight />`               | `new THREE.AmbientLight()`         |
| `<group>`                        | `new THREE.Group()`                |
| `<directionalLight />`           | `new THREE.DirectionalLight()`     |
| `<orbitControls />`(from drei)   | `new OrbitControls()`              |

So your JSX **is** your scene graph.

🧩 Example:

```jsx
<Canvas>
  <group position={[0, 2, 0]}>
    <mesh>
      <sphereGeometry args={[1, 32, 32]} />
      <meshStandardMaterial color="skyblue" />
    </mesh>
  </group>
</Canvas>
```

This translates internally to:

```js
const group = new THREE.Group();
group.position.set(0, 2, 0);

const mesh = new THREE.Mesh(
  new THREE.SphereGeometry(1, 32, 32),
  new THREE.MeshStandardMaterial({ color: 'skyblue' })
);

group.add(mesh);
scene.add(group);
```

### ⚙️ 5. Props Map to Object Properties

Every prop on a JSX element in R3F is mapped directly to the corresponding  **Three.js object property** .

For example:

| JSX Prop                                    | Affects Three.js Property                |
| ------------------------------------------- | ---------------------------------------- |
| `<mesh position={[1, 2, 3]} />`           | `mesh.position.set(1,2,3)`             |
| `<mesh rotation={[0, Math.PI / 2, 0]} />` | `mesh.rotation.set(0, Math.PI / 2, 0)` |
| `<mesh scale={2} />`                      | `mesh.scale.set(2, 2, 2)`              |
| `<directionalLight intensity={0.8} />`    | `light.intensity = 0.8`                |

### 🌀 6. Animation: useFrame()

Instead of `requestAnimationFrame`, use the `useFrame` hook.

```jsx
import { useFrame } from '@react-three/fiber'
import { useRef } from 'react'

function RotatingCube() {
  const ref = useRef()

  useFrame((state, delta) => {
    ref.current.rotation.x += delta
    ref.current.rotation.y += delta
  })

  return (
    <mesh ref={ref}>
      <boxGeometry />
      <meshStandardMaterial color="tomato" />
    </mesh>
  )
}

function App() {
  return (
    <Canvas>
      <ambientLight />
      <RotatingCube />
    </Canvas>
  )
}
```

🧠 Think of `useFrame` as your **Three.js animate() loop** — but integrated into React’s state system.

### 🧮 7. Accessing Three.js Objects

You can still directly access Three.js classes, methods, and math utilities:

```jsx
import * as THREE from 'three'

function Example() {
  const color = new THREE.Color(0xff0000)
  const vec = new THREE.Vector3(1, 2, 3)
  // still usable inside R3F
}
```

### 💡 8. React Three Fiber + Drei Example

```jsx
import { Canvas } from '@react-three/fiber'
import { OrbitControls, Environment } from '@react-three/drei'

export default function App() {
  return (
    <Canvas camera={{ position: [3, 3, 3] }}>
      <ambientLight intensity={0.3} />
      <directionalLight position={[5, 5, 5]} />
      <mesh>
        <boxGeometry args={[1, 1, 1]} />
        <meshStandardMaterial color="orange" metalness={0.5} roughness={0.2} />
      </mesh>
      <Environment preset="sunset" />
      <OrbitControls />
    </Canvas>
  )
}
```

🟢 You get realistic lighting, easy camera movement, and clean React structure — no manual Three.js boilerplate.

### 🧭 9. Summary: How R3F Maps to Three.js

| Concept  | Three.js                             | React Three Fiber               |
| -------- | ------------------------------------ | ------------------------------- |
| Scene    | `new THREE.Scene()`                | `<Canvas>`                    |
| Mesh     | `new THREE.Mesh()`                 | `<mesh>`                      |
| Geometry | `new THREE.BoxGeometry()`          | `<boxGeometry />`             |
| Material | `new THREE.MeshStandardMaterial()` | `<meshStandardMaterial />`    |
| Camera   | `new THREE.PerspectiveCamera()`    | `<Canvas camera={{ ... }} />` |
| Renderer | `new THREE.WebGLRenderer()`        | Managed automatically           |
| Animate  | `requestAnimationFrame(animate)`   | `useFrame()`                  |
| Controls | `new OrbitControls()`              | `<OrbitControls />`from drei  |
| Lights   | `new THREE.DirectionalLight()`     | `<directionalLight />`        |

---

# ----useFrame(), useThree() Hooks and Render Loop

Now you’re stepping into the  **core of React Three Fiber (R3F)** ’s architecture 🔥

The concepts `useThree()`, `useFrame()`, and the **render loop** are the heart of how R3F connects **React’s declarative system** with  **Three.js’s real-time rendering** .

Let’s unpack them carefully, with visual explanations, internal logic, and examples.

### ⚙️ The R3F Render Loop — the Heartbeat of Your 3D Scene

In  **Three.js** , you usually write something like this:

```js
function animate() {
  requestAnimationFrame(animate)
  mesh.rotation.x += 0.01
  renderer.render(scene, camera)
}
animate()
```

That’s the **render loop** — continuously called ~60 times per second.

✅ In  **React Three Fiber** , you  **don’t call or manage `requestAnimationFrame()` yourself** .

R3F has its own **internal render loop** that automatically:

* Updates frames
* Calls `useFrame()` callbacks
* Handles camera controls and animations
* Re-renders when necessary

### ⚡ 1. `useFrame()` — Hook into the render loop

`useFrame()` is the **React Three Fiber equivalent** of writing code inside your `animate()` function.

**Syntax:**

```jsx
useFrame((state, delta) => {
  // This runs every frame (~60fps)
})
```

**Parameters:**

| Parameter | Description                                                                             |
| --------- | --------------------------------------------------------------------------------------- |
| `state` | Contains important objects like `camera`,`scene`,`gl`,`clock`,`pointer`, etc. |
| `delta` | The**time elapsed since the last frame**(useful for smooth animations).           |

##### Example — Rotating Cube

```jsx
import { useFrame } from '@react-three/fiber'
import { useRef } from 'react'

function RotatingCube() {
  const ref = useRef()

  useFrame((state, delta) => {
    ref.current.rotation.x += delta
    ref.current.rotation.y += delta * 0.5
  })

  return (
    <mesh ref={ref}>
      <boxGeometry />
      <meshStandardMaterial color="orange" />
    </mesh>
  )
}
```

Here, R3F calls your callback **every frame** — you never need to manage the render loop manually.

##### ✅ You Can Use Multiple `useFrame()`s

Each component can have its **own** `useFrame()`.

They’ll all be called in sequence during each frame.

> #### ⚙️ The `state` Object of `useFrame()` in Detail
>
> The `state` parameter is **automatically provided** by the R3F renderer for every frame, and it contains  **references to the current 3D context** .
>
> Here are the main properties inside it:
>
> | Property              | Type                                                        | Description                                                                                        |
> | --------------------- | ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
> | `state.gl`          | `THREE.WebGLRenderer`                                     | The main WebGL renderer used by R3F.<br />You can change renderer properties (e.g., tone mapping). |
> | `state.scene`       | `THREE.Scene`                                             | The main Three.js scene object that holds all meshes, lights, etc.                                 |
> | `state.camera`      | `THREE.Camera`                                            | The active camera being used to render the scene.                                                  |
> | `state.clock`       | `THREE.Clock`                                             | A Three.js clock tracking elapsed time.                                                            |
> | `state.size`        | `{ width, height, top, left }`                            | The current size of the canvas.                                                                    |
> | `state.viewport`    | `{ width, height, aspect, factor }`                       | The world-space size of the viewport (useful for scaling).                                         |
> | `state.pointer`     | `{ x, y }`                                                | The current normalized mouse position (-1 to +1).                                                  |
> | `state.mouse`       | `{ x, y }`                                                | Same as `pointer`(legacy alias).                                                                 |
> | `state.controls`    | Your camera controls (if using `drei`OrbitControls, etc.) |                                                                                                    |
> | `state.performance` | Performance data and adaptive settings.                     |                                                                                                    |
> | `state.events`      | The pointer/mouse/touch event system.                       |                                                                                                    |
> | `state.frameloop`   | The current frame mode:`'always'`,`'demand'`, etc.      |                                                                                                    |
>
> ##### 💡 Example: Using `state` inside `useFrame()`
>
> ```jsx
> useFrame((state, delta) => {
>   // Rotate the mesh smoothly
>   meshRef.current.rotation.y += delta;
>
>   // Get elapsed time
>   const t = state.clock.getElapsedTime();
>
>   // Animate camera position
>   state.camera.position.x = Math.sin(t) * 5;
>   state.camera.position.z = Math.cos(t) * 5;
>   state.camera.lookAt(0, 0, 0);
> });
> ```
>
> Here:
>
> * `state.clock.getElapsedTime()` gives total runtime in seconds.
> * `state.camera` allows direct control of the camera each frame.
> * `delta` ensures consistent animation speed across devices.
>
> ##### 🧠 Important Notes
>
> * You **don’t need to call `renderer.render()`** manually — R3F handles rendering automatically after all `useFrame()` hooks finish each frame.
> * Multiple `useFrame()` hooks can exist; they all run each frame in the  **order they were declared** .
> * You can use `useFrame(fn, priority)` where a higher priority runs earlier (useful for camera vs. object updates).
>
> #### ⚡ Example: Camera Following an Object
>
> ```jsx
> function CameraFollow({ target }) {
>   useFrame((state) => {
>     state.camera.position.lerp(
>       target.current.position.clone().add(new THREE.Vector3(0, 2, 5)),
>       0.1
>     );
>     state.camera.lookAt(target.current.position);
>   });
> }
> ```
>
> Here:
>
> * We access `state.camera` to control it every frame.
> * The camera smoothly moves toward the target (a mesh).

### 🧠 2. `useThree()` — Access the R3F internal state

`useThree()` gives you access to the **Three.js objects** managed by React Three Fiber.

**Syntax:**

```jsx
import { useThree } from '@react-three/fiber'

const { camera, scene, gl, clock, size } = useThree()
```

**Returned values:**

| Property      | Description                                |
| ------------- | ------------------------------------------ |
| `gl`        | The WebGLRenderer instance                 |
| `scene`     | The root Three.js scene                    |
| `camera`    | The active camera                          |
| `clock`     | The internal clock                         |
| `size`      | `{ width, height }`of the canvas         |
| `viewport`  | Physical viewport dimensions               |
| `mouse`     | Pointer position in normalized coordinates |
| `pointer`   | `{ x, y }`coordinates of the pointer     |
| `raycaster` | The default Raycaster used for interaction |

**Example — Move Object Based on Mouse**

```jsx
import { useThree, useFrame } from '@react-three/fiber'
import { useRef } from 'react'

function MouseFollower() {
  const ref = useRef()
  const { mouse, viewport } = useThree()

  useFrame(() => {
    ref.current.position.x = mouse.x * viewport.width / 2
    ref.current.position.y = mouse.y * viewport.height / 2
  })

  return (
    <mesh ref={ref}>
      <sphereGeometry args={[0.3, 32, 32]} />
      <meshStandardMaterial color="hotpink" />
    </mesh>
  )
}
```

🎯 Here, `useThree()` gives us the mouse and viewport,

and `useFrame()` updates the sphere’s position every frame.

### 🔁 3. How the Render Loop Works Internally

R3F’s internal render loop roughly does this every frame:

1. Compute `delta` (time between frames)
2. Call **all registered `useFrame()` callbacks**
3. Update any controls (like `OrbitControls`)
4. Render the scene via `gl.render(scene, camera)`
5. Request next animation frame

You can pause, resume, or even manage multiple render loops if needed — but by default, it just works automatically.

### 🧩 4. Combining useThree + useFrame — Full Example

```jsx
import { Canvas, useThree, useFrame } from '@react-three/fiber'
import { OrbitControls } from '@react-three/drei'
import { useRef } from 'react'

function RotatingCube() {
  const ref = useRef()
  const { camera } = useThree()

  useFrame((state, delta) => {
    ref.current.rotation.y += delta
    camera.lookAt(ref.current.position)
  })

  return (
    <mesh ref={ref} position={[0, 0, 0]}>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color="skyblue" />
    </mesh>
  )
}

export default function App() {
  return (
    <Canvas camera={{ position: [3, 3, 3] }}>
      <ambientLight intensity={0.5} />
      <RotatingCube />
      <OrbitControls />
    </Canvas>
  )
}
```

### 🧭 5. Advanced — Control the Loop Manually (rare)

If you really want to **pause or control** the render loop:

```jsx
<Canvas frameloop="demand">  // renders only when needed
```

Then you can trigger a render manually:

```js
const { invalidate } = useThree()
invalidate() // requests one render
```

Or control render frequency (for performance):

```jsx
<Canvas dpr={[1, 2]} performance={{ min: 0.5 }} />
```

That’s where R3F really starts feeling  *alive* .

> #### 🧠 1️⃣ `dpr={[1, 2]}`
>
> **💡 Meaning: Device Pixel Ratio Range**
>
> This controls **rendering resolution** — i.e. how sharp or heavy the 3D render is, relative to your screen’s pixel density.
>
> * A device’s *pixel ratio* is the ratio between **device pixels** and  **CSS pixels** .
>
>   Example:
>
>   * Most normal screens → DPR = 1
>   * Retina displays → DPR = 2 (twice as many pixels per CSS unit)
>
> React Three Fiber lets you set a range of DPR values:
>
> ```js
> dpr={[min, max]}
> ```
>
> Here: `[1, 2]`
>
> So:
>
> * On low-end displays → uses `1` (better performance)
> * On high-end retina displays → can use up to `2` (sharper visuals)
>
> 🎯 **Goal:** Balance between **sharpness and performance.**
>
> It automatically picks the best DPR between 1 and 2 based on the current device.
>
> #### ⚙️ 2️⃣ `performance={{ min: 0.5 }}`
>
> **💡 Meaning: Adaptive performance scaling**
>
> React Three Fiber includes a **performance management system** that automatically adjusts render quality to maintain a stable frame rate.
>
> When performance dips, it can:
>
> * **reduce resolution**
> * **pause certain effects**
> * **lower update frequency**
>
> Here:
>
> ```js
> performance={{ min: 0.5 }}
> ```
>
> means:
>
>> Allow performance scaling down to 50% (half resolution / half update rate)
>>
>> if the system starts struggling to maintain smooth frame rates.
>>
>
> So the scene remains responsive even on weaker GPUs or heavy scenes.
>
> #### 📊 Combined Effect
>
> Together:
>
> ```jsx
> <Canvas dpr={[1, 2]} performance={{ min: 0.5 }} />
> ```
>
> means:
>
> ✅ On a high-end screen → sharp rendering (DPR = 2)
>
> ✅ On a low-end or overloaded system → drop DPR down to 1 or even half-quality temporarily
>
> ✅ Automatically manages performance for you
>
> #### ⚡ Example comparison:
>
> | Setting                        | Visual                   | Performance impact |
> | ------------------------------ | ------------------------ | ------------------ |
> | `dpr={1}`                    | Slightly blurrier        | Fastest            |
> | `dpr={2}`                    | Very crisp               | Heavier on GPU     |
> | `performance={{ min: 0.5 }}` | Auto adjusts dynamically | Keeps FPS stable   |
>
> #### 💬 In simple words:
>
>> `dpr={[1,2]}` = “Render between normal and retina resolution depending on device.”
>>
>> `performance={{min:0.5}}` = “If FPS drops, reduce quality to half temporarily to stay smooth.”
>>

---

# ----Camera

### 🎥 What is a Camera in React Three Fiber?

In  **Three.js** , you explicitly create and position a camera:

```js
const camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 100);
camera.position.set(0, 2, 5);
```

In  **R3F** , the camera is created *automatically* by the `<Canvas>` component unless you provide your own.

It still uses **Three.js cameras under the hood** — usually a `PerspectiveCamera`, but you can override it with an `OrthographicCamera` or your own instance.

### 🧱 1️⃣ Default Camera Behavior

If you don’t specify anything:

```jsx
<Canvas>
  <mesh>
    <boxGeometry />
    <meshStandardMaterial color="hotpink" />
  </mesh>
</Canvas>
```

R3F internally does:

```js
const camera = new THREE.PerspectiveCamera(75, aspect, 0.1, 1000)
camera.position.z = 5
```

So you *start* with a Perspective camera placed at `z = 5`.

### 🎯 2️⃣ Customizing the Camera

You can customize the default camera like this:

```jsx
<Canvas camera={{ position: [0, 2, 10], fov: 50, near: 0.1, far: 200 }}>
  <mesh>
    <boxGeometry />
    <meshStandardMaterial color="orange" />
  </mesh>
</Canvas>
```

📌 The `camera` prop on `<Canvas>` maps directly to  **PerspectiveCamera properties** .

### 🧠 3️⃣ Using a Custom Camera Object

You can also define your own camera inside JSX and make it  **the active one** :

```jsx
<Canvas>
  <PerspectiveCamera makeDefault position={[0, 2, 5]} fov={60} />
  <mesh>
    <boxGeometry />
    <meshStandardMaterial color="lightblue" />
  </mesh>
</Canvas>
```

### ✨ `makeDefault`:

This is key — it tells R3F:

> “Use this camera as the renderer’s default camera instead of the auto one.”

You can even switch cameras dynamically by toggling `makeDefault`.

### 🔍 4️⃣ Types of Cameras

R3F supports all Three.js camera types:

| Camera Type            | JSX Component              | Description                                                                                                      |
| ---------------------- | -------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `PerspectiveCamera`  | `<PerspectiveCamera />`  | Realistic 3D depth (like human eyes). Default camera.                                                            |
| `OrthographicCamera` | `<OrthographicCamera />` | No perspective — objects remain same size regardless of distance.<br />Great for UI, 2D views, isometric games. |
| `CubeCamera`         | `<CubeCamera />`         | Captures 360° environment maps for reflections/refractions.                                                     |

### 🛠️ 5️⃣ Accessing the Camera in Code

You can use the `useThree()` hook to access the current camera:

```js
import { useThree } from '@react-three/fiber'

function DebugCamera() {
  const { camera } = useThree()
  console.log(camera.position)
  return null
}
```

✅ `useThree()` gives you the  **Three.js renderer state** , including:

* `camera`
* `scene`
* `gl` (WebGLRenderer)
* `size`
* `viewport`
* `clock`

### 🔁 6️⃣ Controlling the Camera with `useFrame`

You can animate or move the camera inside the render loop using `useFrame()`:

```js
useFrame((state, delta) => {
  state.camera.position.x = Math.sin(state.clock.elapsedTime) * 5
  state.camera.lookAt(0, 0, 0)
})
```

Here:

* `state.camera` → current camera instance
* `delta` → time between frames
* This continuously updates the camera each frame

### 🕹️ 7️⃣ Using Controls (OrbitControls, etc.)

Attach camera controls easily:

```jsx
import { OrbitControls } from '@react-three/drei'

<Canvas camera={{ position: [0, 2, 5], fov: 60 }}>
  <OrbitControls />
  <mesh>
    <boxGeometry />
    <meshStandardMaterial color="salmon" />
  </mesh>
</Canvas>
```

The `<OrbitControls>` automatically hooks into the default R3F camera (no need to pass explicitly).

### 🧩 8️⃣ Orthographic Example

```jsx
<Canvas orthographic camera={{ zoom: 50, position: [0, 0, 10] }}>
  <mesh>
    <boxGeometry />
    <meshStandardMaterial color="green" />
  </mesh>
</Canvas>
```

Setting the `orthographic` prop on `<Canvas>` automatically creates an `OrthographicCamera` instead of a `PerspectiveCamera`.

### ⚡ Summary Table

| Concept          | Three.js                            | React Three Fiber                                       |
| ---------------- | ----------------------------------- | ------------------------------------------------------- |
| Create camera    | `new THREE.PerspectiveCamera()`   | `<Canvas camera={...} />`or `<PerspectiveCamera />` |
| Add to scene     | `scene.add(camera)`               | R3F adds automatically                                  |
| Update per frame | `camera.position.x += 1`          | `useFrame((state) => state.camera.position.x += 1)`   |
| Switch cameras   | `renderer.render(scene, camera2)` | `<Camera makeDefault />`toggled dynamically           |
| Access           | `camera`variable                  | `useThree().camera`                                   |

---

# ----Lights

### 💡 1️⃣ What are lights in R3F?

Lights in **R3F** are the same  **Three.js light objects** , just written in **JSX** syntax.

So, instead of:

```js
const light = new THREE.DirectionalLight(0xffffff, 1)
light.position.set(2, 2, 2)
scene.add(light)
```

You write:

```jsx
<directionalLight color="white" intensity={1} position={[2, 2, 2]} />
```

👉 Under the hood, R3F creates a `THREE.DirectionalLight` instance and adds it to your scene automatically.

### ⚙️ 2️⃣ How R3F Handles Lights

In R3F:

* You **declare** lights just like React components.
* You can **animate** their properties over time.
* You can **nest** them inside groups or parents (they inherit transforms).
* They **automatically attach** to the scene graph.

Example:

```jsx
<Canvas>
  <ambientLight intensity={0.3} />
  <directionalLight color="white" position={[3, 5, 2]} intensity={1.5} />
  <mesh>
    <sphereGeometry />
    <meshStandardMaterial color="orange" />
  </mesh>
</Canvas>
```

### 🔦 3️⃣ Types of Lights in React Three Fiber (same as Three.js)

| Light Type                               | JSX Component             | Description                                          | Shadows      |
| ---------------------------------------- | ------------------------- | ---------------------------------------------------- | ------------ |
| **AmbientLight**                   | `<ambientLight />`      | Global light that affects all objects equally        | ❌           |
| **DirectionalLight**               | `<directionalLight />`  | Like the sun — light rays parallel from a direction | ✅           |
| **PointLight**                     | `<pointLight />`        | Emits light equally in all directions from a point   | ✅           |
| **SpotLight**                      | `<spotLight />`         | Emits a cone-shaped beam of light                    | ✅           |
| **HemisphereLight**                | `<hemisphereLight />`   | Mix of sky color and ground color                    | ❌           |
| **RectAreaLight**                  | `<rectAreaLight />`     | Emits light from a rectangular surface               | ✅ (special) |
| **AmbientLightProbe / LightProbe** | `<ambientLightProbe />` | Advanced global illumination probes                  | ❌           |

### 💡 4️⃣ Common Light Props

All lights share certain properties:

| Property       | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `color`      | Color of the light (`"white"`or `0xffffff`)              |
| `intensity`  | Brightness (default:`1`)                                   |
| `position`   | Light position in world coordinates                          |
| `target`     | (Directional/Spot) The object or point light aims at         |
| `castShadow` | Enables shadows (must also enable renderer + object)         |
| `shadow-*`   | Shadow map quality (e.g.,`shadow-mapSize`,`shadow-bias`) |

### 🌞 5️⃣ Example: Directional Light with Shadows

```jsx
<Canvas shadows camera={{ position: [5, 5, 5], fov: 50 }}>
  <ambientLight intensity={0.2} />
  <directionalLight
    position={[3, 3, 3]}
    intensity={1.2}
    castShadow
    shadow-mapSize={[1024, 1024]}
  />
  <mesh receiveShadow castShadow position={[0, 0.5, 0]}>
    <boxGeometry args={[1, 1, 1]} />
    <meshStandardMaterial color="skyblue" />
  </mesh>
  <mesh receiveShadow rotation={[-Math.PI / 2, 0, 0]}>
    <planeGeometry args={[10, 10]} />
    <meshStandardMaterial color="lightgray" />
  </mesh>
</Canvas>
```

✅ **Key Notes:**

* `<Canvas shadows>` enables the renderer’s shadow system.
* Each shadow-casting object must have `castShadow`.
* Surfaces that receive shadows must have `receiveShadow`.

### 🔦 6️⃣ Common Light Types Explained Visually

##### 🟢 AmbientLight

```jsx
<ambientLight intensity={0.5} color="#ffffff" />
```

☀️ Provides uniform lighting — no shadows or direction.

Good for soft fill light.

##### ☀️ DirectionalLight

```jsx
<directionalLight position={[5, 10, 5]} intensity={1} castShadow />
```

Acts like sunlight — rays come parallel from a direction.

Creates hard shadows.

##### 💡 PointLight

```jsx
<pointLight position={[0, 3, 0]} intensity={1.5} distance={20} />
```

Omnidirectional — like a light bulb.

Intensity drops with distance.

##### 🔦 SpotLight

```jsx
<spotLight
  position={[3, 5, 3]}
  angle={Math.PI / 6}
  penumbra={0.2}
  intensity={2}
  castShadow
/>
```

Cone-shaped light — good for lamps, flashlights, stage lights.

##### 🌈 HemisphereLight

```jsx
<hemisphereLight skyColor="blue" groundColor="brown" intensity={0.6} />
```

Mixes “sky” and “ground” colors for natural outdoor illumination.

##### 🧱 RectAreaLight

```jsx
<rectAreaLight
  width={5}
  height={2}
  color="white"
  intensity={3}
  position={[0, 3, 0]}
  lookAt={[0, 0, 0]}
/>
```

Emits light from a rectangular surface (requires `RectAreaLightUniformsLib` under the hood, handled by R3F automatically).

### 🔁 7️⃣ Controlling Lights Dynamically

You can animate or modify light properties via React state or hooks:

```jsx
function MovingLight() {
  const ref = useRef()
  useFrame(({ clock }) => {
    ref.current.position.x = Math.sin(clock.elapsedTime) * 3
  })
  return <pointLight ref={ref} intensity={1.5} color="orange" />
}
```

This light moves back and forth every frame.

### 🎚️ 8️⃣ Adjusting Lights Interactively (e.g., Tweakpane or Leva)

You can control light parameters easily with  **Leva** :

```jsx
import { useControls } from 'leva'

function Scene() {
  const { intensity, color } = useControls({ intensity: 1, color: '#ffffff' })
  return <directionalLight intensity={intensity} color={color} />
}
```

This adds a live UI slider for your lights.

### 🧩 9️⃣ Helper Components

Drei (the R3F helper library) gives built-in visual helpers:

```jsx
import { DirectionalLightHelper, PointLightHelper } from '@react-three/drei'

<directionalLight ref={lightRef} position={[5, 5, 5]} />
<DirectionalLightHelper args={[lightRef, 1]} />
```

Shows visual wireframes of lights (great for debugging).

### 🧠 10️⃣ Summary Table

| Light                 | Shape           | Shadows | Best Use          |
| --------------------- | --------------- | ------- | ----------------- |
| **Ambient**     | Global          | ❌      | Fill/base light   |
| **Directional** | Parallel rays   | ✅      | Sunlight          |
| **Point**       | Omnidirectional | ✅      | Bulb, fire        |
| **Spot**        | Cone            | ✅      | Flashlight, stage |
| **Hemisphere**  | Sky+Ground mix  | ❌      | Outdoor ambient   |
| **RectArea**    | Rectangle       | ✅      | Soft panel light  |

---

# ----OrbitControls

🧭 What Are OrbitControls?

In  **Three.js** , `OrbitControls` is a utility that lets you  **orbit the camera around a target** ,  **zoom in/out** , and **pan** using mouse or touch input.

In  **React Three Fiber** , OrbitControls are provided by the **@react-three/drei** helper library — so you import and use them as **a JSX component** instead of manually constructing them.

### 🧩 Installation

If you haven’t installed drei yet:

```bash
npm install @react-three/drei
```

Then import:

```jsx
import { OrbitControls } from '@react-three/drei'
```

### ⚙️ Basic Usage

Here’s the simplest example:

```jsx
import { Canvas } from '@react-three/fiber'
import { OrbitControls } from '@react-three/drei'

function Scene() {
  return (
    <>
      <mesh>
        <boxGeometry />
        <meshStandardMaterial color="orange" />
      </mesh>

      <OrbitControls />
    </>
  )
}

export default function App() {
  return (
    <Canvas camera={{ position: [3, 3, 3] }}>
      <ambientLight intensity={0.5} />
      <Scene />
    </Canvas>
  )
}
```

✅ Now you can:

* **Left-click drag** → orbit (rotate) around the target.
* **Right-click drag** → pan.
* **Scroll** → zoom in/out.

### 🧠 How OrbitControls Works Internally in R3F

In plain Three.js, you’d write:

```js
const controls = new OrbitControls(camera, renderer.domElement)
controls.update()
```

And call `controls.update()` inside every frame.

But in  **R3F** , `OrbitControls` automatically connects to:

* The **active camera** (`useThree().camera`)
* The **WebGL canvas** (`useThree().gl.domElement`)
* The **render loop**

You don’t need to update it manually — R3F does it behind the scenes.

### 🧩 OrbitControls Props

| Prop                | Type          | Description                                                 |
| ------------------- | ------------- | ----------------------------------------------------------- |
| `target`          | `[x, y, z]` | The point the camera orbits around (default `[0, 0, 0]`). |
| `enableZoom`      | `boolean`   | Enable/disable zoom with mouse wheel.                       |
| `enablePan`       | `boolean`   | Enable/disable panning.                                     |
| `enableRotate`    | `boolean`   | Enable/disable rotation.                                    |
| `minDistance`     | `number`    | Minimum zoom distance.                                      |
| `maxDistance`     | `number`    | Maximum zoom distance.                                      |
| `minPolarAngle`   | `number`    | Minimum vertical angle (in radians).                        |
| `maxPolarAngle`   | `number`    | Maximum vertical angle (in radians).                        |
| `autoRotate`      | `boolean`   | Automatically orbit around the target.                      |
| `autoRotateSpeed` | `number`    | Speed of automatic orbiting.                                |
| `makeDefault`     | `boolean`   | Makes this the default camera control in the scene.         |

### 💡 Example with Custom Props

```jsx
<OrbitControls
  target={[0, 1, 0]}
  enablePan={false}
  enableZoom={true}
  minDistance={2}
  maxDistance={10}
  autoRotate
  autoRotateSpeed={2}
  makeDefault
/>
```

This:

* Keeps the camera orbiting smoothly.
* Locks panning.
* Sets zoom limits.
* Focuses on the target at y = 1.

### 🧠 `makeDefault` Explained

`makeDefault` means:

> “Use this controls instance as the **default control system** for the R3F canvas.”

That allows other parts of your app (like the `state.controls` inside `useFrame`) to automatically reference this instance:

```jsx
useFrame((state) => {
  state.controls.update()
})
```

If you  **don’t use `makeDefault`** , R3F won’t know which controls to treat as the global one.

### 🧱 Accessing the Controls via a Ref

You can use a React `ref` if you want to control OrbitControls programmatically:

```jsx
import { useRef } from 'react'
import { OrbitControls } from '@react-three/drei'

function Scene() {
  const controls = useRef()

  useFrame(() => {
    // Access controls directly
    controls.current.autoRotate = true
  })

  return <OrbitControls ref={controls} />
}
```

### 🪄 Example: Smooth Auto-Rotation Camera

```jsx
function Scene() {
  const controls = useRef()

  useFrame((state) => {
    controls.current.update()
  })

  return (
    <>
      <mesh>
        <sphereGeometry args={[1, 32, 32]} />
        <meshStandardMaterial color="skyblue" />
      </mesh>

      <OrbitControls
        ref={controls}
        autoRotate
        autoRotateSpeed={1}
        enableZoom
        makeDefault
      />
    </>
  )
}
```

### ⚙️ How OrbitControls Works in the R3F Render Cycle

When R3F renders each frame:

1. It runs all `useFrame()` callbacks.
2. Then updates controls (like OrbitControls).
3. Then renders the scene with the active camera.

So you can **combine manual camera updates** and OrbitControls behavior easily.

---

# ----Types of Controls

#### 🎮 1️⃣ OrbitControls

✅ **Most common and default camera control**

**Purpose:** Orbit around a target point, zoom, and pan (like inspecting a 3D model).

**Import:**

```jsx
import { OrbitControls } from '@react-three/drei'
```

**Usage:**

```jsx
<OrbitControls
  enableZoom={true}
  enablePan={true}
  enableRotate={true}
  dampingFactor={0.05}
  minDistance={2}
  maxDistance={20}
/>
```

**Unique properties:**

* `target`: The point the camera orbits around (default = `[0, 0, 0]`).
* `enableDamping`: Smooth motion when rotating or panning.
* `screenSpacePanning`: Whether panning occurs in screen space (vs world space).

**Use case:** Product viewer, model inspection, static scenes.

#### 🧭 2️⃣ TrackballControls

**Purpose:** Freely rotate, zoom, and pan without a fixed orbit target.

Gives a “floating camera” feeling.

**Import:**

```jsx
import { TrackballControls } from '@react-three/drei'
```

**Usage:**

```jsx
<TrackballControls rotateSpeed={1} zoomSpeed={1.2} panSpeed={0.8} />
```

**Differences from OrbitControls:**

* No fixed target — you rotate directly around camera axes.
* More “freeform” feeling, often used for CAD-style navigation.

**Use case:** Scientific visualization, 3D graphs, data visualization tools.

#### ✈️ 3️⃣ FlyControls

**Purpose:** Move through space as if flying (like freecam in a game).

**Import:**

```jsx
import { FlyControls } from '@react-three/drei'
```

**Usage:**

```jsx
<FlyControls movementSpeed={5} rollSpeed={0.5} dragToLook={true} />
```

**Properties:**

* `movementSpeed`: How fast you move forward/backward.
* `rollSpeed`: How quickly camera rolls around its axis.
* `dragToLook`: Whether to require mouse drag to rotate view.

**Use case:** Flying through architectural scenes, space exploration, simulation previews.

#### 🧍 4️⃣ FirstPersonControls

**Purpose:** Move and look around like a person in a first-person game (WASD + mouse).

**Import:**

```jsx
import { FirstPersonControls } from '@react-three/drei'
```

**Usage:**

```jsx
<FirstPersonControls
  movementSpeed={3}
  lookSpeed={0.1}
  lookVertical={true}
/>
```

**Properties:**

* `movementSpeed`: Speed of walking.
* `lookSpeed`: Mouse sensitivity.
* `lookVertical`: Whether to allow looking up/down.

**Use case:** FPS game prototypes, immersive walkthroughs.

#### 🔒 5️⃣ PointerLockControls

**Purpose:** Locks the mouse to the canvas, like an FPS camera (used in 3D games).

**Import:**

```jsx
import { PointerLockControls } from '@react-three/drei'
```

**Usage:**

```jsx
<PointerLockControls />
```

* Works with keyboard for movement (usually implemented manually with `useFrame()`).
* Mouse moves the view — cursor disappears (locked).
* Must call `.lock()` to activate pointer lock (automatically handled in drei version when clicked).

**Use case:** First-person navigation in games, simulators.

#### 🗺️ 6️⃣ MapControls

**Purpose:** Orbit-style controls specialized for 2D map navigation.

**Import:**

```jsx
import { MapControls } from '@react-three/drei'
```

**Usage:**

```jsx
<MapControls
  enableRotate={false}
  enablePan={true}
  enableZoom={true}
/>
```

**Differences from OrbitControls:**

* Rotation disabled (by default).
* Camera orbits in an overhead style (ideal for flat scenes).
* Often used with orthographic cameras.

**Use case:** 3D map viewers, top-down editors, strategy-style UI.

#### 🌀 7️⃣ ArcballControls

**Purpose:** Smooth orbiting around a pivot with inertia — combines benefits of Orbit and Trackball.

**Import:**

```jsx
import { ArcballControls } from '@react-three/drei'
```

**Usage:**

```jsx
<ArcballControls enablePan={true} enableZoom={true} />
```

**Features:**

* Rotation around object center with smooth inertia.
* Panning and zooming built-in.
* Keeps consistent camera orientation (no upside-down view).

**Use case:** CAD tools, 3D object manipulation, professional editors.

#### 🧩 Comparison Table

| Control Type                  | Orbit Around Target | Free Fly | FPS-like | Pan | Zoom | Lock Mouse | Common Use            |
| ----------------------------- | ------------------- | -------- | -------- | --- | ---- | ---------- | --------------------- |
| **OrbitControls**       | ✅ Yes              | ❌       | ❌       | ✅  | ✅   | ❌         | Model viewers         |
| **TrackballControls**   | ❌                  | ✅       | ❌       | ✅  | ✅   | ❌         | CAD tools             |
| **FlyControls**         | ❌                  | ✅       | ❌       | ✅  | ✅   | ❌         | Flight/free movement  |
| **FirstPersonControls** | ❌                  | ✅       | ✅       | ❌  | ❌   | ❌         | FPS-style walkthrough |
| **PointerLockControls** | ❌                  | ✅       | ✅       | ❌  | ❌   | ✅         | Games/simulators      |
| **MapControls**         | ✅ (top-down)       | ❌       | ❌       | ✅  | ✅   | ❌         | 2D maps               |
| **ArcballControls**     | ✅ (with inertia)   | ❌       | ❌       | ✅  | ✅   | ❌         | CAD/editors           |

#### ⚙️ Common Tips

* Always use **only one control** at a time (multiple will conflict).
* Attach the control directly under your `<Canvas>` so it has access to the same camera.
* You can dynamically **enable/disable** controls using `enabled` prop.

---

# ----Colors

### 🌈 1️⃣ How Colors Work in R3F

R3F directly maps to **Three.js’s color system** via `THREE.Color`.

That means every color you define in JSX is automatically converted to a `THREE.Color` object under the hood.

You can provide color values in  **several formats** :

```jsx
color="red"             // named color
color="#ff0000"         // hex string
color={0xff0000}        // hex number
color="rgb(255, 0, 0)"  // rgb string
color="hsl(0, 100%, 50%)" // hsl string
color={[1, 0, 0]}       // normalized RGB array (1 = 255)
```

All of these end up as an internal `THREE.Color` instance with normalized values (0–1).

### 🎨 2️⃣ Example — Using Colors in Materials

```jsx
<mesh>
  <boxGeometry />
  <meshStandardMaterial color="hotpink" />
</mesh>
```

* Here `"hotpink"` is converted into `new THREE.Color("hotpink")`.
* R3F automatically detects that `color` prop maps to a Three.js material property and updates it.

### 💡 3️⃣ Colors in Lights

Lights also accept color props:

```jsx
<directionalLight color="white" intensity={1.5} />
<pointLight color="#00ffff" intensity={2} position={[2, 2, 2]} />
```

This works the same way — R3F automatically wraps your color in `THREE.Color`.

### 🌌 4️⃣ Colors in Backgrounds and Fog

**Background:**

```jsx
<color attach="background" args={['#202020']} />
```

* `attach="background"` tells R3F to assign this color to `scene.background`.
* `args` creates a new color instance: `new THREE.Color('#202020')`.

**Fog:**

```jsx
<fog attach="fog" args={['#999999', 1, 10]} />
```

* `args[0]` is fog color.
* Other values are near and far distances.

### 🧠 5️⃣ Colors in State or Props (Dynamic)

You can dynamically change colors in React style:

```jsx
function Box({ color }) {
  return (
    <mesh>
      <boxGeometry />
      <meshStandardMaterial color={color} />
    </mesh>
  )
}

// Usage:
<Box color={hovered ? 'orange' : 'skyblue'} />
```

Colors will automatically update when React re-renders — no need for manual Three.js updates.

### ⚙️ 6️⃣ Procedural / Programmatic Colors

You can also create and manipulate colors in JS:

```jsx
import { Color } from 'three'

const myColor = new Color()
myColor.setRGB(0.2, 0.5, 0.8)
myColor.offsetHSL(0.1, 0.2, 0.0) // shift hue/saturation/lightness
```

This is useful for gradual transitions or random generation:

```jsx
const color = new Color().setHSL(Math.random(), 1, 0.5)
```

You can use that directly in R3F too:

```jsx
<meshStandardMaterial color={color} />
```

### ✨ 7️⃣ Animated Colors with `useFrame()`

```jsx
function AnimatedBox() {
  const ref = useRef()

  useFrame(({ clock }) => {
    const t = clock.getElapsedTime()
    ref.current.material.color.setHSL((Math.sin(t) + 1) / 2, 1, 0.5)
  })

  return (
    <mesh ref={ref}>
      <boxGeometry />
      <meshStandardMaterial color="white" />
    </mesh>
  )
}
```

This smoothly cycles the color through hues over time.

### 🧩 8️⃣ Using Drei’s `<Environment>` and Lighting with Colors

Sometimes you combine color materials with environment lights:

```jsx
<mesh>
  <sphereGeometry />
  <meshStandardMaterial color="#ff0055" roughness={0.4} metalness={0.8} />
</mesh>
<ambientLight intensity={0.2} color="#ffffff" />
<directionalLight intensity={1} color="#ffaa00" position={[5, 5, 5]} />
```

The interaction between **light color** and **material color** defines the final visible result.

### 📘 9️⃣ Color Spaces in R3F (Important for Realism)

Three.js (and R3F) now use **Linear color space** internally and **sRGB encoding** for display.

If you’re loading textures or setting emissive colors:

* Always ensure your color management matches (most often R3F handles this automatically).
* You can configure it with `<Canvas colorManagement />` (default true).

### 🪄 10️⃣ Summary Table

| Where Used              | JSX Example                                                   | Notes                  |
| ----------------------- | ------------------------------------------------------------- | ---------------------- |
| **Material**      | `<meshStandardMaterial color="#ff0000" />`                  | Surface color          |
| **Light**         | `<pointLight color="white" />`                              | Light hue              |
| **Background**    | `<color attach="background" args={['#111']} />`             | Scene background       |
| **Fog**           | `<fog attach="fog" args={['#999', 1, 10]} />`               | Fog color              |
| **Dynamic state** | `<meshStandardMaterial color={hovered ? 'red' : 'blue'} />` | React reactive         |
| **Procedural**    | `.setHSL()`or `.setRGB()`                                 | For smooth transitions |

---

# ----Fog

### ⚙️ Fog in Three.js

In Three.js, fog is applied to the  **scene** , not to individual objects.

```js
scene.fog = new THREE.Fog(color, near, far)
```

or

```js
scene.fog = new THREE.FogExp2(color, density)
```

There are  **two main types of fog** :

| Type              | Description                                              | Syntax                                |
| ----------------- | -------------------------------------------------------- | ------------------------------------- |
| `THREE.Fog`     | Linear fog – fades between `near`and `far`distances | `new THREE.Fog(color, near, far)`   |
| `THREE.FogExp2` | Exponential fog – fades exponentially based on distance | `new THREE.FogExp2(color, density)` |

### 🧩 Fog in React Three Fiber (R3F)

In R3F, everything is  **declarative** , so you don’t manually assign `scene.fog` — you **add fog as a JSX element inside your `<Canvas>`** or `<scene>`.

**✅ Example (Linear Fog)**

```jsx
import { Canvas } from '@react-three/fiber'
import { Box } from '@react-three/drei'

export default function FogExample() {
  return (
    <Canvas>
      {/* Add fog directly to the scene */}
      <fog attach="fog" args={['#aaaaaa', 5, 20]} />

      <ambientLight intensity={0.5} />
      <Box position={[0, 0, -5]}>
        <meshStandardMaterial color="orange" />
      </Box>
      <Box position={[0, 0, -15]}>
        <meshStandardMaterial color="red" />
      </Box>
      <Box position={[0, 0, -25]}>
        <meshStandardMaterial color="purple" />
      </Box>
    </Canvas>
  )
}
```

**🧩 Example (Exponential Fog)**

```jsx
<fogExp2 attach="fog" args={['#ffffff', 0.05]} />
```

**🧠 Breakdown**

| Prop                          | Meaning                                                                                      |
| ----------------------------- | -------------------------------------------------------------------------------------------- |
| `attach="fog"`              | Tells R3F to attach this fog instance to the `scene.fog`property                           |
| `args={[color, near, far]}` | The arguments passed to the fog constructor (similar to `new THREE.Fog(color, near, far)`) |
| `args={[color, density]}`   | For exponential fog (`THREE.FogExp2`)                                                      |

### 🎨 Effect on Materials

Fog interacts  **only with materials that support fog** , such as:

✅ Supports fog:

* `MeshStandardMaterial`
* `MeshLambertMaterial`
* `MeshPhongMaterial`
* `MeshBasicMaterial` (optional, `fog: true`)

❌ Doesn’t support fog:

* `MeshDepthMaterial`
* `MeshNormalMaterial`
* Custom shaders (unless fog is added manually)

If a material supports fog, it automatically uses the scene’s fog unless disabled with:

```jsx
<meshStandardMaterial fog={false} />
```

### 💡 Dynamic Fog Example (Changing Fog Values)

You can even **animate fog** over time:

```jsx
import { Canvas, useFrame, useThree } from '@react-three/fiber'

function AnimatedFog() {
  const { scene } = useThree()

  useFrame((state, delta) => {
    // Animate fog near/far over time
    scene.fog.near = 5 + Math.sin(state.clock.elapsedTime) * 2
    scene.fog.far = 20 + Math.sin(state.clock.elapsedTime) * 2
  })
}

export default function App() {
  return (
    <Canvas>
      <fog attach="fog" args={['#cccccc', 5, 20]} />
      <AnimatedFog />

      <ambientLight />
      <mesh position={[0, 0, -10]}>
        <boxGeometry args={[2, 2, 2]} />
        <meshStandardMaterial color="skyblue" />
      </mesh>
    </Canvas>
  )
}
```

### 🧮 Fog vs FogExp2 – Visual Difference

| Feature     | Fog (Linear)                       | FogExp2 (Exponential)                    |
| ----------- | ---------------------------------- | ---------------------------------------- |
| Fade Type   | Linear between `near`and `far` | Exponential, depends on density          |
| Parameters  | `color, near, far`               | `color, density`                       |
| Appearance  | Smooth linear transition           | More “natural” and denser at distance  |
| Performance | Slightly faster                    | Slightly more expensive (but negligible) |

### 🧠 Common Mistakes

❌ **Putting fog outside `<Canvas>`**

```jsx
// ❌ This won’t work
<fog args={['white', 1, 10]} />
```

✅ **Must be inside `<Canvas>` (scene context)**

```jsx
<Canvas>
  <fog attach="fog" args={['white', 1, 10]} />
</Canvas>
```

❌ **Using color strings incorrectly**

```jsx
<fog attach="fog" args={['white', 1, 10]} /> // ✅ OK
<fog attach="fog" args={[0xffffff, 1, 10]} /> // ✅ OK
<fog attach="fog" args={['#fff', 1, 10]} /> // ✅ OK
<fog attach="fog" args={[white, 1, 10]} /> // ❌ Error
```

### 🔍 Summary

| Concept            | Description                                          |
| ------------------ | ---------------------------------------------------- |
| `Fog`            | Linear fade from near → far                         |
| `FogExp2`        | Exponential fade with density                        |
| `attach="fog"`   | Attaches fog to scene                                |
| `args`           | Constructor parameters                               |
| Works with         | `MeshStandardMaterial`,`MeshPhongMaterial`, etc. |
| Doesn’t work with | `MeshNormalMaterial`,`MeshDepthMaterial`         |
| Animatable         | Yes (change near/far/density dynamically)            |

---

# ----Materials

### 🧩 2. Basic Structure

In Three.js:

```js
const material = new THREE.MeshStandardMaterial({ color: 'red' })
const mesh = new THREE.Mesh(geometry, material)
```

In R3F (Declarative version):

```jsx
<mesh>
  <boxGeometry args={[1, 1, 1]} />
  <meshStandardMaterial color="red" />
</mesh>
```

Everything inside `<mesh>` automatically connects — the geometry defines shape, the material defines appearance.

### 🎨 3. Types of Materials (and When to Use Them)

Let’s go through all major materials used in R3F.

##### 🟥 **1. `meshBasicMaterial`**

**Unlit material** — not affected by lights.

Good for UI-like graphics, skyboxes, or glowing effects.

```jsx
<meshBasicMaterial color="hotpink" wireframe />
```

**Key props:**

* `color` – base color
* `wireframe` – renders edges only
* `map` – texture
* `transparent`, `opacity`

##### 💡 **2. `meshLambertMaterial`**

Simulates **diffuse lighting** — soft, matte appearance.

Uses the Lambertian reflection model (cheap & simple).

```jsx
<meshLambertMaterial color="orange" />
```

**Key props:**

* `color`
* `emissive`
* `map`, `normalMap`, `aoMap`
* `fog`

**Lighting:** reacts to light but no specular highlight (no shiny spots).

##### ✨ **3. `meshPhongMaterial`**

Adds **specular highlights** for shiny surfaces like metal or plastic.

```jsx
<meshPhongMaterial color="skyblue" shininess={80} specular="white" />
```

**Key props:**

* `color` – diffuse color
* `specular` – highlight color
* `shininess` – size/intensity of highlight
* `map`, `bumpMap`, `normalMap`, `envMap`

**Lighting:** reacts to light (diffuse + specular).

##### 🧊 **4. `meshStandardMaterial` (Most Common)**

Physically Based Rendering ( **PBR** ) material.

Balances realism and performance.

```jsx
<meshStandardMaterial color="gold" metalness={0.8} roughness={0.3} />
```

**Key props:**

* `color`
* `metalness` – how metallic the surface is (0–1)
* `roughness` – how smooth/rough the surface is (0–1)
* `map`, `normalMap`, `roughnessMap`, `metalnessMap`, `aoMap`, `envMap`
* `fog`, `transparent`, `opacity`

**Lighting:** realistic physically based reflections.

##### 💎 **5. `meshPhysicalMaterial`**

An extension of `meshStandardMaterial` with more physical realism —

used for  **glass, skin, plastics, and car paint** .

```jsx
<meshPhysicalMaterial
  color="white"
  roughness={0}
  metalness={0}
  clearcoat={1}
  transmission={0.9}
  ior={1.5}
/>
```

**Extra props:**

* `clearcoat`, `clearcoatRoughness`
* `transmission` – transparency (instead of opacity)
* `ior` – index of refraction
* `thickness` – controls subsurface look
* `specularIntensity`, `specularColor`

**Lighting:** best for glass, water, or glossy surfaces.

##### 🌈 **6. `meshToonMaterial`**

Gives a **cartoon-style** look (flat color shading).

```jsx
<meshToonMaterial color="orange" gradientMap={THREE.BasicShadowMap} />
```

**Key props:**

* `color`
* `gradientMap` – controls shading steps

##### 🪞 **7. `meshMatcapMaterial`**

Uses a pre-baked matcap texture to simulate lighting without actual lights.

Perfect for **stylized or metallic look** with high performance.

```jsx
<meshMatcapMaterial matcap={texture} />
```

**Key props:**

* `matcap` (MatCap texture)
* `color`
* `map`

No need for lights — the texture carries the lighting info.

##### 🔳 **8. `meshDepthMaterial`**

Visualizes the depth of objects (distance from camera).

```jsx
<meshDepthMaterial />
```

**Key props:**

* `wireframe`
* `morphTargets`
* Used internally in shadows/post-processing.

##### 🧱 **9. `pointsMaterial`**

Used for rendering particles or point clouds.

```jsx
<pointsMaterial color="white" size={0.05} />
```

**Key props:**

* `size`, `sizeAttenuation`
* `map`, `alphaMap`
* `transparent`

##### 🧩 **10. `shaderMaterial` / `rawShaderMaterial`**

For **custom GLSL shaders** (advanced).

Let you control the vertex and fragment shading pipeline directly.

```jsx
<shaderMaterial
  vertexShader={vertex}
  fragmentShader={fragment}
  uniforms={{ uTime: { value: 0 } }}
/>
```

**Difference:**

* `ShaderMaterial` → adds built-in GLSL chunks (fog, lights)
* `RawShaderMaterial` → gives full control (no built-in GLSL)

### ⚙️ 4. Common Props Across Materials

| Property                          | Description                                                         |
| --------------------------------- | ------------------------------------------------------------------- |
| `color`                         | Base surface color                                                  |
| `map`                           | Texture applied to color                                            |
| `normalMap`                     | Adds surface detail                                                 |
| `roughnessMap`/`metalnessMap` | Used in PBR                                                         |
| `envMap`                        | Environment reflection                                              |
| `emissive`/`emissiveMap`      | Glow effect                                                         |
| `opacity`/`transparent`       | Transparency                                                        |
| `side`                          | Which sides are visible (`FrontSide`,`BackSide`,`DoubleSide`) |
| `fog`                           | Whether fog affects it                                              |

### 🧮 5. Multiple Materials Example

```jsx
<mesh>
  <boxGeometry args={[1, 1, 1]} />
  <meshStandardMaterial attach="material-0" color="red" />
  <meshStandardMaterial attach="material-1" color="green" />
  <meshStandardMaterial attach="material-2" color="blue" />
</mesh>
```

Each face of the geometry can have a different material.

##### 💡 6. Dynamic Material Updates

You can change materials reactively using React state:

```jsx
function DynamicMaterial() {
  const [color, setColor] = useState('orange')

  return (
    <mesh onClick={() => setColor('hotpink')}>
      <sphereGeometry args={[1, 32, 32]} />
      <meshStandardMaterial color={color} />
    </mesh>
  )
}
```

### ⚡ 7. Performance Tip

* `meshBasicMaterial` is fastest (no lighting)
* `meshStandardMaterial` → balanced, physically accurate
* `meshPhysicalMaterial` → realistic but heavier
* Avoid multiple lights + heavy materials on low devices

### 🧠 Summary Table

| Material                 | Lighting              | Use Case             | Realism   | Performance   |
| ------------------------ | --------------------- | -------------------- | --------- | ------------- |
| `meshBasicMaterial`    | ❌ No                 | UI, background       | Low       | 🔋 Fastest    |
| `meshLambertMaterial`  | ✅ Diffuse            | Matte surfaces       | Medium    | ⚡ Fast       |
| `meshPhongMaterial`    | ✅ Diffuse + Specular | Shiny plastic, metal | Medium+   | ⚙️ Moderate |
| `meshStandardMaterial` | ✅ PBR                | Realistic materials  | High      | ⚖️ Balanced |
| `meshPhysicalMaterial` | ✅ Advanced PBR       | Glass, skin, liquids | Very High | 🧮 Heavy      |
| `meshToonMaterial`     | ✅ Stylized           | Cartoon look         | Stylized  | ⚡ Fast       |
| `meshMatcapMaterial`   | ❌ No                 | Stylized metallic    | Medium    | ⚡ Fast       |
| `meshDepthMaterial`    | N/A                   | Depth visualization  | N/A       | Fast          |
| `shaderMaterial`       | Custom                | Custom effects       | Infinite  | Depends       |

---

# ----Textures

### 🧩 How Textures Work

A texture maps onto a surface through **UV coordinates** — a 2D mapping that tells which part of the image goes to which vertex of the mesh.

```
U → horizontal axis (X on image)
V → vertical axis (Y on image)
```

So, the 3D model has “UVs” that describe how to wrap the image around it.

### ⚙️ Loading a Texture in R3F

Simplest example:

```jsx
import { useTexture } from '@react-three/drei'

function TexturedBox() {
  const colorMap = useTexture('/textures/wood.jpg')

  return (
    <mesh>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial map={colorMap} />
    </mesh>
  )
}
```

✅ This applies a wood image to all six faces of the cube.

⚠️ The `map` prop corresponds to the **color map** (the base texture).

### 🧠 Common Types of Texture Maps

Each texture type changes how light interacts with the surface.

| Map                                  | Property            | Description                                                               |
| ------------------------------------ | ------------------- | ------------------------------------------------------------------------- |
| **Color / Albedo Map**         | `map`             | The base color image.                                                     |
| **Normal Map**                 | `normalMap`       | Simulates bumps using RGB vectors (no actual geometry change).            |
| **Bump Map**                   | `bumpMap`         | Creates fake depth using grayscale values (older method).                 |
| **Roughness Map**              | `roughnessMap`    | Controls surface roughness — white = rough, black = smooth.              |
| **Metalness Map**              | `metalnessMap`    | Controls how metallic the surface is — white = metal, black = non-metal. |
| **AO (Ambient Occlusion) Map** | `aoMap`           | Adds shadows in crevices for realism (needs secondary UV set,`uv2`).    |
| **Displacement Map**           | `displacementMap` | Actually modifies vertex positions — adds true depth.                    |
| **Alpha Map**                  | `alphaMap`        | Controls transparency (white = opaque, black = transparent).              |
| **Emissive Map**               | `emissiveMap`     | Makes parts of the surface glow (ignores lighting).                       |
| **Environment Map**            | `envMap`          | Adds reflection or refraction from environment.                           |

### 🧊 Example with Multiple Maps

```jsx
import { useTexture } from '@react-three/drei'

function Rock() {
  const textures = useTexture({
    map: '/textures/rock/color.jpg',
    normalMap: '/textures/rock/normal.jpg',
    roughnessMap: '/textures/rock/roughness.jpg',
    aoMap: '/textures/rock/ao.jpg',
  })

  return (
    <mesh>
      <sphereGeometry args={[1, 64, 64]} />
      <meshStandardMaterial {...textures} />
    </mesh>
  )
}
```

`useTexture` can take an object with multiple map names that directly match the props of the material.

Super clean! ✅

### 🧭 Texture Properties

Each texture object (like `colorMap`, `normalMap`) has important **properties** to control how it repeats, wraps, rotates, or offsets.

##### 🔁 `repeat`

Defines how many times the texture repeats on the surface.

```jsx
texture.repeat.set(2, 2)
```

🧩 Default = `(1,1)`

Doubling these values tiles the image.

##### 🧱 `wrapS` and `wrapT`

Define how the texture behaves when repeating.

| Mode                             | Description                                    |
| -------------------------------- | ---------------------------------------------- |
| `THREE.ClampToEdgeWrapping`    | Stretches edge pixels (default).               |
| `THREE.RepeatWrapping`         | Repeats the texture infinitely.                |
| `THREE.MirroredRepeatWrapping` | Alternates between normal and mirrored images. |

```jsx
texture.wrapS = texture.wrapT = THREE.RepeatWrapping
```

##### 📐 `offset`

Moves (scrolls) the texture around the surface.

```jsx
texture.offset.set(0.5, 0)
```

Shifts the texture 50% to the right.

##### 🔄 `rotation`

Rotates the texture around its center.

```jsx
texture.rotation = Math.PI / 4
texture.center.set(0.5, 0.5) // Set pivot point
```

### 🌍 Environment Maps

Environment maps simulate **reflection** or **refraction** of surroundings (like mirrors, chrome, or glass).

You can easily use them in R3F with Drei’s helper:

```jsx
import { Environment } from '@react-three/drei'

<Environment preset="sunset" background />
```

Or manually:

```jsx
const envMap = useTexture([
  '/px.jpg', '/nx.jpg',
  '/py.jpg', '/ny.jpg',
  '/pz.jpg', '/nz.jpg'
])
```

Then apply:

```jsx
<meshStandardMaterial envMap={envMap} metalness={1} roughness={0.2} />
```

### 💡 AO Map and `uv2`

Ambient Occlusion (AO) maps need a *secondary UV set* called `uv2`.

Many built-in geometries don’t have `uv2`, so you must manually copy it:

```jsx
geometry.setAttribute('uv2', new THREE.BufferAttribute(geometry.attributes.uv.array, 2))
```

This allows your AO map to darken crevices properly.

Excellent — this is an important detail when working with **AO maps (ambient occlusion maps)** in **React Three Fiber (R3F)** because AO maps require a *secondary UV channel* (`uv2`).

Let’s go step-by-step 👇

##### ⚙️ How to do it in **React Three Fiber (R3F)**

Since R3F is declarative and JSX-based, you usually work with components like `<mesh>` and `<boxGeometry />`.

But you can still access and modify the underlying geometry in React.

Here’s how 👇

**✅ Example : Using a ref**

```jsx
import * as THREE from 'three'
import { useRef, useEffect } from 'react'
import { Canvas, useLoader } from '@react-three/fiber'

function BoxWithAO() {
  const meshRef = useRef()
  const aoMap = useLoader(THREE.TextureLoader, '/textures/ao.jpg')

  useEffect(() => {
    const geometry = meshRef.current.geometry
    // Duplicate the UVs to uv2
    geometry.setAttribute(
      'uv2',
      new THREE.BufferAttribute(geometry.attributes.uv.array, 2)
    )
  }, [])

  return (
    <mesh ref={meshRef}>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial
        color="white"
        aoMap={aoMap}
        aoMapIntensity={1}
      />
    </mesh>
  )
}

export default function App() {
  return (
    <Canvas>
      <ambientLight />
      <BoxWithAO />
    </Canvas>
  )
}
```

✅ **Explanation**

* The `ref` gives access to the underlying mesh and geometry.
* In `useEffect()`, we copy `uv → uv2` just like in Three.js.
* Now AO maps will render correctly.

### 🧠 Example: Full Textured Object

```jsx
import { useTexture } from '@react-three/drei'
import * as THREE from 'three'

function BrickWall() {
  const tex = useTexture({
    map: '/textures/brick/color.jpg',
    normalMap: '/textures/brick/normal.jpg',
    roughnessMap: '/textures/brick/roughness.jpg',
    aoMap: '/textures/brick/ao.jpg'
  })

  // repeat the texture
  Object.values(tex).forEach((t) => {
    t.wrapS = t.wrapT = THREE.RepeatWrapping
    t.repeat.set(4, 4)
  })

  return (
    <mesh>
      <boxGeometry args={[3, 3, 3]} />
      <meshStandardMaterial {...tex} />
    </mesh>
  )
}
```

✅ Realistic material

✅ Uses multiple texture maps

✅ Uses repeat wrapping for detail

> 🧠 What `tex` in the above example is
>
> In React Three Fiber (and Three.js), you often load multiple textures at once:
>
> ```js
> const tex = useLoader(TextureLoader, {
>   map: '/color.jpg',
>   normalMap: '/normal.jpg',
>   roughnessMap: '/rough.jpg',
> });
> ```
>
> Here, `tex` is an **object** like:
>
> ```js
> {
>   map: Texture,
>   normalMap: Texture,
>   roughnessMap: Texture
> }
> ```
>
> So the **values** of `tex` are the actual `THREE.Texture` objects.
>
> ##### ✅ Why `Object.values(tex)` is correct
>
> * `Object.values(tex)` → `[Texture, Texture, Texture]`
>
>   → Gives you direct access to each texture object.
> * Then `forEach` lets you modify their properties:
>
>   ```js
>   t.wrapS = t.wrapT = THREE.RepeatWrapping;
>   t.repeat.set(4, 4);
>   ```
>
> So this is the right and **most direct** way to loop through all textures.
>
> ##### ❌ Why not `Object.keys(tex)`
>
> `Object.keys(tex)` → `['map', 'normalMap', 'roughnessMap']`
>
> These are just **strings** — the property names.
>
> You’d have to do one more lookup:
>
> ```js
> Object.keys(tex).forEach((key) => {
>   const t = tex[key];
>   t.wrapS = t.wrapT = THREE.RepeatWrapping;
>   t.repeat.set(4, 4);
> });
> ```
>
> That also works — but it’s more verbose.
>
> So `Object.values()` is just the clean version.

### 📚 Texture Management with `useTexture()`

`useTexture` from **@react-three/drei** supports:

* Lazy loading
* Caching
* Automatic disposal
* Arrays or object mapping
* Suspense for loading states

Examples:

```jsx
// Single texture
const map = useTexture('/image.jpg')

// Multiple (array)
const [map, normalMap] = useTexture(['/color.jpg', '/normal.jpg'])

// Named (object)
const textures = useTexture({
  map: '/color.jpg',
  normalMap: '/normal.jpg'
})
```

### 🧮 Performance Tips

* Compress textures (use `.jpg`, `.ktx2`, `.dds`).
* Keep resolution reasonable (1K–2K for most).
* Use mipmaps for smoother scaling.
* Reuse texture instances (avoid reloading).

### 🧠 Summary Table

| Texture Map         | Controls          | Needs Lights? | Needs UV2? |
| ------------------- | ----------------- | ------------- | ---------- |
| `map`             | Base color        | ✅            | ❌         |
| `normalMap`       | Bumps             | ✅            | ❌         |
| `bumpMap`         | Height illusion   | ✅            | ❌         |
| `roughnessMap`    | Smoothness        | ✅            | ❌         |
| `metalnessMap`    | Metallic behavior | ✅            | ❌         |
| `aoMap`           | Ambient shadows   | ✅            | ✅         |
| `displacementMap` | Real vertex bumps | ✅            | ❌         |
| `alphaMap`        | Transparency      | ❌            | ❌         |
| `emissiveMap`     | Glow              | ❌            | ❌         |
| `envMap`          | Reflection        | ✅            | ❌         |

---

# ----Line, LineSegments, LineLoops and Line Materials and its types

✅ Example in **R3F**

```jsx
import * as THREE from 'three'
import { Canvas } from '@react-three/fiber'

function BasicLine() {
  const points = [
    new THREE.Vector3(-2, 0, 0),
    new THREE.Vector3(0, 2, 0),
    new THREE.Vector3(2, 0, 0),
  ]
  const geometry = new THREE.BufferGeometry().setFromPoints(points)

  return (
    <line geometry={geometry}>
      <lineBasicMaterial color="orange" linewidth={2} />
    </line>
  )
}

export default function App() {
  return (
    <Canvas>
      <BasicLine />
    </Canvas>
  )
}
```

🧠 **Explanation**

* `<line>` corresponds to `THREE.Line`
* The geometry defines the points in space.
* The material sets color and thickness.

### ⚙️ `LineSegments`

**Purpose:**

Used to draw *individual disconnected lines* (pairs of points).

For instance, useful for  **wireframes** ,  **grids** , or  **bounding boxes** .

**✅ Example in R3F**

```jsx
import * as THREE from 'three'
import { Canvas } from '@react-three/fiber'

function Segments() {
  const points = [
    new THREE.Vector3(-2, 0, 0),
    new THREE.Vector3(-1, 1, 0),
    new THREE.Vector3(1, -1, 0),
    new THREE.Vector3(2, 0, 0)
  ]
  const geometry = new THREE.BufferGeometry().setFromPoints(points)

  return (
    <lineSegments geometry={geometry}>
      <lineBasicMaterial color="cyan" />
    </lineSegments>
  )
}

export default function App() {
  return (
    <Canvas>
      <Segments />
    </Canvas>
  )
}
```

🧩 This draws:

* Line 1: between point 1 → point 2
* Line 2: between point 3 → point 4

  (because it takes every pair of points as one segment)

### ⚙️ `LineDashedMaterial`

**Purpose:**

Creates **dashed** or **dotted** lines.

Works **only with** `THREE.Line` (not with meshes or segments directly) and  **requires computing line distances** .

**✨ Key Properties**

| Property      | Description                         |
| ------------- | ----------------------------------- |
| `color`     | Line color                          |
| `linewidth` | Thickness (limited browser support) |
| `scale`     | Scales dash pattern                 |
| `dashSize`  | Length of dashes                    |
| `gapSize`   | Length of gaps                      |
| `fog`       | Whether it reacts to fog            |

**✅ Example in R3F**

```jsx
import * as THREE from 'three'
import { Canvas, useFrame } from '@react-three/fiber'
import { useRef, useEffect } from 'react'

function DashedLine() {
  const ref = useRef()

  useEffect(() => {
    // Required for dashed lines
    ref.current.computeLineDistances()
  }, [])

  const points = [
    new THREE.Vector3(-2, 0, 0),
    new THREE.Vector3(2, 0, 0),
  ]
  const geometry = new THREE.BufferGeometry().setFromPoints(points)

  return (
    <line ref={ref} geometry={geometry}>
      <lineDashedMaterial
        color="hotpink"
        dashSize={0.3}
        gapSize={0.2}
        linewidth={2}
      />
    </line>
  )
}

export default function App() {
  return (
    <Canvas>
      <DashedLine />
    </Canvas>
  )
}
```

🧠 **Explanation**

* `computeLineDistances()` adds an internal attribute so the GPU knows where to alternate dash/gap.
* `dashSize` and `gapSize` define the pattern length.
* `scale` can stretch or compress the dash pattern.

### ⚡ Line Hierarchy and R3F Mapping

| Three.js Object                    | React Three Fiber Equivalent |
| ---------------------------------- | ---------------------------- |
| `new THREE.Line()`               | `<line>`                   |
| `new THREE.LineSegments()`       | `<lineSegments>`           |
| `new THREE.LineLoop()`           | `<lineLoop>`               |
| `new THREE.LineBasicMaterial()`  | `<lineBasicMaterial>`      |
| `new THREE.LineDashedMaterial()` | `<lineDashedMaterial>`     |

All of them can be nested under `<mesh>` or directly under `<Canvas>` just like other Three.js objects.

### 💡 Use Cases

| Use Case               | Example                            |
| ---------------------- | ---------------------------------- |
| Wireframes             | Use `LineSegments`               |
| Grids                  | Use `LineSegments`+ loops        |
| Paths or Curves        | Use `Line`                       |
| Dashed roads or guides | Use `LineDashedMaterial`         |
| Outlines or helpers    | Combine `Line`+ color highlights |

---

# ----Shape and ShapeGeometry

### 🧩 1. What is a Shape?

In  **Three.js** , a `THREE.Shape` is essentially a **2D outline** (a path made of lines or curves) that can be:

* **Filled** (via `ShapeGeometry`),
* **Extruded** (via `ExtrudeGeometry`),
* or **used as a hole/path** inside another shape.

It’s similar to drawing with the *pen tool* in Photoshop or Illustrator — you define vertices and curves, and Three.js fills it in.

In  **R3F** , the same logic applies — you just use `<shapeGeometry />` and `<extrudeGeometry />` inside JSX.

### 🧱 2. Core Classes and Mapping in R3F

| Three.js                                      | React Three Fiber                                     |
| --------------------------------------------- | ----------------------------------------------------- |
| `new THREE.Shape()`                         | `new THREE.Shape()`(created inside component logic) |
| `new THREE.ShapeGeometry(shape)`            | `<shapeGeometry args={[shape]} />`                  |
| `new THREE.ExtrudeGeometry(shape, options)` | `<extrudeGeometry args={[shape, options]} />`       |
| `new THREE.ShapePath()`                     | Usually built via code in useMemo() or function       |

### 🧮 3. Basic Shape Example (in R3F)

Let’s start with a simple custom shape — a  **heart** .

```jsx
import * as THREE from 'three'
import { Canvas } from '@react-three/fiber'
import { useMemo } from 'react'

function HeartShape() {
  const shape = useMemo(() => {
    const heart = new THREE.Shape()
    heart.moveTo(0, 0)
    heart.bezierCurveTo(0, 1, -1.5, 1.5, -1.5, 0)
    heart.bezierCurveTo(-1.5, -1.5, 0, -1.5, 0, -3)
    heart.bezierCurveTo(0, -1.5, 1.5, -1.5, 1.5, 0)
    heart.bezierCurveTo(1.5, 1.5, 0, 1, 0, 0)
    return heart
  }, [])

  return (
    <mesh>
      <shapeGeometry args={[shape]} />
      <meshStandardMaterial color="hotpink" />
    </mesh>
  )
}

export default function App() {
  return (
    <Canvas camera={{ position: [0, 0, 5] }}>
      <ambientLight intensity={0.3} />
      <directionalLight position={[2, 2, 5]} />
      <HeartShape />
    </Canvas>
  )
}
```

**🧠 What’s Happening:**

* We use a **Shape()** to define a 2D outline.
* **moveTo()** sets the start point.
* **bezierCurveTo()** creates smooth curves.
* `<shapeGeometry args={[shape]} />` turns it into a renderable geometry.
* It’s rendered as a flat, filled polygon (like a 2D heart).

### 🧩 4. Extruded Shape Example (3D Shape from 2D Path)

To give a  **3D look** , use `ExtrudeGeometry`.

```jsx
function ExtrudedHeart() {
  const shape = useMemo(() => {
    const heart = new THREE.Shape()
    heart.moveTo(0, 0)
    heart.bezierCurveTo(0, 1, -1.5, 1.5, -1.5, 0)
    heart.bezierCurveTo(-1.5, -1.5, 0, -1.5, 0, -3)
    heart.bezierCurveTo(0, -1.5, 1.5, -1.5, 1.5, 0)
    heart.bezierCurveTo(1.5, 1.5, 0, 1, 0, 0)
    return heart
  }, [])

  const extrudeSettings = useMemo(() => ({
    steps: 2,
    depth: 0.5,
    bevelEnabled: true,
    bevelThickness: 0.1,
    bevelSize: 0.05,
    bevelSegments: 2
  }), [])

  return (
    <mesh>
      <extrudeGeometry args={[shape, extrudeSettings]} />
      <meshStandardMaterial color="tomato" />
    </mesh>
  )
}
```

**✨ Explanation:**

* `depth`: how far it extrudes along z-axis.
* `steps`: number of divisions along depth.
* `bevelEnabled`: whether to bevel (smooth) the edges.
* `bevelThickness`, `bevelSize`, `bevelSegments`: control edge roundness.

### 🔹 5. Shape Methods (used to draw)

| Method                                                     | Description                       |
| ---------------------------------------------------------- | --------------------------------- |
| `.moveTo(x, y)`                                          | Move pen to a start point         |
| `.lineTo(x, y)`                                          | Draw straight line to point       |
| `.quadraticCurveTo(cpX, cpY, x, y)`                      | Quadratic curve (1 control point) |
| `.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`           | Cubic curve (2 control points)    |
| `.absarc(x, y, radius, startAngle, endAngle, clockwise)` | Circular arc                      |
| `.absellipse()`                                          | Elliptical arc                    |
| `.holes.push(otherShape)`                                | Cut a hole using another shape    |

### 💠 6. Adding Holes (Cutouts)

You can make **holes** inside shapes (like a donut).

```jsx
function DonutShape() {
  const shape = useMemo(() => {
    const outer = new THREE.Shape()
    outer.absarc(0, 0, 1.5, 0, Math.PI * 2)

    const hole = new THREE.Path()
    hole.absarc(0, 0, 0.5, 0, Math.PI * 2)
    outer.holes.push(hole)

    return outer
  }, [])

  return (
    <mesh>
      <shapeGeometry args={[shape]} />
      <meshStandardMaterial color="orange" />
    </mesh>
  )
}
```

🧠 **Holes** are defined as separate paths (`THREE.Path`) pushed into the shape’s `.holes` array.

### 🧭 7. Common Shapes Built-In to Three.js

You can use **predefined shapes** instead of manual drawing:

| Shape          | R3F Geometry Component  |
| -------------- | ----------------------- |
| Plane          | `<planeGeometry />`   |
| Circle         | `<circleGeometry />`  |
| Ring           | `<ringGeometry />`    |
| Shape (custom) | `<shapeGeometry />`   |
| Extrude        | `<extrudeGeometry />` |

But `ShapeGeometry` is used for **custom** 2D outlines that can’t be achieved by the defaults.

### 🧠 8. Key Differences Between Shape and Geometry

| Feature        | `ShapeGeometry` | `ExtrudeGeometry`        |
| -------------- | ----------------- | -------------------------- |
| Dimensionality | 2D flat           | 3D solid                   |
| Bevel          | ❌                | ✅                         |
| Depth          | ❌                | ✅                         |
| Holes          | ✅                | ✅                         |
| Use Cases      | Logos, flat icons | 3D lettering, molds, signs |

### 🧩 9. Example: Logo or Text Extrusion

You can even create **3D text or logos** using shapes extracted from SVG paths.

```jsx
import { SVGLoader } from 'three/examples/jsm/loaders/SVGLoader'

function SVGShape({ url }) {
  const [shapes, setShapes] = useState([])

  useEffect(() => {
    new SVGLoader().load(url, (data) => {
      const loadedShapes = []
      data.paths.forEach((path) => {
        path.toShapes(true).forEach((s) => loadedShapes.push(s))
      })
      setShapes(loadedShapes)
    })
  }, [url])

  return (
    <>
      {shapes.map((shape, i) => (
        <mesh key={i}>
          <extrudeGeometry args={[shape, { depth: 0.3, bevelEnabled: false }]} />
          <meshStandardMaterial color="skyblue" />
        </mesh>
      ))}
    </>
  )
}
```

### 🔍 10. Summary

| Concept                                          | Description                                   |
| ------------------------------------------------ | --------------------------------------------- |
| `Shape()`                                      | Defines a 2D outline                          |
| `ShapeGeometry()`                              | Turns the outline into a flat filled mesh     |
| `ExtrudeGeometry()`                            | Adds 3D depth to the shape                    |
| `.holes`                                       | Adds holes/cutouts                            |
| `.moveTo()`,`.lineTo()`,`.bezierCurveTo()` | Used to draw                                  |
| `SVGLoader`                                    | Converts vector logos into shapes             |
| R3F JSX                                          | `<shapeGeometry />`,`<extrudeGeometry />` |

**✅ In short:**

* **`ShapeGeometry`** → flat logos, patterns, icons.
* **`ExtrudeGeometry`** → 3D letters, molds, signs.
* **Both** use the same `Shape` class.
* **R3F** makes it simpler to declaratively compose and render.

---

# ----EdgesGeometry and WireframeGeometry

### ⚙️ 1. What Are EdgesGeometry and WireframeGeometry?

Both are **derived geometries** in **Three.js** — they take an existing geometry (like `BoxGeometry`, `SphereGeometry`, etc.) and convert it into  **lines** .

| Type                  | What it draws                                                                                   | Visual effect               | Common use                     |
| --------------------- | ----------------------------------------------------------------------------------------------- | --------------------------- | ------------------------------ |
| `EdgesGeometry`     | Only the**outer edges**of faces<br /> (where the angle between faces exceeds a threshold) | Crisp, clean object outline | Outlines or stylized rendering |
| `WireframeGeometry` | **All edges of all faces**                                                                | Full mesh wireframe         | Debugging geometry structure   |

In  **React Three Fiber (R3F)** , both are used the same way you’d use any geometry — as `<edgesGeometry />` or `<wireframeGeometry />`.

### 🧱 2. EdgesGeometry — Highlight Visible Edges

`EdgesGeometry` draws only  **distinct edges** , ignoring internal triangles.

It helps show the silhouette of an object — think of it like a 3D “outline” mode.

**🧩 Syntax in R3F**

```jsx
<edgesGeometry args={[geometry, thresholdAngle]} />
```

* **geometry** → Base geometry (e.g., `new THREE.BoxGeometry()`).
* **thresholdAngle** *(optional)* → The minimum angle (in degrees) between face normals to be considered an edge (default is `1`).

**🔧 Example — Cube Outline**

```jsx
import * as THREE from 'three'
import { Canvas } from '@react-three/fiber'

function EdgesExample() {
  return (
    <mesh position={[-1.5, 0, 0]}>
      <boxGeometry args={[1, 1, 1]} />
      <meshBasicMaterial color="lightblue" />
      <lineSegments>
        <edgesGeometry args={[new THREE.BoxGeometry(1, 1, 1), 15]} />
        <lineBasicMaterial color="black" linewidth={1} />
      </lineSegments>
    </mesh>
  )
}

export default function App() {
  return (
    <Canvas camera={{ position: [3, 2, 5] }}>
      <ambientLight />
      <EdgesExample />
    </Canvas>
  )
}
```

✅ **Explanation:**

* `<edgesGeometry />` extracts edges.
* `<lineSegments>` is used to render line-based geometries.
* `thresholdAngle` of 15° means only sharp edges (like box corners) are visible.
* The inner diagonals are hidden — only **visible edges** remain.

### 🧩 3. WireframeGeometry — Full Mesh Wireframe

`WireframeGeometry` converts **every triangle edge** into a line — so you get a **grid-like view** of the geometry.

**🔧 Syntax in R3F**

```jsx
<wireframeGeometry args={[geometry]} />
```

No threshold angle — all edges are shown.

**🧠 Example — Sphere Wireframe**

```jsx
function WireframeExample() {
  return (
    <lineSegments position={[1.5, 0, 0]}>
      <wireframeGeometry args={[new THREE.SphereGeometry(1, 16, 12)]} />
      <lineBasicMaterial color="green" />
    </lineSegments>
  )
}
```

✅ **Explanation:**

* Every polygonal edge in the sphere mesh is rendered as a line.
* Great for debugging smoothness, mesh density, or subdivisions.

### 🧩 4. Side-by-Side Comparison Example

```jsx
import * as THREE from 'three'
import { Canvas } from '@react-three/fiber'
import { OrbitControls } from '@react-three/drei'

function EdgesVsWireframe() {
  return (
    <>
      {/* EdgesGeometry — clean outline */}
      <mesh position={[-1.5, 0, 0]}>
        <boxGeometry args={[1, 1, 1]} />
        <meshStandardMaterial color="#88ccee" />
        <lineSegments>
          <edgesGeometry args={[new THREE.BoxGeometry(1, 1, 1), 5]} />
          <lineBasicMaterial color="black" />
        </lineSegments>
      </mesh>

      {/* WireframeGeometry — full mesh lines */}
      <lineSegments position={[1.5, 0, 0]}>
        <wireframeGeometry args={[new THREE.SphereGeometry(1, 16, 12)]} />
        <lineBasicMaterial color="orange" />
      </lineSegments>
    </>
  )
}

export default function App() {
  return (
    <Canvas camera={{ position: [3, 2, 6] }}>
      <ambientLight />
      <directionalLight position={[5, 5, 5]} />
      <EdgesVsWireframe />
      <OrbitControls />
    </Canvas>
  )
}
```

### 📘 5. Summary Table

| Feature     | `EdgesGeometry`              | `WireframeGeometry` |
| ----------- | ------------------------------ | --------------------- |
| Shows       | Only sharp outer edges         | All edges of faces    |
| Purpose     | Stylized outline / silhouette  | Debug topology        |
| Performance | Faster                         | Slightly heavier      |
| Argument    | `(geometry, thresholdAngle)` | `(geometry)`        |
| Render With | `<lineSegments>`             | `<lineSegments>`    |
| Appearance  | Clean outline                  | Grid mesh             |

### 🧠 6. Practical Uses

✅ **EdgesGeometry:**

* Outline effect around models (like in CAD tools or stylized art).
* Combine with transparent mesh to show both filled + outline.

✅ **WireframeGeometry:**

* Debugging geometry subdivisions.
* Showing internal structure of models.
* Stylized visuals for holographic / tech effects.

### 🎨 7. Styling Tips

You can make it look cool by overlaying both mesh and edges:

```jsx
<mesh>
  <boxGeometry args={[1, 1, 1]} />
  <meshStandardMaterial color="#00aaff" opacity={0.5} transparent />
  <lineSegments>
    <edgesGeometry args={[new THREE.BoxGeometry(1, 1, 1)]} />
    <lineBasicMaterial color="white" linewidth={1.5} />
  </lineSegments>
</mesh>
```

This gives a  **transparent box with white outlines** , like a blueprint or hologram.

### 💡 Bonus Tip

If you need  **animated lines** , use:

* `LineMaterial` (from `three/examples/jsm/lines/LineMaterial`)
* or shader materials with dash effects.

### ✅ TL;DR

| Geometry                    | Description                       | JSX Example                                    |
| --------------------------- | --------------------------------- | ---------------------------------------------- |
| **EdgesGeometry**     | Draws only**sharp edges**   | `<edgesGeometry args={[geometry, angle]} />` |
| **WireframeGeometry** | Draws**all triangle edges** | `<wireframeGeometry args={[geometry]} />`    |
| **Renderer**          | Always use `<lineSegments>`     | `<lineSegments>…</lineSegments>`            |

---

# ----Helpers

### ⚙️ Using Helpers in R3F

In  **Three.js** , you would write:

```js
const light = new THREE.DirectionalLight(0xffffff);
const helper = new THREE.DirectionalLightHelper(light);
scene.add(helper);
```

But in  **React Three Fiber** , you don’t manually call `scene.add()`.

You use the **`<primitive />`** element or the **`useHelper()` hook** from `@react-three/drei`.

### ✅ Option 1: Using `<primitive>`

You can directly add Three.js helper objects into the JSX tree:

```jsx
import * as THREE from 'three'
import { Canvas, useThree } from '@react-three/fiber'

function Scene() {
  const dirLight = new THREE.DirectionalLight(0xffffff, 1)

  return (
    <>
      <primitive object={dirLight} position={[5, 5, 5]} />
      <primitive object={new THREE.DirectionalLightHelper(dirLight, 2)} />
    </>
  )
}

export default function App() {
  return (
    <Canvas>
      <Scene />
    </Canvas>
  )
}
```

⚠️ But this can get messy — which is why **`useHelper()`** is preferred.

### ✅ Option 2: Using `useHelper()` (from Drei)

`useHelper(ref, HelperType, ...args)` lets you easily attach helpers to existing objects.

```jsx
import { Canvas, useFrame } from '@react-three/fiber'
import { useHelper } from '@react-three/drei'
import * as THREE from 'three'
import { useRef } from 'react'

function Scene() {
  const lightRef = useRef()
  useHelper(lightRef, THREE.DirectionalLightHelper, 2, 'hotpink')

  return (
    <>
      <directionalLight ref={lightRef} position={[5, 5, 5]} intensity={1} />
      <mesh position={[0, 0, 0]}>
        <boxGeometry args={[1, 1, 1]} />
        <meshStandardMaterial color="royalblue" />
      </mesh>
    </>
  )
}

export default function App() {
  return (
    <Canvas>
      <Scene />
    </Canvas>
  )
}
```

🟢 Here:

* `useHelper` automatically adds and removes the helper from the scene.
* The helper updates automatically if the light moves.

### 📘 Common Helpers in R3F (from Three.js)

| Helper                                  | Description                               | Example Use                                              |
| --------------------------------------- | ----------------------------------------- | -------------------------------------------------------- |
| `AxesHelper(size)`                    | Shows X (red), Y (green), Z (blue) axes   | `useHelper(ref, THREE.AxesHelper, 5)`                  |
| `GridHelper(size, divisions)`         | Shows a ground grid                       | `<gridHelper args={[10, 10]} />`                       |
| `CameraHelper(camera)`                | Visualizes a camera frustum               | `useHelper(cameraRef, THREE.CameraHelper)`             |
| `DirectionalLightHelper(light, size)` | Shows direction & bounds of light         | `useHelper(lightRef, THREE.DirectionalLightHelper, 5)` |
| `PointLightHelper(light, size)`       | Shows a small sphere at light’s position | `useHelper(lightRef, THREE.PointLightHelper, 1)`       |
| `SpotLightHelper(light)`              | Visualizes spotlight cone                 | `useHelper(lightRef, THREE.SpotLightHelper)`           |
| `BoxHelper(object)`                   | Draws a wireframe box around the object   | `useHelper(meshRef, THREE.BoxHelper)`                  |
| `PlaneHelper(plane, size, color)`     | Visualizes a plane                        | `useHelper(planeRef, THREE.PlaneHelper, 5, 'green')`   |

### ⚡ Example: Multiple Helpers Together

```jsx
import { Canvas } from '@react-three/fiber'
import { useHelper } from '@react-three/drei'
import * as THREE from 'three'
import { useRef } from 'react'

function Scene() {
  const dirLight = useRef()
  const pointLight = useRef()
  const mesh = useRef()

  useHelper(dirLight, THREE.DirectionalLightHelper, 3)
  useHelper(pointLight, THREE.PointLightHelper, 1)
  useHelper(mesh, THREE.BoxHelper, 'orange')

  return (
    <>
      <directionalLight ref={dirLight} position={[5, 5, 5]} intensity={1} />
      <pointLight ref={pointLight} position={[-3, 3, 2]} intensity={2} color="hotpink" />
      <mesh ref={mesh} position={[0, 0, 0]}>
        <boxGeometry args={[1, 1, 1]} />
        <meshStandardMaterial color="skyblue" />
      </mesh>
      <gridHelper args={[10, 10]} />
      <axesHelper args={[5]} />
    </>
  )
}

export default function App() {
  return (
    <Canvas camera={{ position: [3, 3, 5] }}>
      <ambientLight />
      <Scene />
    </Canvas>
  )
}
```

### 💡 Key Points to Remember

* `useHelper()` works only inside the render loop — it automatically updates.
* Helpers don’t affect performance much, but remove them in production.
* You can toggle them on/off conditionally, e.g. `if (debugMode) useHelper(...)`
* They work with  **any Three.js object that extends `Object3D`** .

---

# ----Primitive

let’s go **deep** into `primitive` in  **React Three Fiber (R3F)** , because understanding it gives you full control over the bridge between **Three.js** and  **React** .

### 🧩 What is `<primitive>` in R3F?

In React Three Fiber, every JSX element like `<mesh>`, `<boxGeometry>`, `<directionalLight>`, etc. is just a **React wrapper** around a corresponding  **Three.js class** .

But sometimes, you need to use a **Three.js class** or object that **doesn’t have a direct JSX equivalent** (for example, a helper, a custom shader, a control, or your own geometry).

That’s exactly what `<primitive>` is for.

It lets you **inject any raw Three.js object** directly into the React Three Fiber scene graph.

### 🧠 Concept

```jsx
<primitive object={threeJsObject} />
```

This tells R3F:

> “Hey, here’s a pre-built Three.js object. Treat it as part of the scene graph.”

So R3F doesn’t try to build it — it just attaches it to the scene automatically.

### ⚙️ Example 1 — Using a Three.js Helper

Without `<primitive>`, we can’t add a Three.js helper directly, because helpers aren’t part of the built-in JSX elements.

```jsx
import * as THREE from 'three'
import { Canvas } from '@react-three/fiber'
import { useRef, useEffect } from 'react'

function Scene() {
  const lightRef = useRef()

  useEffect(() => {
    const helper = new THREE.DirectionalLightHelper(lightRef.current, 2)
    lightRef.current.add(helper)
  }, [])

  return (
    <>
      <directionalLight ref={lightRef} position={[2, 2, 2]} />
    </>
  )
}
```

But with `<primitive>`, this becomes super simple:

```jsx
function Scene() {
  const light = new THREE.DirectionalLight(0xffffff, 1)
  const helper = new THREE.DirectionalLightHelper(light, 2)

  return (
    <>
      <primitive object={light} position={[2, 2, 2]} />
      <primitive object={helper} />
    </>
  )
}
```

✅ Both the light and its helper are now part of the R3F scene graph.

### ⚙️ Example 2 — Using a Custom Three.js Object

```jsx
import * as THREE from 'three'
import { Canvas } from '@react-three/fiber'

function CustomObject() {
  const customObj = new THREE.Mesh(
    new THREE.TorusKnotGeometry(1, 0.3, 100, 16),
    new THREE.MeshStandardMaterial({ color: 'hotpink' })
  )
  customObj.rotation.x = Math.PI / 4

  return <primitive object={customObj} />
}

export default function App() {
  return (
    <Canvas camera={{ position: [3, 3, 3] }}>
      <ambientLight />
      <CustomObject />
    </Canvas>
  )
}
```

✅ Here you’re directly inserting a **TorusKnot Mesh** you created manually.

### ⚙️ Example 3 — Using `<primitive>` with Controls

Some Three.js utilities like `OrbitControls` aren’t available as JSX elements (they’re not part of `THREE.Scene` but extend it).

So you can create and attach them like this:

```jsx
import { Canvas, useThree, useFrame } from '@react-three/fiber'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
import { useEffect } from 'react'

function Controls() {
  const { camera, gl } = useThree()
  const controls = new OrbitControls(camera, gl.domElement)

  useEffect(() => {
    controls.enableDamping = true
    controls.dampingFactor = 0.05
    return () => controls.dispose()
  }, [controls])

  useFrame(() => controls.update())

  return <primitive object={controls} />
}

export default function App() {
  return (
    <Canvas>
      <ambientLight />
      <mesh>
        <boxGeometry />
        <meshStandardMaterial color="orange" />
      </mesh>
      <Controls />
    </Canvas>
  )
}
```

✅ `<primitive>` here adds the `OrbitControls` instance into the React lifecycle, so it’s cleaned up and managed automatically.

### ⚙️ Example 4 — Attaching to Parents

If you want the primitive to be a  **child of another object** , you can still do it declaratively:

```jsx
function Scene() {
  const light = new THREE.PointLight(0xffffff, 1)
  const helper = new THREE.PointLightHelper(light, 0.5)

  return (
    <primitive object={light} position={[1, 2, 3]}>
      <primitive object={helper} />
    </primitive>
  )
}
```

✅ The helper becomes a child of the light — just like in pure Three.js with `light.add(helper)`.

### ⚙️ You Can Still Use Props

If the Three.js object supports standard properties like `position`, `rotation`, or `scale`, you can set them directly as JSX props on `<primitive>`:

```jsx
<primitive object={mesh} position={[1, 2, 3]} rotation={[Math.PI / 2, 0, 0]} />
```

### ⚠️ Important Notes

1. **Use `<primitive>` sparingly**

   It bypasses React’s internal reconciliation, so R3F can’t deeply manage it like a JSX mesh.
2. **You can still use refs**

   ```jsx
   <primitive ref={myRef} object={customObj} />
   ```
3. **It’s not reactive**

   If you recreate the Three.js object (like `new THREE.Mesh(...)`) every render, React will destroy and recreate it. Use `useMemo()` to prevent that.

   ```jsx
   const customObj = useMemo(() => new THREE.Mesh(...), [])
   ```

### 🧠 When to Use `<primitive>`

✅ Use `<primitive>` when:

* You have a **Three.js object not available as JSX**
* You are using  **controls** ,  **helpers** ,  **custom shaders** , or **third-party objects**
* You want **fine-grained control** over a manually created Three.js object

🚫 Don’t use `<primitive>` when:

* There’s already a JSX equivalent (like `<mesh>`, `<boxGeometry>`, etc.)
* You want to control things declaratively (e.g. R3F’s `<OrbitControls />` from drei already exists)

### 🧩 Summary

| Feature   | Description                                         |
| --------- | --------------------------------------------------- |
| Purpose   | Injects raw Three.js objects into R3F               |
| Syntax    | `<primitive object={threeObject} />`              |
| Props     | Can set `position`,`rotation`,`scale`,`ref` |
| Lifecycle | Managed by React, auto-add/remove to scene          |
| Use Cases | Helpers, Controls, Custom Objects, Shaders          |

---

# ----Framer Motion 3D in R3F

Now we’re entering one of the *coolest integrations* in the React Three Fiber ecosystem:

 **Animating with Framer Motion 3D (aka `@framer/motion-3d`)** .

This is where React’s declarative UI power meets the cinematic control of 3D animation.

Let’s go step-by-step so you deeply understand **what it is, how it works, syntax, props, and examples.**

### 🎬 1. What is Framer Motion 3D?

Framer Motion 3D is an **extension of Framer Motion** that brings its familiar animation API (`motion.div`, `animate`, `whileHover`, etc.)

into the **Three.js / React Three Fiber world** — so you can animate **meshes, lights, cameras, and even groups** declaratively.

📦 Install:

```bash
npm install framer-motion-3d framer-motion
```

Then:

```jsx
import { motion } from "framer-motion-3d"
```

### 🧠 2. How it Works

Framer Motion 3D wraps Three.js objects (`mesh`, `group`, etc.) in `motion.` versions,

so every property like `position`, `rotation`, `scale`, or even `color` becomes  *animatable* .

Just like how `motion.div` animates DOM properties —

`motion.mesh` animates Three.js object properties.

### ✨ 3. Example — Simple Mesh Animation

```jsx
import { Canvas } from '@react-three/fiber'
import { motion } from 'framer-motion-3d'

export default function App() {
  return (
    <Canvas camera={{ position: [3, 3, 5] }}>
      <ambientLight intensity={0.4} />
      <directionalLight position={[3, 5, 5]} />

      <motion.mesh
        position={[0, 0, 0]}
        scale={1}
        animate={{ 
          scale: 1.5, 
          rotation: [0, Math.PI * 2, 0] 
        }}
        transition={{
          duration: 2,
          repeat: Infinity,
          repeatType: "reverse"
        }}
      >
        <boxGeometry args={[1, 1, 1]} />
        <meshStandardMaterial color="hotpink" />
      </motion.mesh>
    </Canvas>
  )
}
```

✅ Here:

* The cube **spins and scales up/down** continuously.
* You can use **Framer Motion props** like `initial`, `animate`, `transition`, `exit`, etc.
* Animations are handled by Framer’s internal spring or tween system.

### 🧩 4. Supported Animated Properties

Framer Motion 3D can animate most of the following:

| Property               | Example                                  |
| ---------------------- | ---------------------------------------- |
| `position`           | `{ x: 1, y: 2, z: 3 }`or `[1, 2, 3]` |
| `rotation`           | `[Math.PI / 2, 0, 0]`                  |
| `scale`              | `2`or `[2, 2, 2]`                    |
| `color`              | `'#ff0000'`or `new THREE.Color()`    |
| `intensity`(lights)  | `animate={{ intensity: 3 }}`           |
| `opacity`(materials) | `animate={{ opacity: 0.5 }}`           |

💡 R3F properties map directly to Framer Motion’s animatable props.

### 🧭 5. Example — Interactive Hover Animation

```jsx
<motion.mesh
  whileHover={{
    scale: 1.2,
    rotateY: Math.PI / 2,
  }}
  whileTap={{
    scale: 0.9,
  }}
>
  <sphereGeometry args={[1, 32, 32]} />
  <meshStandardMaterial color="skyblue" />
</motion.mesh>
```

✅ Here:

* The sphere scales up and spins when hovered.
* When clicked (tapped), it scales down slightly — just like a button animation.

### 🎨 6. Example — Animating a Group

You can animate entire **groups** of objects:

```jsx
<motion.group
  initial={{ y: -2, opacity: 0 }}
  animate={{ y: 0, opacity: 1 }}
  transition={{ duration: 1 }}
>
  <mesh position={[0, 0, 0]}>
    <boxGeometry args={[1, 1, 1]} />
    <meshStandardMaterial color="orange" />
  </mesh>
  <mesh position={[2, 0, 0]}>
    <sphereGeometry args={[0.6, 32, 32]} />
    <meshStandardMaterial color="lime" />
  </mesh>
</motion.group>
```

✅ This fades and moves the whole group into view like an entrance animation.

### ⚙️ 7. Example — Light Animation

You can animate **light positions** or **intensity** dynamically:

```jsx
<motion.directionalLight
  position={[3, 3, 3]}
  animate={{ 
    intensity: [0.2, 2, 0.5],
    position: [[3,3,3], [-3,3,3], [3,3,3]]
  }}
  transition={{
    duration: 4,
    repeat: Infinity,
    repeatType: "mirror"
  }}
/>
```

✅ Smoothly oscillates both **intensity** and  **position** .

### 🪄 8. Advanced Example — Keyframe Animation

```jsx
<motion.mesh
  animate={{
    position: [
      [0, 0, 0],
      [0, 2, 0],
      [2, 2, 0],
      [0, 0, 0]
    ],
    scale: [1, 1.5, 1]
  }}
  transition={{
    duration: 5,
    repeat: Infinity,
    ease: "easeInOut"
  }}
>
  <boxGeometry args={[1, 1, 1]} />
  <meshStandardMaterial color="hotpink" />
</motion.mesh>
```

✅ This uses **keyframes** to move the cube along a square path.

### 🧠 9. `motionValue` for Continuous Interpolation

You can even use **motion values** from Framer Motion’s core:

```jsx
import { useMotionValue, useSpring } from "framer-motion"
import { useFrame } from "@react-three/fiber"

function MovingBox() {
  const x = useMotionValue(0)
  const smoothX = useSpring(x, { stiffness: 100, damping: 20 })

  useFrame(() => {
    x.set(Math.sin(Date.now() * 0.001) * 2)
  })

  return (
    <motion.mesh position-x={smoothX}>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color="hotpink" />
    </motion.mesh>
  )
}
```

✅ Here, the cube **moves back and forth** smoothly, driven by Framer’s spring physics.

### 🧮 10. Mixing R3F Hooks (`useFrame`, `useThree`) and Framer Motion

Framer Motion handles *declarative* transitions,

while R3F’s `useFrame` is perfect for *manual / procedural* updates.

You can combine both — Framer for smooth transitions, and R3F for physics-like logic.

### 🧩 11. Common Motion 3D Components

| Component                    | Description                                       |
| ---------------------------- | ------------------------------------------------- |
| `motion.mesh`              | Animate a Mesh (position, rotation, scale, color) |
| `motion.group`             | Animate multiple objects as a unit                |
| `motion.spotLight`         | Animate light intensity, position, color          |
| `motion.perspectiveCamera` | Animate camera movement                           |
| `motion.ambientLight`      | Animate ambient light intensity                   |
| `motion.directionalLight`  | Animate directional light                         |

Basically,  **any R3F JSX element can become `motion.` prefixed** .

### 💡 12. Best Practices

✅ Use `initial` and `animate` for entry transitions

✅ Use `whileHover`, `whileTap`, `whileInView` for interactivity

✅ Use `useMemo` for geometries to prevent re-creation

✅ Keep animation logic declarative whenever possible

✅ Combine with Drei components (`<Float>`, `<PresentationControls>`) for expressive 3D UI

### 🚀 13. Real-World Use Cases

* Animate product models in e-commerce (rotate, float, scale on hover)
* Smooth camera transitions between sections
* Interactive buttons in 3D menus
* Scene transitions (fade-in / fade-out)
* Animated 3D dashboards or data visualizations

### 🎯 Summary

| Feature       | Description                                          |
| ------------- | ---------------------------------------------------- |
| Library       | `@framer/motion-3d`                                |
| Purpose       | Declarative animation for 3D objects                 |
| Works With    | React Three Fiber                                    |
| Syntax        | `motion.mesh`,`motion.group`, etc.               |
| Animate Props | position, rotation, scale, color, intensity, opacity |
| Key Advantage | Smooth, declarative, spring-based 3D animation       |

---

# ----Drei Components - Environment, Sky, Star

This is a **super useful topic** because these Drei components —

`<Environment />`, `<Sky />`, and `<Stars />` — help you create *beautiful lighting and realistic backgrounds* in **React Three Fiber (R3F)** with almost zero code.

They are part of  **@react-three/drei** , a utility library that provides ready-made helpers built on top of Three.js.

Let’s go through each one **in detail** with explanations, properties, and examples 👇

### 🌎 1. `<Environment />`

**🎯 Purpose:**

`<Environment />` creates realistic lighting and reflections in your scene by using an **environment map** (either HDRI, cube texture, or preset).

It acts like an invisible **skybox** that surrounds your entire scene and provides:

* **Image-Based Lighting (IBL)** — lights your scene using HDR images
* **Reflections** on metallic and glossy materials
* Optional **background** (skybox) image

**✅ Import:**

```js
import { Environment } from '@react-three/drei'
```

**⚙️ Syntax:**

```jsx
<Environment
  files="/path/to/hdr.hdr"   // or a cube map array
  background={true}          // optional, sets the scene background
  ground={{ height: 10, radius: 60, scale: 100 }}  // optional "ground projected" lighting
  preset="sunset"            // built-in presets
/>
```

**🧠 Explanation:**

* `files`: path to HDRI image (usually `.hdr` or `.exr`)
* `background`: if true, sets that HDRI as the visible background (skybox)
* `preset`: Drei provides several built-in HDR environments:
  * `"sunset"`, `"dawn"`, `"night"`, `"warehouse"`, `"forest"`, `"apartment"`, `"studio"`, `"city"`, `"park"`, `"lobby"`
* `ground`: projects the environment onto a fake “infinite” ground plane to fake soft shadows.

**🧩 Example:**

```jsx
import { Canvas } from '@react-three/fiber'
import { Environment, OrbitControls } from '@react-three/drei'

export default function Scene() {
  return (
    <Canvas camera={{ position: [3, 2, 5] }}>
      <ambientLight intensity={0.2} />

      <mesh>
        <sphereGeometry args={[1, 64, 64]} />
        <meshStandardMaterial metalness={1} roughness={0} color="white" />
      </mesh>

      <Environment preset="sunset" background />  
      <OrbitControls />
    </Canvas>
  )
}
```

✅ The sphere reflects the  **sunset HDRI** , creating realistic metallic reflections.

### ☀️ 2. `<Sky />`

**🎯 Purpose:**

`<Sky />` generates a **procedural, realistic sky dome** that reacts to sun position, atmospheric scattering, and turbidity — like a physical daylight system.

It simulates:

* Day/night cycle
* Sun movement
* Sky color based on turbidity and rayleigh scattering

**✅ Import:**

```js
import { Sky } from '@react-three/drei'
```

**⚙️ Syntax:**

```jsx
<Sky
  distance={450000} // Sky sphere size
  sunPosition={[0, 1, 0]} // Sun direction
  turbidity={10} // "Haze" or cloudiness
  rayleigh={3} // Scattering intensity
  mieCoefficient={0.005} // Dust particles
  mieDirectionalG={0.8} // Sun glow spread
  inclination={0.49} // Vertical angle of sun
  azimuth={0.25} // Horizontal rotation of sun
/>
```

**🧠 Explanation of Props:**

| Prop                | Meaning                                                           |
| ------------------- | ----------------------------------------------------------------- |
| `distance`        | Radius of the sky sphere                                          |
| `sunPosition`     | `[x, y, z]`vector controlling sun direction                     |
| `turbidity`       | Controls haziness of the sky (higher = more dusty/cloudy)         |
| `rayleigh`        | Controls the amount of atmosphere scattering (higher = bluer sky) |
| `mieCoefficient`  | Density of particles causing sun glow                             |
| `mieDirectionalG` | How concentrated the sun’s glow is                               |
| `inclination`     | Vertical tilt of the sun (like time of day)                       |
| `azimuth`         | Horizontal rotation (like direction of sunrise/sunset)            |

**🌤️ Example:**

```jsx
import { Canvas } from '@react-three/fiber'
import { Sky, OrbitControls } from '@react-three/drei'

export default function App() {
  return (
    <Canvas camera={{ position: [0, 2, 5] }}>
      <ambientLight intensity={0.4} />
      <directionalLight position={[3, 3, 3]} intensity={2} />

      <mesh>
        <boxGeometry />
        <meshStandardMaterial color="orange" />
      </mesh>

      <Sky
        distance={450000}
        sunPosition={[100, 20, 100]}
        turbidity={10}
        rayleigh={3}
        mieCoefficient={0.005}
        mieDirectionalG={0.8}
      />

      <OrbitControls />
    </Canvas>
  )
}
```

✅ Produces a dynamic, realistic blue sky with an actual sunlight direction.

You can animate `sunPosition` to create a  **day-night cycle** .

### 🌌 3. `<Stars />`

**🎯 Purpose:**

`<Stars />` generates a beautiful, realistic **starfield** background made of randomly placed points — perfect for space or night scenes.

It’s lightweight and customizable.

**✅ Import:**

```js
import { Stars } from '@react-three/drei'
```

⚙️ Syntax:

```jsx
<Stars
  radius={100}        // Sphere radius
  depth={50}          // Star spread depth
  count={5000}        // Number of stars
  factor={4}          // Star size factor
  saturation={0}      // Star color saturation
  fade={true}         // Stars fade at the edge of view
  speed={1}           // Twinkling speed
/>
```

**🧠 Explanation of Props:**

| Prop           | Meaning                        |
| -------------- | ------------------------------ |
| `radius`     | Radius of the starfield sphere |
| `depth`      | How far stars are distributed  |
| `count`      | Number of stars                |
| `factor`     | Star size scaling              |
| `saturation` | Color saturation (0 = white)   |
| `fade`       | Whether stars fade at edges    |
| `speed`      | Controls the twinkle speed     |

**🌠 Example:**

```jsx
import { Canvas } from '@react-three/fiber'
import { Stars, OrbitControls } from '@react-three/drei'

export default function StarScene() {
  return (
    <Canvas camera={{ position: [0, 0, 5] }}>
      <Stars radius={300} depth={60} count={8000} factor={4} fade speed={1} />
      <OrbitControls />
    </Canvas>
  )
}
```

✅ Creates a mesmerizing **starry sky** background — great for space simulations or nighttime scenes.

### ⚡ Combined Example — Sky + Environment + Stars

```jsx
import { Canvas } from '@react-three/fiber'
import { OrbitControls, Sky, Environment, Stars } from '@react-three/drei'

export default function WorldScene() {
  return (
    <Canvas camera={{ position: [0, 2, 5], fov: 50 }}>
      <ambientLight intensity={0.2} />
      <directionalLight position={[5, 5, 5]} intensity={2} />

      <mesh>
        <sphereGeometry args={[1, 64, 64]} />
        <meshStandardMaterial color="royalblue" metalness={0.5} roughness={0.3} />
      </mesh>

      <Sky sunPosition={[100, 10, 50]} turbidity={8} rayleigh={3} />
      <Environment preset="sunset" />
      <Stars fade radius={200} count={5000} factor={4} />
      <OrbitControls />
    </Canvas>
  )
}
```

✅ This setup gives:

* A dynamic **sky** for realism
* A realistic **HDRI environment** for reflections
* Subtle **stars** visible in darker areas

### 🧩 Summary

| Component                     | Purpose                                             | Key Props                                      | Notes                           |
| ----------------------------- | --------------------------------------------------- | ---------------------------------------------- | ------------------------------- |
| **`<Environment />`** | Adds realistic image-based lighting and reflections | `preset`,`files`,`background`,`ground` | Use for realism and reflections |
| **`<Sky />`**         | Procedural physical sky with sun position           | `sunPosition`,`turbidity`,`rayleigh`     | Great for outdoor daylight      |
| **`<Stars />`**       | Creates dynamic starfield                           | `count`,`fade`,`speed`,`radius`        | Best for space/night scenes     |

---

# ----Drei Components- Html and Text

We are diving into some of the  **most powerful Drei components** :

🧩 `<Html />` and 🧠 `<Text />`.

Both are super handy for adding **labels, UI overlays, and readable text** directly inside your 3D scene in  **React Three Fiber (R3F)** .

Let’s break them down one by one — with  **concepts, use cases, props, and live examples** .

### 🧱 1. `<Html />` — Render Real DOM in 3D Space

**🎯 Purpose:**

The `<Html />` component allows you to render **regular HTML elements** (like `<div>`, `<button>`, `<p>`, etc.) **inside the 3D canvas** — positioned and rotated with your 3D objects.

It acts like a **portal** between React DOM and the Three.js 3D world.

**✅ Import:**

```js
import { Html } from '@react-three/drei'
```

**⚙️ Syntax:**

```jsx
<Html
  as="div"
  center
  distanceFactor={10}
  transform
  sprite
  occlude
  position={[x, y, z]}
  zIndexRange={[100, 0]}
>
  <p>Hello World!</p>
</Html>
```

* ** 🧠 Explanation of Props**

| Prop               | Description                                                      |
| ------------------ | ---------------------------------------------------------------- |
| `as`             | The HTML tag to render (e.g.`"div"`,`"span"`)                |
| `position`       | 3D position `[x, y, z]`of the HTML element                     |
| `center`         | Centers the HTML element around its position                     |
| `distanceFactor` | Scales the HTML size based on distance to camera (default `1`) |
| `transform`      | Makes the HTML follow the 3D object’s rotation, scale, etc.     |
| `sprite`         | Makes it always face the camera (billboard effect)               |
| `occlude`        | If true, hides HTML when it’s behind another mesh               |
| `zIndexRange`    | Sets the CSS z-index range for depth ordering                    |
| `wrapperClass`   | CSS class applied to the containing div                          |
| `portal`         | Portal target for rendering HTML outside canvas (e.g., tooltips) |

##### **📘 Example 1 — Label Above an Object**

```jsx
import { Canvas } from '@react-three/fiber'
import { Html, OrbitControls } from '@react-three/drei'

function BoxWithLabel() {
  return (
    <mesh position={[0, 0, 0]}>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color="orange" />
  
      <Html center position={[0, 1.2, 0]}>
        <div style={{
          background: 'white',
          padding: '4px 8px',
          borderRadius: '6px',
          fontSize: '12px',
          fontWeight: 'bold'
        }}>
          Cube
        </div>
      </Html>
    </mesh>
  )
}

export default function App() {
  return (
    <Canvas camera={{ position: [3, 2, 5] }}>
      <ambientLight />
      <BoxWithLabel />
      <OrbitControls />
    </Canvas>
  )
}
```

✅ The `<Html>` div appears floating  *above the cube* , moves and rotates with it.

##### 📘 Example 2 — Always Facing Camera (Sprite Mode)

```jsx
<Html sprite position={[0, 1.2, 0]}>
  <div style={{ color: 'white', background: 'black' }}>Always Facing You</div>
</Html>
```

✅ The label will  **always face the camera** , useful for name tags or HUDs.

##### 📘 Example 3 — Occluding Behind Objects

```jsx
<Html position={[0, 1, 0]} occlude>
  <div>Hidden when blocked</div>
</Html>
```

✅ The label disappears when behind another mesh (mimics real-world occlusion).

##### 💡 Common Use Cases

* Object labels (like product names or user names)
* Tooltip popups in 3D scenes
* UI elements like buttons or panels attached to 3D models
* Debugging overlays

### 🧾 2. `<Text />` — 3D Text Geometry That Renders in the Scene

🎯 Purpose:

`<Text />` is a Drei component that renders **real 3D text** inside your scene using **Signed Distance Fields (SDF)** for **crisp and scalable** text — no blurring at any distance.

This text lives *inside* the 3D world (unlike `<Html>`, which overlays DOM).

**✅ Import:**

```js
import { Text } from '@react-three/drei'
```

**⚙️ Syntax:**

```jsx
<Text
  position={[x, y, z]}
  font="/fonts/Inter-Bold.woff"
  fontSize={1}
  color="hotpink"
  maxWidth={2}
  textAlign="center"
  anchorX="center"
  anchorY="middle"
  outlineWidth={0.03}
  outlineColor="black"
  strokeWidth={0.02}
  strokeColor="white"
  lineHeight={1.2}
  letterSpacing={0.02}
  depthOffset={1}
>
  Hello R3F!
</Text>
```

**🧠 Explanation of Props**

| Prop                              | Description                                                                  |
| --------------------------------- | ---------------------------------------------------------------------------- |
| `font`                          | URL/path to a .ttf or .woff font file                                        |
| `fontSize`                      | Size of text in world units                                                  |
| `color`                         | Text color                                                                   |
| `maxWidth`                      | Wraps text when it exceeds width                                             |
| `textAlign`                     | "left"                                                                       |
| `anchorX`/`anchorY`           | Controls origin point ("left", "center", "right", "top", "middle", "bottom") |
| `outlineWidth`/`outlineColor` | Add text outline                                                             |
| `strokeWidth`/`strokeColor`   | Add border stroke                                                            |
| `lineHeight`                    | Distance between lines for multiline text                                    |
| `letterSpacing`                 | Adjusts spacing between letters                                              |
| `depthOffset`                   | Pushes text forward or backward for z-fighting avoidance                     |

**📘 Example 1 — Basic 3D Text**

```jsx
import { Canvas } from '@react-three/fiber'
import { Text, OrbitControls } from '@react-three/drei'

export default function TextExample() {
  return (
    <Canvas camera={{ position: [0, 0, 5] }}>
      <ambientLight intensity={1} />
  
      <Text
        fontSize={1}
        color="skyblue"
        position={[0, 0, 0]}
        anchorX="center"
        anchorY="middle"
      >
        FitLab Gym
      </Text>

      <OrbitControls />
    </Canvas>
  )
}
```

✅ Renders clean, sharp text floating in 3D space.

##### 📘 Example 2 — Styled Text

```jsx
<Text
  position={[0, 1, 0]}
  font="/fonts/Roboto-Bold.woff"
  fontSize={0.7}
  color="#ff6b6b"
  outlineWidth={0.05}
  outlineColor="#222"
  letterSpacing={0.05}
  textAlign="center"
>
  Gym E-commerce
</Text>
```

✅ Produces glowing, professional-looking text with outline.

##### 📘 Example 3 — Multiline and Wrapped Text

```jsx
<Text
  maxWidth={3}
  lineHeight={1.5}
  textAlign="center"
  fontSize={0.3}
>
  Discover high-quality gym equipment and supplements!
</Text>
```

✅ Automatically wraps text to the specified max width.

##### 💡 Common Use Cases

* 3D titles, signs, or labels on objects
* Intro animations or landing page scenes
* Floating UI in VR or AR environments
* Product names or prices near models
* Interactive text that reacts to hover or click

### ⚖️ `<Html />` vs `<Text />` Comparison

| Feature     | `<Html />`                       | `<Text />`                    |
| ----------- | ---------------------------------- | ------------------------------- |
| Type        | Real DOM overlay                   | True 3D text geometry           |
| Performance | Less performant if overused        | Highly optimized                |
| Occlusion   | Can occlude (`occlude`prop)      | True 3D occlusion               |
| Lighting    | Not affected by lights             | Affected by 3D lights           |
| Interaction | Supports DOM events (click, hover) | Requires raycasting             |
| Use case    | Tooltips, UI labels                | In-world text, signs, 3D titles |

### 🔥 Combined Example — Label + 3D Text

```jsx
import { Canvas } from '@react-three/fiber'
import { Html, Text, OrbitControls } from '@react-three/drei'

function ProductTag() {
  return (
    <mesh position={[0, 0, 0]}>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color="royalblue" />
  
      {/* Floating 3D text */}
      <Text position={[0, 1.5, 0]} fontSize={0.4} color="white" outlineWidth={0.02}>
        Dumbbell
      </Text>
  
      {/* Tooltip-style DOM overlay */}
      <Html center position={[0, -0.8, 0]}>
        <div style={{
          background: 'white',
          color: 'black',
          padding: '4px 8px',
          borderRadius: '4px',
          fontSize: '12px'
        }}>
          ₹499 only
        </div>
      </Html>
    </mesh>
  )
}

export default function Scene() {
  return (
    <Canvas camera={{ position: [2, 2, 5] }}>
      <ambientLight intensity={0.5} />
      <directionalLight position={[2, 2, 2]} />
      <ProductTag />
      <OrbitControls />
    </Canvas>
  )
}
```

✅ Text label floats above the 3D model, with an HTML price tag below it — great for a **3D product viewer** in your gym e-commerce project!

# ......................................................................................................................

## Applying Framer-motion 3D onto those elements

💯 Excellent question — and yes, you **absolutely can** apply **Framer Motion** animations to both `<Html />` and `<Text />` in React Three Fiber (R3F)!

However — the way you apply it differs slightly between the two because:

* `<Html />` renders **real DOM elements** 🧩 → so you can use Framer Motion  *directly* .
* `<Text />` renders **3D mesh objects** 🧱 → so you must use `motion` wrappers for **Three.js objects** via `framer-motion-3d`.

Let’s go through both cases carefully 👇

#### 🧱 1️⃣ Animating `<Html />` — Easy (Pure DOM Framer Motion)

Since `<Html />` renders real DOM elements (like `<div>`, `<span>`), you can animate them using **normal Framer Motion components** (`motion.div`, `motion.span`, etc.).

**✅ Example:**

```jsx
import { Canvas } from '@react-three/fiber'
import { Html, OrbitControls } from '@react-three/drei'
import { motion } from 'framer-motion'

function AnimatedLabel() {
  return (
    <mesh position={[0, 0, 0]}>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color="orange" />

      <Html center position={[0, 1.2, 0]}>
        <motion.div
          initial={{ scale: 0, opacity: 0 }}
          animate={{ scale: 1, opacity: 1 }}
          transition={{ duration: 1, ease: 'easeOut' }}
          style={{
            background: 'white',
            padding: '6px 12px',
            borderRadius: '8px',
            fontSize: '14px',
            fontWeight: 'bold'
          }}
        >
          🏋️ FitLab Gym
        </motion.div>
      </Html>
    </mesh>
  )
}

export default function App() {
  return (
    <Canvas camera={{ position: [2, 2, 5] }}>
      <ambientLight />
      <OrbitControls />
      <AnimatedLabel />
    </Canvas>
  )
}
```

✅ Here:

* The `<Html>` div smoothly **scales in** and  **fades in** .
* You can use **all Framer Motion features** — hover, variants, keyframes, etc.

  (because it’s real HTML inside the scene!)

#### 🧠 2️⃣ Animating `<Text />` — Using `framer-motion-3d`

When dealing with **Three.js objects** like `<Text />`, `<mesh />`, `<group />`, etc.,

you need `framer-motion-3d` (which comes built-in with `framer-motion@10+`).

It provides `motion.mesh`, `motion.group`, `motion.Text`, etc., that animate **3D transforms** like `position`, `rotation`, `scale`, etc.

**✅ Install (if not already):**

```bash
npm install framer-motion
```

**✅ Example with `<Text />`**

```jsx
import { Canvas } from '@react-three/fiber'
import { Text, OrbitControls } from '@react-three/drei'
import { motion } from 'framer-motion-3d'

export default function AnimatedText() {
  return (
    <Canvas camera={{ position: [0, 0, 5] }}>
      <ambientLight />
      <OrbitControls />

      <motion.group
        initial={{ scale: 0, y: -1 }}
        animate={{ scale: 1, y: 0 }}
        transition={{ duration: 1, type: 'spring' }}
      >
        <Text
          fontSize={1}
          color="hotpink"
          anchorX="center"
          anchorY="middle"
        >
          FitLab Gym
        </Text>
      </motion.group>
    </Canvas>
  )
}
```

✅ The text “FitLab Gym” **springs upward and scales in** elegantly.

**💫 Example — Continuous Rotation Animation**

```jsx
<motion.group
  animate={{ rotateY: [0, Math.PI * 2] }}
  transition={{ duration: 5, repeat: Infinity, ease: 'linear' }}
>
  <Text fontSize={0.8} color="skyblue">Welcome!</Text>
</motion.group>
```

✅ Spins your 3D text infinitely around the Y-axis.

#### ⚙️ Pro Tip — Combine Both

You can animate **3D text in space** and show an **HTML tooltip** that fades in/out together:

```jsx
<motion.group
  initial={{ scale: 0 }}
  animate={{ scale: 1 }}
  transition={{ duration: 1 }}
>
  <Text fontSize={0.6} color="#ff6666">Dumbbell</Text>
  
  <Html position={[0, -1, 0]} center>
    <motion.div
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{ delay: 0.5 }}
      style={{ background: 'white', padding: '4px 8px', borderRadius: '6px' }}
    >
      ₹499 only
    </motion.div>
  </Html>
</motion.group>
```

✅ The text scales up, and the price label fades in afterward.

#### 🧠 Wrapping `<Hmtl` with `motion`

✅ **Yes, but with a caveat:**

You **can** wrap `<Html />` with `motion` (like `<motion.Html />`)  **if you import `motion` from `framer-motion-3d`** , because Drei’s `<Html>` is a  **React component** , and `framer-motion-3d` can animate props such as `position`, `rotation`, `scale`, and even opacity.

However — **Framer Motion does not animate DOM properties inside `<Html>` directly** (like CSS transforms) because `<Html>` internally renders a real `<div>` in the DOM —  *not a mesh in WebGL* .

So you have two animation layers possible:

##### 🧩 Case 1: Animate `<Html />`'s **3D placement** in space

(using `motion.Html` from `framer-motion-3d`)

This works perfectly.

You can animate its `position`, `rotation`, and `scale` like any 3D object.

```jsx
import { Canvas } from '@react-three/fiber'
import { Html, OrbitControls } from '@react-three/drei'
import { motion } from 'framer-motion-3d'

export default function AnimatedHtmlPosition() {
  return (
    <Canvas camera={{ position: [0, 0, 5] }}>
      <ambientLight />
      <OrbitControls />

      <motion.Html
        initial={{ position: [0, -1, 0], scale: 0 }}
        animate={{ position: [0, 1, 0], scale: 1 }}
        transition={{ duration: 1.5, type: 'spring' }}
        center
      >
        <div
          style={{
            background: 'white',
            padding: '10px 20px',
            borderRadius: '10px',
            fontWeight: 'bold',
          }}
        >
          🏋️ FitLab Gym
        </div>
      </motion.Html>
    </Canvas>
  )
}
```

##### 🧩 Case 2: Animate the HTML *content itself*

(using Framer Motion DOM components inside `<Html>`)

If you want to animate CSS properties (opacity, scale, color, etc.), use normal Framer Motion DOM elements *inside* `<Html>`:

```jsx
<Html center>
  <motion.div
    initial={{ opacity: 0, y: 50 }}
    animate={{ opacity: 1, y: 0 }}
    transition={{ duration: 1 }}
    style={{
      background: 'white',
      padding: '12px 24px',
      borderRadius: '8px',
      fontSize: '16px'
    }}
  >
    Welcome to FitLab 💪
  </motion.div>
</Html>
```

✅ Here:

* The HTML content itself fades/slides in.
* The `<Html>`’s 3D position is fixed.

##### ⚡ Combine Both

You can **combine both approaches** beautifully:

```jsx
<motion.Html
  initial={{ position: [0, -2, 0], scale: 0 }}
  animate={{ position: [0, 1, 0], scale: 1 }}
  transition={{ duration: 1.2, type: 'spring' }}
  center
>
  <motion.div
    initial={{ opacity: 0 }}
    animate={{ opacity: 1 }}
    transition={{ delay: 0.6 }}
    style={{
      background: 'white',
      padding: '10px 20px',
      borderRadius: '10px',
      fontWeight: 'bold',
    }}
  >
    💥 FitLab Premium Offer!
  </motion.div>
</motion.Html>
```

✅ So:

* `motion.Html` animates the 3D *placement* in the scene.
* `motion.div` animates the *DOM content* itself (CSS animation).

---

# ----Drei UI/label Components

In  **@react-three/drei** , `<Html>` and `<Text>` are among the most commonly used “overlay” or “annotation / label / UI” type components.

But Drei provides several **other similar or related components** that also deal with *2D/3D text, DOM overlays, labels, annotations, and interactions* in 3D space.

### 🧩 1️⃣ `<Html />`

**Purpose:** Render *regular HTML elements* inside your 3D scene.

* Think of it as “portaling” standard DOM into 3D space.
* Internally, it uses `react-dom` portals and synchronizes position/rotation with the 3D scene.
* Ideal for tooltips, annotations, buttons, HUDs, etc.

```jsx
<Html distanceFactor={10}>
  <div className="tooltip">This is an HTML overlay!</div>
</Html>
```

✅ You can animate with Framer Motion using `<motion.div>` inside it.

### 🧩 2️⃣ `<Text />`

**Purpose:** Render *real 3D text geometry* in the scene.

* Uses [troika-three-text](https://github.com/protectwise/troika) under the hood.
* Supports custom fonts, outlines, strokes, anchors, max width, line height, etc.
* Fully lit by your scene lights (it’s a mesh, not HTML).

```jsx
<Text
  font="/fonts/Roboto-Regular.ttf"
  fontSize={0.5}
  color="white"
  anchorX="center"
  anchorY="middle"
>
  FitLab Gym Store
</Text>
```

✅ You can animate its position, rotation, or scale using Framer Motion 3D: `<motion.mesh>` or `<motion.group>`.

### 🧩 3️⃣ `<Text3D />`

**Purpose:** True 3D *extruded* text geometry.

* Similar to `<Text>`, but uses `THREE.TextGeometry`.
* Requires a font JSON (e.g., from typeface.js or Three.js examples).
* You can extrude and bevel the text.

```jsx
<Text3D font="/fonts/helvetiker_regular.typeface.json" size={1} height={0.3}>
  GymZone
  <meshStandardMaterial color="hotpink" />
</Text3D>
```

✅ Perfect for logos or decorative headings.

### 🧩 4️⃣ `<Billboard />`

**Purpose:** Always face the camera — useful for labels, sprites, and icons.

```jsx
<Billboard>
  <Text fontSize={0.3} color="yellow">Always Facing You!</Text>
</Billboard>
```

You can wrap `<Html>` or `<Text>` inside `<Billboard>` to make sure they always face the viewer.

### 🧩 5️⃣ `<Image />`

**Purpose:** Render flat images (like planes) easily.

* Internally uses `THREE.MeshBasicMaterial` with a loaded texture.
* You can position, scale, and even animate them like other meshes.

```jsx
<Image url="/images/dumbbell.png" scale={2} />
```

### 🧩 6️⃣ `<TextSprite />`

**Purpose:** Lightweight text rendered as a sprite (always facing camera).

* Unlike `<Text>`, it’s not true 3D geometry.
* Very fast, good for many small labels.

```jsx
<TextSprite text="A1" color="red" fontSize={50} />
```

### 🧩 7️⃣ `<HtmlLabel />` (custom pattern)

While not a built-in Drei component, developers often create **custom combinations** of `<Html>` + `<Billboard>` or `<Text>` to make floating labels that follow objects.

```jsx
<Billboard>
  <Html distanceFactor={8}>
    <div className="label">Bench Press</div>
  </Html>
</Billboard>
```

### 🧩 8️⃣ `<Stats />`

**Purpose:** Overlay a performance monitor similar to `stats.js`.

Though not text, it’s also an “overlay-like” utility.

```jsx
<Stats />
```

### ⚙️ Summary Table

| Component          | Type    | Rendered As         | Common Uses          |
| ------------------ | ------- | ------------------- | -------------------- |
| `<Html />`       | DOM     | HTML overlay        | Tooltips, UI, labels |
| `<Text />`       | Mesh    | 2D/3D text (flat)   | Labels, titles       |
| `<Text3D />`     | Mesh    | Extruded 3D text    | Logos, bold titles   |
| `<Billboard />`  | Wrapper | Always faces camera | Signs, labels        |
| `<Image />`      | Mesh    | Plane with texture  | Posters, images      |
| `<TextSprite />` | Sprite  | Camera-facing text  | Many small labels    |
| `<Stats />`      | Overlay | Performance info    | Debugging            |

---

# ----Reflector

Let’s explore **`<Reflector />`** in **@react-three/drei** — what it is, how it works, and whether it exists in plain  **Three.js** .

### 🌊 What is `<Reflector />`?

The `<Reflector />` component in **@react-three/drei** creates a **real-time mirror surface** — a plane that reflects the rest of your scene dynamically.

It’s essentially a React-friendly abstraction over **Three.js’s `Reflector` class** from the [Three.js examples](https://threejs.org/examples/?q=reflect#webgl_mirror).

So, to your second question 👇

✅  **Yes — it *does* exist in Three.js** ,

but not as a *core* class — it’s part of `three/examples/jsm/objects/Reflector.js`.

Drei’s version wraps that class and makes it super easy to use declaratively in React Three Fiber.

**🧱 Example (Basic Usage)**

```jsx
import { Reflector } from '@react-three/drei'

function MirrorFloor() {
  return (
    <Reflector
      resolution={1024}          // reflection texture resolution
      args={[10, 10]}            // plane size (width, height)
      mirror={1}                 // reflectivity (0–1)
      mixBlur={1}                // blur amount for the reflection
      mixStrength={2}            // reflection intensity
      rotation-x={-Math.PI / 2}  // make it horizontal
      position={[0, -1, 0]}      // move it slightly down
    >
      {(Material, props) => <Material color="lightblue" metalness={0.5} roughness={1} {...props} />}
    </Reflector>
  )
}
```

✅ This creates a **mirror floor** — it reflects everything above it (like objects or lights) in real time.

### ⚙️ Props Explained

| Prop                        | Type                     | Description                                             |
| --------------------------- | ------------------------ | ------------------------------------------------------- |
| `args`                    | `[width, height]`      | Size of the reflective plane                            |
| `resolution`              | `number`               | Texture resolution of the reflection (higher = sharper) |
| `mirror`                  | `number`               | Reflectivity intensity (0 = matte, 1 = perfect mirror)  |
| `mixBlur`                 | `number`               | Amount of blur in the reflection                        |
| `mixStrength`             | `number`               | Strength of the reflection                              |
| `depthScale`              | `number`               | Adds a depth-based distortion (fake ripples)            |
| `minDepthThreshold`       | `number`               | Lower bound for depth blending                          |
| `maxDepthThreshold`       | `number`               | Upper bound for depth blending                          |
| `rotation-x`/`position` | `number`/`[x, y, z]` | Plane orientation and position                          |

### 🎨 The Material Function

You’ll notice that `<Reflector>` takes  **children as a function** :

```jsx
{(Material, props) => <Material color="lightblue" roughness={0.5} {...props} />}
```

This function receives:

* The base `MeshReflectorMaterial` (a special Drei material)
* A `props` object with some precomputed reflection uniforms

  You can use these to build your own look (metallic, rough, colored mirrors).

### 🌫️ Example with Environment

```jsx
<Canvas>
  <ambientLight intensity={0.5} />
  <directionalLight position={[10, 10, 5]} />
  
  <mesh position={[0, 1, 0]}>
    <sphereGeometry args={[1, 32, 32]} />
    <meshStandardMaterial color="hotpink" />
  </mesh>

  <MirrorFloor />
</Canvas>
```

You’ll see the pink sphere *reflected* perfectly in the blue reflective ground.

### 🌊 Advanced Variant: `<MeshReflectorMaterial />`

If you want to apply a reflection  *on any geometry* , not just a plane, Drei also gives you:

```jsx
import { MeshReflectorMaterial } from '@react-three/drei'

<mesh rotation-x={-Math.PI / 2}>
  <planeGeometry args={[10, 10]} />
  <MeshReflectorMaterial
    blur={[400, 100]}
    resolution={1024}
    mixBlur={1}
    mixStrength={2}
    roughness={1}
    depthScale={1.2}
    minDepthThreshold={0.4}
    maxDepthThreshold={1.4}
    color="#202020"
    metalness={0.8}
    mirror={1}
  />
</mesh>
```

This one lets you use reflections on **custom shapes** too.

### 🔍 Under the Hood (How it works)

Internally, both `Reflector` and `MeshReflectorMaterial` use the same principle:

1. **Render the scene** from the perspective of a *virtual camera* that’s mirrored across the reflective plane.
2. **Capture that render** into a texture (framebuffer).
3. **Project that texture** onto the surface as if it’s reflecting the world.
4. Optionally **blur** or **blend** the texture to simulate rough reflections.

That’s exactly how **water reflections** or **mirror floors** work in 3D graphics.

### ⚖️ Performance Notes

* Real-time reflections can be expensive (extra render pass each frame).
* Use **lower resolution** or **mixStrength < 1** for better performance.
* Static mirrors can be baked into environment maps instead.

### 🧩 Summary

| Feature         | Drei `<Reflector />`             | Three.js Equivalent                                     |
| --------------- | ---------------------------------- | ------------------------------------------------------- |
| Exists in core? | ✅ Yes (in Drei)                   | ⚠️ No, only in `/examples/jsm/objects/Reflector.js` |
| Type            | React component                    | JS class                                                |
| Typical Use     | Mirror floor, water, puddle, glass | Same, but manual setup                                  |
| Uses            | Offscreen reflection render        | Same                                                    |
| Ease of use     | 1 line in JSX                      | ~30–50 lines manually in Three.js                      |

---

# ----GLTF/GLB Models

This is where **React Three Fiber (R3F)** really shines 🌟

Let’s go step-by-step through **GLTF / GLB models in React Three Fiber (R3F)** — what they are, how to load them, animate them, customize them, and optimize them — all in the  **React way** .

### ⚙️ Loading Models in R3F (with `useGLTF`)

R3F uses Drei’s `useGLTF()` hook — a wrapper around `GLTFLoader`.

It returns the model, materials, nodes, and animations ready for JSX rendering.

**Example:**

```jsx
import { Canvas } from '@react-three/fiber'
import { OrbitControls, useGLTF } from '@react-three/drei'

function Model() {
  const { scene, nodes, materials } = useGLTF('/models/robot.glb')
  return <primitive object={scene} />
}

export default function App() {
  return (
    <Canvas camera={{ position: [2, 2, 5] }}>
      <ambientLight intensity={0.5} />
      <directionalLight position={[2, 5, 3]} />
      <Model />
      <OrbitControls />
    </Canvas>
  )
}
```

✅ The `<primitive object={scene} />` tag adds the loaded Three.js scene directly to R3F’s scene graph.

✅ The model appears exactly as exported (materials, textures, animations, etc.).

##### 🧠 What `useGLTF()` Returns

```js
const { scene, nodes, materials, animations } = useGLTF('/car.glb')
```

| Property             | Description                                                 |
| -------------------- | ----------------------------------------------------------- |
| `scene`            | Root Three.js Object3D of your model                        |
| `nodes`            | Map of all named nodes/meshes (by name in Blender or app)   |
| `materials`        | Map of materials used in the model                          |
| `animations`       | Array of `AnimationClip`objects (if model has animations) |
| `parser`,`asset` | Additional internal metadata                                |

Example:

```js
nodes.Wheel_L.rotation.y = Math.PI / 2
materials.Paint.color.set('red')
```

##### 🎨 Directly Using Nodes and Materials

You can render **specific parts** of a model instead of the entire scene:

```jsx
function Car() {
  const { nodes, materials } = useGLTF('/car.glb')
  return (
    <group>
      <mesh geometry={nodes.Body.geometry} material={materials.Paint} />
      <mesh geometry={nodes.Wheel_FL.geometry} material={materials.Rubber} />
    </group>
  )
}
```

✅ Useful if you want to animate individual parts, like wheels or doors.

✅ `nodes` and `materials` names come directly from your 3D modeling software (like Blender).

### 🎥 Playing Animations

If your `.glb` includes animations (e.g. walking, rotating, opening door):

```jsx
import { useGLTF, useAnimations } from '@react-three/drei'
import { useRef } from 'react'

function Robot() {
  const group = useRef()
  const { scene, animations } = useGLTF('/robot.glb')
  const { actions } = useAnimations(animations, group)

  useEffect(() => {
    actions['Walk'].play()   // Play animation by name
  }, [actions])

  return <primitive ref={group} object={scene} />
}
```

✅ `useAnimations()` manages an internal `AnimationMixer` for you.

✅ You can play, pause, fade, and cross-fade between actions.

### 🧱 Transformations and Props

You can apply all Three.js transforms and props directly:

```jsx
<primitive
  object={scene}
  scale={1.5}
  position={[0, -1, 0]}
  rotation={[0, Math.PI / 2, 0]}
/>
```

Or wrap inside a `<group>`:

```jsx
<group position={[0, 1, 0]}>
  <primitive object={scene} />
</group>
```

### ⚡ Preloading Models

Drei provides:

```js
useGLTF.preload('/car.glb')
```

This loads the model **before it’s needed** — great for performance or route transitions.

### 🧮 Optimizing GLB Files

You can reduce load size and speed up rendering using:

* **Draco compression**
* **Meshopt compression**
* **KTX2 compressed textures**
* **Simplify geometry** in Blender or MeshLab

R3F will automatically handle Draco if your model is exported with it.

### 🧩 Example – Animated Robot Model

```jsx
function RobotModel() {
  const group = useRef()
  const { scene, animations } = useGLTF('/robot.glb')
  const { actions } = useAnimations(animations, group)

  useEffect(() => {
    actions['Idle'].play()
  }, [actions])

  useFrame(() => {
    group.current.rotation.y += 0.01
  })

  return <primitive ref={group} object={scene} scale={1.2} />
}
```

### 🧠 Why R3F Makes GLTF Easier

| Task             | Vanilla Three.js             | React Three Fiber               |
| ---------------- | ---------------------------- | ------------------------------- |
| Load model       | `GLTFLoader`manually       | `useGLTF('/model.glb')`       |
| Render model     | `scene.add(model)`         | `<primitive object={scene}/>` |
| Handle animation | `AnimationMixer`,`Clock` | `useAnimations()`             |
| Reactivity       | Manual                       | React hooks + declarative props |
| Preloading       | Custom loader                | `useGLTF.preload()`           |

So R3F turns what used to be **20+ lines** into  **1–3 React components** .

### ⚙️ Best Practices

✅ Always center & scale model in Blender before export.

✅ Use `.glb` for compact single-file assets.

✅ Name meshes in Blender → easier access in `nodes`.

✅ Optimize textures (WebP, KTX2).

✅ Use `Suspense` for lazy loading.

```jsx
<Suspense fallback={null}>
  <Model />
</Suspense>
```

### 🧾 Summary

| Concept                 | Description                                 |
| ----------------------- | ------------------------------------------- |
| **Format**        | `.gltf`(JSON) or `.glb`(binary)         |
| **Loader Hook**   | `useGLTF()`from Drei                      |
| **Animations**    | `useAnimations()`                         |
| **Preloading**    | `useGLTF.preload('/path')`                |
| **Render**        | `<primitive object={scene} />`            |
| **Optimizations** | Draco, Meshopt, KTX2                        |
| **Transforms**    | via React props (position, rotation, scale) |

---





---
