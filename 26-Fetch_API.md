# Fetch API

## **The Fetch API**

**The Fetch API is a interface JavaScript which is sed to make HTTP requests and communicate with servers.** Whenever a web application needs to retrieve data from a server, send information to a server, or perform operations such as creating, updating, or deleting data, it needs to make an HTTP request. Fetch API provides a simple and Promise-based way to perform these requests asynchronously.

Before Fetch API, JavaScript commonly used `XMLHttpRequest` (XHR) for making HTTP requests. Although XHR can perform the same operations, its API is more complicated because it involves request states, callbacks, and more verbose syntax. Fetch API provides a cleaner interface based on **Promises**, which makes it easier to work with asynchronous operations using `.then()`, `.catch()`, and `async/await`.

Fetch API is not technically a part of the JavaScript language itself. JavaScript provides features such as Promises, while the environment in which JavaScript runs provides APIs such as `fetch()`. Browsers provide Fetch API through their runtime environment, and modern Node.js also provides its own implementation of Fetch API.

### **Basic Syntax of `fetch()`**

The basic syntax is:

```jsx
fetch(url, options)
```

Example

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/')
.then((response) => {
    return response.json()
})
.then((data) => {
    console.log(data);
})
.catch((error) => console.log(error))
```

Here, `url` is the URL of the resource or API endpoint that we want to communicate with. The optional `options` object is used to customize the request, such as specifying the HTTP method, headers, body, credentials, caching behavior, etc. If we do not provide the options object, Fetch uses the default configuration, which means a basic `fetch(url)` performs a GET request.

### **What Does `fetch()` Actually Return?**

The most important thing to understand about Fetch API is that **`fetch()` does not return the requested data directly. It returns a Promise**. This Promise represents the eventual result of the network request.

Conceptually, the flow is:

```
fetch() → Promise → Response → response.json() → Promise → Data
```

For example, if we write `const result = fetch(url)`, `result` will contain a Promise, not the todo object. When the network request successfully produces an HTTP response, that Promise becomes fulfilled with a **Response object**.

This is directly connected to the concept of Promises. A Promise represents a value that is not available immediately but may become available in the future. Therefore, `fetch(url)` essentially means: **start the network operation and give me a Promise representing its eventual result**.

An important nuance is that Fetch API does **not reject its Promise simply because the server returns an HTTP error status**. For example, if the server responds with `404 Not Found` or `500 Internal Server Error`, the Fetch Promise normally still fulfills with a Response object. This is because the network request itself succeeded—the server responded successfully at the HTTP communication level. The error is represented by the response’s status code.

A genuine failure to perform the Fetch operation, such as certain network failures, can cause the Fetch Promise to reject. Therefore, the important distinction is: **HTTP error → Promise usually fulfills with a Response, Fetch/network failure → Promise rejects.**

### **Understanding the Response Object**

When the Fetch Promise fulfills, the value received inside `.then()` is a **Response object**, not the actual JSON data.

For example:

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => {
        console.log(response);
    });
```

The `response` variable represents the HTTP response received from the server. It contains information about the response as well as methods that allow us to read the response body.

Some important properties of the Response object are `response.ok`, `response.status`, `response.statusText`, `response.headers`, and `response.url`. `response.ok` is a Boolean that is `true` when the HTTP status is in the `200–299` range and `false` otherwise. `response.status` gives the numerical HTTP status code such as `200`, `404`, or `500`. `response.statusText` provides the textual status description, while `response.headers` provides access to the HTTP response headers. `response.url` gives the URL associated with the response.

The most commonly used method for JSON APIs is `response.json()`, which allows us to read and parse the response body as JSON.

Because Fetch does not automatically reject for HTTP errors, `response.ok` is particularly useful when we want HTTP errors to be treated as JavaScript errors:

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => {
        if (!response.ok) {
            throw new Error(`HTTP Error: ${response.status}`);
        }

        return response.json();
    })
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

Here, if the server responds with something like `404`, `response.ok` will be `false`, so we manually throw an error. That error then travels through the Promise chain and can be handled by `.catch()`.

### **Reading the Response with `response.json()`**

Calling `response.json()` is necessary when the server has returned JSON and we want to work with that JSON as a JavaScript value. An important point is that **`response.json()` also returns a Promise**. It does not immediately return the parsed object.

This explains why Fetch examples commonly contain two `.then()` calls:

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

The first `.then()` receives the **Response object** produced when the Fetch Promise fulfills. `response.json()` is then called, which returns another Promise. Because that Promise is returned from the first `.then()`, the second `.then()` waits for it and receives the **parsed JSON data**.

