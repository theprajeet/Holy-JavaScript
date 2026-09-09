# Control Flow Statements

## Control Flow

**Control Flow** refers to the order in which statements in a JavaScript program are executed. By default, JavaScript executes code from top to bottom. However, using control flow statements, we can change the normal execution order by making decisions based on conditions.

JavaScript provides the following control flow statements:

- `if`
- `else if`
- `else`
- `switch`

### **`if` Statement**

The **`if` statement** is used to execute a block of code only when a specified condition evaluates to `true`. If the condition is `false`, the code inside the `if` block is skipped.

### **Syntax**

```jsx
if (condition) {
    // Code to execute if the condition is true
}
```

### **Example**

```jsx
let age = 20;

if (age >= 18) {
    console.log("You are eligible to vote.");
}
```

Output:

```
You are eligible to vote.
```

**Note:** If the `if` statement contains only a single statement, curly braces `{}` are optional. However, it is considered a good practice to always use curly braces as they improve readability and help avoid errors.

### **`else if` Statement**

The **`else if` statement** is used when multiple conditions need to be checked. JavaScript evaluates each condition from top to bottom and executes the block corresponding to the first condition that evaluates to `true`. Once a matching condition is found, the remaining conditions are skipped.

### **Syntax**

```jsx
if (condition1) {
    // Code Block
}
else if (condition2) {
    // Code Block
}
else if (condition3) {
    // Code Block
}
else {
    // Default Code Block
}
```

### **Example**

```jsx
let marks = 75;

if (marks >= 90) {
    console.log("Grade: A");
}
else if (marks >= 80) {
    console.log("Grade: B");
}
else if (marks >= 70) {
    console.log("Grade: C");
}
else {
    console.log("Grade: D");
}
```

Output:

```
Grade: C
```

### **`else` Statement**

The **`else` statement** is used along with an `if` or `else if` statement to execute a block of code when none of the specified conditions evaluate to `true`. Since it acts as the default case, an `else` statement does not have a condition.

### **Syntax**

```jsx
if (condition) {
    // Code Block
}
else {
    // Code Block
}
```

### **Example**

```jsx
let age = 15;

if (age >= 18) {
    console.log("Eligible to Vote");
}
else {
    console.log("Not Eligible to Vote");
}
```

Output:

```
Not Eligible to Vote
```

### **Ternary Operator (`?:`)**

The **Ternary Operator** is a shorthand way of writing a simple `if...else` statement. It evaluates a condition and returns one value if the condition is `true` and another value if the condition is `false`.

### **Syntax**

```jsx
condition ? expressionIfTrue : expressionIfFalse;
```

### **Example**

```jsx
let num = 11;

let result = (num % 2 === 0) ? "Even" : "Odd";

console.log(result);
```

Output:

```
Odd
```

The ternary operator is best suited for simple conditions where only two possible outcomes exist.

### **`switch` Statement**

The **`switch` statement** is used to execute different blocks of code based on the value of an expression. It provides a cleaner and more readable alternative to writing multiple `if...else if` statements when comparing the same variable against multiple values.

Each `case` represents a possible value, and the `default` block executes if none of the cases match.

### **Syntax**

```jsx
switch (expression) {
    case value1:
        // Code Block
        break;

    case value2:
        // Code Block
        break;

    default:
        // Default Code Block
}
```

### **Example**

```jsx
let color = prompt("Enter Color:");

switch (color) {
    case "green":
        console.log("This is Green Color");
        break;

    case "yellow":
        console.log("This is Yellow Color");
        break;

    case "red":
        console.log("This is Red Color");
        break;

    default:
        console.log("Invalid Input");
}
```

**Note:** The `break` statement is used to terminate the execution of a matched case. If `break` is omitted, JavaScript continues executing the subsequent cases even if they do not match. This behavior is known as **fall-through**. The `default` case is optional and executes only when no case matches the given expression.

---