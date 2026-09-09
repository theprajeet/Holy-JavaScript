# Strings

# **Strings**

A **String** is a **primitive data type** used to represent textual data in JavaScript. It is simply a sequence of characters enclosed within quotes. A string can contain letters, numbers, symbols, spaces, special characters, and even emojis.

Examples:

```jsx
let firstName = "John";
let city = 'Bengaluru';
let message = "Hello World!";
```

---

## **Ways to Create Strings**

JavaScript provides three ways to create strings.

#### **String Literals - Single Quotes (`' '`)**

```jsx
let name = 'John';
```

#### **String Literals - Double Quotes (`" "`)**

```jsx
let city = "Bengaluru";
```

Both single and double quotes behave exactly the same in JavaScript. You can use either based on your coding style or project conventions.

### **Template Literals**

A **Template Literal** is a string enclosed within **backticks (`` ``)**. Unlike single or double quotes, template literals preserve formatting such as spaces and new lines exactly as written.

Template literals are a **special kind of string syntax** introduced in ES2015. Unlike ordinary string literals, they support features such as:

- **String interpolation**
- **Multiline strings**
- **Embedded JavaScript expressions**

Example:

```jsx
const message = `Hello,
Welcome to JavaScript.
Today we are learning template literals.`;

console.log(message);
```

Output:

```
Hello,
Welcome to JavaScript.
Today we are learning template literals.
```

Everything written inside the backticks appears exactly the same in the output.

### **String Interpolation**

**String Interpolation** allows variables or JavaScript expressions to be embedded directly inside a template literal using the `${}` syntax.

Instead of manually joining strings using the `+` operator, template literals provide a cleaner and more readable approach.

**Old Way (Not Recommended)**

```jsx
const name = "John";
const age = 22;

console.log("My name is " + name + " and I am " + age + " years old.");
```

**Modern Way (Recommended) also known as Embedded JavaScript expressions**

```jsx
const name = "John";
const age = 22;

console.log(`My name is ${name} and I am ${age} years old.`);
```

Output

```
My name is John and I am 22 years old.
```

Anything inside `${}` can be a variable, expression, or even a function call.

Example:

```jsx
let num = 10;
console.log(`Square = ${num * num}`);
```

Output

```
Square = 100
```

#### **Why use Template Literals?**

- Improves readability.
- Eliminates complex string concatenation.
- Supports multiline strings.
- Allows variables and expressions to be embedded directly.

**Best Practice:** Always prefer template literals over string concatenation when writing modern JavaScript.

---

### **Creating Strings using the `String` Object**

Apart from string literals, JavaScript also provides the `String` constructor to create a string object.

```jsx
const str = new String("JavaScript");
```

Although this creates a string, it creates a **String object** rather than a primitive string.

Example

```jsx
const gameName = new String("hitesh-hc-com");
console.log(gameName);
```

Output

```
[String: 'hitesh-hc-com']
```

If expanded in the browser console, you can observe:

- Individual characters stored at numeric indexes.
- The `length` property.
- Numerous built-in string methods available through the prototype.

For example,

```jsx
console.log(gameName[0]);
```

Output

```
h
```

Similarly,

```jsx
console.log(gameName.__proto__);
```

displays the prototype object containing all the built-in string methods.

**Note:** Although `new String()` exists, string literals (`"Hello"`) are preferred because they are simpler, faster, and more commonly used in real-world applications.

---

### **Strings are Immutable**

One of the most important properties of JavaScript strings is that they are **immutable**.

This means that once a string is created, its characters cannot be modified directly. Any operation that appears to change a string actually returns a **new string**, while the original string remains unchanged.

Example

```jsx
let str = "hello";
let upper = str.toUpperCase();

console.log(str);
console.log(upper);
```

Output

```
hello
HELLO
```

Notice that `toUpperCase()` did not modify the original string. Instead, it returned a new string.

This behaviour applies to almost every string method, including:

`toUpperCase()`  `toLowerCase()`  `trim()`  `replace()`  `slice()`  `substring()`  

**Note:** String methods never modify the original string because strings are immutable. They always return a new string.

---

## **String Properties**

Unlike methods, **properties** provide information about a string without performing any operation on it.

### **`length`**

The `length` property returns the total number of characters present in a string, including letters, numbers, symbols, punctuation marks, and spaces.

Syntax

```jsx
string.length
```

