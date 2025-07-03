In JavaScript, the `URL` object represents a Uniform Resource Locator (URL) and provides utility methods for working with URLs. It allows you to parse URLs, construct URLs from their components, and perform various operations on URLs.

Here's an overview of the `URL` object:

1. **Constructor:**
   - The `URL` constructor creates a new `URL` object representing the specified URL string or another `URL` object.
   - Syntax: `new URL(url [, base])`
     - `url`: A string representing the URL to be parsed or another `URL` object to copy.
     - `base` (optional): A base URL to resolve relative URLs against. This is useful when constructing a URL from a relative path.

2. **Properties:**
   - `href`: The full URL string.
   - `origin`: The origin (protocol, hostname, and port) of the URL.
   - `protocol`: The protocol (e.g., "http:", "https:", "ftp:") of the URL.
   - `username`, `password`: The username and password specified in the URL (if any).
   - `hostname`: The hostname of the URL (e.g., "example.com").
   - `port`: The port number of the URL (default port numbers are omitted).
   - `pathname`: The path component of the URL.
   - `search`: The query string component of the URL (including the leading "?" character).
   - `hash`: The fragment identifier component of the URL (including the leading "#" character).

3. **Methods:**
   - `toString()`: Returns the full URL string.
   - `toJSON()`: Returns the full URL string.
   - `searchParams`: Returns a `URLSearchParams` object representing the query parameters of the URL.
   - `href`, `origin`, `protocol`, `username`, `password`, `hostname`, `port`, `pathname`, `search`, `hash`: Getters and setters for each component of the URL.

Here's an example demonstrating the usage of the `URL` object:

```javascript
// Create a new URL object
const url = new URL('https://example.com:8080/path/to/resource?param1=value1&param2=value2#section');

// Access URL components
console.log(url.protocol); // Output: "https:"
console.log(url.hostname); // Output: "example.com"
console.log(url.port);     // Output: "8080"
console.log(url.pathname); // Output: "/path/to/resource"
console.log(url.search);   // Output: "?param1=value1&param2=value2"
console.log(url.hash);     // Output: "#section"

// Get query parameters using URLSearchParams
const params = url.searchParams;
console.log(params.get('param1')); // Output: "value1"
console.log(params.get('param2')); // Output: "value2"

// Modify URL components
url.hostname = 'newexample.com';
url.searchParams.set('param1', 'newvalue1');

console.log(url.href); // Output: "https://newexample.com:8080/path/to/resource?param1=newvalue1&param2=value2#section"
```

In this example, we create a `URL` object representing a sample URL and then access its various components using the properties of the object. We also demonstrate how to modify the URL components and retrieve query parameters using the `URLSearchParams` object.