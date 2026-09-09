# Type Casting

## **Type Casting**

**Type Casting** is the process of converting a value from one data type to another. Since JavaScript is a **dynamically typed** and **weakly typed** language, it automatically converts data types in certain situations. However, there are many cases where we need to manually convert one data type into another before performing operations.

For example, values received from HTML forms, URL parameters, or APIs are often returned as strings even if they represent numbers. If mathematical operations need to be performed on these values, they must first be converted into numbers.

JavaScript supports two types of type casting:

- Implicit Type Casting (Type Coercion)
- Explicit Type Casting

## **Implicit Type Casting (Type Coercion)**

**Implicit Type Casting**, also known as **Type Coercion**, is the automatic conversion of one data type into another by the JavaScript engine during program execution. The developer does not explicitly perform the conversion; instead, JavaScript decides how to convert the values based on the operation being performed.

This usually happens when values of different data types are used together in an expression.

#### **String Concatenation**

When the `+` operator is used with a string, JavaScript converts the other operand into a string and performs string concatenation.

```jsx
console.log("5" + 10);
```

Output:

```
510
```

The number `10` is converted into the string `"10"` before concatenation.

Result:

```
String + Number → String
```

#### **Arithmetic Operations**

Arithmetic operators such as `-`, `*`, `/`, `%`, and `**` require numeric operands. If a string contains a valid numeric value, JavaScript automatically converts it into a number.

```jsx
console.log("10" - 5);
```

Output:

```
5 (String - Number → Number)
```

#### **Boolean Conversion in Conditions**

Conditional statements such as `if`, `while`, and `for` expect a boolean value. If a non-boolean value is provided, JavaScript automatically converts it into either `true` or `false`.

```jsx
if ("Hello") {
    console.log("Executed");
}

if (0) {
    console.log("Won't execute");
}
```

Output:

```
Executed
```

Here, `"Hello"` is a **truthy** value, whereas `0` is a **falsy** value.

---

## **Explicit Type Casting**

**Explicit Type Casting** is the manual conversion of one data type into another using JavaScript’s built-in conversion functions. Unlike implicit conversion, the developer has full control over when and how the conversion takes place.

JavaScript provides several built-in methods for explicit conversion.

### **String Conversion**

String conversion is used when we want to convert any value into a string.

The commonly used methods are:

- `String(value)`
- `value.toString()`

```jsx
let num = 123;

console.log(String(num));
console.log(num.toString());
```

Output:

```
123
123
```

Both methods return the string `"123"`.

---

### **Number Conversion**

Number conversion is used when we want to perform mathematical operations on values that are currently stored as strings or other data types.

The commonly used methods are:

- `Number(value)`
- `parseInt(value)`
- `parseFloat(value)`

```jsx
let str = "456";

console.log(Number(str));
console.log(parseInt(str));
console.log(parseFloat("3.14"));
```

Output

```
456
456
3.14
```

Although these methods convert values into numbers, they behave differently.

- `Number()` converts the entire value into a number. If the complete value cannot be converted, it returns `NaN`.
- `parseInt()` extracts the integer portion from the beginning of the string and ignores everything after the first invalid character.
- `parseFloat()` works similarly but preserves the decimal portion.

For example,

```jsx
console.log(Number("33abc"));
console.log(parseInt("33abc"));
```

Output

```
NaN
33
```

This is because `"33abc"` is not a completely valid number, so `Number()` fails, whereas `parseInt()` successfully extracts the integer `33`.

### **Number Conversion Results**

```jsx
console.log(Number("33"));         // 33
console.log(Number("33.5"));       // 33.5
console.log(Number("33abc"));      // NaN
console.log(Number("abc"));        // NaN
console.log(Number(true));         // 1
console.log(Number(false));        // 0
console.log(Number(null));         // 0
console.log(Number(undefined));    // NaN
```

One important thing to remember is that **`NaN` stands for “Not a Number”, but surprisingly, its data type is still `number`.**

```jsx
console.log(typeof NaN);
```

Output

```
number
```

This is because JavaScript treats `NaN` as a special numeric value that represents an invalid mathematical result rather than a separate data type.

---

### **Boolean Conversion**

Boolean conversion converts any value into either `true` or `false`.

The built-in method used is:

- `Boolean(value)`

```jsx
console.log(Boolean(1));
console.log(Boolean(0));
console.log(Boolean(""));
console.log(Boolean("Hello"));
```

Output

```
true
false
false
true
```

### **Boolean Conversion Results**

```jsx
console.log(Boolean(1));           // true
console.log(Boolean(0));           // false
console.log(Boolean(-1));          // true
console.log(Boolean("Hello"));     // true
console.log(Boolean(""));          // false
console.log(Boolean(null));        // false
console.log(Boolean(undefined));   // false
console.log(Boolean(NaN));         // false
```

Any non-empty string evaluates to `true`, whereas an empty string evaluates to `false`.

This behavior is widely used in conditional statements.

```jsx
let username = "";

if (username) {
    console.log("User exists");
} else {
    console.log("No username");
}
```

Output

```
No username
```

---

### **Date to Number Conversion**

A JavaScript `Date` object can also be converted into a number. The resulting number represents the number of milliseconds that have elapsed since **January 1, 1970 (Unix Epoch).**

```jsx
let date = new Date();

console.log(Number(date));
```

Output

```
1754030902451
```

The exact value depends on the current date and time.

---