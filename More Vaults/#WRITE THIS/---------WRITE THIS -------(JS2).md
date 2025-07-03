9. ==**canvas api in detail with properties and methods==**

The **Canvas API** in JavaScript is used for drawing and manipulating graphics on a web page. It allows you to create 2D drawings and render them inside a `<canvas>` HTML element. It supports various shapes, images, text, and even animations. The Canvas API is part of the HTML5 standard and is widely supported in modern browsers.

### Basic Workflow:
1. **Create a `<canvas>` element** in your HTML.
2. **Access the drawing context** (2D or WebGL) using JavaScript.
3. **Use Canvas methods** to draw shapes, images, text, or apply transformations.

### Example:
HTML:
```html
<canvas id="myCanvas" width="400" height="400"></canvas>
```

JavaScript:
```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');

// Draw a rectangle
ctx.fillStyle = 'blue';
ctx.fillRect(50, 50, 150, 100);
```

### Accessing the Canvas Context:
The primary method to interact with the `<canvas>` element is through the **2D rendering context**:
```javascript
const ctx = canvas.getContext('2d');
```

### Important Properties of `<canvas>`:
1. **`width` and `height`**:
   - Set the dimensions of the canvas element. Example: `<canvas width="400" height="400"></canvas>`.
   - These properties determine the drawing area’s resolution, not the canvas element’s visual size (which can be set via CSS).

### Methods for Drawing:
1. **Rectangles**:
   - `fillRect(x, y, width, height)`: Draws a filled rectangle.
   - `strokeRect(x, y, width, height)`: Draws a rectangular outline.
   - `clearRect(x, y, width, height)`: Clears the specified rectangular area, making it fully transparent.

2. **Paths**:
   - `beginPath()`: Starts a new path.
   - `moveTo(x, y)`: Moves the path to the specified point.
   - `lineTo(x, y)`: Adds a line to the current path.
   - `arc(x, y, radius, startAngle, endAngle)`: Creates an arc or circle.
   - `closePath()`: Closes the path, connecting the current point to the start of the path.
   - `stroke()`: Draws the outline of the shape defined by the path.
   - `fill()`: Fills the shape defined by the path.

   Example:
   ```javascript
   ctx.beginPath();
   ctx.moveTo(50, 50);
   ctx.lineTo(200, 50);
   ctx.lineTo(200, 200);
   ctx.closePath();
   ctx.stroke();
   ```

3. **Text**:
   - `fillText(text, x, y)`: Draws filled text at the specified coordinates.
   - `strokeText(text, x, y)`: Draws the outline of text.
   - `font`: Sets the font (e.g., `ctx.font = "20px Arial"`).
   - `textAlign`: Sets the alignment of text (`left`, `right`, `center`).
   - `textBaseline`: Sets the text baseline (`top`, `hanging`, `middle`, `alphabetic`, `ideographic`, `bottom`).

   Example:
   ```javascript
   ctx.font = "30px Arial";
   ctx.fillText("Hello World", 10, 50);
   ```

4. **Images**:
   - `drawImage(image, x, y)`: Draws an image at specified coordinates.
   - `drawImage(image, x, y, width, height)`: Draws a resized image.

   Example:
   ```javascript
   const img = new Image();
   img.src = 'example.jpg';
   img.onload = () => {
     ctx.drawImage(img, 10, 10);
   };
   ```

5. **Colors and Styles**:
   - `fillStyle`: Sets the color, gradient, or pattern used to fill shapes.
   - `strokeStyle`: Sets the color, gradient, or pattern for the stroke (outline).
   - `globalAlpha`: Sets the opacity level for drawings (between 0.0 and 1.0).

6. **Gradients**:
   - `createLinearGradient(x0, y0, x1, y1)`: Creates a linear gradient.
   - `createRadialGradient(x0, y0, r0, x1, y1, r1)`: Creates a radial gradient.
   - `addColorStop(offset, color)`: Adds a color stop to the gradient.

   Example:
   ```javascript
   const gradient = ctx.createLinearGradient(0, 0, 200, 0);
   gradient.addColorStop(0, 'red');
   gradient.addColorStop(1, 'blue');
   ctx.fillStyle = gradient;
   ctx.fillRect(10, 10, 200, 100);
   ```

7. **Transformations**:
   - `translate(x, y)`: Moves the canvas and its origin to (x, y).
   - `rotate(angle)`: Rotates the canvas by the specified angle (in radians).
   - `scale(x, y)`: Scales the canvas by the factors x and y.
   - `save()`: Saves the current state of the canvas (useful for resetting transformations).
   - `restore()`: Restores the canvas to the last saved state.

