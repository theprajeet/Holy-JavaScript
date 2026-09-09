# Ways To Write JavaScript

## **Ways to Write JavaScript Code**

JavaScript code can be added to an HTML document in **three different ways**:

- Inline JavaScript
- Internal JavaScript
- External JavaScript

### **1. Inline JavaScript**

**Inline JavaScript** means writing JavaScript code directly inside an HTML element using event attributes such as `onclick`, `onmouseover`, `onchange`, etc. This method is mainly used for **small tasks** or **quick testing**. Since the JavaScript code is written within the HTML tag itself, it is easy to implement but becomes difficult to manage as the application grows, and is not recommended in production-grade applications.

#### **Advantages**

- Very simple to write.
- Suitable for small examples and learning.
- No separate `<script>` tag is required.

#### **Disadvantages**

- Mixing HTML and JavaScript reduces code readability.
- Difficult to maintain in large projects.
- Not reusable.
- Not recommended for production applications.

Example

```jsx
<!DOCTYPE html>
<html>
<head>
    <title>Inline JavaScript</title>
</head>

<body>

    <h2 id="heading">Student Details</h2>
    <p id="result">Click the button to display details.</p>

    <button onclick="
        let name = 'Tony';
        let age = 21;
        let course = 'Computer Science';

        document.getElementById('heading').innerHTML = name;
        document.getElementById('result').innerHTML =
            'Age: ' + age + '<br>Course: ' + course;
    ">
        Show Details
    </button>

</body>
</html>
```

---

### **2. Internal JavaScript**

**Internal JavaScript** means writing JavaScript code inside the `<script>` tag within the same HTML file.

The `<script>` tag is usually placed either:

- Inside the `<head>` section, or
- Just before the closing `</body>` tag (recommended).

This approach keeps JavaScript separate from HTML elements, making the code cleaner and easier to maintain compared to inline JavaScript.

#### **Advantages**

- Cleaner than inline JavaScript.
- Easy to develop small projects.
- JavaScript is separated from HTML elements.

#### **Disadvantages**

- JavaScript cannot be reused in multiple HTML pages.
- As the project grows, the HTML file becomes large and difficult to maintain.

Example

```jsx
<!DOCTYPE html>
<html>
<head>
    <title>Internal JavaScript</title>
</head>

<body>

    <h2 id="heading">Student Details</h2>
    <p id="result">Click the button to display details.</p>

    <button onclick="showDetails()">
        Show Details
    </button>

    <script>
        function showDetails() {

            let name = 'Jeet';
            let age = 21;
            let course = 'Computer Science';

            document.getElementById('heading').innerHTML = name;

            document.getElementById('result').innerHTML =
                'Age: ' + age + '<br>Course: ' + course;
        }
    </script>

</body>
</html>
```

---

### **3. External JavaScript**

**External JavaScript** means writing JavaScript code in a separate file with the **`.js`** extension and linking it to the HTML document using the `<script>` tag. This is the **most recommended and widely used method** because it completely separates HTML from JavaScript, making the application easier to maintain, debug, and reuse.

#### **Advantages**

- Keeps HTML and JavaScript completely separate.
- Easy to maintain large applications.
- The same JavaScript file can be reused across multiple webpages.
- Browsers cache external JavaScript files, improving website performance.
- Easier to debug and update.

#### **Disadvantages**

- Requires an additional `.js` file.
- If the external file fails to load, the JavaScript functionality will not work.

Example

```jsx
FILE STRUCTURE

my-project/
│
├── index.html
│
└── script.js
```

```jsx
index.html

<!DOCTYPE html>
<html>
<head>
    <title>External JavaScript</title>
</head>

<body>

    <h2 id="heading">Student Details</h2>
    <p id="result">Click the button to display details.</p>

    <button onclick="showDetails()">
        Show Details
    </button>

    <script src="script.js"></script>

</body>
</html>
```

```jsx
script.js

function showDetails() {

    let name = 'Jeet';
    let age = 21;
    let course = 'Computer Science';

    document.getElementById('heading').innerHTML = name;

    document.getElementById('result').innerHTML =
        'Age: ' + age + '<br>Course: ' + course;
}
```

---