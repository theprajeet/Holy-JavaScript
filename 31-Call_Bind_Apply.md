# Call, Apply, Bind

## **`.call()` in JavaScript**

### **What is `.call()`?**

`call()` is a method available on JavaScript functions that allows you to **invoke a function immediately while explicitly specifying what `this` should refer to**.

The basic syntax is:

```jsx
functionName.call(thisValue, arg1, arg2, ...);
```

The first argument determines the value of `this` inside the function. Any arguments after the first are passed to the function as its normal parameters. =This becomes particularly useful when one function needs to perform some work on behalf of another object. Instead of allowing the called function to determine its own `this`, we can explicitly tell it which object it should work with.

---

#### **Why Do We Need `.call()`?**

Consider two functions where one function is responsible for initializing a particular property:

```jsx
function assignUsername(username) {
    this.username = username;
}

function createAccount(username, email) {
    assignUsername(username);
    this.email = email;
}

const account = new createAccount("Arjun", "arjun@example.com");

console.log(account);
```

At first glance, it may look like `assignUsername()` should add `username` to `account`. However, simply calling `assignUsername(username)` does **not** mean that its `this` automatically becomes the `this` of `createAccount()`.

Each normal function call gets its own `this` behavior. Therefore, if we want `assignUsername()` to operate on the same object currently being constructed by `createAccount()`, we need to explicitly provide that object as its `this`.

This is where `.call()` becomes useful.

#### **Using `.call()` to Share the Same `this`**

We can rewrite the previous example as:

```jsx
function assignUsername(username) {
    this.username = username;
}

function createAccount(username, email) {
    assignUsername.call(this, username);

    this.email = email;
}

const account = new createAccount("Arjun", "arjun@example.com");

console.log(account);
```

Here, `createAccount()` is executed with `new`, so JavaScript creates a new object and makes `this` inside `createAccount()` refer to that new object.

Then this line executes:

```jsx
assignUsername.call(this, username);
```

The first argument to `.call()` is `this`, which means:

**Call `assignUsername()` and make its `this` refer to the same object that `createAccount()` is currently using.** Therefore, when `assignUsername()` executes:

```jsx
this.username = username;
```

its `this` refers to the newly created account object. The final object contains both `username` and `email`.

---

#### **Understanding the `.call()` Syntax**

The most important part of `.call()` is its **first argument**.

```jsx
someFunction.call(object, value1, value2);
```

Here:

- `object` becomes `this` inside `someFunction`.
- `value1` becomes the first function argument.
- `value2` becomes the second function argument.

For example:

```jsx
function showDetails() {
    console.log(this.name);
}

const user = {
    name: "Arjun"
};

const admin = {
    name: "Meera"
};

showDetails.call(user);   // Arjun
showDetails.call(admin); // Meera

```

This is the main reason `.call()` is closely related to the `this` keyword. The function itself hasn’t changed. What changes is the object that `this` refers to. Conceptually `showDetails.call(user)`→ `this = user` → `showDetails.call(admin)` → `this = admin`

This makes `.call()` useful when we want to **reuse a function’s logic with different objects**.

---

### **`.call()` in Constructor Functions**

Let’s break down the constructor example step by step:

```jsx
function SetUsername(username) {
    this.username = username;
}

function createUser(username, email, password) {
    SetUsername.call(this, username);

    this.email = email;
    this.password = password;
}

const user = new createUser(
    "Arjun",
    "arjun@example.com",
    "pass123"
);
```

When `new createUser(...)` is executed, JavaScript creates a new object and makes `this` inside `createUser()` refer to that newly created object. When `SetUsername.call(this, username)` is executed, `.call()` explicitly passes that same object as the `this` value of `SetUsername()`. Therefore, inside `SetUsername()`, its `this` also refers to the **same object** that `createUser()` is working with. So when `SetUsername()` executes `this.username = username`, it adds the `username` property directly to the object being created by `createUser()`. In other words, `createUser()` doesn’t directly get a `username` variable from `SetUsername()` . Instead, both functions operate on the **same object through `this`**, allowing the property created by `SetUsername()` to become part of the final `user` object. So both functions operate on the same object.

A new execution context for `SetUsername()` is pushed onto the call stack. The important part is that `.call(this, ...)` tells JavaScript: “Execute `SetUsername()`, but use the `this` from `createUser()` as the `this` inside `SetUsername()`.”

