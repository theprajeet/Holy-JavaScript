# Operators and Operations

## **Operators**

An **Operator** is a special symbol or keyword that performs an operation on one or more **operands** (values or variables) and produces a result.

An **Operand** is the value or variable on which an operator performs the operation.

For example, in the expression `10 + 5`:

- `10` and `5` are **operands**.
- `+` is the **operator**.

### **Types of Operators in JavaScript**

JavaScript provides different types of operators for performing different operations.

- Arithmetic Operators
- Comparison Operators
- Logical Operators
- Assignment Operators
- Ternary Operator

---

## **Arithmetic Operators**

Arithmetic operators are used to perform mathematical calculations on numeric values.

| **Operator** | **Description** |
| --- | --- |
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |
| `%` | Modulus (Remainder) |
| `**` | Exponentiation (Power) |
| `++` | Increment |
| `--` | Decrement |

```jsx
console.log(2 + 2);  // 4
console.log(10 - 4); // 6
console.log(5 * 4);  // 20
console.log(10 / 2); // 5
console.log(10 % 3); // 1
console.log(2 ** 4); // 16
```

The **modulus (`%`)** operator returns the remainder after division and is commonly used to check whether a number is even or odd.

```jsx
console.log(10 % 2); // 0
console.log(7 % 2);  // 1
```

### **Unary Minus Operator**

The unary minus (`-`) operator converts a positive number into its negative equivalent.

```jsx
let value = 3;

let negativeValue = -value;

console.log(negativeValue);

OUTPUT 
-3
```

---

## **Comparison Operators**

Comparison operators are used to compare two values. Every comparison operation returns a **Boolean** value (`true` or `false`).

| **Operator** | **Description** |
| --- | --- |
| `==` | Equal to (Compares only the value after type coercion, if required) |
| `===` | Strict Equal (Compares both value and data type without type conversion) |
| `!=` | Not Equal (Compares only the value after type coercion, if required) |
| `!==` | Strict Not Equal (Compares both value and data type without type conversion) |
| `>` | Greater Than |
| `<` | Less Than |
| `>=` | Greater Than or Equal To |
| `<=` | Less Than or Equal To |

Basic comparisons are straightforward when both operands belong to the same data type.

```jsx
console.log(2 > 1);  // true
console.log(2 >= 1); // true
console.log(2 < 1);  // false
console.log(2 == 2); // true
console.log(2 != 1); // true
```

### **Comparison Between Different Data Types**

JavaScript allows comparisons between different data types. Before performing the comparison, it attempts to convert the operands into compatible types using **implicit type coercion**.

For example,

```jsx
console.log("2" > 1);
console.log("02" > 1);
```

Output

```
true
true
```

In both cases, JavaScript automatically converts the strings (`"2"` and `"02"`) into numbers before performing the comparison.

Although JavaScript supports this behavior, relying on automatic type conversion can produce confusing and sometimes unexpected results. As a best practice, always compare values of the same data type whenever possible.

### **Comparison with `null`**

Comparisons involving `null` are one of JavaScript’s most confusing behaviors because the **comparison operators (`>`, `<`, `>=`, `<=`)** and the **equality operator (`==`)** handle `null` differently.

Consider the following example.

```jsx
console.log(null > 0);  // false
console.log(null == 0); // false
console.log(null >= 0); // true
```

At first glance, these results appear contradictory, but they occur because JavaScript treats these operators differently.

The **equality operator (`==`)** checks equality using its own comparison algorithm and **does not convert `null` to `0`** in this case.

```jsx
null == 0 // false
```

However, the **comparison operators (`>`, `<`, `>=`, `<=`)** first convert `null` into the numeric value `0` before performing the comparison.

Therefore,

```jsx
null >= 0 becomes 0 >= 0
OUTPUT - true
```

Similarly,

```jsx
null > 0 becomes 0 > 0
OUTPUT - false
```

This difference in internal behavior is the reason for the seemingly inconsistent results.

### **Comparison with `undefined`**

Unlike `null`, `undefined` does not convert into a meaningful numeric value during comparisons.

```jsx
console.log(undefined == 0); // false
console.log(undefined > 0);  // false
console.log(undefined < 0);  // false
```

Regardless of whether you use equality or comparison operators, comparisons involving `undefined` generally evaluate to `false`.

### **Equality (`==`) vs Comparison Operators (`>`, `<`, `>=`, `<=`)**

One important concept to remember is that **equality operators** and **comparison operators** do **not** use the same internal algorithm.

IMP Notes

- `==` performs equality checks using JavaScript’s loose equality rules.
- `>`, `<`, `>=`, `<=` perform relational comparisons and may convert operands into numbers before comparing them.

This is why expressions involving `null` and `undefined` sometimes produce results that appear inconsistent.

For example,

```jsx
null == 0      // false
null >= 0      // true
```

Although both expressions involve `null` and `0`, they are evaluated using different algorithms.

### **Strict Equality (`===`)**

The **Strict Equality Operator (`===`)** compares **both the value and the data type**. Unlike the loose equality operator (`==`), it **does not perform any type conversion** before comparison.

```jsx
console.log("2" === 2); // false
```

Although both values appear to represent the number `2`, one is a **string** and the other is a **number**, so strict equality returns `false`.

Similarly,

```jsx
console.log(2 === 2); // true
```

Both the value and the data type are identical.

### **Loose Equality (`==`) vs Strict Equality (`===`)**

The loose equality operator (`==`) compares values after performing type coercion if necessary.

```jsx
console.log("2" == 2); // true
```

Here, JavaScript converts the string `"2"` into the number `2` before comparing them.

On the other hand,

```jsx
console.log("2" === 2); // false
```

