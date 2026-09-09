# Async and Promises

## **Asynchronous JavaScript**

JavaScript is **synchronous and single-threaded by default**. Synchronous means that JavaScript executes code one operation after another, in order. Single-threaded means that, under the normal JavaScript execution model, there is one main thread responsible for executing JavaScript code.

For example, if we have three statements, the first statement executes, then the second, and then the third. The next statement does not start executing until the previous statement has finished.

```jsx
console.log("1");
console.log("2");
console.log("3");
```

The output will be `1`, `2`, `3`.

The important point is that **each operation waits for the previous operation to complete**. This is the basic synchronous behaviour of JavaScript.

### **Execution Context and the Call Stack**

JavaScript executes code using an **Execution Context**. The Global Execution Context is created when the program starts, and function execution creates its own execution context. The **Call Stack** keeps track of the currently executing functions and their execution contexts. JavaScript executes the code on this stack one step at a time.

For example, when a function is called, its execution context is pushed onto the Call Stack. Once the function finishes, its context is removed from the stack. This follows the **LIFO (Last In, First Out)** principle. For understanding asynchronous JavaScript, the important thing to remember is simply that **JavaScript’s main execution happens on a single Call Stack, one operation at a time**.

### **Blocking vs Non-Blocking Code**

**Blocking code** is code that prevents the rest of the program from continuing until the current operation is completed. In other words, the execution flow has to wait.

A simple way to understand blocking behaviour is to imagine being told, “Wait here until I bring you a glass of water.” Until the water arrives, you cannot continue with your other work. The current task is blocking your progress.

**Non-blocking code**, on the other hand, allows the program to continue doing other work while a time-consuming operation is being handled elsewhere. Once the operation is complete, JavaScript can be notified and continue with the corresponding work.

The important point is that **non-blocking does not mean the operation finishes immediately**. It means that JavaScript does not have to sit idle waiting for that operation to finish.

### **Why Do We Need Non-Blocking Code?**

Consider an operation such as reading a file. The program may have to request the operating system to access the file and retrieve its contents. That operation can take some amount of time. If JavaScript waits synchronously for the file operation to finish, the main execution thread cannot do anything else during that period. This is blocking behaviour.

With a non-blocking approach, JavaScript can effectively say, “Start this operation and let me know when it is finished.” JavaScript can then continue executing other code instead of waiting.

This is one of the major reasons asynchronous programming is important. However, **non-blocking code is not always automatically better**. The choice depends on what the program needs to do.

For example, suppose a user’s registration data needs to be stored in a database. If we immediately send the user `"Registration successful"` without waiting for the database operation to complete, the database operation might actually fail after the message has already been shown.

In such a situation, we need to wait for the database operation to tell us whether it succeeded before confirming the registration. Therefore, **blocking and non-blocking behaviour depend on the requirement of the operation**. We should not assume that every operation must always be non-blocking.

### **The Complete Working**

**JavaScript Engine → Call Stack → Web API → Callback Queue → Event Loop → Call Stack**

<img src="/Assets/Async.png" alt="BOM.png" width="600" >


#### JavaScript Engine

A JavaScript engine, such as V8, is responsible for executing JavaScript. At a basic level, the engine contains components such as the **Memory Heap** and **Call Stack**. However, when JavaScript runs in a browser, it gets additional features from the **browser runtime environment**. Similarly, when JavaScript runs in Node.js, it gets additional features provided by the Node.js runtime. The **JavaScript Engine** contains the **Memory Heap** and **Call Stack**. JavaScript code executes on the Call Stack, one operation at a time. When JavaScript encounters an asynchronous operation such as `setTimeout()`, `setInterval()`, or `fetch()`, it delegates that operation to the appropriate **Web API** instead of keeping it on the Call Stack.

#### Web API

In a browser, the runtime environment provides various **Web APIs** that JavaScript can use. Examples include timer APIs such as `setTimeout()` and `setInterval()`, browser event mechanisms, `fetch()`, and many other browser-provided features. These APIs are **not part of the JavaScript engine itself**. They are provided by the environment in which JavaScript is running.

