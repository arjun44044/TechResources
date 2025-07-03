Memoization is an optimization technique in JavaScript where the results of expensive function calls are cached, so that if the same inputs occur again, the cached result can be returned instead of re-computing the function. This can significantly improve the performance of functions with repeated or redundant calls. Here's a basic explanation along with an example:

### Basic Memoization Example:

```javascript
function memoize(func) {
  const cache = {};

  return function (...args) {
    const key = JSON.stringify(args);

    if (cache[key]) {
      console.log('Returning from cache:', cache[key]);
      return cache[key];
    } else {
      const result = func.apply(this, args);
      cache[key] = result;
      console.log('Calculating and caching:', result);
      return result;
    }
  };
}

// Example function to be memoized
function add(a, b) {
  console.log('Adding:', a, b);
  return a + b;
}

// Memoized version of the function
const memoizedAdd = memoize(add);

// Test the memoized function
console.log(memoizedAdd(2, 3)); // Calculates and caches: 5
console.log(memoizedAdd(2, 3)); // Returning from cache: 5
console.log(memoizedAdd(4, 5)); // Calculates and caches: 9
console.log(memoizedAdd(4, 5)); // Returning from cache: 9
```

In this example:

1. The `memoize` function takes another function (`func`) as its argument.
2. It returns a new function that performs memoization for the original function.
3. The `cache` object is used to store the results of function calls based on their input arguments.
4. When the memoized function is called, it checks if the result for the given arguments is already in the cache. If yes, it returns the cached result; otherwise, it calculates the result, caches it, and returns the result.

### Note:
- This basic example uses simple JSON stringification of the arguments to generate a unique key for the cache. Depending on the use case, a more sophisticated approach might be necessary, especially if the function takes complex objects or functions as arguments.
- Memoization is beneficial for functions where the same inputs result in the same outputs, as it avoids redundant calculations. It's particularly useful for recursive or computationally expensive functions.