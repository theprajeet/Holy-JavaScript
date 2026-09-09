# Data Types

## Data Types in JavaScript

Based on how the data is stored and how the data is accessed, JavaScript data types are broadly classified into **Primitive** and **Non-Primitive (Reference)** types.

### **Primitive Data Types**

Primitive data types are the most basic built-in data types in JavaScript. They represent a **single value**, and that value is **stored directly inside the variable**. 

Primitive values are stored in the **Stack Memory**. The **Stack** is a region of memory used to store simple values directly. Since the actual value is stored inside the variable itself, each variable has its own independent copy of the data. When a primitive variable is copied or assigned to another variable, JavaScript copies the **actual value**, not its memory location. Therefore, changing one variable does not affect the other because both variables store separate copies of the value in the stack.

For example, if a variable stores the number `10`, the variable itself directly contains the value `10`. If this variable is assigned to another variable, JavaScript creates a completely new copy of the value. Therefore, changing one variable does not affect the other.

**Characteristics:**

- Store a single value.
- Stored directly in the variable.
- Stored in **Stack Memory**.
- Immutable (their value cannot be changed; modifying them creates a new value).
- When copied, the actual value is copied.

There are **7 primitive data types**:

| **Data Type** | **Description** | **Example** |
| --- | --- | --- |
| `String` | Represents text | `"Hello"` |
| `Number` | Represents integers and floating-point numbers | `10`, `3.14` |
| `BigInt` | Represents very large integers | `12345678901234567890n` |
| `Boolean` | Represents logical values | `true`, `false` |
| `Undefined` | Variable declared but not assigned a value | `let x;` |
| `Null` | Represents an intentional absence of value | `let x = null;` |
| `Symbol` | Represents a unique and immutable identifier | `Symbol("id")` |

---

### **Non-Primitive (Reference) Data Types**

Non-primitive data types, also known as **Reference Data Types**, are used to store more complex data such as collections of values, properties, and methods. Unlike primitive values, the actual data is **not stored directly inside the variable**. Instead, JavaScript stores the actual object in the **Heap Memory**, while the variable itself is stored in the **Stack Memory**.The variable does not contain the actual object. Instead, it stores a **reference (memory address)** that points to the object’s location in the Heap.

A **reference** can be thought of as the location or address where the actual object exists in memory. Therefore, when a variable contains an object, array, or function, it does not actually contain the data itself—it only contains the address that points to that data.

**Characteristics:**

- Can store multiple values and complex data.
- Stored by reference (memory address).
- Actual data is stored in **Heap Memory**.
- Mutable (their contents can be modified after creation).
- When copied, only the reference is copied, not the actual data.

Examples:

- Object
- Array
- Function
- Date
- Map
- Set
- RegExp

```jsx
let person = {
    name: "John",
    age: 25
};

let numbers = [1, 2, 3];

function greet() {
    console.log("Hello");
}
```

---