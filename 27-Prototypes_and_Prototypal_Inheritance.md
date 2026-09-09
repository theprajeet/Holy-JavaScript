# Prototype and Prototypal Inheritance

## **Prototype**

In JavaScript, a prototype acts as a shared blueprint that stores common methods and properties for objects of the same type. This is one of the fundamental mechanisms behind how JavaScript objects work. Instead of storing every method separately inside every object, JavaScript can store a method in a prototype object and allow other objects to access it through the **prototype chain**.

- Properties and methods added to a prototype are shared across all instances.
- This mechanism helps optimize memory usage and enables inheritance.
- Adding a method or property to an object’s prototype makes it accessible to all its instances.
- Using prototypes avoids code duplication by defining shared methods once, making the code more efficient.

The basic behavior can be remembered as: **“Look here first. If it isn’t found, look higher.”** When JavaScript tries to access a property or method, it first checks the object itself. If it cannot find it there, JavaScript looks at that object’s prototype. If it still isn’t found, it continues looking through the prototype chain until it finds the property/method or reaches `null`.

For example, arrays have methods such as `map()`, `filter()`, `join()`, and `indexOf()`, but these methods are not separately created inside every array. They are available through `Array.prototype`, which is why every array can use them.

### **Prototype Chain and Prototypal Inheritance**

The sequence through which JavaScript searches for properties and methods is called the **prototype chain**. A typical chain looks like this:

Suppose we have an array and try to access `map()`. JavaScript first checks whether `map` exists directly on that array. If it doesn’t, JavaScript checks `Array.prototype`, where `map()` is available. Therefore, the method can be used even though it isn’t stored directly inside the array itself.

<img src="/Assets/Proto.png" alt="Prototypal_Inheritance.png" width="600" >

If JavaScript doesn’t find a property in `Array.prototype`, it continues to its prototype, which is `Object.prototype`. If it isn’t found there either, the chain eventually reaches `null`. At that point, JavaScript stops searching.

This behavior is what makes **prototypal inheritance** possible. An object can access functionality from another object higher in its prototype chain without having that functionality directly stored inside itself.

---

#### **Why `Object.prototype` Is Important?**

`Object.prototype` sits very high in the prototype chain of ordinary JavaScript objects. Many built-in objects eventually connect to it, which means properties and methods available there can be accessed by objects further down the chain.

For example, if a method is added to `Object.prototype`, an array can potentially access it, and so can an ordinary object, because both can eventually reach `Object.prototype` through their prototype chains.

```jsx
Object.prototype.describe = function () {
    console.log("This method came from Object.prototype");
};

const numbers = [10, 20, 30];
const profile = { name: "Arjun" };

numbers.describe();
profile.describe();

OUTPUT 
This method came from Object.prototype
This method came from Object.prototype
```

Here in the above example we are adding a new method called `describe` to **`Object.prototype`**. To understand this, remember that JavaScript objects can inherit properties and methods through the **prototype chain**. `Object.prototype` is the prototype from which ordinary JavaScript objects ultimately inherit. 

The syntax can be broken down as `Object.prototype` → access the prototype object of the built-in `Object` constructor, `.describe` → create/access a property named `describe` on that prototype, and `= function () { ... }` → assign a function to that property. Because the property contains a function, it becomes a **method** that objects can access through their prototype chain. 

The direction is important. If something is added to a **higher level**, objects below it can access it. But if something is added only to a lower level, objects above it do not automatically gain access to it.

So in the example `numbers` is an array, so it first looks for `describe` on its own properties and then on `Array.prototype`. If it isn’t found there, JavaScript continues further up the prototype chain to `Object.prototype`, where it finds the `describe` method we added. Similarly, `profile` is an ordinary object, so after checking the object itself, JavaScript looks at its prototype, which is `Object.prototype`, and finds `describe` there. 

