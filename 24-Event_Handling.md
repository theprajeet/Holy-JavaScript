# Event Handling in DOM

## **1. JavaScript Events**

A **JavaScript event** is an action or occurrence that happens in the browser and can be detected by JavaScript. Events can be caused by the user, such as clicking a button, moving the mouse, pressing a key, entering data into a form, or focusing an input, or they can be caused by the browser itself, such as a page finishing its loading process.

For example, when a user clicks a button, a `click` event occurs. JavaScript can listen for that event and execute some code in response to it. This is what allows webpages to become interactive rather than remaining static HTML documents.

Some commonly used event types are **mouse events** such as `click`, `dblclick`, `mouseover`, `mouseout`, and `mousemove`. K**eyboard events** such as `keydown` and `keyup`; **form events** such as `submit`, `change`, `focus`, and `blur`; and **window events** such as `load`, `resize`, and `scroll`.

The important idea is that **an event itself is simply something that happens, event handling is how we make JavaScript respond to that event.**

---

## **2. Event Handling**

**Event handling** is the process of executing JavaScript code when a particular event occurs. The function that runs in response to an event is called an **event handler**.

There are several ways to attach event handlers to HTML elements. An event can be written directly inside HTML using an event attribute, such as:

```html
<button onclick="alert('Button clicked!')">Click Me</button>
```

JavaScript can also assign a handler through a DOM property:

```jsx
const button = document.querySelector("button");

button.onclick = () => {
    console.log("Button clicked");
};
```

However, the preferred approach is generally `addEventListener()`. 

### `addEventListener()`

`addEventListener()` is the standard DOM method used to attach an event listener to an element. It separates JavaScript from HTML and allows multiple listeners to be attached to the same element for the same event.

The basic syntax is:

```jsx
element.addEventListener("event", handler);
```

```jsx
const button = document.querySelector("#btn");

button.addEventListener("click", function () {
    console.log("Button was clicked");
});
```

Here, `"click"` is the event type and the function is the event handler that should execute when that event occurs.

`addEventListener()` is particularly useful because the same element can have multiple listeners:

```jsx
button.addEventListener("click", () => {
    console.log("First handler");
});

button.addEventListener("click", () => {
    console.log("Second handler");
});

// Both handlers can execute when the button is clicked.
```

Example

```jsx
HTML
<p id="message">Hello World</p>
<button id="btn">Click Me</button>

JS
const button = document.querySelector("#btn");
const message = document.querySelector("#message");

button.addEventListener("click", function () {
	message.textContent = "Button was clicked!";
});

```

---

## **3. Event Object**

When an event occurs, the browser automatically creates an **event object** containing information about that particular event. We can access this object by accepting a parameter in the event handler function. The event object can provide information such as what type of event occurred, which element triggered it, mouse or keyboard information, and other details depending on the event and is commonly represented by `event` or `e`.

```jsx
button.addEventListener("click", (e) => {
    console.log(e);
});
```

```jsx
HTML
<button id="btn">Click Me</button>

JS
const button = document.querySelector("#btn");
button.addEventListener("click", function (event) {
	console.log(event);
});

```

Here, when the button is clicked, the browser creates an event object and automatically passes it to the event handler. We receive that object through the `event` parameter. We can use this object to get information about the event:

```jsx
button.addEventListener("click", function (event) {
    console.log(event.type); 
});

OUTPUT - click
```

### **`event.target`**

`event.target` is a property of the event object that tells us **which element actually triggered the event**. For example, if a button is clicked, `event.target` refers to that button.

For example, if a button is clicked inside a `<div>`, then even if the event is eventually handled by the `<div>`, `event.target` can still tell us that the button was the element that was actually clicked.

```jsx
HTML
<button id="btn">Click Me</button>

JS
const button = document.querySelector("#btn");

button.addEventListener("click", function (event) {
	console.log(event.target);
});

// We can also modify it directly, since it gives us the actual element

button.addEventListener("click", function (event) {
    event.target.textContent = "Clicked!";
});
```

### `event.preventDefault()`

Some HTML elements have a **default browser behavior** associated with them. For example, clicking an `<a>` element normally navigates the browser to the URL specified in its `href`, and submitting a form normally sends the form data. `event.preventDefault()` prevents that default browser action. 

```jsx
HTML
<a href="https://example.com" id="link">Visit Website</a>

JS
const link = document.querySelector("#link");

link.addEventListener("click", function (event) {
	event.preventDefault();
	console.log("Link was clicked, but navigation was prevented.");
);

```

---

## 4. Event **Propagation**

**Event propagation** describes how an event travels through the DOM when it occurs. When an element is inside another element, an event does not necessarily remain limited to the element that was interacted with. It can travel through its ancestors / parent as part of the event propagation process. This allows parent elements to respond to events that originally occurred on their child elements.

Consider this structure:

```html
<div id="parent">
    <button id="child">Click Me</button>
</div>

<script>
    const parent = document.querySelector("#parent");
    const child = document.querySelector("#child");

    child.addEventListener("click", function () {
        console.log("Button clicked");
    });

    parent.addEventListener("click", function () {
        console.log("Div clicked");
    });
</script>
```

When the button is clicked, both event listeners run. The output will normally be:

```jsx
Button clicked
Div clicked
```

This happens because the click event occurs on the `<button>` first and then **propagates upward** to its parent `<div>`.

```jsx
Button is clicked
      ↓
Button's event listener runs
      ↓
Event moves to the parent
      ↓
Parent's event listener runs
```

