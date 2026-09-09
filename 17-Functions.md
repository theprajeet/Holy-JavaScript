# Functions

## **Functions in JavaScript**

A **function** in JavaScript is a reusable block of code designed to perform a specific task. A function is first **defined** using the `function` keyword and a name, followed by parentheses and a block of code containing the instructions to be executed. Defining a function does not execute its code, the function must be **called** using its name followed by `()` whenever we want to execute it. Functions can also accept input values through **parameters**, which are placeholder variables defined in the function declaration, while the actual values supplied when calling the function are called **arguments**. This allows the same function to perform its task with different values each time it is called. Functions therefore provide **code reusability, readability, maintainability, and better program organisation** by breaking a large program into smaller, logical units.

**Syntax**

```jsx
function functionName() {

    // Code to execute

}
```

Example:

```jsx
function sayMyName() {
    console.log("H");
    console.log("I");
    console.log("T");
    console.log("E");
    console.log("S");
    console.log("H");
}
```

The above function only defines the instructions. Nothing is executed until the function is called.

### **Calling (Executing) a Function**

A function is executed by writing its name followed by parentheses `()`.

```jsx
sayMyName();
```

Output

```
H
I
T
E
S
H
```

Writing only the function name does **not** execute it.

```jsx
sayMyName
```

This only refers to the function.

This difference becomes very important in React, DOM events (`onclick`), callbacks, higher-order functions, etc.

### **Function Declaration**

A function created using the `function` keyword is called a **Function Declaration**.

```jsx
function greet() {
    console.log("Hello");
}
```

Characteristics:

- Can be called multiple times.
- Improves code reusability.
- Gets hoisted (explained later).

## **Parameters and Arguments**

Functions often need input values to perform operations.

### **Parameters**

Parameters are variables written while defining a function.

```jsx
function addTwoNumbers(number1, number2) {

}
```

Here,

```jsx
number1
number2
```

are **parameters**.

Think of parameters as placeholders waiting to receive values.

### **Arguments**

Arguments are the actual values passed while calling the function.

```jsx
addTwoNumbers(3, 5);
```

Here,

```jsx
3
5
```

are **arguments**.

---

## **What Happens If No Arguments Are Passed?**

Example

```jsx
function add(a, b) {

    return a + b;

}

console.log(add());
```

Output

```
NaN
```

because internally,

```
undefined + undefined
```

becomes

```
NaN
```

## **Default Parameters**

Sometimes users may not pass any argument.

Instead of getting `undefined`, you can provide a default value.

Example

```jsx
function loginUserMessage(username = "Sam") {

    return `${username} just logged in`;

}
```

Calling it without arguments:

```jsx
console.log(loginUserMessage());
```

Output

```
Sam just logged in
```

---

## **Types of Functions**

## **1. Named Functions**

A **named function** is a function that is explicitly given a name when it is created. The name allows the function to be called and executed whenever it is needed in the program. Named functions are useful when the same piece of code needs to be executed multiple times because instead of rewriting the code, we can simply call the function using its name. In JavaScript, named functions are commonly created using the `function` keyword followed by the function name, parentheses `()`, and a block of code `{}` containing the statements to be executed.

```jsx
function greet() {
    console.log("Hello, World!");
}

greet();
```

Here, `greet` is the **name of the function**, and `greet()` is used to **call (invoke) the function**.

---

There are two ways in which arguments can be passed to a function

1. **Positional Arguments and** 
2. **Named Arguments**

### **Positional Arguments**

In positional arguments, values are passed to a function based on their **position**. The first argument is assigned to the first parameter, the second argument to the second parameter, and so on. JavaScript matches arguments with parameters in the order they appear.

```jsx
function addTwoNumbers(number1, number2) {
    return number1 + number2;
}
```

Here,

- The first argument is assigned to `number1`.
- The second argument is assigned to `number2`.

#### **Passing Values Based on Position**

```jsx
addTwoNumbers(3, 5);
```

JavaScript internally treats it as:

```jsx
number1 = 3
number2 = 5
```

The function returns:

```jsx
8
```

If the positions are changed,

```jsx
addTwoNumbers(5, 3);
```

then

```jsx
number1 = 5
number2 = 3
```

The values assigned to the parameters change because JavaScript only looks at the order in which arguments are passed.

---

### **Named Arguments**

JavaScript does not have true named arguments like some other programming languages. Instead, a similar behaviour can be achieved by passing an **object** to a function. Since object properties have names, we can access values using those property names instead of relying only on position.

### **What are Named Arguments?**

