# Loops

# **Loops**

### **What are Loops?**

A loop is a control flow statement that allows us to execute the same block of code repeatedly without writing it multiple times.

Instead of writing the same statement again and again, we place it inside a loop and specify when the loop should stop.

Loops are useful when performing repetitive tasks such as printing numbers, traversing arrays, processing objects, reading user input repeatedly, or performing calculations multiple times.

Example:

```jsx
console.log(1);
console.log(2);
console.log(3);
console.log(4);
console.log(5);
```

Instead of writing this repeatedly, we can use a loop:

```jsx
for (let i = 1; i <= 5; i++) {
    console.log(i);
}
```

Output

```
1
2
3
4
5
```

---

### **Types of Loops in JavaScript**

JavaScript provides several ways to repeat code.

- `for`
- `while`
- `do...while`
- `for...of`
- `for...in`
- `Array.forEach()`

Each one is designed for a different purpose.

---

## **for Loop**

### **What is a for Loop?**

A `for` loop is used when we know approximately how many times we want to execute a block of code.

It combines initialization, condition checking, and updating the loop variable into one concise statement, making it the most commonly used loop in JavaScript.

---

### **Syntax**

```jsx
for (initialization; condition; increment/decrement) {
    // Code
}
```

Example

```jsx
for (let i = 1; i <= 5; i++) {
    console.log(i);
}
```

Output

```
1
2
3
4
5
```

### **How a for Loop Works**

```jsx
for (let i = 1; i <= 3; i++) {
    console.log(i);
}
```

Execution Flow

```
Initialization
      │
      ▼
Check Condition
      │
True ─────────────► Execute Code
      ▲                 │
      │                 ▼
      └──── Increment ◄──
```

When the condition becomes false, the loop stops.

### **Parts of a for Loop**

```jsx
for (let i = 0; i < 5; i++)
```

#### **Initialization**

Runs only once before the loop starts.

```jsx
let i = 0;
```

#### **Condition**

Checked before every iteration.

```jsx
i < 5
```

If true, execute the loop.

If false, stop.

#### **Increment / Decrement**

Runs after every iteration.

```jsx
i++
```

Updates the loop variable.

### **Nested for Loop**

A loop can contain another loop inside it.

The outer loop controls how many times the inner loop executes.

Example

```jsx
for (let i = 1; i <= 3; i++) {
    for (let j = 1; j <= 3; j++) {
        console.log(i, j);
    }
}
```

Output

```
1 1
1 2
1 3

2 1
2 2
2 3

3 1
3 2
3 3
```

A common real-world use case is generating multiplication tables.

```jsx
for (let i = 1; i <= 10; i++) {
    for (let j = 1; j <= 10; j++) {
        console.log(`${i} × ${j} = ${i * j}`);
    }
}
```

### **Looping Through Arrays using for**

Arrays are commonly traversed using a `for` loop.

Example

```jsx
const superheroes = ["Flash", "Batman", "Superman"];

for (let i = 0; i < superheroes.length; i++) {
    console.log(superheroes[i]);
}
```

Output

```
Flash
Batman
Superman
```

Notice that we use `array.length` so the loop automatically adapts if the array size changes.

---

## **while Loop**

### **What is a while Loop?**

A `while` loop repeatedly executes a block of code as long as the given condition remains true.

Unlike a `for` loop, it is mainly used when we **do not know in advance how many times the loop needs to run**. The loop continues until the required condition becomes false.

### **Syntax**

```jsx
while (condition) {
    // Code
}
```

Example

```jsx
let i = 1;

while (i <= 5) {
    console.log(i);
    i++;
}
```

Output

```
1
2
3
4
5
```

### **How a while Loop Works**

```
Check Condition
      │
True ─────────────► Execute Code
      ▲                 │
      │                 ▼
      └──── Update Variable
```

If the condition is false initially, the loop never executes.

### **Real-world Use Cases**

Sometimes we do not know how many times we need to repeat a task beforehand. In such cases, a `while` loop is more suitable than a `for` loop.

Some common examples include:

- Asking the user to enter the correct password until it matches.
- Waiting until a file finishes downloading.
- Repeatedly checking whether data has become available.
- Reading values continuously until a special value is entered.

Example

```jsx
let password = "";

while (password !== "1234") {
    password = prompt("Enter Password");
}

console.log("Access Granted");
```

The loop keeps asking for the password until the correct one is entered.

---

## **do…while Loop**

### **What is a do…while Loop?**

A `do...while` loop works similarly to a `while` loop, but with one important difference: **the loop body executes first, and only then is the condition checked**.

Because of this, the code inside the loop is guaranteed to run at least once, even if the condition is false from the beginning.

---

### **Syntax**

```jsx
do {
    // Code
} while (condition);
```

Example

```jsx
let i = 1;

do {
    console.log(i);
    i++;
} while (i <= 5);
```

Output

```
1
2
3
4
5
```

### **Difference Between while and do…while**

```jsx
let i = 10;

while (i < 5) {
    console.log(i);
}
```

Output

```
Nothing
```

The condition is checked first, so the loop never runs.

```jsx
let i = 10;
do {
    console.log(i);
} while (i < 5);
```

Output

```
10
```

---

### **When Should You Use do…while?**