The **Web API** handles the asynchronous operation. For example, `setTimeout()` waits for the specified time, while `fetch()` handles a network request. When the operation is ready, its callback is sent to a queue rather than directly to the Call Stack.

#### Callback Registration

When we provide a callback to an asynchronous API, the runtime can **register that callback** and wait for the required condition to occur. For example, with `setTimeout()`, we provide a callback and a delay. The runtime keeps track of the timer. JavaScript does not continuously wait for the timer. Once the timer condition is satisfied, the callback becomes ready to be executed and is placed into a queue. The callback will eventually be picked up and executed by JavaScript.

#### Task Queue and Callback Queue

The **Task Queue** is a queue where callbacks that are ready to be executed can wait until the JavaScript Call Stack is available. The important thing to understand is that a callback does not simply jump directly onto the Call Stack when an asynchronous operation finishes. Instead, it becomes available through the appropriate queue, and the **Event Loop** helps coordinate when that callback can be moved to the Call Stack.

There are two queues shown in the diagram. The **Task Queue (Macrotask Queue)** contains callbacks from operations such as timers and other browser events. The **Callback Queue (**Microtask Queue), shown as the **higher-priority Callback Queue**, contains callbacks associated with **Promises**, such as those used by `fetch()`.

#### Event Loop

The **Event Loop** is the mechanism that coordinates the Call Stack and the queues containing callbacks waiting to execute. If JavaScript is currently executing something on the Call Stack, the Event Loop does not simply interrupt it and insert another callback in the middle.

It continuously checks whether the Call Stack is free. When JavaScript has finished executing the current code and the Call Stack becomes empty, the Event Loop allows waiting callbacks to move to the Call Stack for execution. The higher-priority **Microtask Queue / Callback Queue is processed before the Task Queue**, which is why Promise-related callbacks can execute before normal task callbacks.

So the diagram can be remembered as:

**JavaScript code → Call Stack → asynchronous operation → Web API → callback placed in a queue → Event Loop checks the Call Stack → callback moves to Call Stack → callback executes.**

The most important idea is that **the JavaScript engine remains single-threaded, but the runtime environment provides Web APIs and queues that allow JavaScript to handle asynchronous operations without blocking the main execution flow.**

---

## **`setTimeout()`**

`setTimeout()` is used when you want a particular function to execute **once after a specified delay**.

Its basic syntax is: `setTimeout(handler, timeout)`

Here, `handler` is the function that should execute after the delay, and `timeout` specifies the delay in **milliseconds**. For example, `setTimeout(changeText, 2000)` means that `changeText` should be executed after approximately 2 seconds.

```jsx
setTimeout(function () {
    console.log("Hello");
}, 2000);

const changeText = function () {
    document.querySelector("h1").innerHTML = "Hey, wassup";
};

setTimeout(changeText, 2000);
```

The important thing to notice is that the function is passed as a **reference** rather than being executed immediately. Therefore, we write `setTimeout(changeText, 2000)` and **not** `setTimeout(changeText(), 2000)`. `changeText` tells `setTimeout()` which function to execute later, whereas `changeText()` executes the function immediately and passes its return value.

The first argument of `setTimeout()` is called the **handler**. A handler is essentially a function that should be executed when the timer is ready. It is commonly referred to as a **callback function** because the function is given to another function/API to be called later. The handler does not necessarily have to be an anonymous function. You can directly provide an anonymous function, or you can define a named function separately and pass its reference.

For example, both approaches are valid:

`setTimeout(function () { console.log("Hello"); }, 2000)`

or

```jsx
const sayHello = function () {
    console.log("Hello");
};

setTimeout(sayHello, 2000);
```

In both cases, the function is executed once after the specified delay.

After approximately 2 seconds, `"Hello"` is printed once. It does not continue printing every 2 seconds. If you w ant something to repeatedly execute at regular intervals, you use `setInterval()` instead.

### **Delay in `setTimeout()`**

The second argument represents the delay in **milliseconds**. Therefore, `1000` milliseconds is approximately 1 second, `2000` milliseconds is approximately 2 seconds, and so on.

For example:

`setTimeout(sayHello, 2000)`