Instead of passing multiple individual values, we pass a single object containing related properties. The function then accesses the required values using their property names.

### **Passing Objects to Functions**

An object can be passed to a function just like any other value.

```jsx
const user = {
    username: "Robert",
    price: 199
}

function handleObject(anyObject) {

    console.log(
        `Username is ${anyObject.username} and price is ${anyObject.price}`
    );
}

handleObject(user);
```

The function receives the `user` object and accesses its properties using:

```jsx
anyObject.username
anyObject.price
```

### **Passing an Object Directly**

Creating a separate variable is not mandatory. An object can also be created directly while calling the function.

```jsx
handleObject({
    username: "sam",
    price: 399
});
```

The function behaves exactly the same because it still receives an object.

---

### **Passing Arrays to Functions**

Just like objects, arrays can also be passed as arguments to functions. The function receives the entire array and can access its elements using their index.

### **Passing an Array Variable**

```jsx
const myNewArray = [200, 400, 100, 600];

function returnSecondValue(getArray) {
    return getArray[1];
}

console.log(returnSecondValue(myNewArray));
```

Output

```
400
```

Here,

- `getArray` receives the entire array.
- `getArray[1]` returns the second element because array indexing starts from `0`.

### **Passing an Array Directly**

Instead of creating a variable, the array can be passed directly.

```jsx
console.log(

    returnSecondValue([200, 400, 500, 1000])

);
```

Output

```
400
```

The function behaves exactly the same because it only receives an array.

---

## **Rest Parameters (…)**

### **Why Rest Parameters are Needed**

Sometimes we don’t know how many arguments will be passed to a function. A common example is a shopping cart. Suppose a user adds multiple products to a shopping cart. The number of products can vary each time. One user may add two products, while another may add five or ten.

Instead of creating separate parameters for every possible value, **JavaScript provides Rest Parameters, which collect all remaining arguments into a single array**.

**Syntax**

Rest Parameters are declared using three dots (`...`) before the parameter name.

```jsx
function calculateCartPrice(...num1) {
    return num1;
}
```

Here,

- `...num1` collects all arguments.
- Inside the function, `num1` becomes an array.

Example

Normal parameters can appear before a Rest Parameter.

```jsx
function calculateCartPrice(val1, val2, ...num1) {
    return num1;
}
console.log(
    calculateCartPrice(200, 400, 500, 2000)
);
```

Internally,

```
val1 = 200
val2 = 400
num1 = [500, 2000]
```

Output

```
[500, 2000]
```

The first two arguments are assigned to `val1` and `val2`. The remaining arguments are collected into the `num1` array. This makes Rest Parameters useful when a function accepts a fixed number of initial arguments followed by an unknown number of additional arguments.

---

## **Function Declaration vs Function Expression**

JavaScript provides multiple ways to create functions.

The two most common ways are:

1. Function Declaration
2. Function Expression

Although both create functions, they behave differently, especially during hoisting.

### **Function Declaration**

A Function Declaration is created using the `function` keyword followed by a function name.

#### **Characteristics**

- Declared using the `function` keyword.
- Must have a function name.
- Can be called before its declaration because of hoisting.
- Stored in memory before code execution begins.

Example

```jsx
function addOne(num) {
    return num + 1;
}

console.log(addOne(5));
```

Output

```
6
```

### **Function Expression**

A Function Expression creates a function and stores it inside a variable. It means you are writing a function as part of a larger expression (like assigning it to a variable or passing it as an argument) rather than declaring it standalone

The Function Expression can either be a named function expression or an anonymous function expression.  

**Anonymous Function -** An anonymous function is simply a function without a name.

```jsx
// Named Function
const subTwo = function subtract(num) {
    return num - 2;
}

// Anonymous Function
const addTwo = function(num) {
    return num + 2;
}

console.log(addTwo(2));
console.log(subtTwo(4));

console.log(subtract(4)); // Doesn't work -> Reference Error

// Named function expressions should be called using the variable they’re assigned to. It’s better to avoid named functions in function expressions unless you  want the name for debugging or tracking.
```

Notice that the function itself has no name. The variable `addTwo` is used to access it.

In named function i.e subTwo

---

## **Hoisting**

Hoisting is JavaScript’s behavior of moving declarations to memory before the code starts executing However, Function Declarations and Function Expressions are hoisted differently.

### **Hoisting with Function Declaration**

Consider the following code.

```jsx
console.log(addOne(5));

function addOne(num) {
    return num + 1;
}
```

Output

```
6
```

