# Numbers

## **Number Data Type**

The **Number** data type in JavaScript is used to represent **numeric values**. Unlike many programming languages that have separate data types for integers and floating-point numbers, JavaScript uses a **single `Number` data type** for both. Whether the value is `10`, `3.14`, `-25`, or `0.001`, they are all stored as numbers.

Internally, JavaScript stores all numbers using the **IEEE 754 Double-Precision 64-bit Floating Point** format. This allows JavaScript to represent both whole numbers and decimal numbers using the same data type.

For example,

```jsx
let age = 22;
let price = 99.99;
let temperature = -5;

console.log(age);
console.log(price);
console.log(temperature);
```

### **Ways to Represent Numbers**

JavaScript provides multiple ways to represent numbers depending on the requirement. Since JavaScript uses a single **Number** data type for both integers and floating-point values, numbers can be written in **decimal**, **exponential (scientific notation)**, **hexadecimal**, **binary**, and can also represent special numeric values such as **Infinity** and **-Infinity**.

```jsx
let decimal = 123.45;
let exponential = 1.5e3;
let hexadecimal = 0xFF;
let binary = 0b1010;
let positiveInfinity = 1 / 0;
let negativeInfinity = -1 / 0;

console.log(decimal);
console.log(exponential);
console.log(hexadecimal);
console.log(binary);
console.log(positiveInfinity);
console.log(negativeInfinity);
```

Output:

```
123.45
1500
255
10
Infinity
-Infinity
```

---

## Number Methods

### **`toString(base)`**

The `toString()` method converts a number into its **string representation**. By default, the conversion is performed using **base 10 (decimal)**. However, JavaScript also allows converting numbers into different number systems such as binary, octal, and hexadecimal by specifying the base.

Syntax:

```jsx
number.toString(base)
```

Example:

```jsx
let num = 255;

console.log(num.toString());
console.log(num.toString(16));
console.log(num.toString(2));
```

Output

```
255
ff
11111111
```

Common bases are:

- Base 2 → Binary
- Base 8 → Octal
- Base 10 → Decimal
- Base 16 → Hexadecimal

---

### **`toFixed(digits)`**

The `toFixed()` method formats a number by fixing the number of digits after the decimal point. It returns the result as a **string**, not a number. If necessary, JavaScript rounds the number to match the specified decimal places.

Syntax:

```jsx
number.toFixed(digits)
```

Example:

```jsx
let num = 3.14159;

console.log(num.toFixed(2));
console.log(num.toFixed(4));
```

Output

```
3.14
3.1416
```

This method is commonly used when displaying prices, percentages, currency values, or any numeric value that requires a fixed number of decimal places.

---

### **`isNaN(value)`**

The `isNaN()` function checks whether a given value is **Not-a-Number (NaN)**. If the value cannot be converted into a valid number, the function returns `true`, otherwise, it returns `false`.

Syntax:

```jsx
isNaN(value)
```

Example:

```jsx
console.log(isNaN("Hello" / 2));
console.log(isNaN(123));
```

Output

```
true
false
```

Although `isNaN()` is commonly used, modern JavaScript also provides `Number.isNaN()`, which performs a stricter check and is generally preferred.

---

### **`parseInt(string, radix)`**

The `parseInt()` function converts a string into an integer. It reads the string from left to right and stops parsing as soon as it encounters an invalid character. An optional second parameter, called the **radix**, specifies the number system in which the string should be interpreted.

Syntax:

```jsx
parseInt(string, radix)
```

Example:

```jsx
console.log(parseInt("42"));
```

Output

```
42
```

Example with additional characters:

```jsx
console.log(parseInt("42px"));
```

Output

```
42
```

Since parsing stops at the first non-numeric character, `"42px"` is converted to `42`.

Example using radix:

```jsx
console.log(parseInt("1010", 2));
```

Output

```
10
```

Here, `"1010"` is interpreted as a binary number.

---

### **`toPrecision()`**

The `toPrecision()` method formats a number to the specified **total number of significant digits**. Unlike `toFixed()`, which fixes the digits after the decimal point, `toPrecision()` controls the total number of meaningful digits in the entire number.

If necessary, JavaScript rounds the number to match the specified precision. It returns the result as a **string**.

**Syntax:**

```jsx
number.toPrecision(precision)
```

**Example:**

```jsx
let num = 123.456789;

console.log(num.toPrecision(4));
console.log(num.toPrecision(6));
console.log(num.toPrecision(2));
```

**Output**

```
123.5
123.457
1.2e+2
```

`toPrecision()` is commonly used when the total number of significant digits is more important than the number of decimal places, such as in scientific calculations or measurements.

---

### **`toLocaleString()`**

The `toLocaleString()` method converts a number into a **locale-specific formatted string**. It formats numbers according to the conventions of a particular country or region.

For example, different countries use different digit grouping systems.

- USA: `1,000,000`
- India: `10,00,000`

**Syntax:**

```jsx
number.toLocaleString(locale)
```

**Example (Indian Number System):**

```jsx
let amount = 123456789;

console.log(amount.toLocaleString("en-IN"));
```

**Output**

```
12,34,56,789
```

Here,

- `"en"` represents the English language.
- `"IN"` represents the Indian locale.

Using `"en-IN"` formats numbers according to the Indian numbering system (Lakhs and Crores).

---