means that the callback becomes eligible to execute after approximately 2 seconds. However, **2 seconds does not mean that the callback is guaranteed to execute exactly at the 2-second mark**. The timer only specifies the minimum delay before the callback can be executed. The callback still has to go through the asynchronous execution mechanism and wait for the Call Stack to become available.

So conceptually:

**Delay expires → callback becomes ready → callback enters the appropriate queue → Event Loop eventually moves it to the Call Stack → callback executes.**

This is why a `setTimeout(..., 0)` callback does **not** execute immediately.

**Example**

```jsx
const changeText = function () {
    document.querySelector("h1").innerHTML = "Hey, wassup";
};

setTimeout(changeText, 2000);
```

---

## **`clearTimeout()`**

The clearTimeout() function in javascript clears the timeout which has been set by the **`setTimeout()`** function before that.

Its syntax is: `clearTimeout(timeoutId)`

The important part is that `clearTimeout()` needs to know **which particular timeout should be cancelled**. Therefore, we store the value returned by `setTimeout()`.

```jsx
const changeMe = setTimeout(changeText, 2000);

clearTimeout(changeMe);
```

Here, `setTimeout()` returns a value that identifies the scheduled timer. We store that value in `changeMe`, and then pass it to `clearTimeout()` to cancel that particular timeout.

This is especially useful when the cancellation should happen because of some event, such as clicking a **Stop** button.

**Example**

```html
<p id="message">Waiting...</p>
<button id="cancel">Cancel</button>

<script>
    const message = document.querySelector("#message");
    const cancelButton = document.querySelector("#cancel");

    const timerId = setTimeout(function () {
        message.textContent = "Time is up!";
    }, 3000);

    cancelButton.addEventListener("click", function () {
        clearTimeout(timerId);
        message.textContent = "Timer cancelled!";
    });
</script>
```

---

## **`setInterval()`**

The **setInterval() method** calls a function at specified intervals (in milliseconds). It continues calling the function until clearInterval() is called or the window is closed. It is similar to `setTimeout()`, but instead of executing the callback **once**, it repeatedly executes the callback after each specified interval. This method is useful for tasks that need periodic execution, like updating animations or refreshing data.

Its basic syntax is: `setInterval(handler, interval)`

For example:

```jsx
setInterval(function () {
    console.log("Hello");
}, 1000);
```

Here, the callback is executed approximately every 1 second. Unlike `setTimeout()`, the operation continues repeatedly until the interval is cancelled.

The second argument of `setInterval()` represents the interval in milliseconds. You can see in the example that the callback should be scheduled at approximately 1-second intervals. The word **interval** is important here. Instead of saying “wait for 1 second and execute once,” we are essentially saying “use a 1-second interval between executions.”

The difference can be remembered very simply:

**`setTimeout()` → wait → execute once**

**`setInterval()` → wait → execute → wait → execute → keep repeating**

### **Passing Arguments to `setInterval()`**

`setInterval()` can also receive additional arguments that are passed to the callback function.

For example:

```jsx
const sayDate = function (str) {
    console.log(str, Date.now());
};

setInterval(sayDate, 1000, "hi");
```

Here, `"hi"` is passed as an argument to `sayDate()`. Therefore, the callback behaves approximately like: `sayDate("hi")` on each execution.

So the structure can be understood as:

`setInterval(handler, interval, argument1, argument2, ...)`

The first argument is the callback, the second is the interval, and subsequent arguments are values that should be supplied to that callback.

### **Note: `setTimeout(0)`**

A common misconception is that `setTimeout(callback, 0)` means **“execute the callback immediately.”** It does not.

For example:

```jsx
console.log("1");

setTimeout(() => {
    console.log("2");
}, 0);

console.log("3");
```

The output is:

```html
1
3
2
```

The reason is that even with a delay of `0`, the callback does not jump directly onto the Call Stack. It is handled asynchronously and can execute only after the current synchronous JavaScript execution has completed and the callback gets its opportunity through the Event Loop. Therefore, **`setTimeout(callback, 0)` means “schedule this callback to execute as soon as the asynchronous mechanism allows it,” not “execute it immediately.”**