Even though the function is called before it is declared, the program works correctly. This happens because JavaScript stores the complete function in memory during the creation phase, before executing the code. Therefore, the function is available everywhere within its scope.

### **Hoisting with Function Expression**

Now consider the following example.

```jsx
addTwo(5);

const addTwo = function(num) {

    return num + 2;

}
```

Output

```
ReferenceError (or cannot access before initialization)
```

This code throws an error. Unlike Function Declarations, the variable `addTwo` is created first, but the function is assigned to it **only when JavaScript reaches that line during execution**. Until then, the variable cannot be used.

#### **Why One Works Before Declaration and the Other Doesn’t**

The difference comes from how JavaScript stores them in memory.

A **Function Declaration** works even before it appears in the code because during the **creation phase**, JavaScript **hoists** the entire function (its name and body) into memory, making it available anywhere in its scope. A **Function Expression**, however, is usually assigned to a variable (`let`, `const`, or `var`), and only the variable is hoisted—not the function itself. With `let` and `const`, the variable remains in the **Temporal Dead Zone (TDZ)** until the assignment is reached, so calling it before that results in an error. Therefore, **Function Declarations can be called before they are declared, whereas Function Expressions cannot because the function isn’t assigned to the variable until execution reaches that line.**

- Function Declarations are completely hoisted.
- Function Expressions are assigned to variables, and the assignment happens only during execution, so they cannot be used before that point.

---

## **2. Arrow Functions**

Arrow Functions were introduced in ES6 as a shorter and cleaner way to write functions. They provide a more concise syntax than normal functions and behave differently with the `this` keyword.

### **What are Arrow Functions?**

An **Arrow Function** is a function written using the arrow (`=>`) syntax instead of the `function` keyword.

Normal Function

```jsx
function greet() {
    console.log("Hello");
}
```

Arrow Function

```jsx
const greet = () => {
    console.log("Hello");
}
```

Both functions perform the same task, but the arrow function uses a shorter syntax.**Syntax**

**General Syntax**

```jsx
const functionName = (parameters) => {
    // function body
}
```

If the function accepts parameters,

```jsx
const addTwo = (num1, num2) => {
    return num1 + num2;
}
```

### **Rules of Arrow Functions**

1. **If there is only one parameter, parentheses `()` are optional.**

```jsx
const greet = name => {
    console.log(`Hello ${name}`);
}
```

1. **If there are zero or more than one parameters, parentheses `()` are mandatory.**

```jsx
const greet = () => {
    console.log("Hello");
}

const add = (a, b) => {
    return a + b;
}
```

1. **If the function body contains only one expression, curly braces `{}` and the `return` keyword can be omitted (implicit return).**

```jsx
const square = num => num * num;
```

1. **If the function body contains multiple statements, curly braces `{}` are mandatory, and `return` must be used explicitly (if a value needs to be returned).**

```jsx
const add = (a, b) => {
    const sum = a + b;
    return sum;
}
```

1. **Arrow functions do not have their own `this`. They inherit `this` from the surrounding (lexical) scope.**

```jsx
const person = {
    name: "John",
    greet: () => {
        console.log(this.name); // undefined
    }
}
```

1. **Arrow functions cannot be used as constructors with the `new` keyword.**

```jsx
const Person = (name) => {
    this.name = name;
}

new Person("John"); // Error
```

1. **Arrow functions do not have their own `arguments` object. Use rest parameters (`...args`) instead.**

```jsx
const sum = (...args) => {
    console.log(args);
}
```

1. **Arrow functions cannot be hoisted like Function Declarations because they are Function Expressions.**

```jsx
greet(); // Error

const greet = () => {
    console.log("Hello");
}
```

1. **Arrow functions cannot be used as generator functions (`yield` is not allowed).**

```jsx
// Invalid
const gen = () => {
    yield 1;
}
```

---

## **3. Immediately Invoked Function Expression (IIFE)**

### **What is an IIFE?**

An **Immediately Invoked Function Expression (IIFE)** is a function that executes immediately after it is defined. Instead of declaring a function and calling it later, both operations happen together.

General Syntax

```jsx
(function () {
    // code
})();
```

The first pair of parentheses contains the function definition.

The second pair of parentheses at last, immediately invokes (calls) the function.

### **Why use an IIFE?**

#### **1. Execute Initialization Code Immediately**

Sometimes an application contains code that should run only once, such as establishing a database connection. Instead of creating a function and calling it separately, an IIFE allows the code to execute immediately.

Example

```jsx
(function () {
    console.log("DB CONNECTED");
})();
```

As soon as JavaScript reaches this code, it executes automatically.

