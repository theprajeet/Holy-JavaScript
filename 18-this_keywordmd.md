# The this keyword

## **The `this` Keyword**

The **`this` keyword** in JavaScript refers to the **object that is associated with the current execution context**. Its value is not fixed. It depends on **how and where a function is called**. For example, when `this` is used inside an object method, it usually refers to the object that called the method, allowing the method to access the object’s properties and other methods. The behaviour of `this` is different in regular functions and arrow functions, so understanding the calling context is important when working with `this`.

### **`this` inside an Object**

Inside an object method, `this` refers to the **current object**.

```jsx
const user = {
    username: "robert",
    price: 999,
    
    welcomeMessage: function () {
        console.log(`${this.username}, welcome to website`);
    }
}

user.welcomeMessage();
```

Output

```
robert, welcome to website
```

If the property changes,

```jsx
user.username = "sam";
user.welcomeMessage();
```

Output

```
sam, welcome to website
```

Instead of hardcoding `"robert"` inside the function,

```jsx
console.log(this.username);
```

always refers to the **current object’s username**. This makes the function reusable even if the object’s properties change later.

#### **What happens if `this` is not used here?**

In the following example, `username` is a **property of the `user` object**, not a standalone variable. Inside the `welcomeMessage` function, writing `${username}` tells JavaScript to look for a variable named `username` in the current scope and then in its outer scopes. It does **not** automatically look inside the `user` object. Since no variable named `username` has been declared outside the object, JavaScript cannot find it and throws a **`ReferenceError: username is not defined`**. To access the `username` property belonging to the current object, we use the `this` keyword: `${this.username}`. Here, `this` refers to the `user` object because `welcomeMessage()` is called as `user.welcomeMessage()`.

Example

```jsx
const user = {
    username: "robert",
    price: 999,
    
    welcomeMessage: function () {
        console.log(`${username}, welcome to website`);
    }
}

user.welcomeMessage();
```

Output

```jsx
ReferenceError: username is not defined
```

---

### **`this` in Node.js (Global Scope)**

In the Node.js environment used in the lecture, this prints an empty object.

```jsx
console.log(this);
```

Output

```
{}
```

This happens because there is no object context associated with `this` in the global scope of that environment.

### **`this` in the Browser (Global Scope)**

When you execute `this` in the browser’s console, it refers to the **global object**, which is the `window` object.

```jsx
console.log(this);
```

Output

```jsx
Window { ... }
```

The `window` object is the global object provided by browsers. It contains all global variables, functions, and browser APIs such as `alert()`, `setTimeout()`, `document`, `location`, and many others. Since code executed in the browser’s global scope belongs to the global object, `this` points to `window`.

**Example**

```jsx
console.log(this === window);
```

Output

```jsx
true
```

**Note:** In the **browser’s global scope**, `this` refers to the `window` object. In **Node.js (CommonJS modules)**, `this` refers to `module.exports`, which is initially an empty object (`{}`).

---

### **`this` inside a Normal Function**

In a **normal function**, `this` refers to the **global object** when the function is called normally (not as a method of an object). Therefore, logging `this` inside a normal function prints the global object.

Example

```jsx
function greet() {
    console.log(this);
}

greet();
```

Output (simplified)

```jsx
<ref *1> Object [global] {
  global: [Circular *1],
  setTimeout: [Function],
  clearTimeout: [Function],
  setInterval: [Function],
  ...
}
```

The output is the **Node.js global object**, which contains global functions and properties such as `setTimeout()`, `setInterval()`, `clearTimeout()`, and others. Since `username` is a **local variable** inside the function and not a property of the global object, `this.username` returns `undefined`.

---

#### **Why is `this` Different in the Global Scope and Inside a Normal Function? (Node.js)**

At first, it may seem confusing that `this` returns an **empty object (`{}`)** in the global scope but returns the **global object** inside a normal function. This happens because **they refer to two different execution contexts in Node.js**.

- In the **global scope of a Node.js module**, `this` points to **`module.exports`**, which is an empty object by default.
- Inside a **normal function** that is called directly, `this` points to the **Node.js global object**

The important thing to understand is that **`this` does not always refer to the same object**. Its value depends on **where and how the code is being executed**. In Node.js, every file is treated as a separate **module**. At the top level of a module, `this` refers to **`module.exports`**, which starts as an empty object (`{}`), so `console.log(this)` prints `{}`. However, when you call a **normal function**, JavaScript uses a different execution context for that function. In a regular function call (without any object before it), Node.js binds `this` to the **global object**, which contains built-in functions like `setTimeout()`, `setInterval()`, and others. Therefore, `console.log(this)` inside a normal function prints the global object instead of the empty object.

---

#### **`this` inside a local variable**

Example

```jsx
function greet() {
    let username = "Ram";
    console.log(this.username);
}
greet();
```

Output

```
undefined
```

Even though `username` is declared inside the function, it is a **local variable**, not a property of the object referenced by `this`. In JavaScript, `this.username` looks for a property named `username` on the object that `this` refers to (which is the **global object** in this case). Since the global object does not have a property called `username`, JavaScript cannot find it and returns `undefined`. If you want to access the local variable, you should use `username` directly instead of `this.username`.

---

## **`this` inside an Arrow Function**

Arrow Functions behave differently.

```jsx
const greet = () => {
    let username = "Ram";
    console.log(this);

}

greet();
```

Output

```jsx
{}
```

Unlike a normal function, an **Arrow Function does not create its own `this`**. Instead, it **inherits `this` from the surrounding (lexical) scope** where it is defined. In this example, the arrow function is defined in the **module’s global scope**, where `this` already refers to `module.exports` (an empty object `{}`). Since the arrow function simply reuses that same `this` instead of creating a new one, `console.log(this)` also prints the empty object `{}`.

**Note:** An **Arrow Function never gets its own `this`**. It always uses the `this` value from its surrounding scope, whereas a **Normal Function gets its own `this`** based on how it is called.

---