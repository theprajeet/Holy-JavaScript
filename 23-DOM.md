# DOM

## **DOM (Document Object Model)**

The **DOM (Document Object Model)** is a programming interface provided by the browser that represents an HTML document as a structured, tree-like collection of objects. It allows JavaScript to **access, read, modify, add, and remove HTML elements, attributes, styles, and content** of a webpage. When the browser loads an HTML document, it parses the HTML and creates a DOM representation of that document. JavaScript can then interact with this representation instead of directly manipulating the HTML file.

For example, if the HTML contains `<h1>Hello</h1>`, the browser creates a corresponding DOM element for the `<h1>` and JavaScript can access and modify it.

### **DOM Features**

The DOM allows JavaScript to:

- Modify HTML elements and their content.
- Change CSS styles and classes.
- Add, remove, or replace HTML elements.
- Access attributes such as `id`, `class`, `src`, `href`, etc.
- Handle events such as clicks, keyboard input, form submission, etc.
- Create HTML elements dynamically.
- Traverse the document through parent-child and sibling relationships.

### **DOM as a Tree Structure**

The DOM represents an HTML document as a **tree structure**. Each part of the document is represented as a node, and nodes have relationships such as **parent, child, and sibling**.

<img src="/Assets/DOM-1.png" alt="DOM.png" width="600" >

Consider:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>My Page</title>
</head>

<body>
    <h1 id="main-heading">Welcome</h1>
    <p>This is paragraph</p>
</body>
</html>
```

The DOM can conceptually be represented as:

```
Window
|
Document
└── html
    ├── head
    │   ├── meta
    │   │   └── charset = "UTF-8"
    │   └── title
    │       └── "My Page"
    │
    └── body
        ├── h1
        │   ├── id = "main-heading"
        │   └── "Welcome"
        │
        └── p
            └── "This is paragraph"
```

Here, `document` is the root of the DOM tree. The `<html>` element is its child, and `<head>` and `<body>` are children of `<html>`.

### **DOM and `window`**

The browser provides a global object called **`window`**. It represents the browser window/environment in which the webpage is running.

The `window` object provides access to many browser-related objects and features, including `document`, `location`, `history`, `navigator`, and `screen`.

A simplified relationship is:

```
Window
├── document
├── history
├── location
├── navigator
└── screen
```

The `document` object represents the current webpage and is the main object used when working with the DOM.

---

### **Nodes in the DOM**

Everything in the DOM is represented as a **node**. Common types include element nodes, text nodes, and attribute-related information.

For example:

```html
<h1 id="title">Welcome</h1>
```

The `<h1>` is an **element**, `id="title"` is an attribute, and `"Welcome"` is the text content.

A simplified representation is:

```
h1
├── id = "title"
└── "Welcome"
```

The DOM therefore does not simply represent HTML tags; it represents the document and its different pieces as objects/nodes that JavaScript can work with.

#### **Text Nodes**

A **text node** represents the actual text inside an HTML element.

For example:

```html
<title>My Page</title>
```

Here, `<title>` is an element node and `"My Page"` is its text content, represented in the DOM as a text node.

Similarly:

```html
<h1>Welcome</h1>
```

can conceptually be represented as:

```
h1
└── "Welcome"
```

#### **Attributes in the DOM**

HTML elements can contain attributes, and those attributes are represented as information associated with the corresponding element.

For example:

```html
<h1 id="main-heading" class="heading">Welcome</h1>
```

The `<h1>` element has two attributes:

```
id = "main-heading"
class = "heading"
```

In a DOM tree diagram, attributes can be shown alongside the element:

```
h1
├── id = "main-heading"
├── class = "heading"
└── "Welcome"
```

Attributes are useful for identifying and manipulating specific elements.

---

## **BOM (Browser Object Model)**

**Browser Object Model (BOM)** is a browser-specific convention referring to all the objects exposed by the web browser. The BOM allows JavaScript to “interact with” the browser. The window object represents a browser window and all its corresponding features. For example, JavaScript can work with the browser window, browser history, current URL/location, browser information, and screen information.

The `document` object is specifically the **DOM’s main entry point**, while objects such as `location`, `history`, `navigator`, and `screen` are generally discussed under the BOM.

Important browser-related objects include:

`window` — represents the browser window and acts as the global object.

- `navigator` — provides information about the browser/environment.
- `screen` — provides information about the user’s screen.
- `location` — provides information about the current URL and can also be used for navigation.
- `history` — provides access to the browser’s session history.

<img src="/Assets/BOM.png" alt="BOM.png" width="600" >

### **DOM vs BOM**

**DOM** deals primarily with the **webpage/document**, whereas **BOM** deals primarily with the **browser environment**.

For example, changing an `<h1>` belongs to DOM manipulation, while accessing the current URL through `location` or browser history through `history` belongs to browser-level interaction.

```
DOM → webpage/document
BOM → browser/window environment
```

<img src="/Assets/DOM-BOM.png" alt="BOM.png" width="600" >

---

## DOM Selectors

DOM selectors are methods provided by JavaScript’s `document` object that allow us to **find HTML elements in the webpage**. Once an element is selected, JavaScript can read its content, change its attributes, modify its CSS, or perform other DOM operations on it. Different selector methods return different things: some return **one element**, while others return a **collection of elements** such as a `NodeList` or `HTMLCollection`.

### **1. `document.getElementById()`**

`getElementById()` is used when we want to select an element using its `id` attribute. Since an `id` is supposed to uniquely identify an element on a page, this method returns **one element**, not a collection. Its syntax is `document.getElementById("id")`.

For example, if we have `<h1 id="title">DOM Learning</h1>`, we can write 
`const title = document.getElementById("title");`. Now `title` directly refers to the `<h1>` element, so we can access its properties directly, such as `title.innerText`, `title.innerHTML`, or `title.style.color`.

An important point is that you **do not use `#`** when using `getElementById()`. The `#` is used with CSS selectors such as `querySelector("#title")`, whereas `getElementById()` simply receives the ID name.