If the button is clicked, the event occurs on the button, but it can also reach its parent `<div>`. This upward movement of an event is called **event bubbling**.

---

## **5. Event Bubbling**

**Event bubbling** is the default event propagation behaviour for most DOM events. In bubbling, the event starts at the **target element** where the event occurred and then travels upward through its parent elements and their ancestors.

For example:

```html
<div id="grandparent">
    <div id="parent">
        <button id="child">Click Me</button>
    </div>
</div>
```

```jsx
const grandparent = document.querySelector("#grandparent");
const parent = document.querySelector("#parent");
const child = document.querySelector("#child");

child.addEventListener("click", function () {
    console.log("Child");
});

parent.addEventListener("click", function () {
    console.log("Parent");
});

grandparent.addEventListener("click", function () {
    console.log("Grandparent");
});
```

Clicking the button produces:

```jsx
Child
Parent
Grandparent
```

The event starts at the <button> and bubbles upward through its parent elements.

---

## 6. `stopPropagation()`

Sometimes we don’t want an event to continue propagating to its parent elements. In that situation, we can use `event.stopPropagation()`.

```html
<div id="parent">
    <button id="child">Click Me</button>
</div>

<script>
    const parent = document.querySelector("#parent");
    const child = document.querySelector("#child");

    child.addEventListener("click", function (event) {
        console.log("Button clicked");

        event.stopPropagation();
    });

    parent.addEventListener("click", function () {
        console.log("Div clicked");
    });
</script>
```

Now, when the button is clicked, the output is only:

```html
Button clicked
```

It is important to distinguish this from `preventDefault()`

For example, `preventDefault()` can stop a link from navigating, while `stopPropagation()` can stop a click on a button from reaching its parent.

```html
event.preventDefault()
→ prevents the browser's default action

event.stopPropagation()
→ prevents the event from propagating to other elements
```

---

## **7. Event Delegation**

**Event delegation** is a technique where we attach a single event listener to a **parent element** instead of attaching separate event listeners to each of its child elements. It works because of **event bubbling**: when an event occurs on a child, the event bubbles upward to its parent, allowing the parent to handle the event. This is especially useful when we have many similar child elements or when child elements may be added dynamically later.

Example

```html
<ul id="languages">
    <li>JavaScript</li>
    <li>Python</li>
    <li>Java</li>
</ul>

<script>
    const languages = document.querySelector("#languages");

    languages.addEventListener("click", function (event) {
        console.log(event.target.textContent);
    });
</script>
```

Here, we have three `<li>` elements, but we have attached **only one event listener**, and it is attached to the `<ul>` rather than to each `<li>`.

When we click `"Python"`, the click event first occurs on the `<li>` and then bubbles up to the `<ul>`. The `<ul>`’s event listener runs, and `event.target` tells us that the `<li>` containing `"Python"` was the element that originally triggered the event.

Without event delegation, we could attach a listener to every `<li>` individually:

```jsx
const languages = document.querySelectorAll("#languages li");

languages.forEach(function (language) {
    language.addEventListener("click", function () {
        console.log(this.textContent);
    });
});
```

This works, but if the list contains many elements, we would have many event listeners. With event delegation, we can use **one listener on the parent** to handle events from all the children.

Another important advantage is that event delegation works well with **dynamically added elements**. If we later add another `<li>` to the list, we don’t necessarily need to attach another event listener to that new element because clicks on it can still bubble to the existing `<ul>` listener.

So, in simple terms:

**Event delegation → attach one event listener to a parent and use event bubbling to handle events from its children.**

---

## **8. Event Capturing**

**Event capturing** is another phase of event propagation. Unlike bubbling, where the event travels from the target toward its ancestors, capturing moves from the outermost ancestor toward the target.

For example, if we have:

```
div1
  ↓
div2
  ↓
div3 ← target
```

during capturing, the event travels:

```
div1 → div2 → div3
```

So capturing moves **from parent toward child**, while bubbling moves **from child toward parent**.

By default, event listeners are generally registered for the bubbling phase. To make a listener run during the capturing phase, we pass `true` as the third argument to `addEventListener()`:

```jsx
div1.addEventListener("click", handler, true);
```

The `true` value tells the browser to handle this listener during the **capturing phase**.

Example

```html
<div id="grandparent">
    <div id="parent">
        <button id="child">Click Me</button>
    </div>
</div>

<script>
    const grandparent = document.querySelector("#grandparent");
    const parent = document.querySelector("#parent");
    const child = document.querySelector("#child");

    grandparent.addEventListener("click", function () {
        console.log("Grandparent");
    }, true);

    parent.addEventListener("click", function () {
        console.log("Parent");
    }, true);

    child.addEventListener("click", function () {
        console.log("Child");
    }, true);
</script>
```

When we click the button, the event doesn’t immediately execute the button’s listener. During the **capturing phase**, the event travels from the outermost ancestor toward the target element. So when the button is clicked, the event first **travels downward** from 
`Grandparent → Parent → Child` during capturing. After reaching the target, it can then **travel upward** from `Child → Parent → Grandparent` during bubbling.

The easiest way to remember the two behaviours is:

```
Capturing:  Parent → Child → Target

Bubbling:   Target → Child → Parent
```

Capturing is useful when we need to handle or intercept an event **before it reaches the target element**, whereas bubbling is the normal behaviours used in many everyday event-handling patterns, including event delegation.

---