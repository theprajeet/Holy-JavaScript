## **JavaScript Execution Context**

#### **What is JavaScript Execution Context?**

**Execution Context** is the environment in which JavaScript code is evaluated and executed.

In simple words, when we give JavaScript a file containing code, JavaScript needs an environment in which it can **store variables, store function definitions, and execute the code**. This environment is called an **Execution Context**.

The execution context helps JavaScript understand what variables and functions are available and how the code should be executed.

For example:

```jsx
let val1 = 10;
let val2 = 5;

function addNum(num1, num2) {
    let total = num1 + num2;
    return total;
}
```

JavaScript does not simply start executing every line blindly. It first creates an execution context and processes the code in specific phases.

#### **Types of Execution Context**

The source discusses three types of execution contexts:

- **Global Execution Context**
- **Function Execution Context**
- **Eval Execution Context**

For interview purposes, the two most important ones are **Global Execution Context** and **Function Execution Context**.

### **1. Global Execution Context**

The **Global Execution Context** is created when JavaScript starts executing a JavaScript file.

No matter how small or large the JavaScript program is, the code initially starts inside the **Global Execution Context**.

For example:

```jsx
let val1 = 10;

function addNum() {
    console.log("Hello");
}
```

When this code starts executing, JavaScript first creates the Global Execution Context.

In a browser environment, the global context is associated with the browser’s global object, which is the **`window` object**.

So, in a browser:

```jsx
this
```

generally refers to:

```jsx
window
```

The exact global environment can differ depending on where JavaScript is running. For example, browser JavaScript and Node.js have different global environments.

### **2. Function Execution Context**

Whenever a function is **called/executed**, JavaScript creates a new **Function Execution Context** for that particular function call.

For example:

```jsx
function addNum(num1, num2) {
    let total = num1 + num2;
    return total;
}

addNum(10, 5);
```

When JavaScript reaches:

```jsx
addNum(10, 5);
```

a new Function Execution Context is created for `addNum()`.

This new context contains the function’s own variables and parameters.

For example:

```jsx
num1 → 10
num2 → 5
total → 15
```

Once the function finishes executing and returns, its execution context is removed.

An important point is that **every function call gets its own execution context**. If the same function is called twice, two separate execution contexts are created, one for each call.

### **3. Eval Execution Context**

There is also an **Eval Execution Context**, which is associated with JavaScript’s `eval()` functionality.

For normal JavaScript development and interviews, the important ones to understand deeply are:

**Global Execution Context → Function Execution Context**

---

### **Execution Context Phases**

The execution of JavaScript code inside an execution context happens in two major phases:

1. **Memory Creation Phase**
2. **Execution Phase**

These two phases are extremely important for understanding how JavaScript executes code.

### **Memory Creation Phase**

The **Memory Creation Phase** is also called the **Creation Phase** or simply the **Memory Phase**.

During this phase, JavaScript goes through the code and prepares memory for the variables and functions.

The important thing to understand is that **the actual calculations and normal execution of statements do not happen during this phase**.

For example:

```jsx
let val1 = 10;
let val2 = 5;

function addNum(num1, num2) {
    let total = num1 + num2;
    return total;
}
```

During the Memory Creation Phase, JavaScript prepares memory for:

```
val1
val2
addNum
```

The variables are initially given an `undefined` value, while the function declaration gets its **complete function definition**.

Conceptually:

```
val1 → undefined
val2 → undefined
addNum → function definition
```

The function body is not executed at this point. Its definition is simply stored so that JavaScript knows what `addNum` represents when it is eventually called.

### **Execution Phase**

After the Memory Creation Phase is completed, JavaScript enters the **Execution Phase**.

This is where the actual code starts executing.

For example:

```jsx
let val1 = 10;
let val2 = 5;
```

During the Execution Phase:

```
val1 → 10
val2 → 5
```

The values are now assigned.

Similarly, when JavaScript reaches a function call:

```jsx
addNum(val1, val2);
```

the function is actually executed.

This is where calculations, assignments, function calls, `return`, etc. actually take place.

### **Example: Global Execution Context**