```jsx
const title = document.getElementById("title");

console.log(title);
console.log(title.innerText);
```

If no element with that ID exists, `getElementById()` returns `null`.

---

### **2. `document.getElementsByClassName()`**

`getElementsByClassName()` is used to select elements based on their `class` attribute. Unlike `getElementById()`, multiple elements can have the same class, so this method returns **multiple elements i.e a HTML Collection**.

For example, if we have four elements with `class="list-item"`, then `document.getElementsByClassName("list-item")` returns an **HTMLCollection** containing those four elements.

```jsx
const items = document.getElementsByClassName("list-item");

console.log(items);
console.log(items[0]);
console.log(items.length);
```

An `HTMLCollection` behaves somewhat like an array because its elements can be accessed using indexes such as `items[0]`, `items[1]`, etc., and it has a `length` property. However, it is **not a normal JavaScript Array**.

This distinction is important when manipulating the elements. `items.style.color = "green"` will not work because `items` represents the entire HTMLCollection, not one particular `<li>`. You have to select an individual element, for example `items[0].style.color = "green"`.

Another important nuance is that you cannot directly use `.forEach()` on the HTMLCollection in the demonstrated environment. If you want to use Array methods such as `forEach()`, you can convert the collection into an actual Array using `Array.from(items)`.

```jsx
Array.from(items).forEach(function(item) {
    item.style.color = "green";
});
```

One more important characteristic of HTMLCollections is that they are generally **live collections**. This means that if the DOM changes and an element is added to or removed from the matching elements, the HTMLCollection can automatically reflect that change.

---

### **3. `document.getElementsByTagName()`**

`getElementsByTagName()` selects elements based on their **HTML tag name**. For example, `"h1"` selects `<h1>` elements, `"p"` selects `<p>` elements, and `"li"` selects `<li>` elements.

```jsx
const headings = document.getElementsByTagName("h2");

console.log(headings);
console.log(headings[0]);
```

Just like `getElementsByClassName()`, this method returns an **HTMLCollection** when there are matching elements. Therefore, you can use indexes and `length`, but you cannot treat the returned collection as a single element.

