# OOPS, Building Blocks Of OOPS

### **JavaScript and Classes**

JavaScript supports the `class` syntax, which was introduced with **ES6 (ECMAScript 2015)**. However, there is an important distinction to understand: JavaScript is fundamentally a **prototype-based language**, not a class-based language like Java or C++. The `class` syntax provides a more familiar and convenient way of writing object-oriented code, especially for developers coming from class-based languages.

JavaScript classes are therefore often described as **syntactic sugar over the existing prototype-based mechanism**. This means that classes provide cleaner syntax for creating and working with objects, but the underlying inheritance mechanism of JavaScript is still based on prototypes.

### **Object-Oriented Programming (OOP)**

Object-Oriented Programming (OOP) in JavaScript is a programming paradigm based on the concept of objects that contain data (properties) and behavior (methods). It focuses on designing software that closely represents real-world entities. It is used to:

- Improves code reusability
- Enhances maintainability and scalability
- Makes programs easier to understand and manage
- Closely models real-world entities

The main idea behind OOP is to combine related **data and behavior** into logical units called objects. This becomes particularly useful as an application grows because instead of having many unrelated variables and functions scattered throughout the program, related functionality can be grouped together. For example, instead of separately maintaining `username`, `loginCount`, `isLoggedIn`, and functions related to a user, we can represent them together as a `User` object.

**Why Do We Need OOP?**

For small programs, we can work perfectly well with normal variables and functions. The problem becomes more noticeable as the application grows. When a large amount of related data and functionality is scattered throughout the program, the code can become difficult to understand, maintain, and reuse. This is sometimes referred to as **spaghetti code**, where different parts of the program become unnecessarily tangled together.

OOP provides a way to organize related data and behavior into reusable structures. For example, if an application deals with users, we can define how a user should look and what a user can do, and then create multiple user objects based on that structure. The goal is not simply to use classes because they exist. The goal is to make large applications easier to structure, reuse, maintain, and extend.

### **What is an Object?**

An Object in JavaScript is an instance of a class that represents a real-world entity. It is used to access the properties and methods defined inside a class. Properties represent the data or state of an object, while methods represent the behavior or actions that the object can perform. 

- **State:** Represents the current data or attributes of an object.
- **Behavior:** Represents the actions that an object can perform.
- **Identity:** Every object has a unique identity in memory that distinguishes it from other objects.

**Example:** Car is a class, SUV is an object of that class.

<img src="/Assets/OOPS.png" alt="OOPS.png" width="600" >

---

## **Building Blocks of OOP in JavaScript**

JavaScript provides several concepts for creating and organizing objects. Some of the important ones we will encounter are 

1. **Object Literals** 
2. **Constructor Functions** 
3. **Instances**
4. **Prototypes** (Explained Earlier)
5. **Classes**

An **object literal** is the simplest way to directly create an object. A **constructor function** provides a reusable structure from which multiple objects can be created. **Classes** provide a cleaner syntax for defining object structures, while **prototypes** are the underlying mechanism JavaScript uses for inheritance and shared behavior. And the constructor functions are used to create new instances.

---

### **1. Object Literals**

An **object literal** is the simplest and most direct way of creating an object in JavaScript. We create an object using `{}` and place its properties and methods inside it.

```jsx
const user = {
    username: "rocky",
    loginCount: 8,
    isLoggedIn: true,

    getUserDetails: function() {
        console.log(`Username: ${this.username}`);
    }
};
```

The object contains both data and behavior. `username`, `loginCount`, and `isLoggedIn` are properties, while `getUserDetails()` is a method. The properties can be accessed using dot notation or bracket notation, such as `user.username` or `user["username"]`. Similarly, a method can be executed using `user.getUserDetails()`.

#### **The Problem with Object Literals**

Object literals work very well when we need only a few objects. However, imagine that an application needs hundreds or thousands of users.

We would have to repeatedly write the same structure:

```jsx
const userOne = {
    username: "hitesh",
    loginCount: 12,
    isLoggedIn: true
};

const userTwo = {
    username: "ChaiAurCode",
    loginCount: 11,
    isLoggedIn: false
};
```

The structure of both objects is essentially the same. Only their values are different. Repeating this structure manually is inefficient and makes the code harder to maintain.

This is where **constructor functions** become useful. Instead of manually creating every object, we can define the structure once and use it to create multiple objects.

