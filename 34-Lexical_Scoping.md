# Lexical Scoping and Closure

## **Lexical Scoping in JavaScript**

Lexical scoping means that a function’s access to variables is determined by **where the function is written in the source code**, not by where the function is called. In JavaScript, an inner function can access variables declared in its own scope as well as variables declared in its outer function’s scope. However, the reverse is not true: an outer function cannot access variables that are declared only inside its inner function. Similarly, two sibling functions can access variables from their common parent, but they cannot directly access each other’s local variables.

```jsx
function outer() {
    let username = "Hitesh";
    console.log(secret); // Error

    function inner() {
        let secret = "my123";

        console.log(username); // Accessible
        console.log(secret);   // Accessible
    }

    function innerTwo() {
        console.log(username); // Accessible
        console.log(secret);   // Error
    }

    inner();
    innerTwo();
}

outer();

console.log(username); // Error
```

Here, `username` belongs to the lexical scope of `outer()`. Since both `inner()` and `innerTwo()` are **defined inside `outer()`**, both of them can access `username`. However, `secret` belongs specifically to the scope of `inner()`, so `innerTwo()` cannot access it, and neither can `outer()`. This shows the basic rule of lexical scoping: **inner scopes can access variables from their outer scopes, but outer scopes cannot access variables from their inner scopes**. The important point is that this relationship comes from the functions’ position in the code; JavaScript creates this scope relationship when the functions are defined.

---

## **Closures in JavaScript**

A **closure** is **a feature in JavaScript where an inner function retains access to the variables of its outer (enclosing) function, even after that outer function has finished executing**.

In other words, a closure can be thought of as a **function together with the lexical environment it needs to access**. This becomes particularly interesting when an inner function is returned from an outer function, because normally we might expect the outer function’s execution to be finished and its local variables to no longer be accessible.

Consider a simple counter:

```jsx
function outer() {
    let counter = 0;

    function inner() {
        counter++;
        console.log(counter);
    }

    return inner;
    // Notice that fn's reference has been returned, fn isn't called
}

const fn = outer();

fn(); // 1
fn(); // 2
fn(); // 3
```

When `outer()` executes, it creates the local variable `counter` and the `inner()` function. Instead of executing `inner()` immediately, `outer()` **returns the function itself** using 
`return inner`. Therefore, `fn` now holds a reference to that returned function.

At this point, `outer()` has already finished executing. Normally, you might expect its local variable `counter` to disappear because the execution of `outer()` is over. However, `inner()` was defined inside `outer()`, so according to lexical scoping, `inner()` has access to `counter`. When `inner()` is returned and used later, it **retains access to the lexical environment in which it was created**. This preserved access is what forms the closure.

Therefore, when `fn()` is called for the first time, it can still access `counter` and change it from `0` to `1`. On the next call, it accesses the **same retained `counter`**, changing it from `1` to `2`, and then to `3`.

The important thing to understand is that the closure does not simply store a copy of the value `counter` at the time `outer()` finishes. It retains access to the variable itself, allowing the inner function to **read and modify that variable across multiple calls**.

This is why closures are particularly useful for creating **private state**. The variable `counter` cannot be directly accessed from outside:

```jsx
console.log(counter); // ReferenceError
```

But the returned function can access and modify it:

```jsx
fn(); // 4
fn(); // 5
```

So the `counter` variable is effectively hidden from the outside world while still being accessible through the function that forms the closure.

### **How a Closure Is Created**

A closure is created when a function is **defined inside another scope and maintains access to variables from that surrounding scope**. Simply having a nested function does not necessarily make the concept useful in practice. The interesting part happens when the inner function continues to exist and is used after the outer function has finished executing.

The sequence can be understood as:

```
outer() executes
      ↓
counter is created
      ↓
inner() is created inside outer()
      ↓
inner() has access to counter because of lexical scoping
      ↓
outer() returns inner
      ↓
outer() finishes execution
      ↓
returned inner function still has access to counter
      ↓
that preserved access forms the closure
```

A common mistake is to say that **the outer function’s entire execution context is returned along with the inner function**. That is not quite correct. The execution context of `outer()` finishes when `outer()` finishes. What remains relevant is the **lexical environment required by the returned function**, which JavaScript keeps alive because the function still has a reference to variables from that environment.

This distinction is important: **the execution is over, but the variables required by the closure remain accessible.**

Another important point is that closures preserve the **lexical environment**, not necessarily every variable from every surrounding scope. JavaScript keeps the variables that are still reachable through the closure. For example, if a returned function uses `counter`, that variable remains available to it even though `outer()` has finished.

For example

```jsx
function outer() {
    let counter = 0;
    let name = "Tony"; // inner doesn't use this

    function inner() {
        counter++;
        console.log(counter);
    }

    return inner;
}
```

