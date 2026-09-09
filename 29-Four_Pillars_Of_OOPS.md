# Four Pillars of OOP, Static

The four concepts commonly referred to as the **four pillars of OOP** are 

1. **Abstraction**
2. **Encapsulation** 
3. **Inheritance**
4. **Polymorphism**

## **1. Abstraction**

Abstraction ****in JavaScript is the process of hiding implementation details and showing only the essential features of an object. It helps users focus on what an object does rather than how it does it.

- Hides complexity from the user.
- Improves maintainability.
- Enhances flexibility and modularity.

**Example:** An ATM or a coffee machine represents abstraction, where the user interacts with simple operations while the internal implementation details remain hidden.

It is achieved in JavaScript using classes, closures, modules, and private fields.

- Classes provide a structured way to hide implementation details.
- Closures can restrict direct access to variables.

JavaScript does not provide a dedicated `abstract` keyword like some other object-oriented languages. Instead, abstraction is achieved through language features and programming techniques such as **functions, objects and methods, classes, modules, closures, and private fields**.

#### **Abstraction Using Functions**

Functions are one of the simplest ways to achieve abstraction. A function can contain complex logic internally while exposing only its name and required parameters to the outside code. The caller only needs to know what the function expects and what it returns; the internal implementation can remain hidden.

For example, suppose we need to calculate the area of a circle. Instead of repeatedly writing the mathematical formula wherever the calculation is required, we can encapsulate it inside a function:

```jsx
function calculateCircleArea(radius) {
    return Math.PI * radius * radius;
}

console.log(calculateCircleArea(5));
```

The caller only needs to know that `calculateCircleArea()` accepts a radius and returns the area. It does not need to worry about the value of `Math.PI` or the formula used internally. If the implementation changes later, the code using the function does not necessarily need to change.

This is abstraction because the **implementation is hidden behind a simple interface**.

#### **Abstraction Using Objects and Methods**

Objects can also provide abstraction by grouping related data and functionality together. A method can expose a simple operation while hiding the internal steps required to perform that operation.

For example, consider a car object:

```jsx
const car = {
    brand: "Toyota",

    start() {
        console.log("Car started");
    }
};

car.start();
```

The user only needs to call `car.start()`. They do not need to know what internal operations a real car would perform to start its engine. The `start()` method acts as the interface through which the user interacts with the object.

In a real application, the method could contain considerably more complex logic while keeping the same simple interface:

```jsx
car.start();
```

The important idea is that **the complexity of the implementation can remain inside the object while the outside code interacts through meaningful methods**.

#### **Abstraction and Private Fields**

JavaScript private fields and methods, represented using the `#` syntax, are particularly useful when implementing abstraction because they allow internal data and operations to be hidden from external code.

For example:

```jsx
class BankAccount {
    #balance = 0;

    deposit(amount) {
        this.#balance += amount;
    }

    getBalance() {
        return this.#balance;
    }
}

const account = new BankAccount();

account.deposit(500);

console.log(account.getBalance()); // 500
```

Here, the actual `#balance` cannot be accessed directly from outside the class:

```jsx
console.log(account.#balance); // SyntaxError
```

Instead, the class provides controlled methods such as `deposit()` and `getBalance()` through which the outside code interacts with the account.

This is useful because the internal state and the rules for modifying it can be controlled by the class rather than being directly exposed.

**Note:** Private fields are strongly related to **encapsulation**, while abstraction is the broader concept of exposing only what is necessary and hiding unnecessary implementation details. In JavaScript, encapsulation mechanisms such as private fields can therefore be used to implement abstraction.

#### **Abstraction Using Classes**

Classes provide a more structured way to implement abstraction, especially when an application contains complex objects with multiple properties and methods. A class can expose the operations that users of the object need while keeping implementation details inside its methods.

For example, consider a coffee machine:

```jsx
class CoffeeMachine {

    makeCoffee() {
        this.#boilWater();
        this.#brewCoffee();

        console.log("Coffee is ready!");
    }

    #boilWater() {
        console.log("Boiling water...");
    }

    #brewCoffee() {
        console.log("Brewing coffee...");
    }
}

const coffee = new CoffeeMachine();

coffee.makeCoffee();
```

Here, the user only needs to call:

```jsx
coffee.makeCoffee();
```

The internal steps such as `#boilWater()` and `#brewCoffee()` are implementation details. They are not required to be known by the code using the `CoffeeMachine` object. Therefore, the class provides a simple public interface while keeping internal implementation details hidden.