The important point is that **we did not actually add `describe` directly to the `numbers` array or the `profile` object**. We added it to `Object.prototype`, and JavaScript’s prototype chain allows objects to look upward and find inherited properties and methods that don’t exist directly on themselves.

---

### **Are Functions Also Objects?**

One of the interesting aspects of JavaScript is that a function is not only callable code, a function is also an object. This means a function can have properties attached to it just like other objects.

```jsx
function greet(name) {
    return `Hello, ${name}!`;
}

greet.language = "English";

console.log(greet("Arjun"));      // Hello, Arjun!
console.log(greet.language);      // English
```

Here, `greet` can be **called like a function** using `greet("Arjun")`, but it can also have a **property** called `language`, just like an object. So `greet` is both callable and capable of storing properties.

This dual nature is important because it explains why functions can have a special property called **`prototype`**.

### **Constructor Functions + Prototypes**

A **constructor function** is a regular JavaScript function that is intended to create and initialize multiple objects with a common structure. The `new` keyword is used to create an instance from it.

```jsx
function Account(owner, balance) {
    this.owner = owner;
    this.balance = balance;
}

Account.prototype.deposit = function (amount) {
    this.balance += amount;
};

Account.prototype.showBalance = function () {
    console.log(`Balance: ${this.balance}`);
};

const firstAccount = new Account("Arjun", 5000);
const secondAccount = new Account("Meera", 8000);

firstAccount.deposit(1000);
secondAccount.showBalance();
```

Both objects have their own `owner` and `balance` values, but they are created according to the same constructor function.

We can then place shared methods on the constructor’s prototype, `deposit()` and `showBalance()` are not separately created inside `firstAccount` and `secondAccount`. Both instances can find these methods through `Account.prototype`.

#### **Why Prototype Methods Are Useful?**

Suppose we created 1,000 account objects and placed the same `deposit()` function directly inside every object. We would have 1,000 separate function references. This is unnecessary when the behavior is identical.

With prototypes, the method can be stored once:

```
firstAccount ──────┐
                   ↓
            Account.prototype
                   ↑
secondAccount ─────┘
```

Both objects use the same prototype method. When `firstAccount.deposit()` is called, JavaScript doesn’t need to find a separate `deposit()` function inside `firstAccount`; it follows the prototype chain and finds the shared method.

This is one of the major practical advantages of prototypes: **shared behavior without duplicating the method in every instance**.

### **The `new` Keyword — What Actually Happens?**

The `new` keyword is extremely important when working with constructor functions and prototypes. When we write:

```jsx
const account = new Account("Arjun", 5000);
```

JavaScript performs several important operations behind the scenes.

First, a **new empty object** is created. Then, the new object’s internal prototype is connected to `Account.prototype`. After that, the `Account` function is called with `this` referring to this newly created object, so assignments such as `this.owner = owner` and `this.balance = balance` add properties to that new object. Finally, the newly created object is returned as the result of the `new` expression, provided the constructor does not explicitly return another object.

Conceptually:

```
new Account("Arjun", 5000)

        ↓

1. Create a new object
        ↓
2. Link it to Account.prototype
        ↓
3. Call Account with `this` = new object
        ↓
4. Return the new object
```

So after this operation:

```
account
  ↓
Account.prototype
  ↓
Object.prototype
  ↓
null
```

This prototype connection is one of the most important things that `new` does. It is not simply “a keyword that creates an object.”

### **Why `createUser()` and `new createUser()` Are Different**

Consider a constructor function:

```jsx
function Member(name, points) {
    this.name = name;
    this.points = points;
}

Member.prototype.showPoints = function () {
    console.log(this.points);
};

const memberA = new Member("Arjun", 100);
const memberB = Member("Meera", 200);
```

`memberA` is created using `new`, so JavaScript creates a new object, connects it to `Member.prototype`, and executes the constructor with `this` referring to that new object.

`Member("Meera", 200)`, however, is just a normal function call. The special constructor behavior does not happen. Therefore, `memberB` does not become a properly constructed instance of `Member`.