#### **2. Avoid Global Scope Pollution**

Large applications may contain many global variables from different files. These global variables can accidentally interfere with one another. An IIFE creates its own scope, helping isolate variables and preventing unnecessary interaction with the global scope.

### **Named IIFE**

A Named IIFE includes a function name.

```jsx
(function db() {
    console.log("DB CONNECTED");
})();
```

Here, `db` is the function’s name. The function executes immediately after it is defined.

### **Anonymous IIFE**

An Anonymous IIFE has no function name.

```jsx
(() => {
    console.log("DB CONNECTED TWO");
})();
```

This is commonly used when the function does not need to be referenced elsewhere.

### **Passing Parameters to an IIFE**

Just like normal functions, IIFEs can also accept parameters.

```jsx
((name) => {
    console.log(`DB CONNECTED TWO ${name}`);
})("Ram");
```

Output

```
DB CONNECTED TWO Ram
```

Here,

- `name` is the parameter.
- `"Ram"` is the argument passed during invocation.

### **Multiple IIFEs**

Multiple IIFEs can be written in the same file.

```jsx
(function () {
    console.log("First");
})();

(() => {
    console.log("Second");
})();
```

Output

```
First
Second
```

### **Why a Semicolon (`;`) is Required**

When multiple IIFEs are written one after another, the previous IIFE should end with a semicolon.

```jsx
(function () {
    console.log("First");
})() // --> Semicolon(;) is required here

(() => {
    console.log("Second");
})() // --> Here as well (;)

OUTPUT 
First
TypeError: (intermediate value)(...) is not a function
```

Without the semicolon, JavaScript may treat the second IIFE as a continuation of the first expression, resulting in an **error**. Therefore, when writing multiple IIFEs consecutively, always terminate the previous IIFE with a semicolon.

---

## 4. Higher Order Function (HOF) and Callback Functions

### **What is a Higher-Order Function?**

A **Higher-Order Function (HOF)** is a function that satisfies **at least one** of the following conditions:

1. It **accepts another function as an argument**, or
2. It **returns another function** as its result.

Basically,

A Higher-Order Function is a function that either takes another function as an argument or returns another function as its result.

### **Why do we use Higher-Order Functions?**

Higher-Order Functions help us write reusable and cleaner code.

Instead of writing the same logic repeatedly, we can pass different functions to perform different operations while keeping the main function unchanged.

They are commonly used for tasks like:

- Iterating through data
- Filtering data
- Mapping data
- Reducing data

Because of this, JavaScript provides several built-in Higher-Order Functions.

Some common examples are:

```jsx
map()
filter()
reduce()
```

These methods accept another function as an argument, making them Higher-Order Functions.

### **Passing a Function as an Argument**

One of the most common characteristics of a Higher-Order Function is that it accepts another function as a parameter.

General Syntax

```jsx
function higherOrder(callback) {
    callback();
}
```

Here,

- `higherOrder()` is the Higher-Order Function.
- `callback` is a parameter that expects a function.
- `callback()` executes the function that was passed.

Notice that **`callback` is a function reference**, while **`callback()` executes that function**.

#### **Returning a Function (Definition Only)**

A Higher-Order Function can also return another function.

General Syntax

```jsx
function higherOrder() {
    return anotherFunction;
}
```

In this case, instead of returning a number or a string, the function returns another function.

**Example**

Consider the following code.

```jsx
function sayHello() {
    console.log("Hello from Callback");
}

function greet(callback) {
    console.log("This is HOF");
    callback();
}

greet(sayHello);
```

Output

```
This is HOF
Hello from Callback
```

---

## **Callback Functions**

### **What are Callback Functions?**

A **Callback Function** is a function that is **passed as an argument to another function and is later executed (called) by that function**. In other words, a callback is a function that “calls back” when another function decides to execute it. A callback is used when we want one function to perform some work and then execute another function afterward. 

This concept is widely used in asynchronous programming, event handling, timers, promises, and many JavaScript A

#### **Passing a Function as an Argument**

Normally, we pass values to a function.

```jsx
addTwoNumbers(3, 5);
```

Here,

- `3` and `5` are arguments.

Similarly, JavaScript also allows us to pass an entire function.

```jsx
someFunction(anotherFunction);
```

Notice that **parentheses are not used** after `anotherFunction`.

This is because we are passing the **function itself**, not executing it.

#### **Function Reference vs Function Execution (Revisited)**

This concept was introduced earlier, but it becomes much more important when passing functions as arguments.

#### **Function Reference**

A function reference means passing the function itself.