```jsx
CALL STACK

┌───────────────────────────────────┐
│ SetUsername()                     │ 
│                                   │
│ this ───────────────┐             │
└─────────────────────│─────────────┘
                      │
┌─────────────────────│─────────────┐
│ createUser()        │             │
│                     │             │
│ this ───────────────┤             │
└─────────────────────│─────────────┘
                      │
                      ▼
              ┌─────────────────┐
              │ New User Object │
              │ { }             │
              └─────────────────┘

┌───────────────────────────────────┐
│ Global Execution Context          │
└───────────────────────────────────┘
```

---

### **`.call()` Does Not Automatically Share the Execution Context**

This is an important nuance when understanding the explanation. It can be tempting to think that when one function calls another function, the second function somehow receives the first function’s entire execution context. That is not what happens. Each function invocation gets its **own execution context**.

For example:

```jsx
function outer() {
    const message = "Hello";

    function inner() {
        console.log(message);
    }

    inner();
}

outer();
```

`inner()` gets its own execution context. It can access `message` because of JavaScript’s **lexical scoping**, not because the execution context of `outer()` was transferred to `inner()`. Similarly, `.call()` does not transfer one execution context into another. What it explicitly controls is the value of **`this`** for the called function.

---

## `.apply()` in JavaScript

The `apply()` method is similar to `call()` because it allows us to **explicitly set the value of `this` while immediately calling a function**. The main difference is in how arguments are provided: `call()` accepts arguments individually, whereas `apply()` accepts them together inside an **array (or array-like object)**.

Consider the following complete example:

```jsx
const person = {
    name: "Peter"
};

function greet(city, country) {
    console.log(
        "Hello, " + this.name + " from " + city + ", " + country
    );
}

greet.call(person, "Delhi", "India");
greet.apply(person, ["Noida", "India"]);

OUTPUT
Hello, Peter from Delhi, India
Hello, Peter from Noida, India
```

Here, `greet()` is a regular function that uses `this.name`. If we simply called `greet()`, `this` would not refer to the `person` object. Both `call()` and `apply()` allow us to explicitly tell JavaScript which object should be used as `this`.

With `call()`, the arguments are passed **one by one**:

```jsx
greet.call(person, "Delhi", "India");
```

This immediately calls `greet()` with `this` set to `person`, so `this.name` gives `"Peter"` and the other arguments provide `"Delhi"` and `"India"`.

With `apply()`, the same function can be called like this:

```jsx
greet.apply(person, ["Noida", "India"]);
```

The first argument, `person`, becomes the value of `this`. The second argument is an **array containing the arguments** that should be passed to `greet()`. Therefore, `"Noida"` becomes `city` and `"India"` becomes `country`.

So the key difference between `call()` and `apply()` is simply **how the function arguments are supplied**:

```
call()
  ↓
call(thisArg, arg1, arg2, arg3)

apply()
  ↓
apply(thisArg, [arg1, arg2, arg3])
```

Both methods **immediately execute the function** and allow us to control its `this` value. The practical distinction is that `apply()` is particularly useful when the arguments are already available in an array or array-like structure.

**Remember:** `call()` → arguments individually, `apply()` → arguments as an array.

---

## **`bind()` in JavaScript**

The `bind()` method is a built-in JavaScript function method used to create a **new function with a specific `this` value**. It is mainly useful when a function is passed around as a callback and may otherwise lose the object context from which it was originally defined. In simple terms, `bind()` allows us to tell a function, **“Whenever you execute, use this particular object as `this`.”** This is especially useful with object methods, class methods, event handlers, and callbacks.

### **Why Do We Need `bind()`?**

To understand `bind()`, we first need to understand that the value of `this` is generally determined by **how a function is called**, not simply by where the function is defined. When a method is called directly through an object, `this` refers to that object.

```jsx
const user = {
    name: "Arjun",

    greet() {
        console.log(this.name);
    }
};

user.greet(); // Arjun
```

Here, `greet()` is called as `user.greet()`, so `this` refers to `user`. Therefore, `this.name` gives `"Arjun"`.

However, if the method is extracted from the object and stored in another variable, it is no longer being called through `user`.

```jsx
const greetUser = user.greet;

greetUser();
```

In this situation, the connection between `greet()` and `user` has been lost. The function is now being called independently, so `this` no longer automatically refers to `user`. This is one of the situations where `bind()` becomes useful.

### **Basic Syntax of `bind()`**

The basic syntax is:

```jsx
const newFunction = function.bind(thisArg);
```