---

### **2. Constructor Functions**

A **constructor function** is a regular JavaScript function that is intended to be **used as blueprints to create multiple instances of objects with identical structures and methods,** they must be executed using the `new` keyword.

For example:

```jsx
function User(username, loginCount, isLoggedIn) {
    this.username = username;
    this.loginCount = loginCount;
    this.isLoggedIn = isLoggedIn;
}

// We can then create multiple users from the same constructor
const userOne = new User("rocky", 12, true);
const userTwo = new User("tony", 11, false);

console.log(userOne);
console.log(userTwo);
```

The function defines the structure that every `User` instance should have. The parameters allow us to provide different values whenever a new user is created. Here, `new User(...)` creates a **new empty object**, makes `this` refer to that new object, assigns the properties to it, and finally returns that object. Therefore, `userOne` and `userTwo` are two completely separate objects with their own values.

```jsx
OUTPUT

User {
    username: "rocky",
    loginCount: 12,
    isLoggedIn: true
}

User {
    username: "tony",
    loginCount: 11,
    isLoggedIn: false
}
```

When a constructor is called using `new`, JavaScript already returns the newly created instance by default, provided the constructor does not explicitly return another object. Therefore, writing `return this` is generally unnecessary in this situation.

Now suppose we remove `new`:  Here we are trying to understand what happens if we don’t use `new`  keyword, hence we are returning this here or else it’s unnecessary to return this as `new` keyword will only return the new created instance. If we don’t `return this` then the  `User()` would normally get called, and will **not treated as a constructor**, so JavaScript does not automatically create a new object and bind `this` to it. So, `this` inside the function is `undefined`, so trying to execute `this` will simply return as `undefined`  `undefined`  when we don’t `return this`, here just for understanding we are returning it.

```jsx
function User(username, loginCount, isLoggedIn) {
    this.username = username;
    this.loginCount = loginCount;
    this.isLoggedIn = isLoggedIn;
    return this;
}

const userOne = User("rocky", 12, true);
const userTwo = User("tony", 11, false);

console.log(userOne);
console.log(userTwo);
```

```jsx
User {
    username: "tony",
    loginCount: 11,
    isLoggedIn: false
}
```

When we call the constructor function without the `new` keyword, it is no longer treated as a constructor call. It becomes a **normal function call**, so JavaScript does not create a new object for each call and does not automatically make `this` refer to a separate object. In the typical non-strict browser context, when `User()` is called as a normal function, `this` refers to the **global object** (`window` in a browser). Therefore, the first call: `User("rocky", 12, true)` effectively does something like:

```jsx
window.username = "rocky";
window.loginCount = 12;
window.isLoggedIn = true;
```

Because the function explicitly has: `return this` , `userOne` receives a reference to that global object. When the second call happens, `User("tony", 11, false)` , there is **no new object**. `this` again refers to the same global object. Therefore, the assignments overwrite the values that were created by the first call. Now both `userOne` and `userTwo` refer to the **same global object.**

```jsx
userOne ──┐
          ↓
      Global Object
          ↑
userTwo ──┘
```

So when you execute,  `console.log(userOne)`, `console.log(userTwo)` both refer to the same object, and both show the latest values. The important thing to understand is that **the second user doesn’t overwrite the first variable itself**. Instead, both variables are pointing to the **same object**, and the second function call changes the properties of that shared object. This is exactly what `new` prevents. With `new User(...)`, JavaScript creates a **fresh object on every call** and makes `this` refer to that fresh object. Without `new`, both calls operate on the same `this` object in a non-strict browser context, which is why the second call overwrites the properties set by the first call.

#### **The `new` Keyword**

The `new` keyword is extremely important when working with constructor functions. It tells JavaScript that we want to create a **new instance** using the specified constructor.

```jsx
const userOne = new User("rocky", 12, true);
```

First, a new empty object is created. This new object becomes the instance that we are constructing. Then the `User` constructor function is called with the supplied arguments. During that constructor call, `this` refers to the newly created object. The properties defined using `this` are then added to that object. Finally, the newly created object is returned and assigned to `userOne`.

```
new User(...)
      ↓
Create a new empty object
      ↓
Call User constructor
      ↓
this → newly created object
      ↓
Add properties to the object
      ↓
Return the new object
      ↓
userOne
```

