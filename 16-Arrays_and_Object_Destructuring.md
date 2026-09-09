# Arrays and Object Destructuring

### **Destructuring in JavaScript**

**Destructuring** is an ES6 feature that allows us to extract values from **arrays** or **properties from objects** and store them directly into variables. It provides a shorter and cleaner syntax, eliminating the need to repeatedly access values using indexes or property names.

JavaScript supports two types of destructuring:

- Array Destructuring
- Object Destructuring

### **Array Destructuring**

Array destructuring allows us to extract values from an array and assign them directly to variables. The variables receive values based on their **position (index)** in the array.

Without destructuring,

```jsx
const colors = ["Red", "Green", "Blue"];

const first = colors[0];
const second = colors[1];
const third = colors[2];

console.log(first);
console.log(second);
console.log(third);
```

Using array destructuring,

```jsx
const colors = ["Red", "Green", "Blue"];

const [first, second, third] = colors;

console.log(first);
console.log(second);
console.log(third);
```

Output

```
Red
Green
Blue
```

Here,

- `first` receives the value at index `0`.
- `second` receives the value at index `1`.
- `third` receives the value at index `2`.

The assignment happens according to the position of the elements in the array.

```
Array

["Red", "Green", "Blue"]
    │        │         │
    ▼        ▼         ▼
 first    second     third
```

### **Skipping Elements**

If certain elements are not required, they can be skipped by leaving empty spaces between commas.

```jsx
const numbers = [10, 20, 30, 40];

const [first, , third] = numbers;

console.log(first);
console.log(third);
```

Output

```
10
30
```

The second element is skipped because there is no variable assigned to it.

### **Using the Rest Operator (`...`)**

The rest operator collects all the remaining elements into a new array.

```jsx
const numbers = [10, 20, 30, 40, 50];

const [first, second, ...remaining] = numbers;

console.log(first);
console.log(second);
console.log(remaining);
```

Output

```
10
20
[30, 40, 50]
```

Here, `remaining` becomes a new array containing all the leftover elements.

### **Default Values**

A default value is assigned when the corresponding array element is `undefined`.

```jsx
const numbers = [10];

const [a, b = 20] = numbers;

console.log(a);
console.log(b);
```

Output

```
10
20
```

Since the second element does not exist, `b` receives the default value `20`.

---

### **Object Destructuring**

Object destructuring is a feature introduced in ES6 that allows properties of an object to be extracted and stored into separate variables in a single statement.

It provides a shorter and cleaner way of accessing object properties.

#### **Syntax**

```jsx
const { property1, property2 } = object;
```

#### **Example**

```jsx
const student = {
    name: "Nehru",
    age: 22,
    city: "Bangalore"
};

const { name, age, city } = student;

console.log(name);
console.log(age);
console.log(city);
```

**Output**

```
Nehru
22
Bangalore
```

---

### **Renaming Variables While Destructuring**

During destructuring, property names can be assigned to variables with different names using the colon (`:`) operator.

#### **Syntax**

```jsx
const { propertyName: newVariable } = object;
```

#### **Example**

```jsx
const course = {
    courseName: "JavaScript",
    courseInstructor: "Ram"
};

const { courseInstructor: instructor } = course;

console.log(instructor);
```

**Output**

```
Hitesh
```

Here, the property `courseInstructor` is extracted into a new variable named `instructor`.

---

### **Why Object Destructuring is Useful**

Object destructuring makes the code shorter, cleaner, and easier to read. Instead of repeatedly accessing properties using dot notation, the required properties can be extracted once and used directly.

#### **Without Destructuring**

```jsx
const course = {
    courseName: "JavaScript",
    price: 999,
    instructor: "Ram"
};

console.log(course.courseName);
console.log(course.price);
console.log(course.instructor);
```

#### **With Destructuring**

```jsx
const course = {
    courseName: "JavaScript",
    price: 999,
    instructor: "Ram"
};

const { courseName, price, instructor } = course;

console.log(courseName);
console.log(price);
console.log(instructor);
```

---