---

## **`clearInterval()`**

The `clearInterval()` function in javascript clears the interval which has been set by the **`setInterval()**` function before that.

Its syntax is: `clearInterval(intervalId)`

Just like `setTimeout()`, `setInterval()` returns an identifier for the scheduled interval. We can store that identifier and later use it to cancel the interval.

```jsx
const intervalId = setInterval(sayDate, 1000, "hi");

clearInterval(intervalId);
```

Once `clearInterval(intervalId)` is called, the repeated execution stops.

A common real-world pattern is to start the interval when a **Start** button is clicked and stop it when a **Stop** button is clicked.

**Example**

```html
<p id="counter">0</p>
<button id="stop">Stop Counter</button>

<script>
    const counter = document.querySelector("#counter");
    const stopButton = document.querySelector("#stop");

    let count = 0;

    const intervalId = setInterval(function () {
        count++;
        counter.textContent = count;
    }, 1000);

    stopButton.addEventListener("click", function () {
        clearInterval(intervalId);
        counter.textContent = "Counter stopped";
    });
</script>
```

---

## Promises

A **Promise in JavaScript is an object that represents the eventual completion or failure of an asynchronous operation and its resulting value. It acts as** a **placeholder for a value that we don’t have right now, but expect to receive in the future**.

For example, suppose JavaScript starts an operation that takes some time, such as a database call, file operation, network request, cryptographic operation, etc. Instead of stopping the entire program and waiting for the result, JavaScript can continue executing other code. The Promise keeps track of that operation and tells us what happened when it eventually finishes.

A Promise can either be **fulfilled with a value** or **rejected with an error/reason**.

```jsx
const promise = new Promise((resolve, reject) => {
    // asynchronous operation
});
```

Here, `promise` is a Promise object.

### **The Three States of a Promise**

A Promise has **three possible states**:

**1. Pending** — The operation is still in progress and the result is not available yet.

**2. Fulfilled** — The operation completed successfully and produced a value.

**3. Rejected** — The operation failed and produced a reason/error.

A Promise starts in the **pending** state. Once it becomes fulfilled or rejected, it is considered **settled**. Importantly, a Promise can settle **only once**. Once it becomes fulfilled or rejected, its state cannot change again.

```jsx
const promise = new Promise((resolve, reject) => {
    resolve("Success");
    reject("Error");
});
```

The `reject()` here has no effect because the Promise has already been fulfilled.

---

### **Creating a Promise**

A Promise can be created using the `Promise` constructor:

```jsx
const promise = new Promise((resolve, reject) => {
    // asynchronous operation
});
```

The function passed to `new Promise()` is called the **executor function**. It receives two functions provided by JavaScript: `resolve` and `reject`.

```
                 Promise
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
      resolve()            reject()
          ↓                   ↓
      Fulfilled            Rejected
```

### **`resolve()` and `reject()`**

`resolve()` is called when the asynchronous operation is successful. Calling `resolve(value)` changes the Promise from **pending → fulfilled** and the value passed to `resolve()` becomes available to the code consuming the Promise.

`reject()` is called when the asynchronous operation fails. Calling `reject(error)` changes the Promise from **pending → rejected** and the value passed to `reject()` becomes available to the error-handling code.

For example:

```jsx
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = true;
        if (success) {
            resolve("User created successfully");
        } else {
            reject("Failed to create user");
        }
    }, 2000);
});
```

Here, the Promise initially remains **pending**. After two seconds, if `success` is `true`, `resolve()` is called and the Promise becomes fulfilled. If `success` is `false`, `reject()` is called and the Promise becomes rejected.

An important point is that **`resolve()` and `reject()` settle the Promise; they don’t directly execute `.then()` or `.catch()` themselves.** They communicate the final result of the asynchronous operation to whoever is consuming the Promise. Also, once a Promise has been settled, its state cannot change again. If `resolve()` is called first, a later `reject()` is ignored, and vice versa.

### **`.then()`**

Creating a Promise is only one side of the process. The other side is **consuming the Promise**, which means deciding what to do when the Promise eventually succeeds or fails.