#### **Use Cases of Abstraction**

- **API Development:** Abstraction can be used to create the APIs where the only necessary functionality is exposed and internal logic is hidden.
- **Database Operations:** Database interactions are abstracted using Object-Relational Mapping (ORM) frameworks, so developers work with high-level models instead of raw SQL queries.
- **Middleware in Web Servers:** Web servers use middleware functions to abstract authentication, logging, and error handling.
- **Security and Encryption:** Sensitive operations like password hashing and encryption are abstracted into reusable functions, preventing direct manipulation of security mechanisms.

#### **Benefits of Abstraction in JavaScript**

- **Better Code Quality:** Your code will be simpler and easier to read.
- **No Repeated Code:** You’ll store shared logic in one place instead of copying it everywhere.
- **Easier Updates:** Changing one piece of logic won’t break other parts of the application.
- **Team-Friendly:** Developers can work on different parts without needing to know every detail of the codebase.

---

## **2.  Encapsulation**

Encapsulation ****is the process of wrapping data and methods into a single unit and restricting direct access to the data. It acts as a protective shield that prevents data from being accessed directly from outside the object.

- Data can be hidden using private fields (#).
- Access to data is provided through public methods.
- It improves data security, maintainability, and controlled access.

Example -  The car hides complex mechanical parts like the engine, pistons, and fuel lines under the hood. You only interact with safe, public controls like the steering wheel, gas pedal, and brake.

Encapsulation provides three important ideas: **bundling related data and behavior together, restricting direct access to internal data, and providing controlled access through public methods**.

#### **Encapsulation Using Classes**

ES6 introduced the `class` syntax, which provides a structured way to group related properties and methods together. A class can contain the data belonging to an object along with the methods responsible for operating on that data.

Consider a bank account:

```jsx
class BankAccount {
    constructor(accountNumber, accountHolder, balance) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = balance;
    }

    deposit(amount) {
        this.balance += amount;
    }

    withdraw(amount) {
        if (this.balance >= amount) {
            this.balance -= amount;
        } else {
            console.log("Insufficient balance");
        }
    }
}
```

Here, the account information and the operations performed on that information are grouped inside the `BankAccount` class. The `deposit()` and `withdraw()` methods operate on the account’s balance, keeping the related data and behavior together.

However, in this example, the properties are still publicly accessible:

```jsx
const account = new BankAccount("123456", "Arjun", 1000);

account.balance = 500000;
```

JavaScript allows this modification because `balance` is a normal public property. Therefore, simply placing properties and methods inside a class does **not automatically make the properties private**.

#### **Using Naming Conventions for Encapsulation**

Before JavaScript introduced private fields, developers commonly used an underscore (`_`) as a naming convention to indicate that a property was intended to be treated as internal.

```jsx
class BankAccount {
    constructor(accountNumber, accountHolder, balance) {
        this._accountNumber = accountNumber;
        this._accountHolder = accountHolder;
        this._balance = balance;
    }

    deposit(amount) {
        this._balance += amount;
    }

    withdraw(amount) {
        if (this._balance >= amount) {
            this._balance -= amount;
        } else {
            console.log("Insufficient balance");
        }
    }
}
```

The underscore communicates that `_balance`, `_accountNumber`, and `_accountHolder` are intended for internal use. However, **the underscore does not actually make a property private**.

External code can still access them:

```jsx
const account = new BankAccount("123456", "Arjun", 1000);

console.log(account._balance); // 1000

account._balance = 500000;     // Allowed
```

Therefore, the underscore is only a **naming convention**, not a JavaScript access-control mechanism. It tells other developers, “this property is intended to be internal,” but JavaScript does not enforce that restriction.

#### **Controlled Access Through Public Methods**

A better approach is to expose public methods that control how internal data is accessed or modified. Instead of allowing outside code to directly manipulate the balance, operations such as `deposit()` and `withdraw()` can enforce rules.

For example:

```jsx
class BankAccount {
    constructor(balance) {
        this._balance = balance;
    }

    deposit(amount) {
        if (amount > 0) {
            this._balance += amount;
        }
    }

    withdraw(amount) {
        if (amount <= this._balance) {
            this._balance -= amount;
        } else {
            console.log("Insufficient balance");
        }
    }

    getBalance() {
        return this._balance;
    }
}
```

Now the intended interaction is through methods:

```jsx
const account = new BankAccount(1000);

account.deposit(500);
account.withdraw(200);

console.log(account.getBalance()); // 1300
```

The methods provide **controlled access** to the data. For example, `withdraw()` can prevent a withdrawal when the requested amount is greater than the available balance. This keeps the rules associated with the data inside the class.

However, because `_balance` is still technically public, this approach is convention-based rather than true privacy.

#### **Private Fields (`#`) and Private Methods**

Modern JavaScript provides **private class fields**, which are declared using the `#` prefix. Unlike the underscore convention, private fields provide actual language-level privacy. A private field can only be accessed from within the class that declares it.

```jsx
class BankAccount {
    #balance;

    constructor(balance) {
        this.#balance = balance;
    }

    deposit(amount) {
        this.#balance += amount;
    }

    getBalance() {
        return this.#balance;
    }
}

const account = new BankAccount(1000);

account.deposit(500);

console.log(account.getBalance()); // 1500
```

The `#balance` field cannot be accessed directly from outside the class:

```jsx
console.log(account.#balance); // SyntaxError
```

The only way to interact with the balance is through the methods provided by the class, such as `deposit()` and `getBalance()`.

This provides **true encapsulation** because JavaScript itself prevents external code from directly accessing the private field.

**Private Methods**

```jsx
class BankAccount {
    #balance = 1000;

    #validateAmount(amount) {
        return amount > 0;
    }

    deposit(amount) {
        if (this.#validateAmount(amount)) {
            this.#balance += amount;
        }
    }

    getBalance() {
        return this.#balance;
    }
}
```

Here, `#validateAmount()` is an internal method used by the class. External code cannot call it:

```jsx
account.#validateAmount(500); // SyntaxError
```

This is useful when a class contains helper methods that are required for its internal implementation but should not form part of its public interface.

In this example, `#balance` represents the **internal data**, while `deposit()`, `withdraw()`, and `getBalance()` form the **public interface**. External code cannot directly change the balance, so all modifications must go through the methods provided by the class.

This is a strong example of encapsulation because the class both **bundles the data and behavior together** and **restricts direct access to the internal data**.

### **Encapsulation Using Closures**

Closures can also be used to achieve encapsulation by keeping variables inside a function’s scope and exposing only selected functions that can interact with those variables. The outside code cannot directly access the enclosed variables.

For example, a variable such as `balance` can remain inside a function while `deposit()` and `withdraw()` are returned as the only ways to modify it.

Closures are an important JavaScript concept and provide another way of achieving private-like data. **We will study closures in detail later**, so for now, remember that closures can also be used for encapsulation.

### **Encapsulation vs Abstraction**

Encapsulation and abstraction are closely related, but they focus on different things. **Encapsulation focuses on protecting and controlling access to data and implementation**, whereas **abstraction focuses on exposing only the essential functionality while hiding unnecessary implementation details**.

For example, in a `BankAccount` class, keeping `#balance` private and allowing it to be modified only through `deposit()` and `withdraw()` is **encapsulation**. Providing users with simple operations such as `deposit()` and `withdraw()` without requiring them to understand how the balance is internally maintained is **abstraction**.

A simple way to remember the distinction is:

**Encapsulation → How do we protect and control access to data?**

**Abstraction → What should we expose and what should we hide?**

The two concepts often work together. **Private fields and controlled methods provide encapsulation, while the public methods exposed by the class form the interface through which abstraction can be achieved.**

### **Use Cases of Encapsulation**

Encapsulation improves the overall structure and reliability of an application by keeping related data and behavior together and preventing uncontrolled modification of internal state. It also makes a class easier to maintain because the rules governing its data are contained within the class itself.

The major benefits include:

- **Data protection:** Prevents direct and uncontrolled modification of important data.
- **Controlled access:** Data can be accessed or modified through defined methods.
- **Maintainability:** Internal implementation can be changed without affecting external code that uses the public interface.
- **Modularity:** Related data and behavior remain organized within a single unit.
- **Reduced complexity:** External code does not need to know or interact with every internal detail.

---

## **3. Inheritance**

Inheritance is a core OOP concept in JavaScript that allows one class to acquire the properties and methods of another class using the extends keyword. It represents an "is-a" relationship between classes.

- The class being inherited is called the parent class, and the inheriting class is the child class.
- A child class can use existing features of the parent class and also add its own.
- Inheritance promotes code reusability and reduces redundancy.

For example, suppose an application has a general `User` class containing common functionality such as a name, email, displaying details, and changing a user’s role. If we later need a more specialized type of user, such as an `Admin`, the `Admin` should naturally have all the common functionality of `User` while also being able to provide its own additional functionality.

This relationship can be represented as:

```
User
 │
 │ common functionality
 ▼
Admin
 │
 └── additional functionality
```

The class being inherited from is generally called the **parent/base/superclass**, while the class that inherits from it is called the **child/derived/subclass**.

JavaScript provides the `extends` keyword to establish this relationship.

#### **`extends` Keyword**

The `extends` keyword is used to create a child class from another class.

```jsx
class Admin extends User {
    // additional functionality
}
```

This means that `Admin` inherits the functionality available from `User`. An object created from `Admin` can therefore access methods defined in `User`, even though those methods were not written again inside `Admin`.

The important point is that inheritance does **not** mean that all methods are physically copied into the child object. JavaScript uses its prototype system to make inherited methods available through the prototype chain.

#### **Example of Class Inheritance**

Let’s extend our existing `User` example. A normal user has a name, email, and role, while an administrator should have all of those things plus an ability to manage users.

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

class Admin extends User {
    constructor(name, email) {
        super(name, email, "Admin");
    }

    manageUsers() {
        return `${this.name} can manage users.`;
    }
}

const admin = new Admin(
    "Arjun",
    "arjun@example.com"
);

console.log(admin.getDetails());
console.log(admin.manageUsers());

admin.changeRole("Super Admin");
console.log(admin.getDetails());
```

Here, `Admin` extends `User`, so the `admin` object can use both its own `manageUsers()` method and the inherited `getDetails()` and `changeRole()` methods. Notice that `manageUsers()` belongs specifically to `Admin`, while `getDetails()` and `changeRole()` come from `User`.

The output would be:

```
Arjun - Admin
Arjun can manage users.
Arjun - Super Admin
```

#### **`super()` — Calling the Parent Constructor**

When a child class has its own constructor, it must call `super()` **before using `this`**.

```jsx
class Admin extends User {
    constructor(name, email) {
        super(name, email, "Admin");
    }
}
```

`super()` calls the constructor of the parent class, which in this case is `User`.

So, `super(name, email, "Admin")`  essentially means: “Run the `User` constructor for this new `Admin` object and initialize its inherited properties.”

The values flow like this:

```
new Admin("Arjun", "arjun@example.com")
                │
                ▼
       Admin constructor
                │
                │ super(...)
                ▼
        User constructor
                │
                ▼
      this.name = "Arjun"
      this.email = "arjun@example.com"
      this.role = "Admin"
```

This is why we don’t need to repeat:

```jsx
this.name = name;
this.email = email;
this.role = "Admin";
```

inside the `Admin` constructor.

#### **Why Must `super()` Come Before `this`?**

A derived class does not get its own usable `this` until the parent constructor has been called.

Therefore, this is valid:

```jsx
class Admin extends User {
    constructor(name, email) {
        super(name, email, "Admin");

        this.accessLevel = "full";
    }
}
```

But this is invalid:

```jsx
class Admin extends User {
    constructor(name, email) {
        this.accessLevel = "full"; // Error
        super(name, email, "Admin");
    }
}
```

The reason is that JavaScript needs the parent constructor to participate in setting up the derived object before `this` can be used inside the child constructor.

#### **`super` Is Not Only for Constructors**

`super` can also be used to access methods from the parent class.

Suppose the child wants to extend the behavior of `getDetails()` instead of completely replacing it:

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

class Admin extends User {
    constructor(name, email) {
        super(name, email, "Admin");
    }

    getDetails() {
        return `${super.getDetails()} | Full Access`;
    }
}

const admin = new Admin(
    "Arjun",
    "arjun@example.com"
);

console.log(admin.getDetails());
```

Output

```
Arjun - Admin | Full Access
```

Here, `super.getDetails()` accesses the implementation of `getDetails()` from the parent class.

#### **Inheritance and Method Overriding**

A child class can define a method with the same name as a method in the parent class. This is called **method overriding**.

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

class Admin extends User {
    constructor(name, email) {
        super(name, email, "Admin");
    }

    getDetails() {
        return `${this.name} has administrator access.`;
    }
}

console.log(admin.getDetails());
admin.changeRole("Super Admin");
console.log(admin.getDetails());
console.log(admin.email);
```

```jsx
OUTPUT

Arjun has administrator access.
Arjun has administrator access.
arjun@example.com
```

Now when: `admin.getDetails()` is called, JavaScript finds `getDetails()` in `Admin.prototype` first, so it does not continue searching in `User.prototype`. Notice that after `changeRole("Super Admin")`, the output of `getDetails()` doesn’t change because the overridden `Admin.getDetails()` doesn’t use `this.role`; it only uses `this.name`. If we want to demonstrate the changed role as well, a better overridden method would be:

```
admin
  ↓
Admin.prototype
  │
  └── getDetails() 

  User.prototype
  │
  └── getDetails()  ← not reached
```

This is another direct consequence of the prototype lookup mechanism.

#### **`instanceof` and Inheritance**

JavaScript also provides the `instanceof` operator to check whether an object belongs to a particular class or its inheritance hierarchy.

With our example:

```jsx
console.log(admin instanceof Admin);
console.log(admin instanceof User);
console.log(admin instanceof Object);
```

The result is:

```
true
true
true
```

Why is `admin instanceof User` also `true`?

Because `admin` ultimately has access to `User.prototype` through the prototype chain:

```
admin
  ↓
Admin.prototype
  ↓
User.prototype
```

So `instanceof` follows the prototype chain when determining whether the relevant prototype exists in that chain.

---

## **4. Polymorphism**

Polymorphism means "many forms", where a single entity can behave differently in different situations. In JavaScript, it allows the same method or object to show different behavior based on context.

- Same method, different behavior depending on the object.
- Achieved through method overriding.
- Supports flexibility and extensibility.

**Example:** Different animals represent polymorphism, where the same method speak() produces different outputs like Bark, Meow, and Moo depending on the object.

**Types of Polymorphism**

Polymorphism in JavaScript is mainly of the following types:

- **Runtime Polymorphism (Method Overriding):** Achieved when a child class provides its own implementation of a method already defined in its parent class. The method call is resolved at runtime based on the object.
- **Pseudo Compile-Time Polymorphism**: JavaScript does not support true method overloading like Java. Similar functionality can be achieved using default parameters, optional parameters, or rest parameters.

The main forms of polymorphism commonly discussed in JavaScript are **method overriding** and **method overloading-like behavior**.

#### **Method Overriding**

Method overriding occurs when a child class provides its own implementation of a method that already exists in its parent class. The child class inherits the method from the parent, but by defining a method with the same name, it replaces the inherited implementation for instances of that child class.

```jsx
class Animal {
    speak() {
        console.log("Animal makes a sound");
    }
}

class Dog extends Animal {
    speak() {
        console.log("Dog barks");
    }
}

class Cat extends Animal {
    speak() {
        console.log("Cat meows");
    }
}

const dog = new Dog();
const cat = new Cat();

dog.speak(); // Dog barks
cat.speak(); // Cat meows
```

Here, `Animal` defines a general `speak()` method. Both `Dog` and `Cat` inherit from `Animal`, but each class provides its own implementation of `speak()`. Therefore, calling `speak()` on a `Dog` produces `"Dog barks"`, while calling it on a `Cat` produces `"Cat meows"`. The same method name has **different behavior depending on the object**, which is the essence of polymorphism.

#### **Runtime Polymorphism**

Method overriding in JavaScript is an example of **runtime polymorphism** because the method that actually executes is determined at runtime based on the object on which the method is called.

Consider:

```jsx
const animals = [new Dog(), new Cat()];

for (const animal of animals) {
    animal.speak();
}
```

The same `animal.speak()` statement is used for both objects. When `animal` refers to a `Dog` object, `Dog`’s `speak()` method executes. When it refers to a `Cat` object, `Cat`’s `speak()` method executes.

The calling code does not need to check whether the object is a `Dog` or a `Cat`. Each object provides its own implementation of the common `speak()` method.

This is particularly useful when working with collections of related objects because the same piece of code can operate on different object types while allowing each object to perform the operation in its own way.

#### **Polymorphism Through Functions and Objects**

Polymorphism does not require classes or inheritance. JavaScript’s dynamic nature also allows functions to work with different objects as long as those objects provide the required method or property.

```jsx
const dog = {
    speak() {
        console.log("Dog barks");
    }
};

const cat = {
    speak() {
        console.log("Cat meows");
    }
};

function makeSound(animal) {
    animal.speak();
}

makeSound(dog); // Dog barks
makeSound(cat); // Cat meows
```

The `makeSound()` function does not care whether it receives a dog or a cat. It simply expects the supplied object to have a `speak()` method. When a `dog` is passed, its `speak()` method runs; when a `cat` is passed, the cat’s implementation runs.

This demonstrates an important aspect of JavaScript: **objects can be treated according to the behavior they provide rather than strictly according to their class or type**. This style is often associated with JavaScript’s dynamic and duck-typed nature.

#### **Method Overloading**

In languages such as Java and C++, **method overloading** allows multiple methods to have the same name but different parameter lists. For example, a language may allow separate `add()` methods for one argument, two arguments, or different parameter types.

JavaScript does **not support traditional method overloading** based on function signatures. If multiple methods with the same name are declared in the same class, the later declaration simply replaces the earlier one.

However, JavaScript can achieve **overloading-like behavior** by inspecting the arguments received by a single method. This is sometimes called **simulated method overloading**.

```jsx
class Calculator {
    add(a, b) {
        if (b === undefined) {
            return a + a;
        }

        return a + b;
    }
}

const calc = new Calculator();

console.log(calc.add(5));      // 10
console.log(calc.add(5, 10));  // 15
```

Here, there is only one `add()` method. When one argument is provided, the method doubles that value. When two arguments are provided, it adds the two values. Therefore, the same method behaves differently depending on the arguments supplied.

JavaScript can implement this kind of behavior using techniques such as **default parameters, optional parameters, type checking, or rest parameters**, but this is not true method overloading in the traditional sense.

#### **Method Overloading with Rest Parameters**

Rest parameters can also be used when the number of arguments is not fixed.

```jsx
class Calculator {
    add(...numbers) {
        return numbers.reduce((total, number) => total + number, 0);
    }
}

const calc = new Calculator();

console.log(calc.add(5));          // 5
console.log(calc.add(5, 10));      // 15
console.log(calc.add(5, 10, 20));  // 35
```

The same `add()` method can now handle different numbers of arguments. The method’s behavior adapts to the arguments supplied at runtime.

This demonstrates **overloading-like behavior**, rather than actual method overloading.

### **Polymorphism and Inheritance**

Polymorphism becomes particularly useful when combined with inheritance. A parent class can define a common interface or behavior, while child classes can provide specialized implementations.

For example, an application may have a general `Payment` class with a `pay()` method, while different payment types implement that method differently:

```jsx
class Payment {
    pay() {
        console.log("Processing payment");
    }
}

class CreditCardPayment extends Payment {
    pay() {
        console.log("Processing credit card payment");
    }
}

class UpiPayment extends Payment {
    pay() {
        console.log("Processing UPI payment");
    }
}
```

Now different payment objects can be handled using the same method call:

```jsx
const payments = [
    new CreditCardPayment(),
    new UpiPayment()
];

for (const payment of payments) {
    payment.pay();
}
```

The loop does not need separate logic for each payment type. Each object knows how to perform `pay()` according to its own implementation.

This makes the code easier to extend. If a new payment method is introduced, a new class can provide its own `pay()` implementation without requiring major changes to the code that processes payments.

### **Use Cases of Polymorphism**

Polymorphism makes programs more **flexible, extensible, and maintainable**. Code can be written against common behavior rather than being tightly coupled to a specific object type. This reduces the need for repeated conditional logic such as checking the object’s type and deciding what operation to perform.

Its major benefits include:

- **Flexibility:** The same code can work with different types of objects.
- **Extensibility:** New object types can be added with their own implementations without heavily modifying existing code.
- **Maintainability:** Common operations can be represented by the same method or interface.
- **Reduced conditional logic:** Instead of repeatedly checking object types, each object can define its own behavior.
- **Code reusability:** A common function or method can operate on multiple kinds of objects.

---

## **Static in JavaScript**

### **What is `static`?**

The `static` keyword is used to define a **static method or property that belongs to the class itself rather than to the objects (instances) created from that class**.

Normally, methods defined inside a class are available to its objects:

```jsx
class User {
    constructor(username) {
        this.username = username;
    }
    
    login() {
        return `${this.username} logged in`;
    }
    static createGuest() {
        return "Guest user created";
    }
}

const user = new User("Arjun");
console.log(user.login()); // Arjun logged in
console.log(User.createGuest()); // Error
```

Here, `login()` is an **instance method**, so it is accessed through the object, `user.login()`

If we make a method `static`, the situation changes, Now `createGuest()` belongs to the `User` class itself, It is **not available through an instance.**

The basic distinction is:

```
Instance method
      ↓
belongs to objects
      ↓
user.login()

Static method
      ↓
belongs to the class
      ↓
User.createGuest()
```

### **Why Do We Need Static Methods?**

A static method is useful when a piece of functionality is related to the class as a whole rather than to a particular object.

For example, suppose a `User` class needs a method that generates a unique ID. Generating an ID does not necessarily require information from a particular user object.

```jsx
class User {
    constructor(username) {
        this.username = username;
    }

    static generateId() {
        return Math.floor(Math.random() * 100000);
    }
}

const userOne = new User("Arjun");
console.log(User.generateId()); // Random number will be returned
console.log(userOne.generateId()); // Error
```

Notice that we call, `User.generateId()` rather than, `user.generateId()` . The reason is that `generateId()` belongs to the **class**, not to individual user instances.

### **Static Methods and `this`**

Inside a static method, `this` refers to the **class itself**, not an instance.

```jsx
class User {
    static userCount = 0;

    constructor(username) {
        this.username = username;
        this.constructor.userCount++;
    }

    getUsername() {
        return this.username;
    }

    static getUserCount() {
        return this.userCount;
    }
}

const userOne = new User("Arjun");
const userTwo = new User("Rahul");

console.log(userOne.getUsername());
console.log(userTwo.getUsername());

console.log(User.getUserCount());
```

In this example, there are **two different `this` values depending on where the code is running**. Inside the constructor, `this` refers to the newly created object. Therefore, when `new User("Arjun")` runs, `this.username = username` stores `"Arjun"` on `userOne`. The expression `this.constructor.userCount++` is slightly different: since `this` is the instance (`userOne` or `userTwo`), `this.constructor` refers to the class that created that instance, which is `User`. Therefore, `this.constructor.userCount++` is effectively accessing `User.userCount` and increasing it. When the static method `getUserCount()` is called using `User.getUserCount()`, `this` inside that static method refers directly to the `User` class itself, so `this.userCount` means `User.userCount`. Thus, both sides ultimately work with the same static property: the constructor uses `this.constructor.userCount` to update the class-level count, while the static method uses `this.userCount` to read that class-level count.

### **Static Properties**

The `static` keyword can also be used with properties.

```jsx
class User {
    static totalUsers = 0;

    constructor(username) {
        this.username = username;
        User.totalUsers++;
    }
}

const user1 = new User("Arjun");
const user2 = new User("Rahul");

console.log(User.totalUsers); // 2
```

Here, `totalUsers` is associated with the `User` class itself rather than with each individual user.

We access it using, `User.totalUsers`  not using `user1.totalUsers`

This makes static properties useful for storing information that should be **shared at the class level**, such as counters, configuration values, constants, or other class-wide information.

### **Static Members and Inheritance**

Static members also participate in class inheritance.

Suppose we have:

```jsx
class User {
    constructor(username) {
        this.username = username;
    }

    static generateId() {
        return "USER-" + Math.floor(Math.random() * 1000);
    }
}

class Admin extends User {
    constructor(username, permissions) {
        super(username);
        this.permissions = permissions;
    }
}

const admin = new Admin("Arjun", ["delete", "edit"]);

// Static method inherited by Admin
console.log(Admin.generateId());

// Instance method/property inherited from User
console.log(admin.username);

// Trying to access the static method through the instance
console.log(admin.generateId()); // TypeError
```

```jsx
OUTPUT

USER-742
Arjun
TypeError: admin.generateId is not a function
```

Here, `User` is the **parent class** and `Admin` is the **child class** because `Admin extends User`. This creates two related inheritance paths: the **instance side** and the **static side**. When `new Admin(...)` runs, `super(username)` calls the `User` constructor and initializes `this.username` on the newly created `admin` object. Therefore, `admin.username` works because the `Admin` instance inherits the instance-side behavior of `User`. The static method `generateId()`, however, does not belong to `admin`; it belongs to the `User` class itself. Because `Admin extends User`, the `Admin` class also inherits the static members of `User`, which is why `Admin.generateId()` works. But `admin.generateId()` fails because an instance does not inherit static members. In other words, **class inheritance allows `Admin` to inherit both the instance-side behavior and the static-side behavior of `User`, but those two sides remain separate.**