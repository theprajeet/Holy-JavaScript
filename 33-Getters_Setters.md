# Getters, Setters

## **Getters and Setters in JavaScript**

**JavaScript getters and setters** are special accessor methods that allow us to control what happens when an object’s property is **read or modified**. With normal properties, values can be read and changed directly, which means we have no opportunity to validate or modify the value automatically.

For example, suppose we create a `Rectangle` class that expects `width` and `height` to be positive numbers:

```jsx
class Rectangle {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }
}

const rectangle = new Rectangle(-1000000, "pizza");

console.log(rectangle.width);
console.log(rectangle.height);
```

This code accepts `-1000000` as the width and `"pizza"` as the height even though neither represents a valid rectangle dimension. The same problem occurs when the object is updated later: `rectangle.width = -50` would directly replace the value.

This is where getters and setters become useful. A **setter** allows us to control and validate a value whenever it is assigned, while a **getter** allows us to control what value is returned whenever the property is accessed. This lets us add validation or other logic without changing how the property is used from outside:

```jsx
rectangle.width = 5;  // setter handles the assignment
console.log(rectangle.width);  // getter handles the access
```

Therefore, getters and setters are particularly useful when a property needs **custom behaviour during reading or writing**, such as validation, formatting, transformation, or calculating a value.

---

### **Getter (`get`)**

A getter is used to control what happens when a property is **accessed**. It is defined using the `get` keyword followed by the property name and a function body. The important point is that although a getter is defined using method-like syntax, it is accessed like a normal property, without parentheses.

A getter can simply return the stored value, but it can also perform additional logic before returning it. For example, a rectangle could return its width formatted with a unit:

```jsx
class Rectangle {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }
    get width() {
		    return `${this._width.toFixed(1)} cm`;
		}
}

const rectangle = new Rectangle(-1000000, "pizza");

console.log(rectangle.width);

OUTPUT

3.0 cm
```

When `rectangle.width` is evaluated, JavaScript automatically executes the `width` getter. The getter can read `_width`, format it, and return the resulting value. From outside the class, it still looks like an ordinary property access.  The important point is that **a getter controls reading**. It can return the stored value directly, modify it before returning it, or even calculate and return something that is not actually stored as a property.

---

### **Setter (`set`)**

A setter is used to control what happens when a value is **assigned** to a property. It is defined using the `set` keyword and receives the value being assigned as its parameter. This makes setters particularly useful for **validation**, because every assignment can pass through custom logic before the value is stored.

For example, a rectangle should not accept a negative width:

```jsx
class Rectangle {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }

		set width(newWidth) {
		    if (newWidth > 0) {
		        this._width = newWidth;
		    } else {
		        console.error("Width must be a positive number");
		    }
		}
}

const rectangle = new Rectangle(3, 4);

rectangle.width = 5;   // accepted
rectangle.width = -10; // rejected
```

The setter checks the value before storing it. If the value is invalid, the setter can reject it, display an error, transform it, or perform any other required logic. A setter is meant to handle an **assignment**, so it does not return a value to the caller.

Getters and setters **do not have to be used together**. A getter can be used by itself when you only need to control how a property is read, while a setter can be used by itself when you only need to control how a property is written. When both reading and writing require custom behaviour, they can be used together.

---

### **Why Use Different Variable Names for the Actual Value?**

When a getter or setter has the same name as the property being accessed, we need to be careful about where the **actual value is stored**. The getter/setter property and the actual stored value are generally given different names. A common convention is to prefix the internal property with `_`, such as `_width`, `_height`, `_email`, or `_password`.

The underscore indicates that the property is intended for **internal use** and that outside code should normally not access it directly. However, `_width` is **not genuinely private** in JavaScript. It is still possible to access `rectangle._width` from outside the class. The different name is mainly a convention and, importantly, it also prevents the getter or setter from recursively calling itself.

For example, suppose we try to write the `width` setter without using a separate property:

```jsx
class Rectangle {
    constructor(width) {
        this.width = width;
    }

    set width(value) {
        this.width = value;
    }
}

const rectangle = new Rectangle(3);
```

At first, `this.width = width` inside the constructor looks like it is simply assigning the value `3` to `width`. However, because `width` has a setter, assigning to `this.width` **invokes the `width` setter**.

Inside that setter, we again write:

```jsx
this.width = value;
```

But this is another assignment to `width`, so JavaScript invokes the **same setter again**. The setter again executes `this.width = value`, which invokes the setter again, and this continues indefinitely:

```
this.width = value
        ↓
width setter runs
        ↓
this.width = value
        ↓
width setter runs again
        ↓
this.width = value
        ↓
... continues indefinitely
```

Eventually, JavaScript cannot keep adding more function calls to the call stack and throws a **`RangeError: Maximum call stack size exceeded`** error.