The `.then()` method is used to handle a **fulfilled Promise**.

```jsx
promise.then((result) => {
    console.log(result);
});
```

The important connection is:

```jsx
resolve(value)
       ↓
   Promise fulfilled
       ↓
   .then((value) => {})
       ↓
   value available here
```

So, when we write: `resolve("User created successfully");`  the value `"User created successfully"` is received by the callback passed to `.then()`. Therefore, the argument received by `.then()` comes from the value passed to `resolve()`.

---

#### **Example**

Let’s use one example throughout this section so that the relationship between `resolve()`, `reject()`, `.then()`, `.catch()`, and `.finally()` becomes clear.

```jsx
const createUser = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = true;
        if (success) {
            resolve({
                username: "rocky",
                email: "rocky@example.com"
            });
        } else {
            reject("ERROR: Unable to create user");
        }
    }, 2000);
});

createUser
    .then((user) => {
        console.log(user);
    })
    .catch((error) => {
        console.log(error);
    })
    .finally(() => {
        console.log("User creation operation completed");
    });
```

When the `createUser` Promise is created, it initially enters the **pending state** because the user-creation operation has not finished yet. In our example, we are pretending that the operation takes two seconds, so during those two seconds the Promise is simply waiting for the operation to complete. Once the operation finishes, there are two possible outcomes: **success or failure**.

If the user is successfully created, `resolve(user)` is called. Calling `resolve()` changes the Promise’s state from **pending to fulfilled** and the `user` object passed to `resolve()` becomes the result of the Promise. Because the Promise has now been fulfilled, the callback provided to `.then()` is executed, and that resolved value is automatically passed to the `.then()` callback. In other words, `resolve(user)` is directly responsible for providing the `user` value to `.then(user => {})`. After `.then()` finishes its work, the `.finally()` callback executes because the Promise has now completed its operation.

If the user creation fails, `reject(error)` is called instead. Calling `reject()` changes the Promise’s state from **pending to rejected**, and the error passed to `reject()` becomes the rejection reason. Since the Promise was rejected, the success callback inside `.then()` is skipped, and the error-handling callback inside `.catch()` is executed. The error passed to `reject()` is automatically received by `.catch(error => {})`, allowing us to handle the failure. After the rejection has been handled, `.finally()` still executes because `finally()` does not care whether the Promise was fulfilled or rejected; it runs after the Promise has settled in either case.

Therefore, the overall flow can be understood as: **the Promise starts as pending → the asynchronous operation completes → `resolve()` makes it fulfilled and sends the value to `.then()`, or `reject()` makes it rejected and sends the error to `.catch()` → `.finally()` runs after either outcome.** This is why, in the successful case, we see the user object followed by `"User creation operation completed"`, while in the failed case, `.then()` is skipped, the error is handled by `.catch()`, and `"User creation operation completed"` is still printed by `.finally()`.

---

### **`.catch()`**

`.catch()` is used to handle a **rejected Promise**.

```jsx
createUser.catch((error) => {
    console.log(error);
});
```

The value received by `.catch()` comes from `reject()`:

```jsx
reject("Something went wrong");
```

becomes:

```jsx
.catch((error) => {
    console.log(error);
});
```

So the relationship is:

```
reject(error)
      ↓
Promise becomes rejected
      ↓
.catch(error => {})
      ↓
error received here
```

A useful way to understand `.catch()` is that it provides the error-handling path of a Promise. If the Promise is fulfilled, `.catch()` is skipped.

---

### **`.finally()`**

`.finally()` is used for code that should execute **after the Promise has settled**, regardless of whether it was fulfilled or rejected.

```jsx
createUser
    .then((user) => {
        console.log(user);
    })
    .catch((error) => {
        console.log(error);
    })
    .finally(() => {
        console.log("Operation completed");
    });
```

`finally()` does not care whether the Promise succeeded or failed.

```
                Promise
                   │
          ┌────────┴────────┐
          ↓                 ↓
      Fulfilled          Rejected
          │                 │
        .then()           .catch()
          │                 │
          └────────┬────────┘
                   ↓
               .finally()
```

