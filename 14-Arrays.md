# Arrays

## **Arrays**

An **Array** is a **non-primitive (reference) data type** in JavaScript that is used to store **multiple values in a single variable**. Instead of creating separate variables for each value, an array allows us to group related values together and access them using their **index**.

An array can store values of the **same data type (homogeneous)** as well as **different data types (heterogeneous)**. Since arrays are objects in JavaScript, they are stored in the **Heap Memory**, while the variable storing the array holds a **reference** to that memory location.

For example, instead of writing:

```jsx
let fruit1 = "Apple";
let fruit2 = "Banana";
let fruit3 = "Orange";
```

We can store all the values inside a single array:

```jsx
let fruits = ["Apple", "Banana", "Orange"];
```

This makes the code more organized, easier to manage, and allows us to perform operations on multiple values using loops and built-in array methods.

### **Why do we need Arrays?**

Imagine storing the marks of 100 students.

Without an array, we would have to create 100 separate variables.

```jsx
let marks1 = 85;
let marks2 = 92;
let marks3 = 78;
// ...
```

Managing these variables becomes difficult as the amount of data increases.

Instead, we can store all the marks inside a single array.

```jsx
let marks = [85, 92, 78];
```

### **Characteristics of Arrays**

Arrays have several important characteristics that make them one of the most commonly used data structures in JavaScript.

- Arrays are **non-primitive (reference) data types**.
- Arrays can store **multiple values** inside a single variable.
- Arrays are **ordered collections**, meaning every element has a fixed position (index).
- Array indexing always starts from **0**.
- Arrays are **dynamic**, meaning elements can be added or removed even after the array has been created.
- Arrays can store **homogeneous** as well as **heterogeneous** data.
- Arrays are **mutable**, meaning their contents can be modified after creation.

### **Homogeneous and Heterogeneous Arrays**

A **Homogeneous Array** contains elements of the **same data type**.

```jsx
let numbers = [10, 20, 30, 40];
```

All elements are numbers.

A **Heterogeneous Array** contains elements of **different data types**.

```jsx
let data = [
    "John",
    25,
    true,
    null,
    {
        city: "Bangalore"
    }
];
```

Here the array contains a string, number, boolean, null, and an object.

Unlike many programming languages, JavaScript allows both homogeneous and heterogeneous arrays.

---

## **How Arrays are Stored in Memory**

Since arrays are **non-primitive data types**, they are stored differently from primitive values.

When an array is created, the **actual array is stored in the Heap Memory**, while the variable itself is stored in the **Stack Memory**. The variable does **not** store the array directly; instead, it stores the **reference (memory address)** of the array.

Consider the following example:

```jsx
let fruits = ["Apple", "Banana", "Orange"];
```

Internally, JavaScript stores it similar to this:

```
                Stack Memory                         Heap Memory

        ┌──────────────────────┐          ┌─────────────────────────────┐
        │                      │          │                             │
        │ fruits               │────────► │ ["Apple","Banana","Orange"] │
        │ (Reference)          │          │                             │
        │                      │          │                             │
        └──────────────────────┘          └─────────────────────────────┘
```

Notice that the variable **does not contain the array itself**. It only stores the reference pointing to where the array exists in the Heap Memory.

This is the reason arrays are called **Reference Data Types**.

### **What Happens When an Array is Assigned to Another Variable?**

When one array variable is assigned to another, JavaScript **does not create a new array**. Instead, it copies only the **reference**.

```jsx
let fruits = ["Apple", "Banana"];

let anotherFruits = fruits;
```

Memory representation:

```
                Stack Memory                         Heap Memory

        ┌──────────────────────┐
        │ fruits               │────────┐
        └──────────────────────┘        │
                                        ▼
                                   ┌────────────────────┐
        ┌──────────────────────┐   |                    │
        │ anotherFruits        │-> | ["Apple","Banana"] │
        └──────────────────────┘   │                    │
                                   └--------------------┘
```

Both variables now point to the **same array**.

If we modify the array using either variable,

```jsx
anotherFruits.push("Orange");

console.log(fruits);
console.log(anotherFruits);
```

**Output**

```
["Apple", "Banana", "Orange"]

["Apple", "Banana", "Orange"]
```

Both variables show the updated array because there is **only one array** in memory.

---

### **Creating Arrays**

JavaScript provides two ways to create an array.

#### **Using Array Literal (`[]`)**

This is the most common and recommended way to create an array.

