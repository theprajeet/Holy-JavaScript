# Stack and Heap Memory w.r.t Data Types

## Stack and Heap Memory with respect to Primitive and Non-Primitive Data Types

### **Stack Memory**

**Stack Memory** is a part of the computer’s memory that stores **simple values** and **variables**. It is called a **Stack** because it follows the **LIFO (Last In, First Out)** principle. The last value added is the first one to be removed.

Since primitive data types (`String`, `Number`, `Boolean`, `BigInt`, `Undefined`, `Null`, `Symbol`) store only **one simple value**, JavaScript stores the value directly inside the variable in the Stack.

For example,

```jsx
let language = "JavaScript";
```

When this line executes, JavaScript creates a variable named `language` and stores the actual value `"JavaScript"` directly inside it.

```
                STACK MEMORY
+----------------------------------------+
| language  →  "JavaScript"              |
+----------------------------------------+
```

The variable itself contains the value.

There is **no Heap involved** because primitive values are small and fixed in size.

#### **Copying Primitive Values**

Now consider another variable.

```jsx
let language = "JavaScript";
let course = language;
```

Many think `course` points to `language`.

It doesn’t. JavaScript creates an entirely new copy of the value.

```
                STACK MEMORY
+----------------------------------------+
| language → "JavaScript"                |
|                                        |
| course   → "JavaScript"                |
+----------------------------------------+
```

Both variables have **their own separate copies**.

Now suppose we change one variable.

```jsx
course = "React";
```

Memory now becomes

```
                STACK MEMORY
+----------------------------------------+
| language → "JavaScript"                |
|                                        |
| course   → "React"                     |
+----------------------------------------+
```

Notice that **only `course` changed**.

`language` is completely unaffected because both variables store independent values.

This is why primitive data types are said to be **copied by value**.

---

### **Heap Memory**

The Heap is another region of memory used to store **large and complex data**, such as: 
`Objects`  `Arrays`  `Functions`  `Dates`  `Maps`  `Sets`  

These values can become very large and may change in size during execution. Instead of storing such large data directly inside a variable, JavaScript stores the actual object in the **Heap**. The variable itself remains in the **Stack**, but instead of storing the object, it stores only the **memory address (reference)** of that object.

#### **Why Doesn’t JavaScript Store Objects Directly in the Stack?**

Imagine an object containing hundreds of properties.

```
{
   ...
   many properties
   ...
}
```

If JavaScript copied the entire object every time it was assigned to another variable:

- It would consume a lot of memory.
- Copying would become slow.
- Performance would decrease.

Instead, JavaScript stores the object **once** in the Heap and lets variables simply remember **where it is located**. That “where it is located” is called a **Reference**.

#### **What is a Reference?**

A **reference** is simply the **memory address** of an object stored in the Heap.

Think of it like this:

- Heap = Houses
- Stack = Notebook
- Reference = House Address written in the notebook

The notebook doesn’t contain the house. It only contains the house’s address. Similarly, the Stack doesn’t contain the object, it only contains the object’s memory address.

#### **Creating an Object**

Suppose we create an object.

```jsx
let student = {
    name: "Alex",
    age: 21
};
```

Internally, JavaScript stores it like this.

```
                STACK MEMORY
+----------------------------------------+
| student  ───────────────┐              |
+-------------------------│--------------+
                          │
                          │ Reference
                          ▼
                HEAP MEMORY
+----------------------------------------+
|                                        |
|   {                                    |
|      name: "Alex",                     |
|      age: 21                           |
|   }                                    |
|                                        |
+----------------------------------------+
```

Notice two things:

- The variable `student` is stored in the Stack.
- The actual object is stored in the Heap.

The arrow represents the **reference** (memory address).

#### **Copying a Reference Variable**

Now suppose we do this.

```jsx
let student = {
    name: "Alex",
    age: 21
};

let anotherStudent = student;
```

Many beginners expect JavaScript to create another object. It doesn’t. JavaScript simply copies the **reference**.

```
                STACK MEMORY
+----------------------------------------+
| student ───────────────┐               |
|                        │               |
| anotherStudent ────────|               |
+------------------------│---------------+
                         │
                         ▼
                HEAP MEMORY
+----------------------------------------+
|                                        |
|   {                                    |
|      name: "Alex",                     |
|      age: 21                           |
|   }                                    |
|                                        |
+----------------------------------------+
```

Notice carefully:

There is still **only one object** in the Heap. Both variables point to that same object.

#### **Modifying Through One Variable**

Now suppose we modify the object.

```jsx
anotherStudent.age = 25;
```

JavaScript follows the reference stored in `anotherStudent`. That reference points to the object in the Heap. So JavaScript updates **that single object**.

```
                STACK MEMORY
+----------------------------------------+
| student ───────────────┐               |
|                        │               |
| anotherStudent ────────|               |
+------------------------│---------------+
                         │
                         ▼
                HEAP MEMORY
+----------------------------------------+
|                                        |
|   {                                    |
|      name: "Alex",                     |
|      age: 25                           |
|   }                                    |
|                                        |
+----------------------------------------+
```

Now, if you access the object through either variable, you’ll see the updated value because **both variables point to the same object**. This is why non-primitive values are said to be **copied by reference** (more precisely, JavaScript copies the reference, not the object itself).

#### **Assigning a Completely New Object**

Now suppose we assign a brand-new object.

```jsx
student = {
    name: "David",
    age: 30
};
```

JavaScript **does not modify the existing object**. Instead, it creates another object in the Heap.

Then it changes only the reference stored inside `student`.

```
                STACK MEMORY
+----------------------------------------+
| student ───────────────┐               |
|                        │               |
| anotherStudent ────────┼──────────┐    |
+------------------------│----------│----+
                         │          │
                         ▼          ▼

                HEAP MEMORY
+----------------------------------------+
|                                        |
| Object A                               |
| {                                      |
|   name: "Alex",                        |
|   age: 25                              |
| }                                      |
|                                        |
| Object B                               |
| {                                      |
|   name: "David",                       |
|   age: 30                              |
| }                                      |
|                                        |
+----------------------------------------+
```

After reassignment:

- `student` now points to **Object B**.
- `anotherStudent` still points to **Object A**.

Since the variables now reference different objects, changing one object will no longer affect the other.

#### **What Happens to Unused Objects?**

If no variable points to an object anymore, that object becomes **unreachable**.

For example,

```
Stack

student  ─────► Object B

(No variable points to Object A)
```

Since **Object A** is no longer referenced by any variable, JavaScript knows it cannot be used anymore.

The **Garbage Collector** automatically removes such unreachable objects from the Heap, freeing up memory.

---