Here, `inner` needs `counter`, so `counter` is preserved. `name`, however, is not needed by `inner`, so there is no reason for the closure to keep `name` alive. **The important idea is that a closure preserves access to the variables from its surrounding lexical scope that the function actually needs, not simply every variable that happened to exist in that scope.** Modern JavaScript engines can optimize this internally as well, so you should think in terms of **which variables remain reachable/needed**, rather than imagining that the entire `outer()` execution context is permanently stored in memory.

---

### **Closure Provides Data Privacy / Encapsulation**

Closures can also be used to keep data private. A variable declared inside an outer function is not directly accessible from outside, but functions returned by that outer function can provide controlled access to it.

```jsx
function createBankAccount() {
    let balance = 0;

    return {
        deposit(amount) {
            balance += amount;
        },

        withdraw(amount) {
            if (amount <= balance) {
                balance -= amount;
            } else {
                console.log("Insufficient funds");
            }
        }
    };
}

const account = createBankAccount();

account.deposit(100);
account.withdraw(50);
```

Here, `balance` is declared inside `createBankAccount()`, so outside code cannot directly access it:

```jsx
account.balance; // undefined
```

However, `deposit()` and `withdraw()` were defined inside `createBankAccount()`, so they form closures over the `balance` variable. Even after `createBankAccount()` has finished executing, these methods retain access to the same `balance`.

This provides a simple form of **data privacy and encapsulation**: outside code cannot directly modify `balance`; it can only interact with it through the methods provided by the returned object.

---

### **Closure in Real-World Use Case**

A practical example of closures is creating **reusable event handlers**. Suppose a webpage has multiple buttons, and each button should change the background to a different color when clicked. If we only have two buttons, we could simply write separate event handlers:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Without Closure</title>
</head>
<body>

    <button id="orange">Orange</button>
    <button id="green">Green</button>

    <script>
        document.getElementById("orange").onclick = function () {
            document.body.style.backgroundColor = "orange";
        };

        document.getElementById("green").onclick = function () {
            document.body.style.backgroundColor = "green";
        };
    </script>

</body>
</html>
```

This works, but imagine having **hundreds of buttons**, each with a different color. Repeating the same event-handling logic for every button would be unnecessary and would violate the **DRY (Don’t Repeat Yourself)** principle. We therefore want to create one reusable function that can accept a color and create the appropriate event handler for us.

A first attempt might look like this:

```jsx
function clickHandler(color) {
    document.body.style.backgroundColor = color;
}

document.getElementById("orange").onclick =
    clickHandler("orange");

document.getElementById("green").onclick =
    clickHandler("green");
```

However, this does **not** work as intended. The problem is that `clickHandler("orange")` immediately **calls** the function instead of giving `onclick` a function to execute later. The background changes immediately when the code runs, rather than when the button is clicked. Furthermore, `onclick` expects a function reference, but `clickHandler("orange")` gives it the **result of executing the function**.

What we actually need is a function that **creates and returns another function**. This is where the closure becomes useful:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Closure Example</title>
</head>
<body>

    <button id="orange">Orange</button>
    <button id="green">Green</button>

    <script>
        function clickHandler(color) {

            return function () {
                document.body.style.backgroundColor = color;
            };

        }

        document.getElementById("orange").onclick =
            clickHandler("orange");

        document.getElementById("green").onclick =
            clickHandler("green");
    </script>

</body>
</html>
```

Now `clickHandler("orange")` **does not change the background immediately**. Instead, it creates and returns the inner function:

```jsx
function () {
    document.body.style.backgroundColor = color;
}
```

That returned function is assigned to `onclick`, so the browser will execute it **later when the user clicks the button**.

The interesting part is that when `clickHandler("orange")` finishes executing, its local parameter `color` would normally be part of the function’s local scope. However, the returned inner function was **created inside `clickHandler()`**, so through lexical scoping it has access to `color`. Because that inner function continues to exist after `clickHandler()` has finished, JavaScript preserves the lexical environment needed by that function. This preserved access is the **closure**.

```
clickHandler("orange")
        ↓
color = "orange"
        ↓
inner function is created
        ↓
inner function closes over color
        ↓
inner function is returned
        ↓
assigned to orange.onclick
        ↓
clickHandler() finishes
        ↓
user clicks Orange
        ↓
inner function executes
        ↓
still has access to color
        ↓
background becomes orange
```

The same process happens for the Green button:

```
clickHandler("green")
        ↓
color = "green"
        ↓
new inner function is created
        ↓
that function closes over color
        ↓
assigned to green.onclick
        ↓
user clicks Green
        ↓
background becomes green
```

Therefore, each call to `clickHandler()` creates a **separate closure** with its own `color` value. The orange button’s function retains access to `"orange"`, while the green button’s function retains access to `"green"`.

This is the important practical idea behind the example: **a closure allows a returned function to carry access to the data from the environment in which it was created, so that the function can use that data later—even after the outer function has finished executing.** This pattern is commonly useful with **event handlers, callbacks, timers, asynchronous operations, and functions that need to maintain private state**.

---