```
ERROR!
/tmp/K6d0KIN9MB/main.js:7
this.width = value;
RangeError: Maximum call stack size exceeded
at set width (/tmp/K6dOKIN9MB/main.js:7:20)
at set width (/tmp/K6dOKIN9MB/main.js:7:20)
at set width (/tmp/K6dOKIN9MB/main.js:7:20)
at set width (/tmp/K6dOKIN9MB/main.js:7:20)
at set width (/tmp/K6dOKIN9MB/main.js:7:20)
at set width (/tmp/K6dOKIN9MB/main.js:7:20)
at set width (/tmp/K6dOKIN9MB/main.js:7:20)
at set width (/tmp/K6dOKIN9MB/main.js:7:20)
at set width (/tmp/K6d0KIN9MB/main.js:7:20)
at set width (/tmp/K6dOKIN9MB/main.js:7:20)
Node. js v22.21.1
```

The same problem can occur with a getter if it tries to read the same property that it is responsible for:

```jsx
get width() {
    return this.width;
}
```

When `rectangle.width` is accessed, the getter runs. Inside the getter, `this.width` is accessed again, which invokes the same getter again. That getter accesses `this.width` again, and the process repeats until the call stack is exhausted.

Therefore, we need a **separate property to store the actual value**. This is why `_width` is commonly used:

```jsx
class Rectangle {
    constructor(width) {
        this.width = width;
    }

    set width(value) {
        this._width = value;
    }

    get width() {
        return this._width;
    }
}

const rectangle = new Rectangle(3);

console.log(rectangle.width);
```

Now the process is different. When `this.width = width` executes in the constructor, it invokes the `width` setter. The setter then stores the value in `this._width`. Since `_width` does not have a `width` setter attached to it, assigning to `this._width` does **not** invoke the `width` setter again.

Similarly, when `rectangle.width` is accessed, JavaScript invokes the `width` getter. The getter reads `this._width`, which is the actual stored value, so it can return the value without invoking itself again.

Therefore, the roles are separated:

- **`width`** → the controlled property exposed to outside code; its getter/setter controls how the property is read or written.
- **`_width`** → the internal property where the actual value is stored.

The underscore in `_width` is only a **developer convention** indicating that the property is intended for internal use. It does **not** make `_width` genuinely private, so code outside the class can still access `rectangle._width`.

The same pattern applies to other properties:

```jsx
set email(value) {
    this._email = value;
}

get email() {
    return this._email;
}
```

Here, `email` is the **controlled/accessed property**, while `_email` is the **actual storage location**. This separation allows the getter and setter to perform their own logic without accidentally triggering themselves again.

#### **Modern Private Properties**

The `_property` naming convention should not be confused with JavaScript’s actual private class fields. A name such as `_email` is only a convention; outside code can still access `user._email` directly.

Modern JavaScript also provides private class fields using the `#` syntax:

```jsx
class User {
    #email;

    constructor(email) {
        this.#email = email;
    }

    get email() {
        return this.#email;
    }
}
```

Here `#email` is genuinely private and cannot be accessed directly from outside the class. This is different from `_email`, which merely communicates the developer’s intention that the property should be treated as an internal implementation detail.

---

## **Class-Based Getter/Setter Example**

A practical example is a `Rectangle` class where the setters validate the dimensions and the getters control how those dimensions are returned:

```jsx
class Rectangle {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }

    set width(newWidth) {
        if (newWidth > 0) {
            this._width = newWidth;
        } else {
            console.error("Width must be a positive number");
        }
    }

    set height(newHeight) {
        if (newHeight > 0) {
            this._height = newHeight;
        } else {
            console.error("Height must be a positive number");
        }
    }

    get width() {
        return `${this._width.toFixed(1)} cm`;
    }

    get height() {
        return `${this._height.toFixed(1)} cm`;
    }

    get area() {
        return `${(this._width * this._height).toFixed(1)} cm²`;
    }
}

const rectangle = new Rectangle(3, 4);

console.log(rectangle.width);   // 3.0 cm
console.log(rectangle.height);  // 4.0 cm
console.log(rectangle.area);    // 12.0 cm²

rectangle.width = 5;
rectangle.height = 6;

console.log(rectangle.width);   // 5.0 cm
console.log(rectangle.height);  // 6.0 cm

rectangle.width = -100;
rectangle.height = "pizza";
```

When `new Rectangle(3, 4)` executes, the constructor does not directly store the values in `_width` and `_height`. Instead, `this.width = width` and `this.height = height` perform property assignments, so the corresponding setters are automatically invoked. The setters validate the values and, because `3` and `4` are valid positive numbers, store them in `_width` and `_height`.

The same thing happens when the properties are changed later. `rectangle.width = 5` invokes the `width` setter, which validates `5` and stores it in `_width`. If we instead assign `rectangle.width = -100`, the setter rejects the value, so the previously stored valid width remains unchanged.

When `rectangle.width` is read, the `width` getter executes and formats the internally stored number using `toFixed(1)`. Therefore, the actual stored value can remain the number `3`, while the getter returns `"3.0 cm"` to the outside code. The getter can therefore **modify or format a value when it is read without changing the original stored value**.

The `area` getter demonstrates another important use: a getter can represent a **computed property**. There is no `area` value stored in the constructor, but `rectangle.area` works because the getter calculates `_width * _height` whenever the property is accessed. Since it behaves like a property, we write `rectangle.area`, not `rectangle.area()`.