No type conversion occurs, so the comparison fails because the data types are different.

---

## **Logical Operators**

Logical operators are used to combine or invert multiple conditions. They also return a **Boolean** value (`true` or `false`).

| **Operator** | **Description** |
| --- | --- |
| `&&` | Logical AND |
| `||` | Logical OR |
| `!` | Logical NOT |

---

## **Assignment Operators**

Assignment operators are used to assign values to variables and update existing values.

| **Operator** | **Description** |
| --- | --- |
| `=` | Assign |
| `+=` | Add and Assign |
| `-=` | Subtract and Assign |
| `*=` | Multiply and Assign |
| `/=` | Divide and Assign |
| `%=` | Modulus and Assign |
| `**=` | Exponentiate and Assign |

---

## **Ternary Operator (`?:`)**

The **Ternary Operator** is a shorthand way of writing an `if...else` statement. It evaluates a condition and returns one value if the condition is `true`, otherwise it returns another value if the condition is `false`.

Syntax:

```jsx
condition ? expressionIfTrue : expressionIfFalse;
```

Example:

```jsx
let age = 20;

let message = age >= 18 ? "Eligible to Vote" : "Not Eligible to Vote";

console.log(message);
```

Output:

```
Eligible to Vote
```

The above statement is equivalent to:

```jsx
let age = 20;
let message;

if (age >= 18) {
    message = "Eligible to Vote";
} else {
    message = "Not Eligible to Vote";
}

console.log(message);
```

The ternary operator is preferred when there is a simple condition with only two possible outcomes, as it makes the code shorter and easier to read.

---

## Operations

### **How the `+` Operator Behaves with Different Data Types**

The `+` operator behaves differently depending on the types of values being used. If either operand is a string, JavaScript generally treats the operation as string concatenation.

Consider the following examples.

```jsx
console.log("1" + 2); // 12
```

The number `2` is converted into the string `"2"` before concatenation.

```jsx
console.log(1 + "2"); // 12
```

Again, the number is converted into a string.

Now consider this example.

```jsx
console.log("1" + 2 + 2); // 122
```

Here, JavaScript evaluates expressions from **left to right**.

Step 1:

```
"1" + 2 becomes "12"
```

Step 2:

```
"12" + 2 becomes "122"
```

Now compare it with the next example.

```jsx
console.log(1 + 2 + "2"); // 32
```

Evaluation happens as follows.

```
STEP 1 -> 1 + 2 becomes 3
STEP 2 -> 3 + "2" becomes "32"
```

The important thing to remember is that JavaScript evaluates expressions **from left to right**. As soon as a string participates in the addition, the remaining additions become string concatenations.

---

## **Operator Precedence**

Operator precedence determines the order in which JavaScript evaluates operators within an expression.

For example,

```jsx
console.log(3 + 4 * 5); // 23
```

Multiplication has higher precedence than addition, so JavaScript performs the multiplication first.

This is evaluated as

```
3 + (4 × 5)
```

If you want a different order of execution, always use parentheses.

```jsx
console.log((3 + 4) * 5); // 35
```

Using parentheses makes expressions easier to understand and avoids confusion. Even if you know the operator precedence rules, using parentheses improves code readability.

---

### **Unary Plus (`+`)**

The unary plus operator attempts to convert its operand into a number.

```jsx
console.log(+true); // 1
```

```jsx
console.log(+""); // 0 
```

This happens because JavaScript performs an implicit number conversion before evaluating the expression.

Although this is valid JavaScript, it is generally avoided in real-world code because it reduces readability. Using `Number(value)` makes the intention much clearer.

Instead of

```jsx
+value
```

prefer

```jsx
Number(value)
```

---

### **Assignment Chaining**

JavaScript allows multiple variables to be assigned in a single statement.

```jsx
let num1, num2, num3;

num1 = num2 = num3 = 2 + 2;
```

Evaluation happens from **right to left**.

```
First,      2 + 2 becomes 4
Then,       num3 = 4
After that, num2 = 4
Finally,    num1 = 4

OUTPUT
num1 = 4
num2 = 4
num3 = 4
```

Although this syntax is valid, assigning variables separately often makes the code easier to understand.

---

## **Prefix and Postfix Increment Operators**

The increment operator (`++`) increases a variable’s value by `1`.

JavaScript provides two forms of the increment operator:

- Prefix Increment (`++x`)
- Postfix Increment (`x++`)

Although both increase the value by one, the difference lies in **when the updated value becomes available**.

#### **Prefix Increment (`++x`)**

With the prefix operator, JavaScript first increments the variable and then returns the updated value.

```jsx
let x = 5;

let y = ++x;

console.log(x); // 6
console.log(y); // 6
```

Execution:

1. Increment `x` from `5` to `6`.
2. Assign the updated value (`6`) to `y`.

#### **Postfix Increment (`x++`)**

With the postfix operator, JavaScript first returns the current value and only then increments the variable.

```jsx
let x = 5;

let y = x++;

console.log(x); // 6
console.log(y); // 5
```

Execution:

1. Store the current value (`5`) in `y`.
2. Increment `x` to `6`.

### **Prefix vs Postfix**

| **Prefix (**`++x`**)** | **Postfix (**`x++`**)** |
| --- | --- |
| Variable is incremented first | Original value is returned first |
| Updated value is used immediately | Updated value is available from the next statement onward |
| `x = 5`, `y = ++x` → `x = 6`, `y = 6` | `x = 5`, `y = x++` → `x = 6`, `y = 5` |

When the increment operator is used **by itself**, both forms produce the same final value.

```jsx
let count = 100;

count++;

console.log(count);
```

Output

```
101
```

The difference only becomes important when the expression uses the returned value immediately.

---