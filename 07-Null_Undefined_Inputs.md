# Null, Undefined, Inputs

## Null and Undefined

Both `null` and `undefined` represent the absence of a value in JavaScript, but they are used in different situations. Understanding the difference between them is important because they behave differently during type checking, comparisons, and type coercion.

`undefined` indicates that a variable exists but has **not been assigned a value yet**. The JavaScript engine automatically assigns it.

`null`, on the other hand, represents the **intentional absence of a value**. It is assigned explicitly by the programmer when they want to indicate that a variable currently holds no meaningful value.

| `undefined` | `null` |
| --- | --- |
| Represents the absence of a value because it has **not been assigned yet**. | Represents the intentional absence of a value. |
| Automatically assigned by JavaScript. | Explicitly assigned by the programmer. |
| Default value of an uninitialized variable. | Used when you intentionally want “no value”. |
| Type is `"undefined"`. | Due to a historical bug, `typeof` returns `"object"`. |

### **Example**

```jsx
let a;

console.log(a); // undefined
```

Since no value has been assigned to `a`, JavaScript automatically assigns it `undefined`.

```jsx
let b = null;

console.log(b); // null
```

Here, `null` is assigned intentionally to indicate that the variable currently has no value.

### **`typeof` with `null` and `undefined`**

```jsx
let a;
let b = null;

console.log(typeof a);
console.log(typeof b);
```

Output

```jsx
undefined
object
```

### **Why does `typeof null` return `"object"`?**

This is one of JavaScript’s oldest and most well-known historical bugs.

When JavaScript was first developed, values were internally represented using **type tags**. The type tag reserved for objects was `0`, and unfortunately `null` was also represented using the same tag. As a result, `typeof null` incorrectly returns `"object"`.

Although this bug was discovered later, it could not be fixed because millions of existing websites depended on this behaviour. Changing it would have broken backward compatibility across the web.

Even though

```jsx
typeof null
```

returns

```jsx
"object"
```

**`null` is still a primitive data type**, not an object.

### **Comparison with `null`**

One of the most confusing aspects of JavaScript is how `null` behaves during comparisons. This happens because **comparison operators (`>`, `<`, `>=`, `<=`)** and the **loose equality operator (`==`)** use different internal comparison algorithms.

Consider the following example.

```jsx
console.log(null > 0);  // false
console.log(null == 0); // false
console.log(null >= 0); // true
```

At first glance, these results appear contradictory, but they occur because JavaScript treats equality checks and relational comparisons differently.

The **loose equality operator (`==`)** checks equality using JavaScript’s loose equality rules. In this case, it **does not convert `null` into `0`**, so the comparison evaluates to `false`.

```jsx
null == 0
```

Output

```jsx
false
```

However, the relational comparison operators (`>`, `<`, `>=`, `<=`) first convert `null` into the numeric value `0` before performing the comparison.

For example,

```jsx
null >= 0 internally becmes 0 >= 0 
OUTPUT -> true
```

Similarly,

```jsx
null > 0 internally becomes 0 > 0
OUTPUT -> false
```

This difference in behavior is why comparisons involving `null` often produce results that seem inconsistent.

### **Comparison with `undefined`**

Unlike `null`, `undefined` is **not converted into a meaningful numeric value** during comparisons.

```jsx
console.log(undefined == 0); // false
console.log(undefined > 0);  // false
console.log(undefined < 0);  // false
console.log(undefined >= 0); // false
```

Whether you use the loose equality operator or relational comparison operators, comparisons involving `undefined` almost always evaluate to `false`.

---

## Inputs in JavaScript

An **Input** is the process of accepting data or values from the user while the program is running. Since the values are provided during execution rather than being hardcoded, they are known as **dynamic inputs**.

JavaScript provides multiple ways to accept input from users depending on where the program is running and how the application is designed.

### **Ways to Accept Input in JavaScript**

JavaScript can accept input from various sources, such as:

- Browser Prompt (`prompt()` method)
- HTML Form Elements (Textboxes, Radio Buttons, Checkboxes, etc.)
- Event Listeners (Click, Keyboard, Mouse, Input Events, etc.)

### **Using the `prompt()` Method**

The **`prompt()`** method is a built-in browser function that displays a dialog box asking the user to enter a value. When the prompt appears, the execution of the JavaScript program is paused until the user either enters a value and clicks **OK** or clicks **Cancel**.

The value entered by the user is returned as a **string**. If the user clicks **Cancel**, the method returns `null`.

### **Syntax**

```jsx
let variableName = prompt(message, defaultValue);
```

- `message` – The text displayed inside the dialog box.
- `defaultValue` – An optional value that appears in the input field by default.

### Note

- `prompt()` is available only in web browsers and is **not supported in Node.js**.
- It always returns the user’s input as a **string**, even if a number is entered.
- To perform mathematical operations on numeric input, convert the returned string into a number using methods such as `Number()` or `parseInt()`.
- The program waits for the user to respond before executing the next line of code. This behavior is known as **blocking execution**.

### **Example**

```jsx
let name = prompt("What is your name?", "Guest");

console.log("Hello, " + name + "!");
```

If the user enters:

```
Steve
```

Output:

```
Hello, Steve!
```

If the user clicks **Cancel**, `prompt()` returns `null`.

```jsx
let name = prompt("What is your name?");

console.log(name);
```

Output:

```
null
```

---