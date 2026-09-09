# JavaScript Variables, Hoisting, TDZ

## **JavaScript Variables**

Variables in JavaScript are used to store data values that can be accessed and manipulated throughout a program. Think of a variable as a **named container** or **storage location** in memory where a value is stored. Instead of using the actual value repeatedly, we assign it to a variable and use the variable name whenever we need that value.

JavaScript is a **dynamically typed language**, which means you do not need to specify the data type of a variable while declaring it. The JavaScript engine automatically determines the data type at runtime based on the value assigned to the variable.

Variables in JavaScript can be declared using three keywords:

- `var`
- `let`
- `const`

```jsx
// Old way
var a = 10;

// Preferred for variables whose value may change
let b = 20;

// Preferred for variables whose value should not change
const c = 30;

console.log(a);
console.log(b);
console.log(c);
```

### **1. var Keyword**

The `var` keyword is the original way of declaring variables in JavaScript. Variables declared using `var` are **function-scoped**, which means they are accessible throughout the entire function in which they are declared, regardless of the block (`{}`) in which they are written. If declared outside any function, they become globally scoped. Variables declared with `var` are also **hoisted**, meaning their declarations are moved to the top of their scope during the compilation phase. Additionally, `var` allows both **reassignment** and **redeclaration**, making it more flexible but also more prone to accidental bugs.

```jsx
var name = "John";

var age = 20;
var age = 25;  // Redeclaration allowed

console.log(name);
console.log(age);
```

### **2. let Keyword**

The `let` keyword was introduced in **ES6 (ECMAScript 2015)** as a safer alternative to `var`. Variables declared using `let` are **block-scoped**, meaning they can only be accessed within the block (`{}`) in which they are declared. Like `var`, variables declared with `let` can be **reassigned** whenever needed, but unlike `var`, they **cannot be redeclared within the same scope**. Although `let` declarations are technically hoisted, they remain inaccessible until their declaration is reached during execution because they exist inside the **Temporal Dead Zone (TDZ)**.

```jsx
let city = "Bengaluru";
let temperature = 30;

let num = 20;
let num = 25; // Redeclaration not allowed

console.log(city);
console.log(temperature);
```

### **3. const Keyword**

The `const` keyword was also introduced in **ES6 (ECMAScript 2015)** and is used to declare variables whose values should not change after initialisation. Like `let`, `const` is **block-scoped** and exists inside the **Temporal Dead Zone (TDZ)** until its declaration is executed. A variable declared with `const` **must be initialised at the time of declaration** and **cannot be reassigned or redeclared** later. However, if a `const` variable stores an object or an array, the contents of that object or array can still be modified because `const` protects only the variable’s reference, not the data stored inside it.

```jsx
const pi = 3.14159;
const country = "India";

const name = "Jeff";
name = "Elon"; // Error

const person = {
    name: "Musk",
    age: 22
};

person = {
    name: "Bezos",
    age: 25
}; // You’re trying to make person refer to a different object. Not Allowed

person.name = "Zuck";
person.age = 25; // Allowed 
// Array/Object's content can be changed but not the reference.

console.log(person);

console.log(pi);
console.log(country);
```

#### **Why Should `var` Always Be Avoided?**

Although `var` is still supported for backward compatibility, it is generally recommended to avoid using it in modern JavaScript. Since `var` is **function-scoped** instead of **block-scoped**, variables declared inside blocks such as `if`, `for`, or `while` remain accessible outside those blocks, often leading to unexpected behaviour. It also allows **redeclaration** within the same scope, increasing the chances of accidentally overwriting existing variables. Furthermore, because `var` is hoisted and initialised with `undefined`, it can produce confusing results that make debugging more difficult. These issues were the primary reasons why `let` and `const` were introduced in ES6. In modern JavaScript development, `const` should be the default choice whenever a variable’s value does not need to change, while `let` should be used only when reassignment is required. Using these keywords results in safer, more predictable, and easier-to-maintain code.

```jsx
// FUNCTION AND BLOCK SCOPING
function example() {
    if (true) {
        var message = "Hello";
    }
    // You might expect message to be unavailable in this scope
    // But var ignores block scope.
    console.log(message); // "Hello"
}

// In case of let
function example() {
     if (true) {
         let message = "Hello";
     }
     console.log(message); // ReferenceError
}
 
// REDECLARATION

var age = 20;
var age = 50; 
// Accidentally declaring age again is allowed.
// This can overwrite the existing variable without warning.

// HOISTING

console.log(name); // undefined
var name = "Tony";
console.log(name); // Tony
// The declaration is processed before execution,
// but the assignment happens only when execution reaches it.
// If it was let, the compiler would have thrown an error.

console.log(name); // ReferenceError: Cannot access 'name' before initialization
let name = "Tony";
console.log(name); 
 
```

### **Rules for Naming Variables**

