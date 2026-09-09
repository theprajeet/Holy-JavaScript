# Strict Mode

### **What is Strict Mode?**

**Strict Mode** is a feature in JavaScript that makes the language **stricter about certain programming mistakes and unsafe behaviours**. It helps developers write cleaner and safer code by changing some of JavaScript’s normally permissive behaviours into errors.

Strict mode is enabled using the following statement:

```jsx
"use strict";
```

Once strict mode is enabled, JavaScript applies a stricter set of rules to the code.

For example, JavaScript normally allows us to accidentally create a global variable by assigning a value to a variable that was never declared:

```jsx
username = "Robert";

console.log(username);
```

This may appear to work in normal JavaScript because JavaScript can implicitly create a global variable called `username`.

However, this is usually a programming mistake. We probably intended to write:

```jsx
let username = "Robert";
```

Strict mode prevents this accidental behaviour:

```jsx
"use strict";

username = "Robert";
```

```
OUTPUT

ReferenceError: username is not defined
```

Instead of silently allowing the mistake, strict mode makes JavaScript **throw an error**, helping us identify the problem immediately.

---

### **Why Do We Need Strict Mode?**

JavaScript was designed to be flexible and forgiving. As a result, some mistakes that should ideally produce errors were historically allowed by the language.

For example:

```jsx
x = 10;
```

Without strict mode, JavaScript may create a global variable `x` accidentally. This can cause difficult-to-find bugs, especially in larger applications where many variables and functions exist.

Strict mode changes such behaviour so that JavaScript **fails loudly instead of silently doing something unintended**.

Therefore, strict mode is mainly useful for:

- catching common programming mistakes
- preventing accidental global variables
- making certain unsafe operations throw errors
- making JavaScript code easier to reason about
- helping developers write more predictable code

---

### **How to Enable Strict Mode**

Strict mode can be enabled for an entire script by placing `"use strict";` at the beginning:

```jsx
"use strict";

let name = "Robert";

console.log(name);
```

It can also be enabled only inside a particular function:

```jsx
function test() {
    "use strict";

    x = 10; // Error
}

test();
```

Here, strict mode applies to the code inside `test()`.

If `"use strict"` is placed at the top level of a JavaScript file, it applies to the **entire script**.

---

### **Strict Mode Prevents Accidental Global Variables**

One of the most important examples is assigning a value to an undeclared variable.

Without strict mode:

```jsx
x = 10;

console.log(x);
```

JavaScript may create `x` as a global variable.

With strict mode:

```jsx
"use strict";

x = 10;
```

JavaScript throws:

```
ReferenceError: x is not defined
```

This is useful because accidentally creating global variables can lead to unexpected changes elsewhere in a program.

The correct approach is to explicitly declare the variable:

```jsx
"use strict";

let x = 10;
```

---

### **Strict Mode Makes Certain Invalid Assignments Throw Errors**

Consider assigning a value to a property that is read-only.

```jsx
"use strict";

const obj = {};

Object.defineProperty(obj, "name", {
    value: "Robert",
    writable: false
});

obj.name = "Rahul";
```

Because `name` was explicitly made non-writable, strict mode throws an error instead of silently ignoring the assignment.

Without strict mode, some such invalid assignments are simply ignored.

This illustrates an important idea behind strict mode: **when an operation is not allowed, strict mode prefers to report the mistake rather than silently continue.**

---

### **Strict Mode Changes the Behaviour of `this`**

Strict mode also changes how `this` behaves inside a normal function.

Consider:

```jsx
function test() {
    console.log(this);
}

test();
```

In non-strict mode, `this` inside a regular function called this way can refer to the global object.

In strict mode:

```jsx
"use strict";

function test() {
    console.log(this);
}

test();
```

the value of `this` is:

```
undefined
```

This makes the behaviour more predictable because JavaScript does not automatically substitute the global object for `this`.

**Important:** This applies to **regular functions**. Arrow functions have different `this` behaviour because they inherit `this` from their surrounding lexical scope.

---