8. **Shadows**:
   - `shadowOffsetX`, `shadowOffsetY`: Horizontal and vertical offset of the shadow.
   - `shadowBlur`: Blurring effect for shadows.
   - `shadowColor`: Color of the shadow.

9. **Clipping**:
   - `clip()`: Clips a region of the canvas so that future drawing operations are restricted to that area.

### Canvas Pixel Manipulation:
- **`getImageData(x, y, width, height)`**: Returns an `ImageData` object containing the pixel data.
- **`putImageData(imageData, x, y)`**: Puts image data onto the canvas at the specified location.

### Example of pixel manipulation:
```javascript
const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
// Modify pixel data (e.g., invert colors)
for (let i = 0; i < imageData.data.length; i += 4) {
  imageData.data[i] = 255 - imageData.data[i];       // Red
  imageData.data[i+1] = 255 - imageData.data[i+1];   // Green
  imageData.data[i+2] = 255 - imageData.data[i+2];   // Blue
}
ctx.putImageData(imageData, 0, 0);
```

### Canvas Event Handling:
- The `<canvas>` element does not inherently handle events. You have to use standard DOM event listeners like `click`, `mousemove`, `mousedown`, etc.
  
  Example:
  ```javascript
  canvas.addEventListener('click', function(event) {
    const x = event.offsetX;
    const y = event.offsetY;
    console.log(`Clicked at: ${x}, ${y}`);
  });
  ```

### Summary of Key Methods:
- **Drawing Rectangles**: `fillRect()`, `strokeRect()`, `clearRect()`.
- **Drawing Paths**: `beginPath()`, `moveTo()`, `lineTo()`, `arc()`, `fill()`, `stroke()`.
- **Drawing Text**: `fillText()`, `strokeText()`.
- **Drawing Images**: `drawImage()`.
- **Transformations**: `translate()`, `rotate()`, `scale()`, `save()`, `restore()`.
- **Pixel Manipulation**: `getImageData()`, `putImageData()`.

Canvas provides a versatile API for creating dynamic and interactive graphics. It’s often used for games, interactive animations, data visualizations, and image manipulation.


The `filter` property in the Canvas API allows you to apply various image filters (such as blur, brightness, contrast, grayscale, etc.) to the content you are drawing on the canvas. This property is similar to the CSS `filter` property and works with canvas 2D contexts to manipulate visual effects on drawn images, text, or shapes.

### Basic Syntax:

```javascript
ctx.filter = 'filter-function';
```

### Example:

```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');

// Set a blur filter
ctx.filter = 'blur(5px)';

// Draw a rectangle with the blur effect
ctx.fillStyle = 'red';
ctx.fillRect(10, 10, 100, 100);

// Reset filter to none (removes the filter effect for subsequent drawings)
ctx.filter = 'none';
```

### Filter Functions:

Here are some of the most common filter functions you can apply to a canvas:

1. **`blur(radius)`**:
   - Applies a Gaussian blur to the image.
   - Example: `blur(5px)` will blur the image by 5 pixels.
   
   ```javascript
   ctx.filter = 'blur(10px)';
   ```

2. **`brightness(value)`**:
   - Adjusts the brightness of the image.
   - `1` is the default (no change), values greater than `1` increase brightness, and values between `0` and `1` decrease brightness.
   
   ```javascript
   ctx.filter = 'brightness(0.5)'; // Darken the image
   ctx.filter = 'brightness(2)';   // Increase brightness
   ```

3. **`contrast(value)`**:
   - Adjusts the contrast of the image.
   - `1` is the default, values greater than `1` increase contrast, and values between `0` and `1` decrease contrast.
   
   ```javascript
   ctx.filter = 'contrast(2)';  // Double the contrast
   ```

4. **`grayscale(value)`**:
   - Converts the image to grayscale.
   - A value of `1` fully desaturates the image (makes it grayscale), while `0` applies no grayscale effect.
   
   ```javascript
   ctx.filter = 'grayscale(1)';  // Convert image to grayscale
   ```

5. **`invert(value)`**:
   - Inverts the colors of the image.
   - A value of `1` fully inverts the colors, while `0` applies no effect.
   
   ```javascript
   ctx.filter = 'invert(1)';  // Fully invert colors
   ```

6. **`opacity(value)`**:
   - Changes the opacity of the image.
   - Values range from `0` (completely transparent) to `1` (fully opaque).
   
   ```javascript
   ctx.filter = 'opacity(0.5)';  // Set opacity to 50%
   ```

7. **`saturate(value)`**:
   - Adjusts the saturation of the image.
   - A value of `1` is the default. Values greater than `1` increase saturation, and values between `0` and `1` decrease saturation.
   
   ```javascript
   ctx.filter = 'saturate(2)';  // Double the saturation
   ```

