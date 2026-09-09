# Objects

# **JavaScript Objects**

An **object** is a non-primitive data type used to store related data in the form of **key-value pairs**. Unlike arrays, where elements are accessed using indexes, objects allow data to be accessed using meaningful names called **keys**. An object can store values of any data type, including strings, numbers, booleans, arrays, functions, and even other objects.

**Syntax**

```jsx
const objectName = {
    key1: value1,
    key2: value2
};
```

**Example**

```jsx
const student = {
    name: "Raja",
    age: 21,
    city: "Bangalore"
};
```

In the above example:

- `name`, `age`, and `city` are **keys**.
- `"Raja"`, `21`, and `"Bangalore"` are **values**.

---

## **Ways to Create Objects**

JavaScript provides two ways to create objects.

### **1. Object Literal (Most Common)**

Object literals are the simplest and most commonly used way to create objects. The object is created using curly braces `{}`.

**Syntax**

```jsx
const obj = {
    key: value
};
```

**Example**

```jsx
const person = {
    name: "Raja",
    city: "Chennai"
};
```

This is the preferred method because it is simple, readable, and used in almost every JavaScript application.

### **2. Constructor Method**

Objects can also be created using the `Object` constructor with the `new` keyword.

**Syntax**

```jsx
const obj = new Object();
```

**Example**

```jsx
const person = new Object();

person.name = "Raja";
person.city = "Chennai";
```

This creates an empty object first, after which properties can be added individually.

### **Singleton vs Non-Singleton Objects**

When an object is created using an **object literal (`{}`)**, JavaScript creates a normal object. Every time the code runs, a new object is created.

When an object is created using the **constructor (`new Object()` or `Object.create()`)**, it is referred to as a **Singleton Object**.

Remember:

- Objects created using `{}` → **Non-Singleton**
- Objects created using `new Object()` or `Object.create()` → **Singleton**

**Note:** In real-world development, both behave as normal objects for most practical purposes. The “singleton” distinction is mainly discussed in interviews.

---

## Types of Objects

### **Nested Objects**

One object can contain another object as its value. This is called a **Nested Object**.

**Example**

```jsx
const user = {
    email: "user@gmail.com",

    fullname: {
        userfullname: {
            firstname: "Elon",
            lastname: "Musk"
        }
    }
};
```

To access nested values, continue using dot notation for each level.

```jsx
console.log(user.fullname.userfullname.firstname);
```

**Output**

```
Hitesh
```

---

### **Array of Objects**

In real-world applications, data received from databases or APIs is usually stored as an **array of objects**. Each object represents one record.

**Example**

```jsx
const users = [
    {
        id: 1,
        email: "a@gmail.com"
    },
    {
        id: 2,
        email: "b@gmail.com"
    },
    {
        id: 3,
        email: "c@gmail.com"
    }
];
```

Accessing the second user’s email:

```jsx
console.log(users[1].email);
```

**Output**

```
b@gmail.com
```

This structure is very common while working with APIs, databases, and JSON data.

---

## **Accessing Object Properties**

JavaScript provides two ways to access object properties.

### **1. Dot Notation**

Dot notation is the most commonly used way to access object properties. It is simple and should be preferred whenever the property name is a valid JavaScript identifier.

**Syntax**

```jsx
object.property
```

**Example**

```jsx
const person = {
    name: "Raja",
    city: "Bangalore"
};

console.log(person.name);
console.log(person.city);
```

**Output**

```
Raja
Bangalore
```

---

### **2. Bracket Notation**

Bracket notation accesses properties using a string. It is mainly used when:

- The property name contains spaces.
- The property name is stored in a variable.
- The property is a Symbol.
- The property name is not a valid identifier.

**Syntax**

```jsx
object["property"]
```

**Example**

```jsx
const person = {
    "full name": "Raja Kumar"
};

console.log(person["full name"]);
```

**Output**

```
Raja Kumar
```

This property **cannot** be accessed using dot notation.

```jsx
person.full name // Space can't be given
```

---

## **Modifying Objects**

Objects are mutable, meaning their properties can be added, updated, or deleted after creation.

### **1. Adding a Property**

```jsx
const person = {
    name: "Raja"
};

person.city = "Bangalore";
```

Now the object becomes:

```jsx
{
    name: "Raja",
    city: "Bangalore"
}
```

### **2. Updating a Property**