For example, if the page contains three `<h2>` elements, `document.getElementsByTagName("h2")` gives an HTMLCollection containing all three. To change only the first one, you would use `headings[0].style.color = "green"`.

The main difference between the two methods is simply **what they search for**: `getElementsByClassName("list-item")` searches by class, while `getElementsByTagName("li")` searches by tag name. Both return an HTMLCollection and therefore have the same collection-related considerations.

---

### **4. `document.querySelector()`**

`document.querySelector()` is a DOM method used to select an element from the document using a **CSS selector**. It searches the DOM and returns the **first element that matches the specified selector**. If multiple elements match the selector, only the first matching element is returned. The returned value is an **Element object**, which means you can directly access or modify its properties, styles, attributes, and content. If no element matches the selector, the method returns `null`.

For example, if the document contains multiple elements with the class `.text`, `querySelector()` returns only the first one.

```jsx
HTML
<h2>This is a Heading-2</h2>
<p class="text">Hello</p>
<p class="text">World</p>
```

```jsx
JS
const element = document.querySelector(".text");
console.log(element); // <p class="text">Hello</p>
```

This is why the value returned by `querySelector()` can directly use element properties. If no element matches the selector, `querySelector()` returns `null`.

```jsx
const heading = document.querySelector("h2");

heading.style.color = "green";
heading.innerText = "Hello";
```

A useful distinction to remember is that `querySelector()` uses **CSS selector syntax**, whereas methods such as `getElementById()` and `getElementsByClassName()` have their own specific arguments.

For example:

```jsx
document.getElementById("title");       // ID name
document.getElementsByClassName("box"); // class name

document.**querySelector**("#title");  <--  // CSS ID selector
document.**querySelector**(".box");    <--  // CSS class selector
```

---

### **5. `document.querySelectorAll()`**

`document.querySelectorAll()` is used to select **all elements** in the document that match a specified **CSS selector**. Unlike `querySelector()`, which returns only the first matching element, `querySelectorAll()` returns a **NodeList** containing all matching elements. The returned `NodeList` can be accessed using indexes and its `length` property can be used to determine the number of matching elements. If no elements match the selector, it returns an **empty NodeList**, rather than `null`.

For example, `document.querySelectorAll("h2")` selects every `<h2>` element on the page.

Unlike `getElementsByClassName()` and `getElementsByTagName()`, `querySelectorAll()` returns a **NodeList**, not an HTMLCollection.

```jsx
const headings = document.querySelectorAll("h2");

console.log(headings);
console.log(headings.length);
console.log(headings[0]);
```

A NodeList also allows you to access individual elements using indexes. Therefore, `headings[0]` gives you the first actual `<h2>` element.

This explains why something like `headings.style.color = "green"` doesn’t work. `headings` is the NodeList itself, while `.style` belongs to the individual HTML element. You could instead write `headings[0].style.color = "green"`.

One of the useful features of a NodeList returned by `querySelectorAll()` is that it supports `.forEach()`. This allows us to perform an operation on every selected element:

```jsx
const headings = document.querySelectorAll("h2");

headings.forEach(function(h) {
    h.style.color = "red";
});
```

Here, `headings` is the NodeList, while `h` represents **one individual `<h2>` element at a time**.

---

### **NodeList vs HTMLCollection**

The two collections can look very similar because both contain multiple DOM elements, but they are not the same thing.

`querySelectorAll()` returns a **NodeList**, while `getElementsByClassName()` and `getElementsByTagName()` return an **HTMLCollection**. Both support indexing, such as `[0]`, and both have a `length` property. However, the NodeList returned by `querySelectorAll()` supports `.forEach()`, whereas the HTMLCollection does not. 

HTMLCollections are generally live, while the NodeList returned by `querySelectorAll()` is static. When we say an `HTMLCollection` is **live**, it means that the collection automatically reflects changes made to the DOM. For example, if you use `getElementsByClassName()` and initially there are two elements with the class `"text"`, the returned `HTMLCollection` contains those two elements. If you later add another element with the same class to the DOM, the **same collection automatically updates** and now contains three elements. Similarly, if one of those elements is removed, the collection automatically reflects that removal. You don’t need to call `getElementsByClassName()` again.

