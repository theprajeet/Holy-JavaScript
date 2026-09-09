# Math Library

# **Math Library**

JavaScript provides a built-in **Math Library** that contains many useful mathematical functions and constants.

A **library** is simply a collection of **pre-written functions, methods, properties, and utilities** that programmers can use instead of writing everything from scratch.

For example, instead of writing your own logic to round numbers or generate random numbers, JavaScript already provides these functionalities through the **Math** library.

The Math library is a built-in object, so there is **no need to import it**.

Its syntax is:

```jsx
Math.methodName()
```

or

```jsx
Math.propertyName
```

### **`Math.abs()`**

The `Math.abs()` method returns the **absolute (positive) value** of a number. An absolute number is a number that is either positive or zero.

If the number is already positive, it returns the same value. If it is negative, it removes the negative sign.

**Syntax:**

```jsx
Math.abs(number)
```

**Example:**

```jsx
console.log(Math.abs(-25));
console.log(Math.abs(18));
```

**Output**

```
25
18
```

---

### **`Math.round()`**

The `Math.round()` method rounds a number to the **nearest integer**.

- Decimal part **less than 0.5** → rounds down.
- Decimal part **0.5 or greater** → rounds up.

**Syntax:**

```jsx
Math.round(number)
```

**Example:**

```jsx
console.log(Math.round(4.3));
console.log(Math.round(4.7));
```

**Output**

```
4
5
```

---

### **`Math.PI`**

`Math.PI` is a built-in mathematical constant that represents the value of **π (Pi)**.

Its approximate value is:

```
3.141592653589793
```

It is commonly used in calculations involving circles and geometry.

**Example:**

```jsx
console.log(Math.PI);
```

**Output**

```
3.141592653589793
```

---

### **`Math.ceil()`**

The `Math.ceil()` method always rounds a number **upwards** to the nearest integer.

Even if the decimal part is very small, the number is rounded to the next integer.

**Syntax:**

```jsx
Math.ceil(number)
```

**Example:**

```jsx
console.log(Math.ceil(4.1));
console.log(Math.ceil(7.9));
```

**Output**

```
5
8
```

---

### **`Math.floor()`**

The `Math.floor()` method always rounds a number **downwards** to the nearest integer.

It simply removes the decimal part.

**Syntax:**

```jsx
Math.floor(number)
```

**Example:**

```jsx
console.log(Math.floor(4.9));
console.log(Math.floor(7.1));
```

**Output**

```
4
7
```

---

### **`Math.min()`**

The `Math.min()` method returns the **smallest** value from the given numbers.

**Syntax:**

```jsx
Math.min(value1, value2, ...)
```

**Example:**

```jsx
console.log(Math.min(12, 4, 25, 9));
```

**Output**

```
4
```

---

### **`Math.max()`**

The `Math.max()` method returns the **largest** value from the given numbers.

**Syntax:**

```jsx
Math.max(value1, value2, ...)
```

**Example:**

```jsx
console.log(Math.max(12, 4, 25, 9));
```

**Output**

```
25
```

---

## **`Math.random()`**

The `Math.random()` method generates a **random decimal number** between **0 (inclusive)** and **1 (exclusive)**.

This means the generated number:

- Can be **0**
- Can be any decimal value greater than 0
- Will **never be exactly 1**

**Syntax:**

```jsx
Math.random()
```

**Example:**

```jsx
console.log(Math.random());

**Possible Output**
0.438394 OR 0.912731 OR 0.054672
```

Every execution produces a different random decimal number.

---

## Generating a Random Number

### **Why do we use `Math.floor(Math.random() * 10) + 1`?**

When generating a random integer, `Math.random()` alone is not enough because it returns a **decimal number between `0` (inclusive) and `1` (exclusive)**.

```jsx
Math.random()
```

Range:

```
0 ≤ number < 1
```

To generate a random integer between **1 and 10**, we use:

```jsx
Math.floor(Math.random() * 10) + 1
```

Here’s what each part does:

### **`10`**

Multiplying by `10` changes the range from:

```
0 ≤ number < 1
```

to

```
0 ≤ number < 10
```

The result is **still a decimal number**.

Possible outputs:

```
2.45
7.81
9.13
0.56
```

### **`Math.floor()`**

`Math.floor()` removes the decimal part by rounding the number **down** to the nearest integer.

```jsx
Math.floor(Math.random() * 10)
```

Possible outputs:

```
0
1
2
...
9
```

### **`+ 1`**

Adding `1` shifts the entire range by one.

```jsx
Math.floor(Math.random() * 10) + 1
```

Possible outputs:

```
1
2
3
...
10
```

Therefore,

```jsx
Math.floor(Math.random() * 10) + 1
```

generates a **random integer between `1` and `10` (both inclusive)**.

**If  + 1 is not used, then it generates a random integer between  `0` and `10`  (both inclusive).**

---

### **Generating a Random Number Between Any Range**

Suppose we want to generate a random integer between **10 and 20**.

```jsx
const min = 10;
const max = 20;

console.log(
    Math.floor(Math.random() * (max - min + 1)) + min
);
```

This formula can be understood by breaking it into three parts:

### **`(max - min + 1)`**

This calculates the **total number of possible values** in the range.

For example,

```jsx
max = 20
min = 10
```

```
20 - 10 + 1 = 11
```

So, there are **11 possible numbers (10 to 20)**.

### **`Math.floor(Math.random() * (max - min + 1))`**

`Math.random()` generates a decimal number between `0` and `1`.

Multiplying it by `11` changes the range to:

```
0 ≤ number < 11
```

Using `Math.floor()` removes the decimal part, producing integers from:

```
0
1
2
...
10
```

### **`+ min`**

Finally, adding `min` shifts the entire range to start from the required minimum value.

```
10
11
12
...
20
```

Therefore,

```jsx
Math.floor(Math.random() * (max - min + 1)) + min
```

generates a **random integer between `min` and `max` (both inclusive)**. By simply changing the values of `min` and `max`, the same formula can be used to generate random numbers within any desired range.

---