This makes `.finally()` useful for **cleanup operations**, such as stopping a loading spinner, closing a resource, resetting UI state, etc.

---

## **Promise Chaining**

Promise chaining allows us to perform multiple asynchronous or dependent operations one after another without creating deeply nested callbacks.

A chain looks like:

```jsx
promise
    .then(...)
    .then(...)
    .then(...)
    .catch(...)
    .finally(...);
```

The most important rule to understand about chaining is: **Every `.then()` returns a new Promise.** This is what makes chaining possible.

Using our same `createUser` example:

```jsx
createUser
    .then((user) => {
        console.log(user);
        return user.username;
    })
    .then((username) => {
        console.log(username);
    })
    .catch((error) => {
        console.log(error);
    })
    .finally(() => {
        console.log("Operation completed");
    });
```

The first `.then()` receives the object produced by `resolve()`:

```jsx
resolve({
    username: "rocky",
    email: "rocky@example.com"
});
```

Inside that `.then()`, we return:

```jsx
return user.username;
```

That returned value becomes the result of the **new Promise returned by the first `.then()`**.

Therefore, the next `.then()` receives it:

```jsx
.then((username) => {
    console.log(username);
});
```

The flow is:

```
resolve(user)
      ↓
first .then(user)
      ↓
return user.username
      ↓
new Promise fulfilled with username
      ↓
second .then(username)
```

This is the fundamental idea behind Promise chaining.

### **Returning a Value from `.then()`**

Whenever a `.then()` returns a normal value, that value becomes available to the next `.then()`.

```jsx
promise
    .then((user) => {
        return user.username;
    })
    .then((username) => {
        console.log(username);
    });
```

So:

```
First .then()
     ↓
returns "hitesh"
     ↓
Next .then()
     ↓
receives "hitesh"
```

This is why we don’t need to manually create another Promise just to pass a value to the next step.

### **What Happens When a `.then()` Throws an Error?**

A `.then()` can also produce a rejection if an error is thrown inside it.

```jsx
createUser
    .then((user) => {
        throw new Error("Something went wrong");
    })
    .catch((error) => {
        console.log(error);
    });
```

The error causes the Promise returned by that `.then()` to become rejected, so the `.catch()` can handle it.

This gives us another useful mental model:

```
Promise rejection
        ↓
      .catch()

OR

throw inside .then()
        ↓
new Promise becomes rejected
        ↓
      .catch()
```

A rejection can travel through the Promise chain until a `.catch()` handles it.

```jsx
createUser
    .then((user) => {
        return getUserProfile(user.username);
    })
    .then((profile) => {
        return getUserOrders(profile.id);
    })
    .then((orders) => {
        console.log(orders);
    })
    .catch((error) => {
        console.log(error);
    });
```

If any operation in the chain rejects, the following success `.then()` callbacks are skipped and the rejection moves toward `.catch()`. This is one of the major advantages of Promise chaining over deeply nested callbacks.

---

## Async and Await

`async` and `await` provide a cleaner syntax for working with Promises. They don’t replace Promises, they provide another way of consuming them.

Instead of writing:

```jsx
createUser
    .then((user) => {
        console.log(user);
    })
    .catch((error) => {
        console.log(error);
    });
```

we can write:

```jsx
async function consumeUser() {
    try {
        const user = await createUser;
        console.log(user);
    } catch (error) {
        console.log(error);
    }
}
consumeUser();
```

The underlying operation is still Promise-based. `async/await` simply allows us to write the code in a style that looks more like normal sequential code.

### **`async`**

The `async` keyword is used before a function to make that function **asynchronous**. An `async` function always returns a **Promise**, even if you return a normal value.

```jsx
async function greet() {
    return "Hello World";
}

console.log(greet());
```

The function doesn’t directly return `"Hello World"`. Because it is an `async` function, it returns a Promise that eventually fulfills with `"Hello World"`.

---

### **`await`**

The `await` keyword is used to **wait for a Promise to settle** and obtain its fulfilled value. `await` can normally be used inside an `async` function.