---

### **Getters and Setters Are Special Property Accessors**

Getters and setters are best understood as **special property accessors**. They look like methods when they are defined, but from outside the class they behave like properties.

```jsx
rectangle.width;      // invokes the width getter
rectangle.width = 5;  // invokes the width setter
```

There is no need to write:

```jsx
rectangle.width();        // No need
rectangle.setWidth(5);    // No need
```

The `get` and `set` keywords tell JavaScript that these functions should be automatically executed when their corresponding property is accessed or assigned. This is what allows us to keep the simple property syntax while adding validation, formatting, transformation, or computation behind the scenes.

---

## **Function-Based Getter/Setter Example**

Getters and setters can also be implemented when using a **constructor function** instead of the `class` syntax. The underlying idea remains the same: define a property whose reading and writing are controlled by getter and setter functions.

```jsx
function User(email, password) {
    this._email = email;
    this._password = password;

    Object.defineProperty(this, "email", {
        get: function () {
            return this._email.toUpperCase();
        },
        set: function (value) {
            this._email = value;
        }
    });

    Object.defineProperty(this, "password", {
        get: function () {
            return this._password.toUpperCase();
        },
        set: function (value) {
            this._password = value;
        }
    });
}

const user = new User("user@example.com", "abc");

console.log(user.email);
console.log(user.password);
```

Here, `User` is a constructor function, and `new User()` creates the object. Instead of declaring `get` and `set` directly inside a class, we use `Object.defineProperty()` to define accessor properties on the newly created object. The behaviour is essentially the same as the class-based version.

---

### **`Object.defineProperty()`**

`Object.defineProperty()` is a method used to define or modify a property on an object using a **property descriptor**. Along with descriptors such as `writable`, `enumerable`, and `configurable`, a descriptor can contain `get` and `set` functions to create an accessor property.

Its basic structure is:

```jsx
Object.defineProperty(object, propertyName, {
    get: function () {
        // value returned when property is read
    },

    set: function (value) {
        // logic executed when property is assigned
    }
});
```

The first argument specifies the object on which the property should be defined, the second specifies the property name, and the third is the descriptor containing the property’s configuration and behaviour. This is the same property-descriptor mechanism discussed earlier; getters and setters are simply another capability provided by property descriptors.

---

## **Object-Based Getter/Setter**

Getters and setters are not limited to classes and constructor functions. They can also be defined directly inside an object literal.

```jsx
const User = {
    _email: "user@example.com",
    _password: "abc",

    get email() {
        return this._email.toUpperCase();
    },

    set email(value) {
        this._email = value;
    }
};

console.log(User.email);

User.email = "new@example.com";

console.log(User.email);
```

Here, `email` looks like a normal property when accessed, but the `get email()` definition causes the getter to execute whenever `User.email` is read. Similarly, assigning to `User.email` invokes the setter. The object itself therefore controls how its `email` property is read and modified.

The video also demonstrates that such an object can be used as a prototype:

```jsx
const User = {
    _email: "user@example.com",

    get email() {
        return this._email.toUpperCase();
    },

    set email(value) {
        this._email = value;
    }
};

const tea = Object.create(User);

console.log(tea.email);
```

`tea` does not have its own `email` property, but JavaScript searches its prototype when `tea.email` is accessed. It finds the getter on `User` and executes it with `tea` as `this`. This connects getters and setters directly with the prototype chain: **accessors can also be inherited like other properties and methods**.

---

## Getters and Setters in Real World Use Case

The main advantage of getters and setters is that they allow us to **keep a simple property interface while controlling what happens behind it**. A setter can validate input before storing it, which is useful for things such as ensuring a person’s age is non-negative, a rectangle’s dimensions are positive, or a name is a non-empty string. A getter can transform or format a value when it is read, or calculate a value dynamically.

For example, a `Person` class could reject invalid names and ages through setters while using getters to expose a full name:

```jsx
class Person {
    constructor(firstName, lastName, age) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
    }

    set firstName(value) {
        if (typeof value === "string" && value.length > 0) {
            this._firstName = value;
        } else {
            console.error("First name must be a non-empty string");
        }
    }

    set lastName(value) {
        if (typeof value === "string" && value.length > 0) {
            this._lastName = value;
        } else {
            console.error("Last name must be a non-empty string");
        }
    }

    set age(value) {
        if (typeof value === "number" && value >= 0) {
            this._age = value;
        } else {
            console.error("Age must be a non-negative number");
        }
    }

    get firstName() {
        return this._firstName;
    }

    get lastName() {
        return this._lastName;
    }

    get fullName() {
        return `${this._firstName} ${this._lastName}`;
    }

    get age() {
        return this._age;
    }
}
```

Now values are validated when they are assigned, while `fullName` can be accessed like a normal property even though it is **calculated rather than stored**. This is one of the most useful patterns with getters: they can make derived information feel like a normal property to the code using the object.

The same idea is used internally by JavaScript itself. For example, when we access an array’s `length` property, JavaScript can provide a value based on the array’s current state rather than treating `length` as an ordinary manually maintained property. Getters provide a mechanism for this kind of controlled property access.