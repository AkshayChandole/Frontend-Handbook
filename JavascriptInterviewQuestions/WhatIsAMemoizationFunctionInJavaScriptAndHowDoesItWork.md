# [What is a memoization function in JavaScript, and how does it work](#what-is-a-memoization-function-in-javascript-and-how-does-it-work)

### **Question:**  
What is a memoization function in JavaScript, and how does it work?  

### **Answer:**  
Memoization is an optimization technique used to improve the performance of functions by **caching** previously computed results. If the function is called with the same inputs, the cached result is returned instead of recomputing the value.  

#### ✅ **Basic Memoization Example**  
```javascript
function memoize(fn) {
    const cache = {}; // Store computed results
    return function (...args) {
        const key = JSON.stringify(args); // Create a unique key for arguments
        if (cache[key]) {
            console.log("Fetching from cache:", key);
            return cache[key]; // Return cached result
        }
        console.log("Computing result for:", key);
        const result = fn(...args); // Compute and store the result
        cache[key] = result;
        return result;
    };
}

// Example: Memoized Fibonacci function
function fibonacci(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

const memoizedFibonacci = memoize(fibonacci);

console.log(memoizedFibonacci(10)); // Computed
console.log(memoizedFibonacci(10)); // Cached result
```

![image](https://github.com/user-attachments/assets/889ab72f-3365-4b9e-bde9-31dfb450dfa9)


### 🔹 **How It Works?**  
1. A `cache` object stores previous results using function arguments as the key.  
2. When the function is called, it first checks if the result is already in the cache.  
3. If cached, it returns the stored value; otherwise, it computes the result and stores it.  

### ✅ **Benefits of Memoization:**  
- Reduces redundant calculations, improving performance.  
- Particularly useful for recursive functions like Fibonacci and Factorial.  
- Speeds up expensive computations in applications.  

This technique is commonly used in **dynamic programming**, optimizing functions with repeated calls for the same inputs. 🚀