```jsx
let fruits = ["Apple", "Banana", "Orange"];
```

#### **Using the `Array` Constructor**

Arrays can also be created using the `Array` constructor.

```jsx
let fruits = new Array("Apple", "Banana", "Orange");
```

Both approaches create the same array.

However, the **array literal (`[]`)** is preferred because it is shorter, easier to read, and avoids certain constructor-related confusion.

---

### **Accessing Array Elements**

Every element in an array has an **index** that represents its position.

Array indexing always starts from **0**.

```
Index      0         1         2

        ["Apple","Banana","Orange"]
```

Elements can be accessed using square brackets (`[]`).

```jsx
let fruits = ["Apple", "Banana", "Orange"];

console.log(fruits[0]);
console.log(fruits[2]);
```

**Output**

```
Apple
Orange
```

If an index does not exist, JavaScript returns `undefined`.

```jsx
console.log(fruits[5]);
```

**Output**

```
undefined
```

---

### **The `length` Property**

Every array has a built-in `length` property that returns the **total number of elements** present in the array.

```jsx
let fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.length);
```

**Output**

```
3
```

The `length` property is commonly used while looping through arrays because it automatically adjusts whenever elements are added or removed.

For example,

```jsx
let fruits = ["Apple", "Banana"];

console.log(fruits.length);

fruits.push("Orange");

console.log(fruits.length);
```

**Output**

```
2
3
```

Since arrays are dynamic, the `length` property also changes dynamically to reflect the current number of elements.

---

## **Array Methods**

JavaScript provides several built-in methods to perform common operations on arrays, such as adding, removing, searching, and converting elements. These methods make working with arrays much easier and eliminate the need to write complex logic for everyday tasks.

### **`push()`**

The `push()` method is used to **add one or more elements to the end of an array**. It modifies the original array and returns the **new length** of the array.

**Syntax**

```jsx
array.push(element1, element2, ...)
```

**Example**

```jsx
let fruits = ["Apple", "Banana"];

fruits.push("Orange");

console.log(fruits);
```

**Output**

```
["Apple", "Banana", "Orange"]
```

You can also add multiple elements at once.

```jsx
let fruits = ["Apple", "Banana"];

fruits.push("Orange", "Mango");

console.log(fruits);
```

**Output**

```
["Apple", "Banana", "Orange", "Mango"]
```

Since `push()` modifies the original array, the existing array itself gets updated.

---

### **`pop()`**

The `pop()` method removes the **last element** from an array. It modifies the original array and returns the removed element.

**Syntax**

```jsx
array.pop()
```

**Example**

```jsx
let fruits = ["Apple", "Banana", "Orange"];

let removedFruit = fruits.pop();

console.log(fruits);
console.log(removedFruit);
```

**Output**

```
["Apple", "Banana"]

Orange
```

If the array is empty, `pop()` returns `undefined`.

```jsx
let numbers = [];

console.log(numbers.pop());
```

**Output**

```
undefined
```

---

### **`unshift()`**

The `unshift()` method adds one or more elements to the **beginning** of an array. It modifies the original array and returns the new length of the array.

**Syntax**

```jsx
array.unshift(element1, element2, ...)
```

**Example**

```jsx
let fruits = ["Banana", "Orange"];

fruits.unshift("Apple");

console.log(fruits);
```

**Output**

```
["Apple", "Banana", "Orange"]
```

Unlike `push()`, which adds elements at the end, `unshift()` inserts elements at the beginning.

---

### **`shift()`**

The `shift()` method removes the **first element** from an array. It modifies the original array and returns the removed element.

**Syntax**

```jsx
array.shift()
```

**Example**

```jsx
let fruits = ["Apple", "Banana", "Orange"];

let removedFruit = fruits.shift();

console.log(fruits);
console.log(removedFruit);
```

**Output**

```
["Banana", "Orange"]

Apple
```

If the array is empty, `shift()` returns `undefined`.

---

### **`includes()`**

The `includes()` method checks whether a specified element exists in an array. It returns a **boolean value**.

- `true` → Element exists.
- `false` → Element does not exist.

**Syntax**

```jsx
array.includes(valueToFind)
```

**Example**

```jsx
let fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.includes("Banana"));
console.log(fruits.includes("Mango"));
```

**Output**

```
true
false
```

This method is commonly used before performing operations that depend on whether an element exists in an array.

---

### **`indexOf()`**

The `indexOf()` method returns the **index of the first occurrence** of a specified element.