This is why `new` is not simply a normal function call. It changes how the function is executed and allows the function to act as a constructor.

---

#### **Understanding `this` in a Constructor Function**

In JavaScript, the `this` keyword refers to the **object that the current function is working with**. Its exact value depends on **how the function is called**. For now, we only need a basic understanding of `this`. Inside a constructor function used with `new`, `this` refers to the **new object/instance currently being created**.

Therefore:

```jsx
this.username = username;
```

means that the `username` value received as an argument is being assigned to the `username` property of the newly created object.

The two `username`s have different meanings here. The `username` on the right side is the parameter passed to the function, while `this.username` on the left side is the property belonging to the current object.

For example, when we execute:

```jsx
const userOne = new User("rocky", 12, true);
```

the `this` inside that particular constructor call refers to the newly created `userOne` object. The `this` keyword is used to attach those properties to the particular object that is currently being created.

---

#### **Adding Methods to a Constructor Function**

A constructor function can initialize methods as well as properties.

```jsx
function User(username, loginCount, isLoggedIn) {
    this.username = username;
    this.loginCount = loginCount;
    this.isLoggedIn = isLoggedIn;

    this.greeting = function() {
        console.log(`Welcome ${this.username}`);
    };
}

const userOne = new User("Rocky", 12, true);
userOne.greeting();

OUTPUT
Welcome Rocky
```

Now every instance created through the constructor has access to the `greeting()` method. When `userOne.greeting()` runs, `this` inside `greeting()` refers to `userOne`, so `this.username` gives `"rocky"`.

---

#### **The `constructor` Property**

JavaScript objects created through constructor-based mechanisms have access to a `constructor` property through their prototype chain.

For example:

```jsx
console.log(userOne.constructor);
```

The result refers to the `User` constructor function that was used to create the instance.

Conceptually: userOne → was created using → User constructor

Therefore, `userOne.constructor` points to the `User` function.

---

### 3. Instance

An **instance** is an individual object created from a constructor function or a class. Think of a class as a architectural blueprint for a house, while the instance is the actual physical house built using that blueprint. You can create multiple instances from a single blueprint, and each instance maintains its own separate data

If `User` is our constructor function, then:

```jsx
// User -> Constructor Function
function User(username, loginCount, isLoggedIn) {
    this.username = username;
    this.loginCount = loginCount;
    this.isLoggedIn = isLoggedIn;
}

// userOne and userTwo -> Instances of User
const userOne = new User("rocky", 12, true);
const userTwo = new User("tonyy", 11, false);
```

They have the same general structure defined by `User`, but they contain their own values. This is similar to a blueprint analogy. The constructor function defines how a `User` should be structured, while every object created using `new User(...)` is an individual instance based on that structure.

#### **`instanceof`**

JavaScript provides the `instanceof` operator to check whether an object is an instance associated with a particular constructor.

For example:

```jsx
console.log(userOne instanceof User); // true
console.log(userTwo instanceof User); // true
```

This produces `true` because `userOne` was created using `new User(...)`.

The `instanceof` operator is based on JavaScript’s **prototype chain**, so its complete behavior will become clearer when we study prototypes. For now, remember that it can be used to check whether an object is an instance of a particular constructor.

---

## Prototypes

Explained in the last page

---

## Classes in JavaScript

#### **What are Classes?**

A **class** in JavaScript is a blueprint for creating multiple objects that share the same structure and behavior. Instead of manually creating separate objects and repeatedly defining the same properties and methods, a class allows us to define the common structure once and then create as many objects as required from it.

JavaScript classes were introduced in **ES6 (ECMAScript 2015)**. Although the syntax looks similar to classes in languages such as Java or C++, JavaScript’s class system is built on top of its existing **prototype-based object model**. In other words, classes provide a cleaner and more convenient syntax for working with the prototype system rather than replacing it.

A class is defined using the `class` keyword:

```jsx
class User {
    // properties and methods
}
```

Once the class is defined, objects can be created from it using the `new` keyword.

#### **Constructor in a Class**

The `constructor()` is a special method inside a class that is automatically executed whenever a new object is created using `new`. It is mainly used to initialize the properties of the newly created object.

For example, suppose an application needs to represent users. Every user should have a name, email, and role. These common properties can be initialized through the constructor:

```jsx
class User {
    constructor(name, email, role) {
        this.name = name;
        this.email = email;
        this.role = role;
    }
}

const userOne = new User("Arjun", "arjun@example.com", "Admin");
```

