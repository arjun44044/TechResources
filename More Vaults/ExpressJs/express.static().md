--`express.static()` is a built-in middleware function in Express.js that serves static files, such as HTML, images, CSS, and JavaScript files, from a specified directory. This middleware is commonly used to serve the client-side assets of a web application.

--Here's how you can use `express.static()` in an Express.js application:
```javascript
const express = require('express');
const app = express();

// Serve static files from the 'public' directory
app.use(express.static('public'));

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}/`);
});
```
   In this example, the `express.static('public')` middleware is used to serve static files from the 'public' directory.

##### -->**Specifying Multiple Static Directories:**
   Eg--
   ```javascript
const express = require('express');
const app = express();

// Serve static files from the 'public' and 'images' directories
app.use(express.static('public'));
app.use(express.static('images'));

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}/`);
});
```
  In this example, static files are served from both the 'public' and 'images' directories. The middleware checks each directory in the order they are specified for the requested file.

#### --> 2nd Parameter of express.static()
You can provide options as the second parameter to configure the behavior of `express.static()`. In this example, the `maxAge` option is set to cache static assets for one year in milliseconds.
  ```javascript
const express = require('express');
const app = express();

// Serve static files from the 'public' directory with specific options
app.use(express.static('public', { maxAge: 31536000 }));

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}/`);
});
```

Some of the commonly used options properties--
1.**`dotfiles` (default: 'ignore'):**
- Determines how dotfiles (files/folders starting with a dot `.`) are treated.
- Possible values: `'allow'`, `'deny'`, `'ignore'`.
Eg--` app.use(express.static('public', { dotfiles: 'ignore' }));`
2.**`extensions` (default: false):**
- An array of file extensions to serve. If set to `false`, it will serve files with any extension.
- Example: `{ extensions: ['html', 'css', 'js'] }`
Eg-- `app.use(express.static('public', { extensions: ['html', 'css', 'js'] }));`
3.**`fallthrough` (default: true):**
   - If set to `false`, this middleware will return a 404 response if a file is not found instead of continuing to the next middleware.
- `app.use(express.static('public', { fallthrough: false }));`
4.**`immutable` (default: false):**
    If set to `true`, it will set the `Cache-Control` header to `'immutable'`, indicating that the resource is considered immutable and will not change.
     `app.use(express.static('public', { immutable: true }));`
5.**`index` (default: 'index.html'):**
    - The file to serve when a directory is requested.
    - Example: `{ index: 'default.html' }`
    `app.use(express.static('public', { index: 'default.html' }));`
6 **`maxAge` (default: 0):**
    - Sets the maximum age (in milliseconds) for caching static assets in the browser.
    - Example: `{ maxAge: 86400000 }` (cache for one day)
    `app.use(express.static('public', { maxAge: 86400000 }));`
```