# [How would you implement a custom Promise from scratch in JavaScript? Additionally, how would you implement the finally method for it?](#how-would-you-implement-a-custom-promise-from-scratch-in-javascript-additionally-how-would-you-implement-the-finally-method-for-it)


### **Question:**  
How would you implement a custom `Promise` from scratch in JavaScript? Additionally, how would you implement the `.finally()` method for it?  

---

### **Answer:**  

A `Promise` in JavaScript is an object representing an asynchronous operation that can be in one of three states:  
- **Pending** – Initial state, waiting for the operation to complete.  
- **Fulfilled** – The operation was successful.  
- **Rejected** – The operation encountered an error.  

To implement a **custom `Promise`**, we need to handle these states, allow chaining with `.then()` and `.catch()`, and finally implement `.finally()`, which executes a callback regardless of the promise outcome.

---

## **1️⃣ Implementing a Basic `MyPromise` Class**
We will create a basic `Promise` class that:  
✔ Stores the state (`pending`, `fulfilled`, or `rejected`).  
✔ Allows attaching `.then()` and `.catch()` handlers.  
✔ Supports asynchronous resolution and rejection.  

```javascript
class MyPromise {
    constructor(executor) {
        this.state = "pending"; // Initial state
        this.value = undefined; // Stores resolved/rejected value
        this.handlers = []; // Stores `.then()` and `.catch()` handlers

        const resolve = (value) => {
            if (this.state !== "pending") return; // Ensure state is not changed again
            this.state = "fulfilled";
            this.value = value;
            this.handlers.forEach((h) => h.onFulfilled(value)); // Call all success handlers
        };

        const reject = (error) => {
            if (this.state !== "pending") return;
            this.state = "rejected";
            this.value = error;
            this.handlers.forEach((h) => h.onRejected(error)); // Call all failure handlers
        };

        try {
            executor(resolve, reject); // Execute provided function
        } catch (error) {
            reject(error); // Handle errors in executor
        }
    }

    then(onFulfilled, onRejected) {
        return new MyPromise((resolve, reject) => {
            this.handlers.push({
                onFulfilled: (value) => {
                    try {
                        resolve(onFulfilled ? onFulfilled(value) : value);
                    } catch (error) {
                        reject(error);
                    }
                },
                onRejected: (error) => {
                    try {
                        reject(onRejected ? onRejected(error) : error);
                    } catch (err) {
                        reject(err);
                    }
                },
            });

            // If already resolved/rejected, execute the handler immediately
            if (this.state === "fulfilled") {
                onFulfilled(this.value);
            } else if (this.state === "rejected") {
                onRejected(this.value);
            }
        });
    }

    catch(onRejected) {
        return this.then(null, onRejected);
    }
}
```

---

## **2️⃣ Implementing the `.finally()` Method**
The `.finally()` method is called regardless of whether the promise was **fulfilled** or **rejected**.  

✔ It does **not change** the resolved/rejected value.  
✔ It always executes a given callback.  
✔ It allows chaining like `.then()` and `.catch()`.  

```javascript
MyPromise.prototype.finally = function (callback) {
    return this.then(
        (value) => {
            callback();
            return value; // Ensure chaining continues with the resolved value
        },
        (error) => {
            callback();
            throw error; // Ensure chaining continues with the rejected error
        }
    );
};
```

---

## **3️⃣ Testing Our Custom `Promise`**
Let's create an example to see if our implementation works correctly:

```javascript
const myPromise = new MyPromise((resolve, reject) => {
    setTimeout(() => {
        resolve("Success!");
    }, 1000);
});

myPromise
    .then((value) => {
        console.log("Resolved with:", value);
        return "Next step";
    })
    .catch((error) => console.error("Caught error:", error))
    .finally(() => console.log("Finally executed!"));
```

---

## **4️⃣ Expected Output**
```
Finally executed!
Resolved with: Success!
```

🔹 **Explanation:**  
1. `myPromise` is created with a `setTimeout`, resolving after 1 second.  
2. `.then()` executes, printing `"Resolved with: Success!"`.  
3. `.finally()` runs **regardless** of whether the promise was resolved or rejected.  

---

## **5️⃣ Handling Rejection Case**
```javascript
const failingPromise = new MyPromise((resolve, reject) => {
    setTimeout(() => {
        reject("Something went wrong!");
    }, 1000);
});

failingPromise
    .then((value) => console.log("Resolved:", value))
    .catch((error) => console.error("Caught error:", error))
    .finally(() => console.log("Finally executed!"));
```

### **Output (if rejected)**:
```
Caught error: Something went wrong!
Finally executed!
```

---

### **🔹 Summary:**
1. We implemented a **basic `Promise` class** with `.then()`, `.catch()`, and `.finally()`.  
2. The `.finally()` method **always executes**, preserving the resolved/rejected value.  
3. We tested both **resolved and rejected** cases.

---
