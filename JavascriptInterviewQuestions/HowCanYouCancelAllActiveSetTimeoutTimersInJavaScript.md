# [How can you cancel all active setTimeout timers in JavaScript](#how-can-you-cancel-all-active-settimeout-timers-in-javascript)

### **Question:**  
How can you cancel all active `setTimeout` timers in JavaScript?  

### **Answer:**  
JavaScript does not provide a built-in function to clear all `setTimeout` timers at once. However, you can achieve this manually by tracking timeout IDs or looping through possible timeout IDs.  

#### ✅ **Method 1: Loop Through Timeout IDs**  
```javascript
(function clearAllTimeouts() {
    const highestId = setTimeout(() => {}); // Get the highest timeout ID
    for (let i = 0; i <= highestId; i++) {
        clearTimeout(i); // Cancel each timeout
    }
})();
```

##### **🔹 How It Works?**  
1. Calls `setTimeout(() => {})` to obtain the highest assigned timeout ID.  
2. Iterates from `0` to that ID, calling `clearTimeout(i)` on each.  
3. Cancels all pending timeouts before they execute.  

---

#### ✅ **Method 2: Store and Clear Timeout IDs**  
A better approach is storing timeout IDs in an array and clearing them explicitly:  

```javascript
const timeouts = [];

timeouts.push(setTimeout(() => console.log("Task 1"), 1000));
timeouts.push(setTimeout(() => console.log("Task 2"), 2000));
timeouts.push(setTimeout(() => console.log("Task 3"), 3000));

// Clear all stored timeouts
timeouts.forEach(clearTimeout);
```

##### **🔹 Why This Method?**  
- More efficient since it only clears timeouts you created.  
- Avoids iterating over unnecessary IDs.  

Both methods effectively prevent delayed code execution from `setTimeout()`. 🚀