If a property already exists, assigning a new value replaces the old value.

```jsx
person.city = "Mumbai";
```

Now,

```jsx
console.log(person.city);
```

**Output**

```
Mumbai
```

### **3. Deleting a Property**

The `delete` keyword removes a property from an object.

```jsx
delete person.city;
```

Now,

```jsx
console.log(person);
```

**Output**

```jsx
{
    name: "Raja"
}
```

---

## **Symbols as Object Keys**

A **Symbol** is a unique and immutable primitive data type introduced in ES6. Symbols are mainly used to create unique property keys so that they do not accidentally conflict with other property names.

When a Symbol is used as an object key, it **must** be enclosed inside square brackets `[]`. Otherwise, JavaScript treats it as a normal string.

**Syntax**

```jsx
const symbolName = Symbol("description");

const object = {
    [symbolName]: value
};
```

**Example**

```jsx
const mySym = Symbol("key1");

const user = {
    name: "Hitesh",
    [mySym]: "myKey"
};

console.log(user[mySym]);
```

**Output**

```
myKey
```

**Incorrect Example**

```jsx
const mySym = Symbol("key1");

const user = {
    mySym: "myKey"
};
```

Here, `mySym` becomes a normal string property instead of a Symbol key.

---

## **Object Methods**

Since functions are first-class citizens in JavaScript, they can be stored inside objects as properties. A function stored inside an object is called an **Object Method**.

Object methods usually perform operations related to that object.

**Syntax**

```jsx
const object = {
    methodName: function () {

    }
};
```

**Example**

```jsx
const user = {
    name: "Raja",

    greet: function () {
        console.log("Hello JavaScript");
    }
};

user.greet();
```

**Output**

```
Hello JavaScript
```

---

### **Methods Can Access Object Properties**

An object method can access other properties of the same object using the `this` keyword.

**Example**

```jsx
const user = {
    name: "Hitesh",

    greet: function () {
        console.log(`Hello ${this.name}`);
    }
};

user.greet();
```

**Output**

```
Hello Hitesh
```

Here, `this.name` refers to the `name` property of the object that called the method.

---

### **The `this` Keyword**

The `this` keyword refers to the object that is currently calling the function. It helps a method access the properties and methods of its own object.

In simple words, `this` answers the question:

**“Which object called this function?”**

The value of `this` is determined at runtime based on how the function is called.

**Example**

```jsx
const person = {
    name: "Nehru",

    greet: function () {
        console.log("Hello " + this.name);
    }
};

person.greet();
```

**Output**

```
Hello Nehru
```

Here,

- `person` calls `greet()`.
- Therefore, `this` refers to `person`.
- Hence, `this.name` becomes `"Nehru"`.

---

### **Reusing Object Methods**

A function can be reused by assigning it as a method to another object.

**Example**

```jsx
const person1 = {
    name: "Nehru",

    greet: function () {
        console.log("Hello " + this.name);
    }
};

const person2 = {
    name: "Raja",
    greet: person1.greet
};

person1.greet();
person2.greet();
```

**Output**

```
Hello Nehru
Hello Raja
```

Although both objects use the same function, the value of `this` changes depending on which object calls the function.

---

### **Freezing an Object**

The `Object.freeze()` method makes an object **immutable**. After freezing an object, new properties cannot be added, existing properties cannot be removed, and existing values cannot be modified.

**Syntax**

```jsx
Object.freeze(object);
```

**Example**

```jsx
const product = {
    name: "Phone",
    price: 50000
};

Object.freeze(product);

product.price = 60000;
product.brand = "Samsung";
delete product.name;

console.log(product);
```

**Output**

```jsx
{
    name: "Phone",
    price: 50000
}
```

All modification attempts are ignored.

#### **Checking if an Object is Frozen**

The `Object.isFrozen()` method checks whether an object has been frozen.

**Syntax**

```jsx
Object.isFrozen(object);
```

**Example**

```jsx
const product = {
    name: "Phone"
};

Object.freeze(product);

console.log(Object.isFrozen(product));
```

**Output**

```
true
```

---

### **Sealing an Object**

The `Object.seal()` method partially protects an object. It prevents adding or deleting properties but still allows modifying the values of existing properties.

**Syntax**

```jsx
Object.seal(object);
```

**Example**