Example

```jsx
let str = "Hello World";
console.log(str.length); // Output - 11
```

Spaces are also counted as characters.

Example

```jsx
let str = "Hello World!";
console.log(str.length); // Output - 12
```

**Note**

- `length` is a **property**, not a method, so **do not use parentheses**.

Correct:

```jsx
str.length // Correct
str.length() // Incorrect
```

- Time Complexity: **O(1)** (constant time), since the length is stored internally.

---

## **String Conversion Methods**

### **`toUpperCase()`**

The `toUpperCase()` method returns a new string with all alphabetic characters converted to uppercase. The original string remains unchanged.

Syntax

```jsx
string.toUpperCase()
```

Example

```jsx
let str = "hello";
console.log(str.toUpperCase());
```

Output

```
HELLO
```

---

### **`toLowerCase()`**

The `toLowerCase()` method returns a new string with all alphabetic characters converted to lowercase.

Syntax

```jsx
string.toLowerCase()
```

Example

```jsx
let str = "HELLO";
console.log(str.toLowerCase());
```

Output

```
hello
```

Example

```jsx
let email = "User@Gmail.com";
console.log(email.toLowerCase());
```

Output

```
user@gmail.com
```

---

### **`charAt(index)`**

The `charAt()` method returns the character present at the specified index.

Syntax

```jsx
string.charAt(index)
```

Example

```jsx
let str = "JavaScript";
console.log(str.charAt(4));
```

Output

```
S
```

If the index is outside the valid range, an empty string is returned.

```jsx
console.log(str.charAt(100));
```

Output

```
""
```

**Important Notes**

- Indexing starts from **0**.
- Returns a string containing one character.
- Does not modify the original string.

---

### **`indexOf(substring)`**

The `indexOf()` method returns the index of the **first occurrence** of the specified character or substring.

If the substring is not found, it returns `-1`.

Syntax

```jsx
string.indexOf(substring)
```

Example

```jsx
let str = "Hello World";
console.log(str.indexOf("World"));
```

Output

```
6
```

If the substring is absent,

```jsx
console.log(str.indexOf("Java"));
```

Output

```
-1
```

---

### **`lastIndexOf(substring)`**

The `lastIndexOf()` method returns the index of the **last occurrence** of the specified substring.

Syntax

```jsx
string.lastIndexOf(substring)
```

Example

```jsx
let str = "Hello World Hello";
console.log(str.lastIndexOf("Hello"));
```

Output

```
12
```

---

## **Extracting Parts of a String**

JavaScript provides several methods to extract a portion of a string. The most commonly used methods are `slice()` and `substring()`. Although both appear similar, they behave differently in certain situations, especially when working with negative indexes.

### **`slice(start, end)`**

The `slice()` method extracts a portion of a string starting from the specified `start` index up to, but **not including**, the `end` index. It returns a new string without modifying the original string.

Syntax

```jsx
string.slice(start, end)
```

Example

```jsx
let str = "JavaScript";
console.log(str.slice(0, 4)); // Java
```

If only the starting index is provided, extraction continues until the end of the string.

```jsx
console.log(str.slice(4)); // Script
```

One of the biggest advantages of `slice()` is that it supports **negative indexes**. Negative indexing starts counting from the end of the string.

```jsx
let str = "JavaScript";
console.log(str.slice(-6)); // Script
```

**Note**

- The `end` index is **excluded**.
- Supports negative indexes.
- Returns a new string.
- Does not modify the original string.

---

### **`substring(start, end)`**

The `substring()` method also extracts a portion of a string between two indexes and returns a new string.

Syntax

```jsx
string.substring(start, end)
```

Example

```jsx
let str = "JavaScript";
console.log(str.substring(0, 4)); // Java
```

Unlike `slice()`, `substring()` **does not support negative indexes**. If a negative value is passed, JavaScript treats it as `0`.

Example

```jsx
let str = "JavaScript";
console.log(str.substring(-3, 4));
```

Output

```
Java
```

Since `-3` becomes `0`, the result is equivalent to:

```jsx
str.substring(0, 4)
```

**Note**

- The `end` index is excluded.
- Negative indexes are treated as `0`.
- **Returns a new string**.
- Does not modify the original string.

### **Difference between `slice()` and `substring()`**

