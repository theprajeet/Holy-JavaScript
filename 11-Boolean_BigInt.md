# Boolean, BigInt

## Boolean

The **Boolean** data type is used to represent **logical values** in JavaScript. A Boolean can have only **one of two possible values**: `true` or `false`. These values are commonly used to make decisions, control the flow of a program, and evaluate conditions in statements such as `if`, `else`, `while`, and loops.

Unlike strings or numbers, Boolean values do not represent text or numeric data. Instead, they represent the outcome of a logical expression or condition.

For example,

```jsx
let isLoggedIn = true;
let isAdmin = false;

console.log(isLoggedIn);
console.log(isAdmin);
```

Output:

```
true
false
```

### **Ways to Create Boolean Values**

In JavaScript, Boolean values can be created in two different ways.

- By directly assigning `true` or `false`.
- By using the `Boolean()` function, which converts another value into a Boolean.

```jsx
// Direct Assignment
let isAvailable = true;
let isActive = false;

// Using Boolean() Function
console.log(Boolean(1));
console.log(Boolean(0));
console.log(Boolean("Hello"));
console.log(Boolean(""));

console.log(isAvailable);
console.log(isActive);
```

Output:

```
true
false
true
false
true
false
```

The different ways of creating Boolean values are explained below.

- **Direct Assignment** is the simplest way to create a Boolean. The variable is directly assigned either `true` or `false`.
- **Boolean() Function** converts any JavaScript value into its corresponding Boolean value. Depending on the value passed, it returns either `true` or `false`. This conversion is known as **Boolean Type Conversion** or **Boolean Coercion**.

### **`Boolean()` Function**

The `Boolean()` function converts a given value into its Boolean equivalent. It is commonly used to determine whether a value is considered **truthy** or **falsy** in JavaScript.

Syntax:

```jsx
Boolean(value)
```

Example:

```jsx
console.log(Boolean(100));
console.log(Boolean(0));
console.log(Boolean("JavaScript"));
console.log(Boolean(""));
console.log(Boolean(null));
```

Output:

```
true
false
true
false
false
```

### **Truthy and Falsy Values**

Whenever JavaScript evaluates a condition, it automatically converts the value into either `true` or `false`. This automatic conversion is known as **Boolean Coercion**.

Every value in JavaScript is classified as either **truthy** or **falsy**.

A **falsy value** is any value that becomes `false` when converted to a Boolean.

JavaScript has only **eight falsy values**.

- `false`
- `0`
- `0`
- `0n` (BigInt Zero)
- `""` (Empty String)
- `null`
- `undefined`
- `NaN`

Every other value in JavaScript is considered **truthy**.

Some common truthy values include:

- Any non-zero number (`1`, `10`, `3.14`)
- Any non-empty string (`"Hello"`, `"0"`, `"false"`)
- Empty arrays (`[]`)
- Empty objects (`{}`)
- Functions
- Dates

For example,

```jsx
console.log(Boolean(1));
console.log(Boolean(-20));
console.log(Boolean("Hello"));
console.log(Boolean([]));
console.log(Boolean({}));

console.log(Boolean(0));
console.log(Boolean(""));
console.log(Boolean(null));
console.log(Boolean(undefined));
console.log(Boolean(NaN));
```

Output:

```
true
true
true
true
true
false
false
false
false
false
```

### **Boolean Values in Conditions**

Boolean values are primarily used when making decisions in a program. Whenever a condition is evaluated, JavaScript expects the result to be either `true` or `false`. If the condition evaluates to `true`, the corresponding block of code executes; otherwise, JavaScript moves to the next condition or executes the `else` block.

Example:

```jsx
let isAdmin = true;
let isUser = false;

if (isAdmin && isUser) {
    console.log("You are both an Admin and a User.");
}
else if (isAdmin || isUser) {
    console.log("You are either an Admin or a User.");
}
else {
    console.log("You have no access.");
}
```

Output:

```
You are either an Admin or a User.
```

In the above example:

- `&&` (AND) returns `true` only if **both** operands are `true`.
- `||` (OR) returns `true` if **at least one** operand is `true`.