```jsx
function getData() {
    return new Promise(function (resolve) {
        setTimeout(function () {
            resolve("Data received");
        }, 2000);
    });
}

async function showData() {
    const result = await getData();

    console.log(result);
}

showData();

// After approximately 2 seconds, we get the OUTPUT as
Data received
```

Here, `getData()` returns a Promise. The `await` keyword tells the `showData()` function to wait until that Promise is fulfilled before assigning its value to `result`. 

The important thing to understand is that `await` **does not block the entire JavaScript program**. It pauses the execution of the current `async` function until the Promise settles, while JavaScript can continue handling other work.

Without `async`/`await`, we might handle the same Promise using `.then()`:

```jsx
function getData() {
    return new Promise(function (resolve) {
        setTimeout(function () {
            resolve("Data received");
        }, 2000);
    });
}

getData().then(function (result) {
    console.log(result);
});

// USING ASYNC AND AWAIT

async function showData() {
    const result = await getData();

    console.log(result);
}
```

Both approaches work with the same Promise. `async`/`await` mainly provides a more readable way to write Promise-based asynchronous code, especially when multiple asynchronous operations need to be performed sequentially.

#### **`await` With a Rejected Promise**

If the Promise is rejected, `await` produces an error at that point. For example:

```jsx
const createUser = new Promise((resolve, reject) => {
    reject("Unable to create user");
});

async function consumeUser() {
    const user = await createUser;
    console.log(user);
}
```

will result in a rejected async operation because `await` encountered the rejected Promise.

This is why `await` is commonly used with `try...catch`.

```jsx
async function consumeUser() {
    try {
        const user = await createUser;
        console.log(user);
    } catch (error) {
        console.log(error);
    }
}
```

#### **`async/await` With Multiple Operations**

Suppose we have two operations where the second depends on the first:

```jsx
async function processUser() {

    try {
        const user = await createUser;
        const profile = await getUserProfile(user.username);

        console.log(profile);

    } catch (error) {
        console.log(error);
    }
}
```

#### Example

```jsx
function getUserName() {
    return new Promise(function (resolve) {
        setTimeout(function () {
            resolve("Prajeet");
        }, 1000);
    });
}

function getCourse() {
    return new Promise(function (resolve) {
        setTimeout(function () {
            resolve("JavaScript");
        }, 1000);
    });
}

function getProgress() {
    return new Promise(function (resolve) {
        setTimeout(function () {
            resolve("80%");
        }, 1000);
    });
}

async function showUserDetails() {
    const name = await getUserName();
    console.log("Name:", name);

    const course = await getCourse();
    console.log("Course:", course);

    const progress = await getProgress();
    console.log("Progress:", progress);
}

showUserDetails();
```

---

## The Promise Settlement Of Async, await and .then()

An `async` function **always returns a Promise**, even if you return a normal value such as a string, number, or object from that function. For example, when `greet()` returns `"Hello"`, JavaScript automatically wraps that value inside a fulfilled Promise. Conceptually, `async` function behaves like it is returning `Promise.resolve("Hello")`.

```jsx
async function greet() {
    return "Hello";
}
```

Therefore, calling `greet()` does **not** directly give us `"Hello"`. It gives us a Promise whose state is fulfilled and whose value is `"Hello"`.

Conceptually:

`greet()` → `Promise { "Hello" }`

The important thing to understand is that **the Promise and the value inside the Promise are two different things**. The Promise is an object representing the eventual result, while `"Hello"` is the actual result represented by that Promise.

### **Promise Settlement**

A Promise has three possible states: **pending, fulfilled, and rejected**. Initially, a Promise can be pending. Once the asynchronous operation finishes successfully, the Promise becomes fulfilled and contains a value. If the operation fails, it becomes rejected and contains a reason/error.

The transition from pending to either fulfilled or rejected is called **settling the Promise**.

For example, if a Promise eventually produces `"Hello"`, its lifecycle can be thought of as:

`pending → fulfilled → "Hello"`

A Promise **does not require `await` or `.then()` to settle**. The Promise settles automatically when the operation represented by it finishes. `await` and `.then()` are simply mechanisms for consuming the result of that Promise.

### **Example 1: Using `.then()`**