On the other hand, the `NodeList` returned by `querySelectorAll()` is **static**, meaning it represents the elements that matched the selector **at the moment `querySelectorAll()` was called**. If you initially have two `.text` elements and run `querySelectorAll(".text")`, the returned `NodeList` contains those two elements. If you later add another `.text` element to the DOM, the existing `NodeList` **does not automatically include it**. To get the updated set of elements, you need to call `querySelectorAll()` again.

So, in simple terms, **a live collection stays connected to the current DOM and automatically updates when the DOM changes, whereas a static collection is like a snapshot of the matching elements at the time you performed the query.** 
Remember: 
`getElementsByClassName()` → **live `HTMLCollection`**, while 
`querySelectorAll()` → **static `NodeList`**.

| **Selector** | **Searches by** | **Returns** |
| --- | --- | --- |
| `getElementById()` | ID | Single Element |
| `getElementsByClassName()` | Class | HTMLCollection |
| `getElementsByTagName()` | Tag | HTMLCollection |
| `querySelector()` | CSS selector | First matching Element |
| `querySelectorAll()` | CSS selector | NodeList |

---

## **Changing Element Attributes**

HTML elements contain **attributes** that provide additional information about the element. Examples include `id`, `class`, `src`, `href`, `type`, `name`, `placeholder`, and `disabled`.

For example:

```html
<input type="password" id="password" name="userPassword">
```

Here, `type`, `id`, and `name` are attributes of the `<input>` element.

JavaScript provides `setAttribute()` and `getAttribute()` for working with these attributes.

### **1. `setAttribute()`**

`setAttribute()` is used to **add a new attribute or change an existing attribute**.

Its syntax is:

```jsx
element.setAttribute("attribute", "value");
```

For example:

```jsx
const input = document.querySelector("input");

input.setAttribute("type", "text");
```

If the input was previously a "password” field, its `type` attribute is now changed to `"text"`.

You can also add an attribute that didn’t previously exist:

```jsx
input.setAttribute("placeholder", "Enter your password");
```

The important thing to understand is that `setAttribute()` works on the **actual element**, not on a NodeList or HTMLCollection. Therefore, you first select the element and then call `setAttribute()` on it.

Example

```jsx
HTML
<p class="text">Hello</p>
<p class="text">World</p>
<p class="text">Welcome</p>

<div class="box">Box 1</div>
<div class="box">Box 2</div>

JS
// querySelectorAll() returns a NodeList
const textElements = document.querySelectorAll(".text");

// textElements is a NodeList containing 3 elements
// NodeList(3) [p.text, p.text, p.text]

// Select the first element from the NodeList
const firstText = textElements[0];

// Now we have an actual Element, so setAttribute() can be used
firstText.setAttribute("id", "first-text");

// getElementsByClassName() returns an HTMLCollection
const boxes = document.getElementsByClassName("box");

// boxes is an HTMLCollection containing 2 elements
// HTMLCollection(2) [div.box, div.box]

// Select the second element from the HTMLCollection
const secondBox = boxes[1];

// Now we have an actual Element, so setAttribute() can be used
secondBox.setAttribute("id", "second-box");
```

---

### **2.  `getAttribute()`**

`getAttribute()` is used to **read or retrieve the value of an attribute** from an HTML element. Just like `setAttribute()`, it works on an **individual Element**, not directly on a `NodeList` or an `HTMLCollection`. Therefore, if a selector returns multiple elements, you first select the required element using its index and then call `getAttribute()` on that element. The method returns the attribute’s value as a string. If the specified attribute does not exist on the element, it returns `null`.

Its syntax is:

```jsx
element.getAttribute("attribute");
```

For example:

```jsx
const input = document.querySelector("input");

input.getAttribute("type");
```

If the input has `type="password"`, this returns:

```
"password"
```

Similarly, `input.getAttribute("id")` returns the value of its `id` attribute.

Example