So the flow is:

```
fetch() → Response → response.json() → Data
```

The first `.then()` deals with the HTTP response, while the second `.then()` deals with the actual data contained in the response body.

This distinction is important: `response` is a Response object, whereas `data` is the JavaScript representation of the JSON returned by the server.

---

### **Fetch with `.then()` and `.catch()`**

The general Promise-based Fetch pattern is `fetch() → first .then() → response.json() → second .then() → .catch()`. The first `.then()` receives the Response, the second `.then()` receives the parsed data, and `.catch()` handles rejected Promises or errors thrown anywhere earlier in the chain.

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => {
        if (!response.ok) {
            throw new Error(`HTTP Error: ${response.status}`);
        }

        return response.json();
    })
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.error('Error:', error);
    });
```

The `return response.json()` is important because it returns the Promise produced by `response.json()` to the Promise chain. Consequently, the next `.then()` receives the eventual result of that Promise.

The `.catch()` at the end can handle a network-level Fetch rejection, an error manually thrown because `response.ok` was false, or an error that occurs while processing the response.

### **Fetch with `async/await`**

The same Fetch operation can be written using `async/await`. Since Fetch and `response.json()` both return Promises, we normally use two `await`s:

```jsx
async function getTodo() {
    try {
        const response = await fetch(
            'https://jsonplaceholder.typicode.com/todos/1'
        );

        if (!response.ok) {
            throw new Error(`HTTP Error: ${response.status}`);
        }

        const data = await response.json();

        console.log(data);
    } catch (error) {
        console.error(error);
    }
}

getTodo();
```

The first `await` waits for the Promise returned by `fetch()` and gives us the Response object. The second `await` waits for the Promise returned by `response.json()` and gives us the parsed data.

Therefore, these two statements represent two different asynchronous operations: `await fetch()` gives us the **Response**, while `await response.json()` gives us the **actual data**.

This is equivalent to the Promise-chain version:

```jsx
fetch(url)
    .then(response => response.json())
    .then(data => console.log(data));
```

The main difference is that `.then()` and `.catch()` explicitly express the Promise chain, whereas `async/await` makes the asynchronous code appear more like synchronous code.

When using `async/await`, `try...catch` is commonly used for error handling. However, the same Fetch rule still applies: a `404` or `500` does not automatically cause `fetch()` to throw. We still need to check `response.ok` and throw an error ourselves if we want HTTP errors to be caught by `catch`.

---

## **HTTP Methods in Fetch API**

HTTP methods describe what operation we want to perform on a resource. The commonly used methods are **GET, POST, PUT, PATCH, and DELETE**. GET is used to retrieve data, POST is generally used to create a resource, PUT is generally used to replace or update a resource, PATCH is generally used to partially update a resource, and DELETE is used to remove a resource.

Fetch uses GET by default, so a simple request such as `fetch('https://jsonplaceholder.typicode.com/todos/1')` performs a GET request. If we want to specify another method, we use the `options` object.

For example, a POST request can be written as:

```jsx
fetch('https://jsonplaceholder.typicode.com/todos', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
        title: 'Learn Fetch API',
        completed: false,
        userId: 1
    })
})
    .then(response => response.json())
    .then(data => console.log(data));
```

Here, `method` specifies the HTTP method, `headers` provides metadata about the request, and `body` contains the data being sent to the server.

When sending JSON, we commonly specify `"Content-Type": "application/json"` and use `JSON.stringify()` to convert the JavaScript object into a JSON string. The server can then interpret the request body as JSON.

A PUT request can be used to update or replace a resource:

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/1', {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        id: 1,
        title: 'Learn Fetch API',
        completed: true,
        userId: 1
    })
});
```

DELETE can be used to request deletion:

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/1', {
    method: 'DELETE'
});
```

JSONPlaceholder is a fake REST API used for learning and testing, so these write operations simulate the behavior of a real API rather than permanently modifying a production database.

The second argument of `fetch()` is called the **options object**. It can contain many configuration options, including `method`, `headers`, `body`, `mode`, `credentials`, `cache`, `redirect`, and `signal`. Therefore, `fetch(url)` represents the basic request, while `fetch(url, options)` gives us control over how that request is performed.

---

## **Fetch API and the JavaScript Runtime**

To understand how Fetch works internally, we need to distinguish between the **JavaScript engine** and the **runtime environment**.

The JavaScript engine is responsible for executing JavaScript and contains components such as the Memory Heap and Call Stack. However, the JavaScript engine does not itself provide every capability required by an application. The surrounding runtime environment provides additional APIs.

<img src="/Assets/Async.png" alt="Async.png" width="600" >

For example, a browser provides APIs such as DOM APIs, timers, and Fetch API. Modern Node.js also provides Fetch API, although Node.js is not a browser and does not have browser-specific objects such as the DOM.

Therefore, it is better to think of the environment as:

```
JavaScript Engine + Host/Runtime APIs + Event Loop
```

rather than thinking that everything such as `fetch()`, `setTimeout()`, or DOM APIs is directly part of the JavaScript engine.

Historically, Node.js did not have the browser’s Fetch API, so developers commonly used Node’s `http` and `https` modules or third-party libraries. Modern Node.js provides Fetch natively, which gives Node.js developers a familiar Promise-based API for making HTTP requests.

### **What Happens Internally When `fetch()` Runs?**

This is the most important internal concept from the video. When JavaScript encounters:

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/1')
```