Here, `thisArg` is the object that should become the `this` value when the new function is executed. The important point is that `bind()` **does not execute the function immediately**. Instead, it returns a **new function** with the specified `this` value attached to it.

For example:

```jsx
const user = {
    name: "Arjun",

    greet() {
        console.log(this.name);
    }
};

const boundGreet = user.greet.bind(user);

boundGreet(); // Arjun
```

`user.greet.bind(user)` creates a new function called `boundGreet`. When `boundGreet()` is eventually called, `this` inside `greet()` refers to the `user` object.

### **`bind()` Returns a New Function**

One of the most important things to remember about `bind()` is that it **returns a new function instead of calling the original function immediately**.

```jsx
const boundGreet = user.greet.bind(user);
```

At this point, `greet()` has not executed. `bind()` has simply created another function that remembers that `this` should refer to `user`.

The function executes only when we call it:

```jsx
boundGreet();
```

Therefore:

```
bind() → creates and returns a new function
new function() → can be executed later
```

This behavior makes `bind()` particularly useful when a function needs to be supplied as a callback.

### **`bind()` Can Also Pre-set Arguments**

Apart from setting `this`, `bind()` can also **pre-set arguments** for a function. The arguments passed after the `thisArg` are stored and automatically supplied when the bound function is later called.

```jsx
function multiply(a, b) {
    return a * b;
}

const double = multiply.bind(null, 2);

console.log(double(5)); // 10
```

Here, `null` is used because the function does not need `this`, while `2` is supplied as the first argument. Therefore, when `double(5)` is called, JavaScript effectively performs `multiply(2, 5)`.

The general syntax can therefore be written as:

```jsx
const newFunction = function.bind(thisArg, arg1, arg2, ...);
```

The main purpose of `bind()` is setting `this`, while pre-setting arguments is an additional capability.

---

### **`bind()` with Event Handlers → IMP**

A common situation where `bind()` is required is when a class method is passed as a callback, such as an event handler. The problem occurs because `this` depends on **how a function is called**, not simply on where the function was originally defined. Consider the following complete example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bind Example</title>
</head>
<body>

    <button>Click Me</button>

    <script>
        class React {
            constructor() {
                this.library = "React";
                this.server = "https://localhost:3000";

                document
                    .querySelector("button")
                 // .button.addEventListener("click", this.handleClick);
                    .addEventListener("click", this.handleClick.bind(this));
            }

            handleClick() {
                console.log("Button clicked");
                console.log("Library:", this.library);
                console.log("Server:", this.server);
            }
        }

        const app = new React();
    </script>

</body>
</html>
```

When `new React()` is executed, the constructor runs and `this` refers to the newly created `React` object, which is stored in `app`. Therefore, `this.library` and `this.server` are properties of that object. The constructor then gets the button and registers `handleClick()` as its click handler.

The important part is:

```jsx
this.handleClick.bind(this)
```

To understand why `bind()` is needed, first consider what would happen if we wrote:

```jsx
button.addEventListener("click", this.handleClick);
```

Here, `this.handleClick` is **not being called**. We are simply passing the function itself to `addEventListener()`. The browser will call that function later when the button is clicked. This is different from:

```jsx
this.handleClick();
```

In `this.handleClick()`, the method is called through the `React` object, so `this` inside `handleClick()` refers to that `React` instance. But when `this.handleClick` is passed as a callback, it is detached from that method-call context. When the browser later invokes it as an event handler, `this` refers to the event target (the button) rather than the `React` instance. Consequently, `this.library` and `this.server` would not refer to the properties of `app`.

`bind()` solves this by creating a **new function with a permanently bound `this` value**. Therefore:

```jsx
this.handleClick.bind(this)
```

means: get the `handleClick` function and create a new version of it whose `this` will always refer to the current `React` instance.

The two `this` values have different roles. The first one accesses the method:

```jsx
this.handleClick
```

while the second one tells `bind()` which object should become `this` when the new function executes:

```jsx
this.handleClick.bind(this)
```

So the flow is simply:

```
React instance
     ↓
this.handleClick
     ↓
bind(this)
     ↓
new function with React instance bound as this
     ↓
addEventListener()
     ↓
button clicked
     ↓
handleClick() executes with this = React instance
```

As a result, when the button is clicked, `this.library` correctly gives `"React"` and `this.server` correctly gives `"https://localhost:3000"`.

**The key point to remember is:** `bind()` does not immediately execute the function. It **creates and returns a new function** with the specified `this` value fixed to the object you provide. This makes it especially useful when class methods are passed as callbacks or event handlers.

---