```jsx
HTML
<p class="text" id="first-text">Hello</p>
<p class="text" id="second-text">World</p>
<p class="text" id="third-text">Welcome</p>

<div class="box" id="box-1">Box 1</div>
<div class="box" id="box-2">Box 2</div>

JS
// querySelectorAll() returns a NodeList
const textElements = document.querySelectorAll(".text");

// Select the first element from the NodeList
const firstText = textElements[0];

// getAttribute() reads the value of an attribute
const idValue = firstText.getAttribute("id");

console.log(idValue); // "first-text"

// getElementsByClassName() returns an HTMLCollection
const boxes = document.getElementsByClassName("box");

// Select the second element from the HTMLCollection
const secondBox = boxes[1];

// Read the value of its id attribute
const boxId = secondBox.getAttribute("id");

console.log(boxId); // "box-2"

// If the attribute does not exist
const classValue = secondBox.getAttribute("title");

console.log(classValue); // null
```

---

### **CSS Manipulation**

Once we have selected an individual DOM element, we can manipulate its CSS through the element’s `.style` property. This changes the element’s **inline styles**.

For example:

```jsx
const title = document.querySelector("#title");

title.style.color = "green";
title.style.backgroundColor = "green";
title.style.padding = "15px";
title.style.borderRadius = "15px";
```

An important syntax difference appears here because CSS property names containing hyphens are written in **camelCase** in JavaScript. Therefore, CSS `background-color` becomes `backgroundColor`, `border-radius` becomes `borderRadius`, and so on.

The `.style` property belongs to an **individual element**, which is why this works:

```jsx
title.style.color = "green";
```

but this does not:

```jsx
const headings = document.querySelectorAll("h2");

headings.style.color = "green"; // Doesn't work
```

`headings` is a NodeList. You need to select an individual element:

```jsx
headings[0].style.color = "green";
```

or iterate through the collection:

```jsx
headings.forEach(function(h) {
    h.style.color = "green";
});
```

This same idea applies to HTMLCollections: select an individual element using its index or convert the collection to an Array and iterate over it.

---

## **DOM Element Relationships and Traversing the DOM**

The DOM represents an HTML document as a **tree structure**, where elements have relationships with each other. An element can be a **parent**, **child**, or **sibling** of another element. JavaScript provides several properties that allow us to move through this tree after we have selected an element.

For example, consider this structure:

```html
<div class="parent">
    <div class="day">Monday</div>
    <div class="day">Tuesday</div>
    <div class="day">Wednesday</div>
    <div class="day">Thursday</div>
</div>
```

Here, `.parent` is the **parent** of all four `.day` elements, while Monday, Tuesday, Wednesday, and Thursday are its **child elements**. Monday and Tuesday are **siblings** because they have the same parent.

#### **`children`**

The `children` property gives us the **HTML elements that are direct children** of a particular element.

```jsx
const parent = document.querySelector(".parent");

console.log(parent.children);
console.log(parent.children.length);
console.log(parent.children[1]);
console.log(parent.children[1].innerHTML);
```

`parent.children` returns an **HTMLCollection** containing the four `<div class="day">` elements. Since HTMLCollection is indexed, we can access individual elements using `[index]`. The index starts from `0`, so `parent.children[0]` is Monday, `parent.children[1]` is Tuesday, and so on.

`children.length` tells us how many **element children** the parent has.

`children` is also a **live collection**, meaning changes to the DOM can be reflected in the collection automatically.

#### **Accessing and looping through children**

Since `children` is indexed, we can use a normal loop to access every child.

```jsx
const parent = document.querySelector(".parent");

for (let i = 0; i < parent.children.length; i++) {
    console.log(parent.children[i].innerHTML);
}
```

This prints:

```
Monday
Tuesday
Wednesday
Thursday
```

We can also directly manipulate a particular child because `parent.children[index]` gives us an actual element.

```jsx
parent.children[1].style.color = "orange";
```

Here, `parent.children[1]` selects the Tuesday `<div>`, and then its text color is changed.

---

#### **`firstElementChild` and `lastElementChild`**

Instead of using indexes when we specifically need the first or last child, we can use `firstElementChild` and `lastElementChild`.

