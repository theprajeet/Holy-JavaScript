# More About Objects

## **Advanced Object Property Details in JavaScript**

JavaScript objects contain more information about their properties than just a **key and value**. Every property also has certain internal characteristics that determine whether the property can be changed, whether it appears during iteration, and whether its configuration can be modified. These characteristics are called **property descriptors**.

For example, when we write:

```jsx
const chai = {
    name: "ginger chai",
    price: 250,
    isAvailable: true
};
```

we normally think of `name`, `price`, and `isAvailable` simply as properties containing values. Internally, however, JavaScript also maintains information about how each of these properties behaves.

### **Property Descriptors**

#### `Object.getOwnPropertyDescriptor()`

The `Object.getOwnPropertyDescriptor()` method allows us to inspect the descriptor of a particular own property of an object.

```jsx
const chai = {
    name: "ginger chai",
    price: 250,
    isAvailable: true
};

console.log(
    Object.getOwnPropertyDescriptor(chai, "name")
);

OUTPUT

{
    value: "ginger chai",
    writable: true,
    enumerable: true,
    configurable: true
}
```

The four important properties here are `value`, `writable`, `enumerable`, and `configurable`. `value` represents the actual value stored in the property. `writable` determines whether the property’s value can be changed. `enumerable` determines whether the property appears during property enumeration, such as with `Object.keys()` or `Object.entries()`. `configurable` determines whether the property’s descriptor can be changed or whether the property can be deleted.

These flags give JavaScript much finer control over object properties than simply using normal assignment.

---

### **`Object.defineProperty()`**

`Object.defineProperty()` allows us to **create or modify a property with specific descriptors**.

Its basic syntax is:

```jsx
Object.defineProperty(object, propertyName, descriptor);
```

For example:

```jsx
const chai = {
    name: "ginger chai",
    price: 250
};

Object.defineProperty(chai, "name", {
    writable: false,
    enumerable: true,
    configurable: true
});
```

Here, we are configuring the `name` property so that its value cannot be changed, while it can still participate in enumeration and its configuration can still be modified.

An important detail is that when `Object.defineProperty()` is used on an **existing property**, any descriptor fields that are not specified generally retain their existing values. When defining a **new property**, omitted descriptor flags such as `writable`, `enumerable`, and `configurable` default to `false`.

For example:

```jsx
Object.defineProperty(chai, "name", {
    enumerable: true
});
```

If `name` already existed, this changes its `enumerable` setting while leaving its other existing descriptor settings unchanged.

---

### **`writable` Property**

The `writable` descriptor determines whether the value of a property can be changed.

For example:

```jsx
const chai = {
    name: "ginger chai"
};

Object.defineProperty(chai, "name", {
    writable: false
});

chai.name = "masala chai";

console.log(chai.name);
```

Since `writable` is `false`, the value of `name` cannot be changed through normal assignment. The property still exists and can be read, but its value is effectively read-only.

#### Math.PI —> IMP

This is why a property such as `Math.PI` cannot normally be changed: 

```jsx
console.log(Math.PI);

Math.PI = 5;

console.log(Math.PI);

console.log(
    Object.getOwnPropertyDescriptor(Math, "PI")
);

OUTPUT

3.141592653589793
3.141592653589793

{
	value: 3.141592653589793,
	writable: false,
	enumerable: false,
	configurable: false
}
```

The value remains the same because the `PI` property is defined with `writable: false`.

We can inspect this using:

```jsx
console.log(
    Object.getOwnPropertyDescriptor(Math, "PI")
);
```

The descriptor for `Math.PI` has `writable: false`, which is what prevents its value from being overwritten.

---

### **`enumerable` Property**

The `enumerable` descriptor controls whether a property participates in enumeration. Enumeration means going through an object’s properties using mechanisms such as `Object.keys()`, `Object.values()`, `Object.entries()`, or a `for...in` loop.

For example:

```jsx
const chai = {
    name: "ginger chai",
    price: 250,
    isAvailable: true
};

Object.defineProperty(chai, "name", {
    enumerable: false
});

console.log(Object.entries(chai));

console.log(chai.name);

OUTPUT
[ [ 'price', 250 ], [ 'isAvailable', true ] ]
ginger chai
```