When naming variables in JavaScript, the following rules must be followed:

- A variable name must begin with a **letter (A–Z or a–z)**, an **underscore (_)**, or a **dollar sign ($)**.
- After the first character, a variable name may contain **letters, numbers, underscores, or dollar signs**.
- Variable names are **case-sensitive**, meaning `age`, `Age`, and `AGE` are treated as three different variables.
- JavaScript **reserved keywords** (such as `function`, `return`, `class`, `if`, etc.) cannot be used as variable names.
- Variable names should be meaningful and descriptive to improve code readability.

```jsx
let userName = "Rahul";   // Valid
let $price = 100;         // Valid
let _count = 5;           // Valid

// Invalid
// let 123name = "John";
// let function = "Hello";
```

---

## Hoisting

Hoisting is part of JavaScript’s execution model. Before executing any code, the JavaScript engine performs a **memory creation phase**, where it scans the entire scope and allocates memory for variables and functions. This allows the engine to know what identifiers exist before execution begins and enables features such as function declarations being callable before their definition. Before we learn more about Hoisting, it is important to understand the Temporal Dead Zone (TDZ).

### **Temporal Dead Zone (TDZ)**

The **Temporal Dead Zone (TDZ)** is the period between the **beginning of a block scope** and the point where a variable declared with `let` or `const` is initialised. During this period, the variable exists in memory because it has been **hoisted**, but it **cannot be accessed**. If you try to access it before its declaration is executed, JavaScript throws a **ReferenceError**. In simple terms, the Temporal Dead Zone is a safety mechanism introduced in **ES6 (ECMAScript 2015)** to prevent developers from accidentally using variables before they have been initialised.

- Variables declared with let and const are hoisted but not initialized.
- Accessing these variables before their declaration throws a ReferenceError.
- Initialization occurs only when execution reaches the declaration line.
- TDZ exists only within the scope where the variable is declared.
- It applies only to let and const, not to var (which is initialised as undefined).

**Hoisting** is JavaScript’s default behaviour of moving **declarations** of variables and functions to the top of their scope during the **memory creation phase** (also called the creation phase) before the code begins executing. This does **not** mean that the code is physically moved to the top of the file. Instead, the JavaScript engine internally allocates memory for declarations before executing the program.

Because of hoisting, some variables and functions can be referenced before they appear in the source code. However, the behaviour depends on whether they are declared using `var`, `let`, `const`, or as functions.

For example,

```jsx
console.log(a);
var a = 10;
console.log(a);
```

Output:

```
undefined
10
```

Although `a` is declared after `console.log()`, the program does not throw an error because the declaration of `a` is hoisted to the top of its scope. During the memory creation phase, memory is allocated for `a` and it is initialized with the value `undefined`. Later, during the execution phase, the value `10` is assigned to `a`.

Internally, the JavaScript engine behaves as if the code were:

```jsx
var a = undefined;
console.log(a);
a = 10;
```

It is important to note that **only the declaration is hoisted, not the assignment**. The value is assigned only when the execution reaches the assignment statement.

### **Hoisting with `let` and `const`**

Variables declared using `let` and `const` are also hoisted, but unlike `var`, they are **not initialized** during the memory creation phase. Instead, they remain in a special state called the **Temporal Dead Zone (TDZ)** from the beginning of their scope until the declaration statement is executed.

Attempting to access them before their declaration results in a **ReferenceError**.

For example,

```jsx
console.log(a);
let a = 10;
```

Output:

```
ReferenceError: Cannot access 'a' before initialization
```

This error occurs because the variable exists in memory but is inside the **Temporal Dead Zone**, making it inaccessible until its declaration is executed.

The same behavior applies to `const`.

```jsx
console.log(PI);
const PI = 3.14;
```

Output:

```
ReferenceError: Cannot access 'PI' before initialization
```

### **Hoisting with Functions**

Function declarations are also hoisted. Unlike variables, the **entire function definition** is stored in memory during the memory creation phase. Therefore, a function can be called even before it appears in the source code.

```jsx
greet();

function greet() {
    console.log("Hello");
}
```

Output:

```
Hello
```

This works because the complete function is available in memory before the execution phase begins.

However, **function expressions** and **arrow functions** behave according to the variable keyword (`var`, `let`, or `const`) used to store them.

```jsx
greet();

var greet = function () {
    console.log("Hello");
};
```

Output:

```
TypeError: greet is not a function
```

Here, only the variable `greet` is hoisted and initialised with `undefined`. Since `undefined` is not a function, calling it results in a **TypeError**.

Similarly,

```jsx
greet();

const greet = () => {
    console.log("Hello");
};
```

Output:

```
ReferenceError: Cannot access 'greet' before initialization
```

Since `greet` is declared using `const`, it remains inside the **Temporal Dead Zone** until its declaration is executed.

---