Consider the following code:

```jsx
let val1 = 10;
let val2 = 5;

function addNum(num1, num2) {
    let total = num1 + num2;
    return total;
}

let result1 = addNum(val1, val2);
let result2 = addNum(10, 2);
```

When JavaScript starts, it creates the **Global Execution Context**.

The Global Execution Context goes through the Memory Creation Phase first.

Conceptually, memory looks like:

```
val1    → undefined
val2    → undefined
addNum  → function definition
result1 → undefined
result2 → undefined
```

Notice that `addNum` contains the complete function definition instead of `undefined`.

### **Execution of the Global Code**

After the Memory Creation Phase, JavaScript enters the Execution Phase.

The first line is:

```jsx
let val1 = 10;
```

So:

```
val1 → 10
```

Next:

```jsx
let val2 = 5;
```

So:

```
val2 → 5
```

The function declaration itself does not need to be executed again because its definition was already stored during the Memory Creation Phase.

JavaScript then reaches:

```jsx
let result1 = addNum(val1, val2);
```

At this point, JavaScript needs to execute `addNum()`.

This is where a **new Function Execution Context** is created.

### **Function Execution Context for `addNum()`**

The function is:

```jsx
function addNum(num1, num2) {
    let total = num1 + num2;
    return total;
}
```

And it is called as:

```jsx
addNum(val1, val2);
```

At this point:

```
num1 → 10
num2 → 5
```

A separate execution environment is created for this function call.

The function’s own variables are independent of the variables in the Global Execution Context.

### **Memory Creation Phase Inside a Function**

The Function Execution Context also goes through the same two phases.

First comes the **Memory Creation Phase**.

For:

```jsx
function addNum(num1, num2) {
    let total = num1 + num2;
    return total;
}
```

the function context conceptually prepares:

```
num1  → undefined
num2  → undefined
total → undefined
```

The parameters and local variables belong to this function’s execution context.

### **Execution Phase Inside a Function**

After the Memory Creation Phase, the function enters its Execution Phase.

The arguments passed to the function are assigned to the parameters:

```jsx
addNum(10, 5);
```

Therefore:

```
num1 → 10
num2 → 5
```

Then this statement executes:

```jsx
let total = num1 + num2;
```

The calculation takes place:

```
total → 10 + 5
total → 15
```

Then:

```jsx
return total;
```

returns:

```
15
```

back to the code that called the function.

### **Returning From a Function Execution Context**

The value returned by the function goes back to the place where the function was called.

In:

```jsx
let result1 = addNum(val1, val2);
```

`addNum()` returns:

```
15
```

Therefore:

```
result1 → 15
```

Once the function has completed its execution, its Function Execution Context is removed.

The function’s temporary execution environment is no longer required.

<img src="/Assets/Exec_Context-1.png" alt="Exec-Context-Img.png" width="600" >

### **Every Function Call Creates a New Execution Context**

Consider:

```jsx
let result1 = addNum(val1, val2);
let result2 = addNum(10, 2);
```

Although both calls are made to the **same function**, each call creates a separate Function Execution Context.

First:

```jsx
addNum(val1, val2);
```

creates one Function Execution Context.

After it finishes, that context is removed.

Then:

```jsx
addNum(10, 2);
```

creates another Function Execution Context.

Its values are:

```
num1  → 10
num2  → 2
total → 12
```

Then `12` is returned and stored in:

```
result2 → 12
```

So the same function can have multiple execution contexts during the lifetime of a program, depending on how many times it is called.

<img src="/Assets/Exec_Context-2.png" alt="Exec_Context.png" width="600" >

---

## **Call Stack**

Execution Context explains **where code executes**, while the **Call Stack** helps JavaScript keep track of which function is currently being executed.

The Call Stack is a stack-like structure used to keep track of function execution.

When JavaScript starts running the program, the **Global Execution Context** is placed into the Call Stack.

Conceptually:

```
Global Execution Context
```

When a function is called, its Function Execution Context is pushed onto the Call Stack.

For example:

```jsx
function one() {
    console.log("one");
}

one();
```