```jsx
const parent = document.querySelector(".parent");

console.log(parent.firstElementChild);
console.log(parent.lastElementChild);
```

`firstElementChild` returns the first **element child**, while `lastElementChild` returns the last **element child**.

So in our example, `firstElementChild` gives Monday and `lastElementChild` gives Thursday.

---

#### **`parentElement`**

We can also move **upwards** in the DOM tree. If we already have a child element and want to find its parent, we can use `parentElement`.

```jsx
const dayOne = document.querySelector(".day");

console.log(dayOne.parentElement);
```

Since `dayOne` represents Monday, `dayOne.parentElement` returns the `.parent` `<div>`.

This is useful because once we have a reference to an element, we don’t necessarily need to run another `querySelector()` to find its parent.

---

#### **`nextElementSibling`**

`nextElementSibling` allows us to move from an element to the **next sibling element**.

```jsx
const dayOne = document.querySelector(".day");

console.log(dayOne.nextElementSibling);
```

`querySelector(".day")` selects the first `.day`, which is Monday. Therefore, `nextElementSibling` gives us Tuesday.

Similarly, from Tuesday, `nextElementSibling` would give Wednesday.

The important point is that `nextElementSibling` moves to the **next element**, not simply the next DOM node.

---

#### **`childNodes` and NodeList**

The `children` property gives us only HTML elements, but `childNodes` works differently.

```jsx
const parent = document.querySelector(".parent");

console.log(parent.childNodes);
```

`childNodes` returns a **NodeList** containing all direct child nodes, not just HTML elements. This can include element nodes, text nodes, comment nodes

For example, the spaces and line breaks between our `<div>` elements can become **text nodes** in the DOM.

So this:

```html
<div class="parent">
    <div class="day">Monday</div>
    <div class="day">Tuesday</div>
</div>
```

does not necessarily produce only two nodes inside `.parent`. The whitespace/newlines between the elements can also appear as text nodes.

This is why you might see something like:

```
NodeList(...)
0: text
1: div.day
2: text
3: div.day
4: text
```

The exact number depends on the whitespace and other content present in the HTML.

---

## Creating, Editing, Removing HTML Elements using DOM

### 1. Creating HTML Elements Using DOM

JavaScript can be used to create new HTML elements dynamically and add them to the DOM. This is useful when the content of a webpage needs to be generated or modified while the application is running, such as creating list items from user input or displaying data received from an API. The main method used to create a new HTML element is `document.createElement()`.

#### **`document.createElement()`**

`document.createElement()` creates a new HTML element and returns a reference to that element. However, creating an element does not automatically add it to the webpage. The element initially exists only in memory until it is attached to the DOM.

```jsx
HTML
<ul class="language">
	<li>Javascript</li>
</ul>

JS
const div = document.createElement("div");

console.log(div);
```

The same method can be used to create different types of HTML elements by passing the element’s tag name as an argument. At this point, these elements have been created, but they are not yet visible on the webpage because they have not been attached to the document.

```jsx
document.createElement("div");
document.createElement("p");
document.createElement("h1");
document.createElement("li");
```

#### **Setting Properties of a Newly Created Element**

Once an element has been created, it can be configured just like an existing DOM element. We can set its `className`, `id`, attributes, styles, and other properties before adding it to the document.

```jsx
const div = document.createElement("div");

div.className = "main";
div.id = "container";
div.setAttribute("title", "Generated title");

div.style.backgroundColor = "green";
div.style.padding = "12px";
```

Here, the `<div>` is first created and then configured by assigning a class, an ID, an attribute, and some inline styles. Since the element is still not attached to the DOM, none of these changes will be visible on the webpage yet.

Properties can also be assigned dynamically. For example, an ID can be generated using a random value:

```jsx
div.id = Math.round(Math.random() * 10 + 1);
```

The important idea is that **creating an element and adding an element to the DOM are two separate operations**.

#### **Adding Content to a Newly Created Element**

After creating an element, we can add content to it using properties such as `innerText` or `textContent`, or we can explicitly create a text node using `document.createTextNode()`.

For example:

```jsx
const div = document.createElement("div");

div.innerText = "Hello World";
```