```jsx
const person = {
    name: "Raja",
    city: "Bangalore"
};

Object.seal(person);

person.city = "Mumbai";
person.age = 22;
delete person.name;

console.log(person);
```

**Output**

```jsx
{
    name: "Raja",
    city: "Mumbai"
}
```

Here,

- Existing property values can be changed.
- New properties cannot be added.
- Existing properties cannot be deleted.

#### **Checking if an Object is Sealed**

The `Object.isSealed()` method checks whether an object has been sealed.

**Syntax**

```jsx
Object.isSealed(object);
```

**Example**

```jsx
const person = {
    name: "Raja"
};

Object.seal(person);

console.log(Object.isSealed(person));
```

**Output**

```
true
```

---

### **`Object.keys()`**

The `Object.keys()` method returns an array containing all the enumerable property names (keys) of an object.

**Syntax**

```jsx
Object.keys(object);
```

**Example**

```jsx
const person = {
    name: "Nehru",
    city: "Bangalore",
    age: 22
};

console.log(Object.keys(person));
```

**Output**

```jsx
["name", "city", "age"]
```

Since the result is an array, array methods such as `length`, `map()`, and loops can be used on it.

---

### **`Object.values()`**

The `Object.values()` method returns an array containing all the values of an object.

**Syntax**

```jsx
Object.values(object);
```

**Example**

```jsx
const person = {
    name: "Nehru",
    city: "Bangalore",
    age: 22
};

console.log(Object.values(person));
```

**Output**

```jsx
["Nehru", "Bangalore", 22]
```

---

### **`Object.entries()`**

The `Object.entries()` method returns an array of key-value pairs. Each key-value pair is stored as a separate array.

**Syntax**

```jsx
Object.entries(object);
```

**Example**

```jsx
const person = {
    name: "Nehru",
    city: "Bangalore"
};

console.log(Object.entries(person));
```

**Output**

```jsx
[
    ["name", "Nehru"],
    ["city", "Bangalore"]
]
```

This method is useful when both the key and value are required while looping through an object.

---

### **`hasOwnProperty()`**

The `hasOwnProperty()` method checks whether a property exists directly inside an object. It returns a boolean value.

**Syntax**

```jsx
object.hasOwnProperty(propertyName);
```

**Example**

```jsx
const user = {
    id: "123abc",
    name: "Sammy",
    isLoggedIn: false
};

console.log(user.hasOwnProperty("name"));
console.log(user.hasOwnProperty("age"));
```

**Output**

```jsx
true
false
```

This method is useful when working with dynamic objects or API responses where some properties may or may not exist.

---

## **Looping Through an Object**

### **`for...in`**

The `for...in` loop iterates over all enumerable property names (keys) of an object. Each key can then be used to access its corresponding value.

**Syntax**

```jsx
for (let key in object) {

}
```

**Example**

```jsx
const student = {
    id: 101,
    name: "Raja",
    city: "Bangalore"
};

for (let key in student) {
    console.log(key + " : " + student[key]);
}
```

**Output**

```
id : 101
name : Raja
city : Bangalore
```

Here, `student[key]` is used instead of `student.key` because `key` is a variable whose value changes during each iteration.

---

### **Printing Nested Objects and Arrays**

When looping through an object, nested objects and arrays are also of type `"object"`. To display them in a readable format, `JSON.stringify()` can be used.

**Example**

```jsx
const student = {
    id: 101,
    name: "Raja",
    subjects: ["Math", "Science"],
    address: {
        city: "Bangalore",
        pincode: 560072
    }
};

for (let key in student) {

    const value = student[key];

    if (typeof value === "object") {
        console.log(key + " : " + JSON.stringify(value));
    } else {
        console.log(key + " : " + value);
    }
}
```

**Output**

```
id : 101
name : Raja
subjects : ["Math","Science"]
address : {"city":"Bangalore","pincode":560072}
```

---

## **Object Destructuring**

Object destructuring is a feature introduced in ES6 that allows properties of an object to be extracted and stored into separate variables in a single statement.

It provides a shorter and cleaner way of accessing object properties.

**Syntax**

```jsx
const { property1, property2 } = object;
```

**Example**

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

**Syntax**

```jsx
const { propertyName: newVariable } = object;
```

**Example**

```jsx
const course = {
    courseName: "JavaScript",
    courseInstructor: "Arjun"
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

**Without Destructuring**

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

**With Destructuring**

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