If the element is not found, it returns `-1`.

**Syntax**

```jsx
array.indexOf(element)
```

**Example**

```jsx
let fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.indexOf("Banana"));
console.log(fruits.indexOf("Mango"));
```

**Output**

```
1
-1
```

Since array indexing starts from `0`, `"Banana"` is present at index `1`.

---

### **`Array.isArray()`**

The `Array.isArray()` method checks whether a given value is an array. It returns a boolean value.

**Syntax**

```jsx
Array.isArray(value)
```

**Example**

```jsx
let fruits = ["Apple", "Banana"];

console.log(Array.isArray(fruits));
console.log(Array.isArray("Apple"));
console.log(Array.isArray(100));
```

**Output**

```
true
false
false
```

This method is useful because arrays are technically objects in JavaScript.

For example,

```jsx
let fruits = ["Apple", "Banana"];

console.log(typeof fruits);
```

**Output**

```
object
```

Since `typeof` returns `"object"` for arrays, we use `Array.isArray()` whenever we specifically need to check whether a value is an array.

---

### **`join()`**

The `join()` method combines all the elements of an array into a **single string**. The separator between elements can be specified; otherwise, JavaScript uses a comma (`,`) by default.

It **does not modify** the original array.

**Syntax**

```jsx
array.join(separator)
```

**Example 1: Default Separator**

```jsx
let fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.join());
```

**Output**

```
Apple,Banana,Orange
```

### **Example 2: Custom Separator**

```jsx
let fruits = ["Apple", "Banana", "Orange"];

console.log(fruits.join(" - "));
```

**Output**

```
Apple - Banana - Orange
```

### **Example 3: Understanding the Return Type**

Although the output may look similar to an array, `join()` actually returns a **string**.

```jsx
let fruits = ["Apple", "Banana", "Orange"];

let result = fruits.join();

console.log(result);
console.log(typeof result);
```

**Output**

```
Apple,Banana,Orange

string
```

The original array remains unchanged.

```jsx
let fruits = ["Apple", "Banana", "Orange"];

fruits.join();

console.log(fruits);
```

**Output**

```
["Apple", "Banana", "Orange"]
```

---

### **`slice()`**

The `slice()` method is used to **extract a portion of an array** and return it as a **new array**. It **does not modify** the original array.

The method extracts elements starting from the **start index** up to, but **not including**, the **end index**.

**Syntax**

```jsx
array.slice(startIndex, endIndex)
```

- `startIndex` – Index from where extraction begins.
- `endIndex` – Index where extraction stops (**excluded**).

**Example**

```jsx
let fruits = ["Apple", "Banana", "Orange", "Mango", "Grapes"];

let newFruits = fruits.slice(1, 4);

console.log(newFruits);
console.log(fruits);
```

**Output**

```
["Banana", "Orange", "Mango"]

["Apple", "Banana", "Orange", "Mango", "Grapes"]
```

Notice that:

- The element at index `1` is included.
- The element at index `4` is **not included**.
- The original array remains unchanged.

---

### **`splice()`**

The `splice()` method is used to **add, remove, or replace elements** in an array.

Unlike `slice()`, **`splice()` modifies the original array**.

**Syntax**

```jsx
array.splice(startIndex, deleteCount, item1, item2, ...)
```

- `startIndex` – Index from where the operation begins.
- `deleteCount` – Number of elements to remove.
- `item1, item2...` – Optional elements to insert.

### **Removing Elements Using `splice()`**

```jsx
let fruits = ["Apple", "Banana", "Orange", "Mango", "Grapes"];

let removed = fruits.splice(1, 2);

console.log(removed);
console.log(fruits);
```

**Output**

```
["Banana", "Orange"]

["Apple", "Mango", "Grapes"]
```

The removed elements are returned as a new array, while the original array is modified.

### **Adding Elements Using `splice()`**

To add elements without removing any existing element, set `deleteCount` to `0`.

```jsx
let fruits = ["Apple", "Orange"];

fruits.splice(1, 0, "Banana");

console.log(fruits);
```

**Output**

```
["Apple", "Banana", "Orange"]
```

### **Replacing Elements Using `splice()`**

Replacement is simply removing some elements and inserting new ones at the same position.

```jsx
let fruits = ["Apple", "Banana", "Orange"];

fruits.splice(1, 1, "Mango");

console.log(fruits);
```

**Output**

```
["Apple", "Mango", "Orange"]
```