| **Feature** | `slice()` | `substring()` |
| --- | --- | --- |
| Supports negative indexes |  Yes | No |
| End index included | No | No |
| Returns new string | Yes | Yes |
| Modifies original string | No | No |

---

## **Modifying Strings**

Since strings are immutable, these methods **do not modify the original string**. Instead, they return a new string containing the updated value.

### **`trim()`**

The `trim()` method removes whitespace from both the beginning and the end of a string. It is commonly used to sanitise user input, where users may accidentally enter leading or trailing spaces.

Syntax

```jsx
string.trim()
```

Example

```jsx
let str = "    Hello World    ";
console.log(str.trim()); // Hello World
```

The original string remains unchanged.

**Related Methods**

- `trimStart()` – Removes whitespace only from the beginning.
- `trimEnd()` – Removes whitespace only from the end.

---

### **`replace(searchValue, newValue)`**

The `replace()` method replaces the **first occurrence** of the specified substring with a new value.

Syntax

```jsx
string.replace(searchValue, newValue)
```

Example

```jsx
let str = "Hello World";
console.log(str.replace("World", "JavaScript")); // Hello JavaScript
```

It only replaces the **first matching occurrence**.

Example

```jsx
let str = "Hello Hello";
console.log(str.replace("Hello", "Hi")); // Hi Hello
```

Example

```jsx
const url = "https://example.com/hello%20world";
console.log(url.replace("%20", "-"));
// Output - https://example.com/hello-world
```

---

### **`replaceAll(searchValue, newValue)`**

The `replaceAll()` method replaces **every occurrence** of the specified substring.

Syntax

```jsx
string.replaceAll(searchValue, newValue)
```

Example

```jsx
let str = "Hello Hello Hello";

console.log(str.replaceAll("Hello", "Hi")); // Hi Hi Hi
```

**Note:** `replaceAll()` was introduced in **ES2021**.

### **Difference between `replace()` and `replaceAll()`**

| **Feature** | `replace()` | `replaceAll()` |
| --- | --- | --- |
| Replaces first occurrence only | Yes | No |
| Replaces all occurrences | No | Yes |

---

## **Splitting and Joining Strings**

### **`split(separator)`**

The `split()` method divides a string into an array based on a specified separator.

Syntax

```jsx
string.split(separator)
```

Example

```jsx
let str = "Hello,World,JavaScript";
console.log(str.split(","));
```

Output

```jsx
["Hello", "World", "JavaScript"]
```

Example

```jsx
const gameName = "wiki-pedia-com";
console.log(gameName.split("-"));
```

Output

```jsx
["wiki", "pedia", "com"]
```

The separator can be:

- Space (`" "`)
- Comma (`","`)
- Hyphen (`"-"`)
- Any other character or substring

---

### **`concat()`**

The `concat()` method joins two or more strings and returns a new string.

Syntax

```jsx
string.concat(string2)
```

Example

```jsx
let first = "Hello";
let second = "World";

console.log(first.concat(second));
```

Output

```
HelloWorld
```

With separator

```jsx
console.log(first.concat(" ", second));
```

Output

```
Hello World
```

**Best Practice:** Template literals are generally preferred over `concat()` because they are easier to read.

---

## **Searching Strings**

### **`includes(substring)`**

The `includes()` method checks whether a substring exists within a string. It returns a Boolean value.

Syntax

```jsx
string.includes(substring)
```

Example

```jsx
let str = "JavaScript is fun";

console.log(str.includes("fun")); // true
console.log(str.includes("Python"));  // false

const url = "https://wikipedia.com";

console.log(url.includes("wikipedia")); // true
```

---

### **`startsWith(substring)`**

The `startsWith()` method checks whether a string begins with the specified substring.

Syntax

```jsx
string.startsWith(substring)
```

Example

```jsx
let str = "JavaScript is fun";

console.log(str.startsWith("Java")); // true
```

---

### **`endsWith(substring)`**

The `endsWith()` method checks whether a string ends with the specified substring.

Syntax

```jsx
string.endsWith(substring)
```

Example

```jsx
let str = "JavaScript is fun";

console.log(str.endsWith("fun")); // true
```

---

## **Repeating Strings**

### **`repeat(count)`**

The `repeat()` method creates a new string by repeating the original string the specified number of times.

Syntax

```jsx
string.repeat(count)
```

Example

```jsx
let str = "Hello";

console.log(str.repeat(3));
```

Output

```
HelloHelloHello
```

---