Here, `name` still exists and can be accessed directly:

```jsx
console.log(chai.name);
```

but it will not appear in `Object.entries(chai)` because its `enumerable` flag is `false`.

Therefore, **enumerability does not control whether a property exists or can be accessed; it controls whether the property participates in enumeration.** So, think of `enumerable: false` as **“don’t include this property when listing the object’s enumerable properties.”** It is similar to having a property that is still part of the object but is excluded when JavaScript performs certain property-listing operations. It does **not** mean “private,” “inaccessible,” or “deleted.”

---

### **`configurable` Property**

The `configurable` descriptor determines whether a property’s descriptor can be changed and whether the property can be deleted.

For example:

```jsx
const chai = {
name: "ginger chai"
};

Object.defineProperty(chai, "name", {
configurable: false
});

console.log([chai.name](http://chai.name/));
chai.name = "masala chai"
delete [chai.name](http://chai.name/);
console.log([chai.name](http://chai.name/));

Object.defineProperty(chai, "name", { 
    enumerable: false
}); // --> This will throw an error, when we reconfigure the defineProperty

OUTPUT
ginger chai
masala chai
TypeError: Cannot redefine property: name
```

Once a property is made non-configurable, its descriptor cannot freely be reconfigured later, and the property cannot be deleted using `delete`. The second important restriction is **reconfiguration**. Because `name` was made `configurable: false`, we cannot later freely change its descriptor. Therefore,  throws a `TypeError`, because we are trying to change the `enumerable` descriptor of a non-configurable property. However, `configurable: false` does **not** mean that the property’s value can never change. For example, `chai.name = "masala chai"`
still works and produces, `masala chai` provided the property remains writable.

So, `configurable: false` essentially **locks the property’s configuration**. The property cannot be deleted, and its descriptor cannot be freely changed later. But it can still be accessed normally, and its value can still be changed if `writable` is `true`. This is useful when an object needs to expose certain properties whose configuration should not be changed by other code.

But if the configurable was true then, which by default is set to true

```jsx
const chai = {
name: "ginger chai"
};

console.log([chai.name](http://chai.name/));

delete [chai.name](http://chai.name/);

console.log([chai.name](http://chai.name/));

OUTPUT
ginger chai
undefined
```

### **Property Descriptors and Object Iteration**

Property descriptors become particularly useful when dealing with objects received from external libraries, APIs, or other parts of an application. A property may exist on an object but not appear when you try to iterate over it because its `enumerable` flag is disabled.

Consider:

```jsx
const chai = {
    name: "ginger chai",
    price: 250,
    isAvailable: true,

    orderChai: function () {
        console.log("Chai is being prepared");
    }
};

for (let [key, value] of Object.entries(chai)) {
    if (typeof value !== "function") {
        console.log(`${key} : ${value}`);
    }
}

OUTPUT

name: ginger chai
price: 250
isAvailable : true
```

A normal JavaScript object is not directly iterable with a `for...of` loop because, unlike arrays, it does not provide the iteration mechanism that `for...of` expects. This means you cannot simply use `for...of` on an object to go through its properties one by one. `Object.entries()` solves this by taking the object’s own enumerable properties and converting them into a new array, where each property is represented as a key-value pair. For example, an object’s `name`, `price`, and `isAvailable` properties become entries such as `["name", "ginger chai"]`, `["price", 250]`, and `["isAvailable", true]`. Since the result of `Object.entries()` is an array, it is iterable, so a `for...of` loop can then go through each key-value pair. In other words, **`for...of` cannot directly iterate over a normal object, but `Object.entries()` converts the object’s properties into an iterable array of key-value pairs, allowing us to iterate over them.**

`Object.entries(chai)` returns the object’s **own enumerable string-keyed properties** as key-value pairs. The `typeof` check is then used to ignore the `orderChai` function, so only the data properties are processed. If we didn’t have the conditional check on the `orderChai` function then this would have been the output.

```jsx
name: ginger chai
price: 250
isAvailable : true
orderChai : function () {
	console.log("Chai is being prepared");
}
```

The important distinction is that `Object.entries()` does not simply return every property that exists on the object. A property must be **enumerable** to appear in its result.

---