Consider this example:

```jsx
async function greet() {
    return "Hello";
}

function main() {
    greet().then((result) => {
        console.log(result);
    });
}

main();
```

When `greet()` is called, it returns a fulfilled Promise containing `"Hello"`.

The `.then()` method is used to specify what should happen when that Promise is fulfilled. The callback passed to `.then()` receives the **value contained inside the fulfilled Promise**.

Therefore, in:

```jsx
greet().then((result) => {
    console.log(result);
});
```

the `result` parameter contains `"Hello"`, not `Promise { "Hello" }`.

So inside the callback: `result === "Hello"`

The Promise itself is not passed to the callback; its fulfilled value is passed to the callback. However, there is another important point: **`.then()` itself returns a new Promise.**

For example:

```jsx
const x = greet().then((result) => {
    return result;
});
```

Here, `result` inside the callback is `"Hello"`, however when we ask to return the value that .then() holds that’s when it returns a Promisifed value i.e `Promise { "Hello" }` so `x` is a new Promise.

Conceptually:

`greet()` → `Promise { "Hello" }`

`.then()` receives `"Hello"` → callback returns `"Hello"`

`.then()` → `Promise { "Hello" }`

This is one of the reasons Promise chaining is possible. If the callback returns a value, the new Promise returned by `.then()` becomes fulfilled with that value.

If the callback doesn’t explicitly return anything:

```jsx
const x = greet().then((result) => {
    console.log(result);
});
```

then the callback returns `undefined`, so the new Promise returned by `.then()` becomes fulfilled with `undefined`. Thus, `.then()` has two important aspects: **the callback receives the Promise’s value**, while **the `.then()` method itself returns another Promise**.

### **Example 2: Using `await`**

Now let’s perform the same operation using `await`:

```jsx
async function greet() {
    return "Hello";
}

async function main() {
    const result = await greet();

    console.log(result);
}

main();
```

Again, `greet()` returns a fulfilled Promise containing `"Hello"`.

The difference is that `await` allows us to obtain the **value represented by that Promise** directly.

So:

```jsx
const result = await greet();
```

means conceptually: “Take the Promise returned by `greet()`. If it has not settled yet, wait until it settles. Once it is fulfilled, give me the value contained inside it.”

Therefore:

`greet()` → `Promise { "Hello" }`

`await greet()` → `"Hello"`

So in this example:

`result === "Hello"`

and **not**:

`result === Promise { "Hello" }`

### **What does “wait” actually mean in await?**

The word “wait” can be slightly misleading because JavaScript is not simply freezing the entire program. Suppose a Promise is still pending: `Promise → pending` and eventually becomes fulfilled: `Promise → fulfilled → "Hello"`

When JavaScript reaches:

```jsx
const result = await somePromise;
```

the current `async` function pauses at that point until the Promise settles. Once the Promise is fulfilled, execution of that async function continues, and the fulfilled value is assigned to `result`.

For example, let’s consider a setTimeout() example where a Promise takes three seconds to complete, `await` waits for that Promise to settle before continuing the `async` function.

```
Promise created
      ↓
   pending
      ↓
  3 seconds
      ↓
  fulfilled
      ↓
   "Hello"
```

But if the Promise is **already fulfilled**, as happens with our simple `greet()` example, there isn’t any meaningful asynchronous work left to wait for. `await` simply obtains its fulfilled value.

### **The complete mental model**

You can think of the whole concept like this, an `async` function **produces a Promise**. The Promise represents a result that may be available immediately or may become available later. The Promise eventually **settles** by becoming either fulfilled or rejected. It does this automatically; `.then()` and `await` are not responsible for settling it. `.then()` lets you say, “When this Promise is fulfilled, give its value to this callback.”  `await` lets you say: “Give me the value represented by this Promise, and if it isn’t settled yet, pause this async function until it settles.”

So for our example:

```jsx
async function greet() {
    return "Hello";
}
```

the overall flow is: **`async` returns a Promise, `.then()` receives the Promise’s fulfilled value through its callback and itself returns a new Promise, while `await` gives you the fulfilled value directly inside an `async` function.**

---