# [Build an JS function to measure the performance of function (sync or async)](#build-an-js-function-to-measure-the-performance-of-function-sync-or-async)

## Question: 
Build an JS function to measure the performance of function (sync or async).

## Solution:
Here’s a reusable JavaScript utility function called `measurePerformance` that works with both **synchronous** and **asynchronous** functions and measures their execution time in milliseconds.

---

### ✅ `measurePerformance(fn, ...args)`

#### 🔧 Code:

```javascript
async function measurePerformance(fn, ...args) {
    const start = performance.now();

    // Check if the function is async (returns a Promise)
    const result = fn(...args);
    if (result instanceof Promise) {
        await result;  // wait for async function to complete
    }

    const end = performance.now();
    const duration = end - start;

    console.log(`Execution Time: ${duration.toFixed(2)} ms`);

    return result instanceof Promise ? await result : result;
}
```

---

#### ✅ Usage Examples:

#### 1. **Synchronous Function**

```javascript
function syncSum(n) {
    let sum = 0;
    for (let i = 0; i < n; i++) {
        sum += i;
    }
    return sum;
}

measurePerformance(syncSum, 1e6); // Example call
```

---

#### 2. **Asynchronous Function**

```javascript
async function asyncDelay(ms) {
    return new Promise(resolve => setTimeout(() => resolve("Done!"), ms));
}

measurePerformance(asyncDelay, 1000).then(result => console.log(result));
```

---

#### 🧠 Notes:

* Uses `performance.now()` for high-resolution timing (in milliseconds).
* Works seamlessly for:

  * **Sync functions**
  * **Async functions (returning a Promise)**

---

Let me know if you'd like a version that logs more details (like function name or output size).