```jsx
sayMyName
```

The function is **not executed**.

Instead, JavaScript passes its reference.

#### **Function Execution**

Adding parentheses executes the function immediately.

```jsx
sayMyName();
```

Now JavaScript executes the function and uses its returned value.

When passing a function as an argument, we almost always pass the **reference**, not the execution.

Correct

```jsx
someFunction(sayMyName);
```

Incorrect

```jsx
someFunction(sayMyName());
```

The second example executes `sayMyName()` immediately and passes its return value instead of the function. This distinction is extremely important and is used extensively in callbacks and event handling.

#### **Callback Example**

```jsx
function greet(name) {
    console.log(`Hello ${name}`);
}

function executeFunction(callback) {
    callback("Robert");
}

executeFunction(greet);
```

Output

```
Hello Robert
```

Here,

- `greet` is passed as an argument to `executeFunction()`.
- Inside `executeFunction()`, the parameter `callback` stores the reference to `greet`.
- When `callback()` is executed, it actually calls the `greet()` function.
- Therefore, `greet` is called a **Callback Function** because it is executed by another function.

---

### **Relationship of Callback Function with Higher-Order Functions**

Both concepts always work together.

- The function receiving another function is called the **Higher-Order Function**.
- The function being passed is called the **Callback Function**.

Example

```jsx
greet(sayHello); // Here, greet is a Higher Order Function
```

Example

```jsx
function greet(name) {
    return "Hello " + name;
}

function processUser(name, callback) {
    console.log(callback(name));
}

processUser("Raja", greet);
```

Output

```jsx
Hello Raja
```

| **Higher-Order Function** | **Callback Function** |
| --- | --- |
| Receives another function as an argument or returns one. | A function passed into another function. |
| Controls when the callback executes. | Waits until it is executed by the Higher-Order Function. |
| Example: `greet(callback)` | Example: `sayHello`, `greet` |

---

## **5. Generator Functions**

### **What is a Generator Function?**

A **Generator Function** is a special type of function that can **pause** its execution and later **continue from exactly where it stopped**. Unlike a normal function, which runs completely from start to finish in one execution, a Generator Function executes only until it reaches a `yield` statement. Each time it is resumed, it continues from the previous `yield`.

### **`function*`**

A Generator Function is declared using an asterisk (`*`) after the `function` keyword.

Syntax

```jsx
function* functionName() {
	// function body
}
```

Example

```jsx
function* speak() {
    yield "Hello";
    yield "Welcome";
    yield "Let's begin";
}
```

The `*` tells JavaScript that this is a Generator Function instead of a normal function.

### **`yield`**

The `yield` keyword is unique to Generator Functions. It behaves like a combination of **pause** and **return**.

When JavaScript encounters a `yield` statement:

- It returns the specified value.
- It pauses the function.
- The next execution resumes immediately after that `yield`.

Example

```jsx
yield "Hello";
```

This returns

```
Hello
```

and pauses the function at that point. When execution resumes, it continues with the next line.

### **`next()`**

Calling a Generator Function does **not** execute its body immediately.

Instead, it returns a **Generator Object**.

```jsx
const gen = speak();
```

Nothing is printed yet.

To start execution, we call

```jsx
gen.next();
```

Each call to `next()` moves the Generator to the next `yield`.

**Example** 

```jsx
function* speak() {
    yield "Hello";
    yield "Welcome";
    yield "Let's begin";
}

const gen = speak();
console.log(gen.next().value);
console.log(gen.next().value);
console.log(gen.next().value);
console.log(gen.next().value);
```

Output

```
Hello
Welcome
Let's begin
undefined
```

Here the first three calls to `gen.next().value` return `"Hello"`, `"Welcome"`, and `"Let's begin"` respectively. After the last `yield`, the generator has no more values to produce, so the fourth call returns `undefined` because the generator has finished executing.

### **Real-time Use Case (Paginated Data Loader)**

One practical use case of Generator Functions is loading paginated data. Instead of loading every product at once, products can be returned page by page.

Example

```jsx
function* loadProducts() {
    yield ["Product 1", "Product 2", "Product 3"];
    yield ["Product 4", "Product 5", "Product 6"];
    yield ["Product 7", "Product 8", "Product 9"];
}

const productGen = loadProducts();
console.log(productGen.next().value);
console.log(productGen.next().value);
console.log(productGen.next().value);
```

Instead of loading all nine products at once, the Generator returns one page at a time. This approach is memory-efficient and matches how many real-world applications load data progressively, such as product listings, social media feeds, and search results.

---