Alternatively, we can create a text node and append it to the element:

```jsx
const div = document.createElement("div");

const text = document.createTextNode("Hello World");

div.appendChild(text);
```

`document.createTextNode()` creates a **Text node** containing the specified text. This text node can then be attached to an element using `appendChild()`.

The second approach makes the DOM structure more explicit: first the element is created, then the text node is created, and finally the text node becomes a child of the element.

#### **`appendChild()`**

`appendChild()` adds a node as the **last child** of another node. It is commonly used to attach newly created elements or text nodes to the DOM.

```jsx
const div = document.createElement("div");
const text = document.createTextNode("Hello World");

div.appendChild(text);
document.body.appendChild(div);
```

The first `appendChild()` makes the text node a child of the `<div>`. The second `appendChild()` makes the `<div>` a child of the `<body>`. At this point, the `<div>` becomes part of the document and can appear on the webpage.

Therefore, creating an element alone is not enough to display it. The element must eventually be attached to an existing element in the DOM.

The basic process is:

```
Create the element
        ↓
Configure the element
        ↓
Add content
        ↓
Attach it to the DOM
```

#### **`appendChild()` Can Move Existing Elements**

`appendChild()` does not create a copy when the node being appended already exists in the DOM. Instead, it **moves the existing node** from its current position to the new parent. For example, if an element is currently inside one `<div>` and we append that same element to another `<div>`, it is removed from its original parent and placed inside the new parent. Therefore, `appendChild()` can be used both to **add newly created nodes** and to **move existing nodes**.

Example

```jsx
function addLanguage(langName) {
    const li = document.createElement("li");

    li.appendChild(document.createTextNode(langName));

    document.querySelector(".language").appendChild(li);
}

addLanguage("Python");
addLanguage("TypeScript");
addLanguage("Golang");
```

#### **`innerHTML` vs `createTextNode()`**

When adding content to a newly created element, `innerHTML` can be used when the content is intentionally meant to be interpreted as HTML.

```jsx
const li = document.createElement("li");

li.innerHTML = "<strong>JavaScript</strong>";
```

Here, the browser interprets the string as HTML and creates the corresponding DOM structure. However, if the value is simply plain text, a text-oriented API such as `textContent` or `createTextNode()` is generally more appropriate.

```jsx
const li = document.createElement("li");

li.appendChild(document.createTextNode("JavaScript"));
```

The important difference is that `innerHTML` treats the provided value as **HTML markup**, whereas `createTextNode()` treats the provided value as **text**.

Conceptually:

```
innerHTML
    ↓
HTML string
    ↓
Browser parses the HTML
    ↓
DOM structure is created/updated
```

Whereas:

```
createTextNode()
    ↓
Text node is created directly
    ↓
Node is attached to the DOM
```

This makes `createTextNode()` a direct approach when the intention is only to insert text. It also avoids interpreting the supplied value as HTML, which is particularly important when dealing with untrusted or user-provided input.

This does not mean that `innerHTML` is always bad or that `createTextNode()` is always faster. `innerHTML` is useful when we intentionally need to insert HTML markup. The important distinction is:

**Use `innerHTML` when you need to work with HTML markup; use `textContent` or `createTextNode()` when you simply need text.**

---

### **2. Editing Existing HTML Elements**

JavaScript allows us to modify HTML elements that already exist in the DOM. To edit an existing element, we first need to obtain a **reference to that element** using one of the DOM selectors we have learned, such as `querySelector()`, `getElementById()`, or `getElementsByClassName()`. Once we have a reference to the required element, we can modify its content, attributes, or even replace the entire element with a different one.

For example:

```jsx
HTML
<ul class="language">
	<li>Javascript</li>
	<li>Python</li>
  <li>Java</li>
</ul>

JS
const secondLang = document.querySelector("li:nth-child(2)");
OR
const languages = document.querySelectorAll(".language li");
const secondLanguage = languages[1];
console.log(secondLanguage);
secondLanguage.innerHTML = "Mojo"
```

Here, `querySelector()` returns the second `<li>` element, and the variable `secondLanguage` now holds a reference to that element. We can use this reference to perform different types of modifications.