When `new User("Arjun", "arjun@example.com", "Admin")` is executed, the constructor runs automatically, and `this` refers to the newly created object. Therefore, `this.name`, `this.email`, and `this.role` become properties of that particular object.

A class can have only **one `constructor()` method**. If no constructor is explicitly written, JavaScript provides a default constructor automatically.

#### **Methods in Classes**

Methods are functions defined inside a class that describe the behavior shared by objects created from that class. Unlike regular object methods, we do not need to write the `function` keyword when defining a method inside a class.

For example:

```jsx
class User {
    constructor(name, email, role) {
        this.name = name;
        this.email = email;
        this.role = role;
    }

    getDetails() {
        return `${this.name} - ${this.role}`;
    }

    changeRole(newRole) {
        this.role = newRole;
    }
}
```

Here, `getDetails()` and `changeRole()` are methods. They can access the properties of the object through `this`. Importantly, these methods are **shared through the class’s prototype**, rather than a separate copy of each method being created inside every object.

#### **Creating Multiple Objects from a Class**

The main advantage of a class is that one class can be used to create multiple independent objects. Each object gets its own instance properties, while the methods defined by the class are shared through the prototype.

```jsx
const user1 = new User(
    "Arjun",
    "arjun@example.com",
    "Admin"
);

const user2 = new User(
    "Meera",
    "meera@example.com",
    "Editor"
);

console.log(user1.getDetails());
console.log(user2.getDetails());
```

Both `user1` and `user2` are objects created from the same `User` class, but their `name`, `email`, and `role` values are different. Changing a property of `user1` does not change the corresponding property of `user2`.

Conceptually:

```
                    User Class
                       │
             ┌─────────┴─────────┐
             │                   │
          user1                user2
             │                   │
      name: "Arjun"       name: "Meera"
      role: "Admin"       role: "Editor"
```

The class therefore gives us a reusable structure for creating many independent objects with the same capabilities.

### **Behind the Scenes of Classes**

JavaScript classes can look like a completely different object-oriented system, but internally they are closely connected to the **constructor-function and prototype mechanism** discussed earlier. The `class` syntax provides a cleaner way of writing that mechanism.

Consider the same `User` example:

```jsx
class User {
    constructor(name, email, role) {
        this.name = name;
        this.email = email;
        this.role = role;
    }

    getDetails() {
        return `${this.name} - ${this.role}`;
    }

    changeRole(newRole) {
        this.role = newRole;
    }
}

const user = new User(
    "Arjun",
    "arjun@example.com",
    "Admin"
);

console.log(user.getDetails()); // To get tthe user details
user.changeRole("Editor"); 
console.log(user.getDetails()); // Get the updated details
```

Conceptually, the class syntax is doing something similar to the older constructor-function approach:

```jsx
function User(name, email, role) {
    this.name = name;
    this.email = email;
    this.role = role;
}

User.prototype.getDetails = function () {
    return `${this.name} - ${this.role}`;
};

User.prototype.changeRole = function (newRole) {
    this.role = newRole;
};

const user = new User(
    "Arjun",
    "arjun@example.com",
    "Admin"
);
```

The important part is that the **instance properties** such as `name`, `email`, and `role` belong to the individual object, while the methods are available through `User.prototype`.

Conceptually, the relationship looks like this:

```
                    User
              (constructor function)
                       │
                       │ .prototype
                       ▼
              ┌─────────────────────┐
              │ User.prototype      │
              │                     │
              │ getDetails()        │
              │ changeRole()        │
              └──────────┬──────────┘
                         │
                    prototype link
                         │
                         ▼
              ┌─────────────────────┐
              │ user                │
              │                     │
              │ name: "Arjun"       │
              │ email: "..."        │
              │ role: "Admin"       │
              └─────────────────────┘
```

When `new User(...)` is executed, the same fundamental `new` mechanism discussed earlier comes into play. A new object is created, its prototype is linked to `User.prototype`, the `User` constructor is executed with `this` referring to that new object, and the resulting object is returned.

So the class syntax:

```jsx
class User {
    constructor(...) { ... }

    getDetails() { ... }
}
```

does **not** mean that every object receives its own independent copy of `getDetails()`. Instead, the method is placed on the class’s prototype and instances access it through the prototype chain.

---