the `fetch()` call initially runs on the JavaScript Call Stack. However, the actual network operation is not performed synchronously on the Call Stack. Fetch communicates with the host environment, which handles the networking operation.

At the same time, `fetch()` returns a Promise to JavaScript. This Promise represents the eventual result of the Fetch operation. JavaScript therefore does not sit on the Call Stack waiting for the server to respond. It can continue executing other JavaScript while the network request is being handled by the environment. When the network operation eventually produces a response, the Fetch Promise can be fulfilled with a Response object. If the Fetch operation itself fails, the Promise can be rejected.

The important nuance is that the **HTTP response status does not determine whether the Fetch Promise is fulfilled or rejected**. A `404` is still an HTTP response, so Fetch normally fulfills its Promise with a Response object. We then inspect `response.ok` or `response.status` to determine whether the HTTP operation was successful from our application’s perspective.

### **Why Fetch Uses the Microtask Queue**

Because `fetch()` returns a Promise, the callbacks registered using `.then()` are Promise reactions. Promise reactions are processed as **microtasks**.

For example:

```jsx
fetch(url)
    .then(response => {
        console.log(response);
    });
```

When the Fetch Promise becomes fulfilled, the callback associated with `.then()` becomes ready to execute and is scheduled through the microtask mechanism. The Event Loop eventually allows that microtask to execute when the Call Stack is available.

<img src="/Assets/Async.png" alt="Async" width="600" >

The overall conceptual flow is:

```
fetch()
   ↓
Network operation
   ↓
Promise settles
   ↓
.then() reaction
   ↓
Microtask Queue
   ↓
Event Loop
   ↓
Call Stack
```

Microtask Queue is given a higher priority compared with the normal Task Queue. Promise callbacks are microtasks, whereas callbacks such as those from `setTimeout()` are tasks.

For example:

```jsx
console.log('A');

setTimeout(() => console.log('B'), 0);

Promise.resolve().then(() => console.log('C'));

console.log('D');
```

The output is:

```
A
D
C
B
```

`A` and `D` execute synchronously. The Promise callback becomes a microtask, while the timer callback becomes a task. Once the synchronous code finishes, the microtask is processed before the next task.

The important point is that **microtask priority does not mean Fetch itself is faster than `setTimeout()`**. The network request still takes however long it takes. A Fetch Promise can only place its reaction into the Microtask Queue after the relevant Promise has settled. Therefore, we should not memorize “Fetch always runs before `setTimeout()`.” The correct rule is that **once a Promise reaction is ready, it is processed as a microtask, and microtasks are processed before the next task**.

### **The Internal Flow of Fetch**

The diagrams from the video give us a useful conceptual model for what happens internally. Calling `fetch()` can be thought of as involving two parallel aspects: JavaScript receives a Promise representing the eventual result, while the host environment handles the actual network operation.

Conceptually:

```
                    fetch()
                      ↓
          ┌───────────┴───────────┐
          ↓                       ↓
      Promise              Network Request
          ↓                       ↓
     JavaScript              Host Runtime
                                  ↓
                               Server
                                  ↓
                           Response arrives
                                  ↓
                         Promise is fulfilled
                                  ↓
                         Microtask Queue
                                  ↓
                            Event Loop
                                  ↓
                           Call Stack
```

There is a conceptual idea of `onFulfilled` and `onRejected` handlers associated with a Promise. When the operation succeeds, the Promise follows its fulfilled path; when the relevant operation fails, it follows its rejected path. The callbacks registered through `.then()` and `.catch()` are then scheduled according to Promise job/microtask semantics.

This should be understood as a **conceptual model**, not as literal JavaScript source code inside the browser or Node.js. The actual Fetch specification and runtime implementations are much more complex.

---