---

#### **`replaceWith()`**

Sometimes we don’t want to modify the contents of an existing element. Instead, we want to **remove the existing element and put a completely different element in its place**. The `replaceWith()` method is used for this purpose.

```jsx
const languages = document.querySelectorAll(".language li");
const secondLanguage = languages[1];

const newLi = document.createElement("li");
newLi.textContent = "Mojo";

secondLanguage.replaceWith(newLi);
```

Here, `secondLanguage` refers to the existing `<li>`. A new `<li>` is created and given the text `"Mojo"`. Calling `replaceWith()` removes the original `<li>` from the DOM and places `newLi` in exactly the same position.

The important difference is:

```
innerHTML
→ changes the content inside the existing element

replaceWith()
→ removes the existing element and replaces it with another element
```

#### **`outerHTML`**

`outerHTML` represents the **entire HTML element**, including the element’s own opening and closing tags. Therefore, assigning a new value to `outerHTML` replaces the complete element.

```jsx
const firstLang = document.querySelector("li:first-child");

firstLang.outerHTML = "<li>TypeScript</li>";
```

If the original HTML is:  `<li>JavaScript</li>`  it becomes `<li>TypeScript</li>`. The important difference between `innerHTML` and `outerHTML` is what part of the HTML they represent.

For example - `<li>JavaScript</li>` `innerHTML` refers only to “JavaScript”  whereas `outerHTML` refers to `<li>JavaScript</li>` . Assigning to `outerHTML` therefore replaces the existing element, whereas assigning to `innerHTML` keeps the existing element and replaces only its contents.

```
innerHTML
→ works with the content inside the element

outerHTML
→ includes the element itself
```

---

#### **`replaceChild()`**

`replaceChild()` is another DOM method used to replace an existing child element with a new child element. Unlike `replaceWith()`, it is called on the **parent element**, and we provide both the new child and the old child. When we already have a reference to the element that needs to be replaced, `replaceWith()` is generally easier to read because we can directly tell that element what should replace it.

The syntax is:

```jsx
parent.replaceChild(newChild, oldChild);
```

For example:

```jsx
const parent = document.querySelector(".language");
const oldLi = parent.children[1];

const newLi = document.createElement("li");
newLi.textContent = "Mojo";

parent.replaceChild(newLi, oldLi);
```

Here, `parent` refers to the `.language` element, while `oldLi` refers to its second child. A new `<li>` is created, and `replaceChild()` tells the parent to replace `oldLi` with `newLi`.

The important difference in how the two replacement methods are used is:

```
replaceWith()
→ called on the element being replaced

oldElement.replaceWith(newElement)
```

Whereas:

```
replaceChild()
→ called on the parent element

parent.replaceChild(newElement, oldElement)
```

---

### **3. Removing HTML Elements**

Removing an element is simpler than creating or replacing one. First, we need a reference to the element we want to remove.

#### **`remove()`**

The **`remove()`** method **removes a specified HTML element or node directly from the DOM.** The important thing to understand is that `remove()` operates on the **element itself**. Once removed, the element is no longer part of the document.

```jsx
HTML
<ul class="language">
	<li>Javascript</li>
	<li>Python</li>
  <li>Java</li>
</ul>

JS
const lastLang = document.querySelector("li:last-child");
lastLang.remove();
```

Here, `querySelector("li:last-child")` selects the last `<li>`, and `remove()` removes that element from the DOM.

---

#### **`removeChild()`**

**The `removeChild()` method** **removes a specified child node from a parent element** in the DOM.

Unlike `remove()` where it is called one the specified element, `removeChild()`  is called on the **parent**.

```jsx
const parent = document.querySelector(".language");
const lastLang = parent.lastElementChild;

parent.removeChild(lastLang);
```

So the difference is:

```
element.remove()
→ the element removes itself from the DOM

parent.removeChild(child)
→ the parent removes the specified child
```

For modern JavaScript, `remove()` is generally the simpler approach when you already have a reference to the element. `removeChild()` is still important to understand because it is part of the DOM API and appears frequently in older code and tutorials.

---