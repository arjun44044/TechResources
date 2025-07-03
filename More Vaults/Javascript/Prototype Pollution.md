Prototype pollution is a security vulnerability that occurs when an attacker manipulates the prototype of an object to inject malicious properties or methods into all instances of that object. This can lead to unexpected behavior in the application.

### How Prototype Pollution Works:

1. **Prototypal Inheritance:**
   - JavaScript objects have a prototype, which is a reference to another object.
   - If a property or method is not found on an object, JavaScript looks for it in the object's prototype chain.

2. **Manipulating the Prototype:**
   - An attacker may attempt to modify the prototype of a widely used object, such as `Object` or `Array`.
   - By adding or modifying properties in the prototype, these changes affect all instances of the object.  on the way

### Example of Prototype Pollution:

```javascript
// Initial object
const innocentObject = {};

// Malicious payload
const maliciousPayload = {
  __proto__: {
    pollution: 'Malicious property',
  },
};

// Apply the payload
Object.assign(innocentObject, maliciousPayload);

// Now all objects inherit the pollution property
console.log(innocentObject.pollution); // Outputs: Malicious property
```

In this example, the prototype of `innocentObject` is modified to include a `pollution` property. Any object that inherits from this prototype will now have the `pollution` property.

### Mitigations:

1. **Avoid Pollutable Objects:**
   - Be cautious with objects that are widely shared or used across the application.

2. **Input Validation:**
   - Validate and sanitize user inputs to prevent malicious data.

3. **Freeze Prototypes:**
   - Use `Object.freeze()` to make an object's prototype immutable.

   ```javascript
   Object.freeze(Object.prototype);
   ```

4. **Use Map or WeakMap:**
   - Instead of using plain objects, consider using `Map` or `WeakMap` for storing key-value pairs.

   ```javascript
   const myMap = new Map();
   myMap.set('key', 'value');
   ```

5. **Library Awareness:**
   - Be aware of the security practices of the libraries and frameworks you use.

### Real-world Consequences:

- Prototype pollution can lead to unauthorized access, data tampering, or unintended behavior in applications.
- It is crucial to be aware of the security implications and adopt best practices to prevent and mitigate prototype pollution vulnerabilities. 