When `one()` is called:

```
Function Execution Context of one()
        ↓
Global Execution Context
```

The function is executed, and after it finishes, its context is removed from the Call Stack.

The Global Execution Context remains.

### **LIFO — Last In, First Out**

The Call Stack follows the **LIFO** principle:

**Last In, First Out**

This means the execution context that enters the Call Stack last will be the first one to leave.

For example:

```jsx
function one() {
    console.log("one");
}

function two() {
    console.log("two");
}

function three() {
    console.log("three");
}

one();
two();
three();
```

The functions execute one after another:

```
one()
two()
three()
```

When `one()` finishes, it is removed before `two()` starts. Similarly, `two()` is removed before `three()` starts.

### **Nested Function Calls and Call Stack**

The Call Stack becomes more interesting when one function calls another function.

Consider:

```jsx
function one() {
    console.log("one");
    two();
}

function two() {
    console.log("two");
    three();
}

function three() {
    console.log("three");
}

one();
```

Execution begins with:

```jsx
one();
```

So `one()` is added to the Call Stack.

Conceptually:

```
one()
Global
```

Inside `one()`, JavaScript encounters:

```jsx
two();
```

So `two()` is added on top:

```
two()
one()
Global
```

Inside `two()`, JavaScript encounters:

```jsx
three();
```

So `three()` is added on top:

```
three()
two()
one()
Global
```

At this moment, `three()` is the function currently being executed.

<img src="/Assets/Exec_Context-3.png" alt="Exec_Context.png" width="600" >

### **How the Nested Call Stack Gets Removed**

Once `three()` finishes, it is removed first:

```
two()
one()
Global
```

Then `two()` finishes and is removed:

```
one()
Global
```

Finally, `one()` finishes:

```
Global
```

This demonstrates **LIFO — Last In, First Out**.

`three()` came in last, so it goes out first.

---

### **Execution Context vs Call Stack**

These two concepts are related but should not be confused.

An **Execution Context** is the environment in which JavaScript code is executed. It contains the information required to execute that particular code.

The **Call Stack** is the stack structure that keeps track of the execution contexts/functions that are currently active.

A simple way to remember it is:

```
Execution Context → Environment where code executes

Call Stack → Keeps track of active execution
```

When a function is called, a Function Execution Context is created and its execution is tracked through the Call Stack.

### **Complete Execution Context Example**

For the following code:

```jsx
let val1 = 10;
let val2 = 5;

function addNum(num1, num2) {
    let total = num1 + num2;
    return total;
}

let result1 = addNum(val1, val2);
let result2 = addNum(10, 2);
```

The overall flow can be understood as:

```
JavaScript starts
      ↓
Global Execution Context created
      ↓
Memory Creation Phase
      ↓
Variables/functions are prepared
      ↓
Execution Phase
      ↓
val1 → 10
val2 → 5
      ↓
addNum() is called
      ↓
Function Execution Context created
      ↓
Memory Creation Phase
      ↓
Execution Phase
      ↓
num1 → 10
num2 → 5
total → 15
      ↓
return 15
      ↓
Function Execution Context removed
      ↓
result1 → 15
      ↓
addNum() called again
      ↓
New Function Execution Context
      ↓
num1 → 10
num2 → 2
total → 12
      ↓
return 12
      ↓
Function Execution Context removed
      ↓
result2 → 12
```

### **Complete Call Stack Example**

```jsx
function one() {
    console.log("one");
    two();
}

function two() {
    console.log("two");
    three();
}

function three() {
    console.log("three");
}

one();
```

The execution can be visualized as:

```
Global
  ↓
one()
  ↓
two()
  ↓
three()
```

At the deepest point, the Call Stack is:

```
three()
two()
one()
Global
```

Then JavaScript starts removing them in reverse order:

```
three()  ← removed first
two()    ← removed second
one()    ← removed third
Global   ← remains
```

This is **LIFO — Last In, First Out**.

The browser’s Chrome DevTools lets you actually pause this execution with breakpoints and observe the Call Stack, Scope, and currently executing line instead of having to understand it only through diagrams.