**Tip**

Remember this simple rule:

- **`slice()` → “S” for Separate Copy**
- **`splice()` → “P” for Permanent Change**

---

### **`concat()`**

The `concat()` method is used to **merge two or more arrays** and returns a **new array**.

It **does not modify** the original arrays.

**Syntax**

```jsx
array1.concat(array2, array3, ...)
```

**Example**

```jsx
const marvelHeroes = ["Thor", "Iron Man", "Spider-Man"];
const dcHeroes = ["Batman", "Superman", "Flash"];

const allHeroes = marvelHeroes.concat(dcHeroes);

console.log(allHeroes);
```

**Output**

```
[
  "Thor",
  "Iron Man",
  "Spider-Man",
  "Batman",
  "Superman",
  "Flash"
]
```

### **What happens if we use `push()` instead?**

Many beginners try this:

```jsx
const marvelHeroes = ["Thor", "Iron Man"];
const dcHeroes = ["Batman", "Flash"];

marvelHeroes.push(dcHeroes);

console.log(marvelHeroes);
```

**Output**

```
[
  "Thor",
  "Iron Man",
  ["Batman", "Flash"]
]
```

Instead of merging the arrays, `push()` adds the **entire second array as a single element**, creating a **nested array**.

---

## **Spread Operator (`...`)**

The **Spread Operator** expands the elements of an array individually.

It is commonly used for merging arrays, copying arrays, and passing array elements as separate arguments.

**Syntax**

```jsx
[...array]
```

**Example**

```jsx
const marvelHeroes = ["Thor", "Iron Man", "Spider-Man"];
const dcHeroes = ["Batman", "Superman", "Flash"];

const allHeroes = [...marvelHeroes, ...dcHeroes];

console.log(allHeroes);
```

**Output**

```
[
  "Thor",
  "Iron Man",
  "Spider-Man",
  "Batman",
  "Superman",
  "Flash"
]
```

Although both `concat()` and the Spread Operator produce the same result, the **Spread Operator is generally preferred** because it is cleaner, more readable, and allows combining arrays with additional values.

Example:

```jsx
const allHeroes = [
    ...marvelHeroes,
    "Doctor Strange",
    ...dcHeroes
];
```

---

### **`concat()` vs Spread Operator**

| **Feature** | `concat()` | **Spread Operator (**`...`**)** |
| --- | --- | --- |
| Original Array Modified | No | No |
| Returns | New Array | New Array |
| Readability | Good | Better |
| Preferred in Modern JavaScript | No | Yes |

---

### **`flat()`**

The `flat()` method is used to **flatten nested arrays** into a single array.

It returns a **new array** and **does not modify** the original array.

**Syntax**

```jsx
array.flat(depth)
```

The `depth` parameter specifies how many levels of nesting should be flattened.

**Example**

```jsx
const numbers = [1, 2, [3, 4], [5, 6]];

console.log(numbers.flat());
```

**Output**

```
[1, 2, 3, 4, 5, 6]
```

---

### **Using `Infinity` with `flat()`**

If an array has multiple levels of nesting, we can use `Infinity`.

```jsx
const numbers = [
    1,
    2,
    [3, 4],
    [5, [6, 7, [8, 9]]]
];

console.log(numbers.flat(Infinity));
```

**Output**

```
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

Using `Infinity` tells JavaScript to flatten **all levels**, regardless of how deeply nested the array is.

---

## **`Array.from()`**

The `Array.from()` method is used to **create a new array from an iterable object or an array-like object**.

An **iterable object** is an object whose elements can be accessed one by one, such as a **String, Map, Set**, etc.

An **array-like object** is an object that has a `length` property and indexed elements, but is not actually an array.

`Array.from()` is commonly used to convert strings, NodeLists, HTMLCollections, Sets, and similar structures into real arrays.

**Syntax**

```jsx
Array.from(object)
```

**Example 1: Converting a String into an Array**

```jsx
console.log(Array.from("JavaScript"));
```

**Output**

```
[
  "J", "a", "v", "a",
  "S", "c", "r", "i",
  "p", "t"
]
```

Each character of the string becomes a separate element in the array.

### **Example 2: Understanding an Interesting Case**

```jsx
console.log(Array.from({ name: "Hitesh" }));
```

**Output**

```
[]
```

This surprises many beginners.

`Array.from()` expects either:

- An **iterable object**, or
- An **array-like object**.

The object

```jsx
{ name: "Hitesh" }
```

is neither iterable nor array-like because:

- It is not iterable.
- It does not contain a `length` property.

Therefore, JavaScript cannot determine how many elements should be placed into the array, so it returns an empty array.

---

## **`Array.of()`**

The `Array.of()` method creates a new array using the values passed as arguments.

Unlike the `Array()` constructor, `Array.of()` always treats every argument as an array element.

**Syntax**

```jsx
Array.of(element1, element2, ...)
```

**Example**

```jsx
let score1 = 100;
let score2 = 200;
let score3 = 300;