Since `isAdmin` is `true` and `isUser` is `false`, the first condition evaluates to `false`, while the second condition evaluates to `true`. Therefore, the `else if` block is executed.

### **Difference Between Boolean Values and Truthy/Falsy Values**

A **Boolean value** is an actual value of the Boolean data type and can only be `true` or `false`.

A **truthy** or **falsy** value, on the other hand, belongs to any JavaScript data type, such as Number, String, Object, Array, or Null. When JavaScript evaluates a condition, it automatically converts these values into either `true` or `false`.

For example,

```jsx
console.log(Boolean(100));
console.log(Boolean(""));
console.log(Boolean([]));
```

Output:

```
true
false
true
```

Here, `100`, `""`, and `[]` are **not Boolean values**. They are Number, String, and Array values respectively. However, JavaScript converts them into Boolean values whenever they are evaluated in a Boolean context such as an `if` statement.

Understanding the difference between Boolean values and truthy/falsy values is essential because JavaScript performs this automatic conversion frequently while evaluating conditions.

---

## BigInt

### **BigInt Data Type**

The **BigInt** data type is a primitive data type introduced in **ES2020 (ECMAScript 2020)** that is used to represent **integers of arbitrary size**. Unlike the `Number` data type, which can safely represent integers only up to a certain limit, `BigInt` can store and perform arithmetic on extremely large integers without losing precision.

Unlike **String** and **Number**, **BigInt** does **not have its own built-in methods**. A BigInt is a primitive data type, and it inherits methods from the `Object` wrapper just like other primitives. Therefore, there are no commonly used methods such as `toUpperCase()` for String or `toFixed()` for Number.

The `Number` data type in JavaScript follows the **IEEE 754 Double-Precision Floating Point** format, which means it can safely represent integers only between:

```
-(2^53 - 1)  to  (2^53 - 1)
```

or

```
-9007199254740991  to  9007199254740991
```

This range is known as the **Safe Integer Range**. Numbers outside this range may lose precision.

For example,

```jsx
console.log(9007199254740992);
console.log(9007199254740993);
```

Output:

```
9007199254740992
9007199254740992
```

Notice that the second number should have been `9007199254740993`, but JavaScript loses precision because it exceeds the safe integer limit.

To solve this problem, JavaScript introduced the **BigInt** data type.

### **Ways to Create a BigInt**

A BigInt can be created in two different ways.

- By appending `n` to the end of an integer.
- By using the `BigInt()` function.

```jsx
// Using n
let bigNumber1 = 123456789012345678901234567890n;

// Using BigInt()
let bigNumber2 = BigInt("123456789012345678901234567890");

console.log(bigNumber1);
console.log(bigNumber2);
```

Output:

```
123456789012345678901234567890n
123456789012345678901234567890n
```

The different ways of creating a BigInt are explained below.

- **Appending `n`** is the simplest and most common way to create a BigInt. Any integer followed by the letter `n` is treated as a BigInt.
- **Using the `BigInt()` function** converts a valid integer value or string into a BigInt. This approach is useful when the value is received as user input or from an external source.

### **`BigInt()` Function**

The `BigInt()` function converts a numeric value or a numeric string into a BigInt.

Syntax:

```jsx
BigInt(value)
```

Example:

```jsx
console.log(BigInt(100));
console.log(BigInt("999999999999999999999999"));
```

Output:

```
100n
999999999999999999999999n
```

### **Limitations of BigInt**

Although BigInt is useful for representing very large integers, it has some important limitations.

#### **BigInt Cannot Store Decimal Numbers**

BigInt is designed to represent **integers only**. Decimal or floating-point numbers are not allowed.

```jsx
let num = 10.5n;
```

Output:

```
SyntaxError
```

#### **Number and BigInt Cannot Be Mixed**

Arithmetic operations cannot be performed directly between a `Number` and a `BigInt`.

```jsx
let a = 10;
let b = 20n;

console.log(a + b);
```

Output:

```
TypeError: Cannot mix BigInt and other types
```

If required, both values must first be converted to the same type.

```jsx
let a = BigInt(10);
let b = 20n;

console.log(a + b);
```

Output:

```
30n
```

---