This is why simply calling a constructor function like an ordinary function is not equivalent to using `new`.

The `new` keyword establishes the important relationship between the newly created object and the constructor’s prototype.

### **`this` and the Current Object**

The important idea is that **when a prototype method is called through an object, `this` refers to the object that is currently using the method**.

```jsx
function Player(name, score) {
    this.name = name;
    this.score = score;
}

Player.prototype.increaseScore = function () {
    this.score++;
};

const playerOne = new Player("Arjun", 20);
const playerTwo = new Player("Meera", 50);

playerOne.increaseScore();
playerTwo.increaseScore();
```

The same `increaseScore()` method exists on `Player.prototype`, but when `playerOne.increaseScore()` is called, `this` refers to `playerOne`. When `playerTwo.increaseScore()` is called, `this` refers to `playerTwo`.

Therefore, the same shared method can operate on different objects.

If JavaScript evaluates `playerOne.increaseScore()`, it first checks `playerOne`. The method isn’t stored directly there, so JavaScript checks `Player.prototype` and finds `increaseScore()`. If we tried to access a method that wasn’t present there, JavaScript would continue to `Object.prototype`. If it still couldn’t find the requested property, the search would eventually reach `null`.

This lookup process happens automatically. We don’t normally have to manually traverse the chain ourselves.

---

## **Prototypal Inheritance**

**Prototypal inheritance** means that an object can inherit/access properties and methods from another object through its prototype relationship.

For example, imagine a `Teacher` object that should be able to access common information stored in a `User` object. Instead of copying those properties into `Teacher`, we can establish a prototype relationship between them.

```jsx
const user = {
    name: "Arjun",
    email: "arjun@example.com"
};

const teacher = {
    canTeach: true
};

Object.setPrototypeOf(teacher, user);

console.log(teacher.canTeach); // true
console.log(teacher.name);     // Arjun
console.log(teacher.email);    // arjun@example.com
```

`teacher` does not directly contain `name` or `email`. JavaScript first checks `teacher`, doesn’t find them, and then follows its prototype link to `user`.

Conceptually:

```
teacher
   ↓
user
   ↓
Object.prototype
   ↓
null
```

This is prototypal inheritance: **one object is able to access another object’s properties through the prototype chain.**

### **`__proto__` vs `.prototype`**

These two terms look similar but represent different things, and confusing them is extremely common.

The `.prototype` property is associated with constructor functions. It points to the prototype object that instances created by that constructor can inherit from.

```jsx
function Member(name, points) {
    this.name = name;
    this.points = points;
}

Member.prototype.showPoints = function () {
    console.log(this.points);
};

const memberA = new Member("Arjun", 100);
```

`__proto__`, on the other hand, is an accessor through which an object’s prototype can be accessed.

```jsx
const animal = {
  isAlive: true,
  eat() {
    console.log("Eating...");
  }
};

const dog = {
  bark() {
    console.log("Woof!");
  }
};

// Setting the prototype link
dog.__proto__ = animal; 

// Accessing properties via the prototype chain
console.log(dog.isAlive); // true (inherited from animal)
dog.eat();                // "Eating..." (inherited from animal)
```

Here, `animal` is a normal object containing an `isAlive` property and an `eat()` method, while `dog` is another object containing its own `bark()` method. When we write `dog.__proto__ = animal`, we are establishing a **prototype relationship** between the two objects, meaning `dog` will now inherit properties and methods from `animal`. The `dog` object itself does not contain `isAlive` or `eat()`, so when JavaScript encounters `dog.isAlive`, it first checks `dog` , since it doesn’t find the property there, it follows the prototype link to `animal` and finds `isAlive`, returning `true`. The same thing happens when we call `dog.eat()`—JavaScript doesn’t find `eat()` directly on `dog`, so it looks at its prototype, `animal`, finds the method there, and executes it. Therefore, the prototype chain in this example is `dog → animal → Object.prototype → null`.

