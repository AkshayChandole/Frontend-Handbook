# [How can you implement `Promise.any()` in JavaScript?](#how-can-you-implement-promise-any-in-javascript)

### **Question:**  
How can you implement `Promise.any()` in JavaScript?  

---

### **Answer:**  
`Promise.any()` is a built-in JavaScript method that takes an array of promises and returns the first promise that **resolves**. If all promises reject, it returns an `AggregateError`.  

If `Promise.any()` is not available (e.g., in older environments), we can implement it manually.  

---

### ✅ **Custom Implementation of `Promise.any()`**  
```javascript
function promiseAny(promises) {
    return new Promise((resolve, reject) => {
        let errors = []; // Store errors from rejected promises
        let pendingCount = promises.length; // Track remaining promises

        if (pendingCount === 0) {
            reject(new AggregateError([], "All promises were rejected"));
            return;
        }

        promises.forEach((promise, index) => {
            Promise.resolve(promise)
                .then(resolve) // Resolve as soon as one promise succeeds
                .catch((error) => {
                    errors[index] = error; // Store errors by index
                    pendingCount--;

                    if (pendingCount === 0) {
                        reject(new AggregateError(errors, "All promises were rejected"));
                    }
                });
        });
    });
}
```

---

### ✅ **Example Usage & Output:**  
```javascript
const p1 = new Promise((_, reject) => setTimeout(() => reject("Error in P1"), 1000));
const p2 = new Promise((resolve) => setTimeout(() => resolve("P2 Resolved"), 500));
const p3 = new Promise((_, reject) => setTimeout(() => reject("Error in P3"), 700));

promiseAny([p1, p2, p3])
    .then(console.log)  // Expected output: "P2 Resolved"
    .catch((error) => console.log(error.errors)); // If all fail, logs all errors
```

---

### ✅ **Output:**
```
P2 Resolved
```
_(If all promises reject, an `AggregateError` with all error messages is thrown.)_

---

### 🔹 **How It Works?**  
1. Wraps each promise in `Promise.resolve()` to handle non-promise values.  
2. Resolves as soon as the first promise fulfills.  
3. If all promises reject, it returns an `AggregateError` containing all rejection reasons.  

---

### ✅ **Key Benefits of `Promise.any()`**  
✔ Useful when you need the first successful result.  
✔ Avoids waiting for slow failing promises.  
✔ Great for implementing **fallback strategies** in API requests. 🚀