console.log(Array.of(score1, score2, score3));
```

**Output**

```
[100, 200, 300]
```

This method is useful when multiple values need to be combined into a single array.

---

### **`Array()` vs `Array.of()`**

Although both create arrays, they behave differently when only one numeric argument is passed.

```jsx
console.log(new Array(5));
console.log(Array.of(5));
```

**Output**

```
[ <5 empty items> ]

[5]
```

`new Array(5)` creates an array with a length of `5`, whereas `Array.of(5)` creates an array containing the single element `5`.

For this reason, `Array.of()` is generally safer and less confusing.

---

## **Multidimensional Arrays**

A **Multidimensional Array** is an array that contains one or more arrays as its elements.

In simple words, it is an **array inside another array**.

Multidimensional arrays are commonly used to represent data in the form of tables, matrices, grids, game boards, and similar structures.

### **Two-Dimensional (2D) Array**

A **Two-Dimensional Array** is an array whose elements are themselves arrays.

```jsx
let matrix = [
    [10, 20, 30],
    [40, 50, 60],
    [70, 80, 90]
];
```

Visually,

```
        Column

         0    1    2
      -----------------
0 |    10   20   30
1 |    40   50   60
2 |    70   80   90

↑
Row
```

The first index represents the **row**, and the second index represents the **column**.

### **Accessing Elements in a 2D Array**

To access an element, use two indexes.

```jsx
array[row][column]
```

**Example**

```jsx
let matrix = [
    [10, 20, 30],
    [40, 50, 60],
    [70, 80, 90]
];

console.log(matrix[0][1]);
console.log(matrix[2][0]);
```

**Output**

```
20
70
```

### **Three-Dimensional (3D) Array**

A **Three-Dimensional Array** is an array whose elements are 2D arrays.

```jsx
let data = [
    [
        [1, 2],
        [3, 4]
    ],
    [
        [5, 6],
        [7, 8]
    ]
];
```

Accessing an element requires three indexes.

```jsx
console.log(data[1][0][1]);
```

**Output**

```
6
```

The indexes represent:

- First Index → Block
- Second Index → Row
- Third Index → Column

---

## **Array Destructuring**

**Array Destructuring** is an ES6 feature that allows us to extract values from an array and store them into separate variables in a concise and readable way. Instead of accessing each element using indexes, destructuring automatically assigns array elements to variables based on their position.

**Syntax**

```jsx
const [variable1, variable2, ...] = array;
```

### **Basic Destructuring**

```jsx
const fruits = ["Apple", "Banana", "Orange"];

const [first, second, third] = fruits;

console.log(first);
console.log(second);
console.log(third);
```

**Output**

```
Apple
Banana
Orange
```

The first variable receives the first element, the second variable receives the second element, and so on.

### **Skipping Elements**

If certain elements are not required, they can simply be skipped by leaving an empty space.

```jsx
const numbers = [10, 20, 30, 40];

const [first, , third] = numbers;

console.log(first);
console.log(third);
```

**Output**

```
10
30
```

### **Default Values**

If the array does not contain enough elements, default values can be provided.

```jsx
const colors = ["Red"];

const [first, second = "Blue"] = colors;

console.log(first);
console.log(second);
```

**Output**

```
Red
Blue
```

### **Swapping Variables**

One of the most common uses of destructuring is swapping two variables without using a temporary variable.

```jsx
let a = 10;
let b = 20;

[a, b] = [b, a];

console.log(a);
console.log(b);
```

**Output**

```
20
10
```

### **Example**

Suppose a function returns multiple values inside an array.

```jsx
function getUser() {
    return ["Mike", 22, "US"];
}

const [name, age, country] = getUser();

console.log(name);
console.log(age);
console.log(country);
```

**Output**

```
Mike
22
US
```

Destructuring makes the code shorter, cleaner, and easier to read compared to accessing each value using indexes.

---