Use a `do...while` loop when the task must execute at least once before deciding whether to continue.

Examples include:

- Displaying a menu once before asking whether to repeat.
- Asking for user input at least one time.
- Game menus that must appear before checking the player’s choice.

---

## **break Statement**

The `break` statement immediately terminates the loop, even if the loop condition is still true.

Example

```jsx
for (let i = 1; i <= 10; i++) {
    if (i === 5) {
        break;
    }
    console.log(i);
}
```

Output

```
1
2
3
4
```

Once `5` is encountered, the loop exits completely.

---

## **continue Statement**

The `continue` statement skips only the current iteration and proceeds directly to the next iteration.

Example

```jsx
for (let i = 1; i <= 5; i++) {
    if (i === 3) {
        continue;
    }
    console.log(i);
}
```

Output

```
1
2
4
5
```

Only `3` is skipped.

---

## **for…of Loop**

### **What is for…of?**

The `for...of` loop is used to iterate over iterable objects such as arrays, strings, maps, and sets.

Instead of accessing elements using indexes, it gives each value directly.

### **Syntax**

```jsx
for (const value of iterable) {
    // Code
}
```

Example (Array)

```jsx
const numbers = [10, 20, 30];

for (const num of numbers) {
    console.log(num);
}
```

Output

```
10
20
30
```

Example (String)

```jsx
const text = "Hello";

for (const ch of text) {

    console.log(ch);

}
```

Output

```
H
e
l
l
o
```

### **Iterating over a Map**

A `Map` stores data as key-value pairs while maintaining the insertion order. Unlike objects, duplicate keys are not allowed, and a `Map` itself is iterable, which means it works directly with a `for...of` loop.

Example

```jsx
const map = new Map();

map.set("IN", "India");
map.set("USA", "United States");

for (const [key, value] of map) {
    console.log(key, value);
}
```

Output

```
IN India
USA United States
```

---

## **for…in Loop**

### **What is for…in?**

The `for...in` loop iterates over the enumerable property names (keys) of an object.

Unlike `for...of`, it gives you the **keys**, not the values.

### **Syntax**

```jsx
for (const key in object) {
    // Code
}
```

Example

```jsx
const languages = {
    js: "JavaScript",
    cpp: "C++",
    py: "Python"
};

for (const key in languages) {
    console.log(key, languages[key]);
}
```

Output

```
js JavaScript
cpp C++
py Python
```

### **Using for…in with Arrays**

Arrays can also be used with `for...in`, but instead of returning the array elements directly, it returns the indexes.

```jsx
const arr = ["Java", "Python", "JavaScript"];

for (const index in arr) {
    console.log(index, arr[index]);
}
```

Output

```
0 Java
1 Python
2 JavaScript
```

Although this works, `for...of` is generally a better choice for arrays because it directly provides the element values rather than the indexes.

### **Why Doesn’t for…in Work with Maps?**

`Map` objects are iterable, but their entries are **not enumerable object properties**. Since `for...in` only iterates over enumerable properties of plain objects, it cannot iterate over a `Map`.

```jsx
const map = new Map();
map.set("IN", "India");
for (const key in map) {
    console.log(key);
}
```

Output

```
(No output)
```

To iterate over a `Map`, use `for...of` instead.

---

## **forEach()**

### **What is forEach()?**

`forEach()` is an array method that executes a callback function once for every element in the array.

Unlike traditional loops, you do not have to manage indexes or loop counters manually.

### **Syntax**

```jsx
array.forEach(function(element) {
    // Code
});
```

Example

```jsx
const coding = ["JavaScript", "Python", "Java"];

coding.forEach((language) => {
    console.log(language);
});
```

Output

```
JavaScript
Python
Java
```

### **Callback Parameters**

The callback function can receive three parameters:

```jsx
array.forEach((item, index, array) => {

});
```

- `item` → Current element.
- `index` → Current index.
- `array` → Original array.

Example

```jsx
coding.forEach((item, index) => {

    console.log(index, item);

});
```

### **Iterating Over an Array of Objects**

`forEach()` is especially useful when working with arrays of objects.

Example

```jsx
const myCoding = [

    {
        languageName: "JavaScript",
        languageFileName: "js"
    },

    {
        languageName: "Python",
        languageFileName: "py"
    }

];

myCoding.forEach((item) => {
    console.log(item.languageName);
});
```

Output

```
JavaScript
Python
```

The important thing to remember is that **`forEach()` does not return a new array**. If you try to store its result:

```jsx
const values = coding.forEach((item) => {
    return item;
});

console.log(values);
```

the result will be:

```
undefined
```

Even though you wrote `return item`, `forEach()` itself does not collect those returned values and create an array from them.

So `forEach()` is mainly used when you want to **do something with each element**, such as printing values, updating something externally, performing an operation, etc.

### **`forEach()` returning values**

Consider:

```jsx
const myNums = [1, 2, 3, 4, 5];

const result = myNums.forEach((num) => {
    return num * 2;
});

console.log(result);
```

The callback technically returns `2`, `4`, `6`, `8`, and `10`, but `forEach()` ignores those returned values. Therefore:

```
result → undefined
```

If your intention is to create a **new array based on the values of the original array**, `map()` is generally the appropriate method.

---