8. **`sepia(value)`**:
   - Converts the image to sepia tones.
   - A value of `1` fully applies the sepia effect, while `0` applies no effect.
   
   ```javascript
   ctx.filter = 'sepia(1)';  // Apply sepia effect
   ```

9. **`drop-shadow(offsetX offsetY blurRadius color)`**:
   - Applies a drop shadow to the image.
   - You can specify the horizontal and vertical offsets (`offsetX`, `offsetY`), the blur radius, and the shadow color.
   
   ```javascript
   ctx.filter = 'drop-shadow(10px 10px 5px black)';
   ```

### Multiple Filters:

You can apply multiple filters by separating them with spaces, similar to how you apply multiple CSS filters.

Example:
```javascript
ctx.filter = 'grayscale(1) blur(3px) contrast(2)';
```

This example will first convert the image to grayscale, then apply a 3-pixel blur, and finally increase the contrast by a factor of 2.

### Resetting the Filter:

After applying a filter, subsequent drawings will inherit the filter effect unless the filter is reset to `none`. Resetting the filter ensures that any new drawings are not affected by the previously applied filters.

Example:
```javascript
ctx.filter = 'none';
```

### Combining `filter` with Drawing Operations:

The `filter` property can be combined with other canvas drawing operations, such as drawing images, shapes, or text.

```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');

// Apply multiple filters
ctx.filter = 'blur(4px) brightness(0.8)';

// Draw an image with the filters applied
const img = new Image();
img.src = 'image.jpg';
img.onload = () => {
    ctx.drawImage(img, 0, 0);
};

// Reset the filter for future drawings
ctx.filter = 'none';
```

### Browser Support:

The `filter` property in the Canvas API is supported by most modern browsers, but older browsers may not fully support it. It is important to check compatibility if targeting older browsers.

### Summary of Methods:
- `ctx.filter = "filter-function"`: Applies the filter.
- Multiple filters can be chained together by separating them with spaces.
- Reset filters with `ctx.filter = "none"`.

This feature is powerful for enhancing visual effects on images and shapes without needing external image manipulation tools.


10. ==**canvas save() vs restore()==**

In the Canvas API, `save()` and `restore()` are used to manage the state of the drawing context. They allow you to save and restore specific settings, such as transformations, styles, clipping paths, etc., so that you can temporarily modify the canvas state without affecting other drawings. These methods are especially useful when you want to apply certain effects or transformations to only specific parts of your canvas drawing.

### `save()`:
- **Purpose**: Saves the current state of the entire canvas context (e.g., transformations, styles, paths).
- **How it works**: It pushes the current state onto a stack, allowing you to restore it later with `restore()`.
- **Common use case**: When you want to make temporary changes (such as scaling or rotating) to the canvas, and then revert to the previous state after the temporary effect.

### `restore()`:
- **Purpose**: Restores the most recent saved state of the canvas from the stack.
- **How it works**: It pops the last saved state from the stack and applies it back to the canvas context.
- **Common use case**: When you want to revert back to the original state of the canvas after making temporary changes.

### Example of `save()` and `restore()` in action:

```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');

// Draw a rectangle (default settings)
ctx.fillStyle = 'blue';
ctx.fillRect(10, 10, 100, 100);

// Save the current state (blue fill color)
ctx.save();

// Change the fill color and apply rotation
ctx.fillStyle = 'red';
ctx.rotate(45 * Math.PI / 180);  // Rotate by 45 degrees
ctx.fillRect(150, 10, 100, 100); // Draw a red rotated rectangle

// Restore the previous state (blue fill color, no rotation)
ctx.restore();

// Draw another rectangle, it will be blue (as the state was restored)
ctx.fillRect(300, 10, 100, 100);
```

### Key Points:
1. **State Stack**: Every time `save()` is called, the current state is pushed onto the stack. When `restore()` is called, the last saved state is popped from the stack.
2. **Restorable Properties**:
   - Transformations (like `rotate()`, `scale()`, and `translate()`).
   - Stroke and fill styles (`fillStyle`, `strokeStyle`).
   - Line styles (`lineWidth`, `lineCap`, etc.).
   - Shadow effects (`shadowBlur`, `shadowColor`, etc.).
   - Clipping paths.
3. **Temporary Changes**: Use `save()` before making changes (like rotations or scaling) and `restore()` to undo those changes.

### When to Use `save()` and `restore()`:
- **Complex Drawings**: When drawing complex scenes with multiple styles or transformations, `save()` and `restore()` help maintain consistency and manage the drawing process.
- **Modular Drawing**: If you're applying transformations or changes to only specific parts of your drawing and you don't want those changes to affect the rest of the canvas.

These methods provide an elegant way to preserve and manage the canvas's state in a controlled and efficient manner.