A useful way to remember the distinction is:

**Constructor → `.prototype`  |  Object → its prototype**

The `__proto__` property is **deprecated and considered outdated**. You should avoid using it directly in modern development. Standard methods such as `Object.getPrototypeOf()` and `Object.setPrototypeOf()` are clearer.

### **`Object.setPrototypeOf()`**

`Object.setPrototypeOf()` is a standard way to explicitly set the prototype of an object. It is a built-in JavaScript method used to **set or change the prototype of an object**. It takes two arguments: the object whose prototype you want to change and the object you want to make its prototype.

`Object.setPrototypeOf()` was standardized in **ES6 (ECMAScript 2015)**. Before that, JavaScript developers commonly used the non-standard/legacy `__proto__` mechanism to access or modify an object’s prototype

```jsx
const animal = {
isAlive: true,
eat() {
console.log("Eating...");
}
};

const dog = {
bark() {
console.log("Woof!");
}
};

// Setting the prototype link
Object.setPrototypeOf(dog, animal);

// Accessing properties via the prototype chain
console.log(dog.isAlive); // true
dog.eat();               // Eating...
```

In this example, `animal` is an object containing the `isAlive` property and `eat()` method, while `dog` is another object containing its own `bark()` method. `Object.setPrototypeOf(dog, animal)` explicitly sets `animal` as the **prototype of `dog`**, creating the relationship `dog → animal`. Because of this relationship, when JavaScript tries to access `dog.isAlive`, it first searches the `dog` object, doesn’t find the property, and then follows the prototype link to `animal`, where it finds `isAlive`. The same process happens with `dog.eat()`, allowing `dog` to use the method inherited from `animal`.

The main difference from `__proto__` is **how the prototype relationship is accessed or modified**. With `__proto__`, we write `dog.__proto__ = animal`, where `__proto__` is a legacy accessor that exposes an object’s prototype. With `Object.setPrototypeOf()`, we explicitly tell JavaScript to set the prototype using a standard built-in method: `Object.setPrototypeOf(dog, animal)`. Both establish the same prototype relationship, but **`Object.setPrototypeOf()` is generally preferred because it is the standard, explicit API for changing an object’s prototype**, whereas `__proto__` is a legacy feature that is mainly retained for compatibility. In modern JavaScript, you will generally see `Object.getPrototypeOf()` used to **read** an object’s prototype and `Object.setPrototypeOf()` used to **change** it.

This is essentially the same prototype relationship that JavaScript uses throughout its object system. Modern `class` syntax provides a more familiar syntax for developers coming from class-based languages, but the underlying prototype mechanism still exists.

---

### **Some Custom Prototype Examples**

JavaScript’s built-in objects also use prototypes. Arrays inherit from `Array.prototype`, strings inherit from `String.prototype`, and these eventually connect to `Object.prototype`.

Because of this, it is technically possible to add custom functionality to these prototypes.

For example, we can add a method to `Array.prototype`:

```jsx
Array.prototype.customMessage = function () {
    console.log(`This array contains ${this.length} elements.`);
};

const numbers = [10, 20, 30, 40];

numbers.customMessage();

OUTPUT
This array contains 4 elements.
```

`customMessage()` is not directly defined inside `numbers`. JavaScript looks at `numbers`, doesn’t find it, then checks `Array.prototype`, where it finds the method.

The same idea can be applied to strings:

```jsx
String.prototype.trueLength = function () {
    console.log(`Actual length: ${this.trim().length}`);
};

const username = "  Peter  ";

username.trueLength();
"Steve Rogers".trueLength();

OUTPUT
Actual length: 5
Actual length: 12
```

When `username.trueLength()` is called, `this` refers to the string value being used to call the method. The method can therefore use `this.trim()` to remove the extra spaces and then calculate the actual length.

---