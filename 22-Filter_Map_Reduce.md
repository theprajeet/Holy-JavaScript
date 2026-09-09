# Filter, Map, Reduce

## **`filter()`**

The filter() method in JavaScript is a higher order function that takes another function as a callback function. It is used to filter out unwanted elements from an array by creating a new array containing only the elements that satisfy a condition defined in a callback function.

- Iterates over each element of the array
- Includes elements when the callback returns true
- Excludes elements when the callback returns false

For example:

```jsx
const myNums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const newNums = myNums.filter((num) => {
    return num > 4;
});

console.log(newNums);
```

The callback is executed for every element. If the condition returns `true`, that element is included in the new array. If the condition returns `false`, that element is excluded.

The result is:

```
[5, 6, 7, 8, 9, 10]
```

So the basic idea is:

```
filter() → "Which elements should I keep?"
```

### **How `filter()` works**

For:

```jsx
const newNums = myNums.filter((num) => {
    return num > 4;
});
```

JavaScript effectively checks:

```
1 > 4  → false → exclude 1
2 > 4  → false → exclude 2
3 > 4  → false → exclude 3
4 > 4  → false → exclude 4
5 > 4  → true  → keep 5
6 > 4  → true  → keep 6
...
10 > 4 → true  → keep 10
```

The elements for which the callback returns a truthy value are included in the resulting array.

### **`filter()` returns a new array**

Unlike `forEach()`, `filter()` **returns a new array**.

```jsx
const newNums = myNums.filter((num) => num > 4);
```

The original `myNums` array remains unchanged:

```
myNums  → [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

newNums → [5, 6, 7, 8, 9, 10]
```

This is one of the important characteristics of these array methods: they allow you to generate a result without directly modifying the original array.

### **`filter()` using `forEach()`**

Anything done with `filter()` can technically be done using `forEach()`, but you have to manually build the result.

For example:

```jsx
const myNums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const newNums = [];

myNums.forEach((num) => {
    if (num > 4) {
        newNums.push(num);
    }
});

console.log(newNums);
```

This produces:

```
[5, 6, 7, 8, 9, 10]
```

Here, we manually created an empty array and pushed values into it whenever the condition was satisfied.

With `filter()`, the same operation is much simpler:

```jsx
const newNums = myNums.filter((num) => num > 4);
```

The important difference is not that `forEach()` is “wrong”. Both can accomplish the task, but `filter()` communicates the intention more clearly: **filter this array based on a condition**.

### **Filtering an array of objects**

`filter()` becomes especially useful when working with data received from an API or database.

For example:

```jsx
const books = [
    { title: 'Book One', genre: 'Fiction', publish: 1981, edition: 2004 },
    { title: 'Book Two', genre: 'Non-Fiction', publish: 1992, edition: 2008 },
    { title: 'Book Three', genre: 'History', publish: 1999, edition: 2007 },
    { title: 'Book Four', genre: 'Non-Fiction', publish: 1989, edition: 2010 },
    { title: 'Book Five', genre: 'Science', publish: 2009, edition: 2014 },
    { title: 'Book Six', genre: 'Fiction', publish: 1987, edition: 2010 },
    { title: 'Book Seven', genre: 'History', publish: 1986, edition: 1996 },
    { title: 'Book Eight', genre: 'Science', publish: 2011, edition: 2016 },
    { title: 'Book Nine', genre: 'Non-Fiction', publish: 1981, edition: 1989 }
];
```

Suppose we want only books whose genre is `"History"`.

```jsx
let userBooks = books.filter((bk) => bk.genre === "History");

console.log(userBooks);
```

`bk` represents each book object. Therefore:

```jsx
bk.genre
```

accesses the `genre` property of that particular book.

The result contains:

```
Book Three
Book Seven
```

### **`filter()` with multiple conditions**

You can combine multiple conditions inside `filter()`.

For example, suppose we want books that were published in or after 1995 **and** belong to the History genre:

```jsx
userBooks = books.filter((bk) => {
    return bk.publish >= 1995 && bk.genre === "History";
});
```

Here, both conditions must be true because `&&` is being used.

The result is:

```
Book Three
```

because `Book Three` has:

```
publish → 1999
genre   → History
```

`Book Seven` is also History, but it was published in 1986, so it does not satisfy the publication condition.

---

## **`map()`**

The map() [**m**](https://www.geeksforgeeks.org/javascript/javascript-array-map-method/)ethod in JavaScript creates a new array by applying a callback function to transform each element of the original array.

- Iterates through every element of the array
- Executes the callback function for each element
- Stores the returned value in the new array

For example:

```jsx
const myNumbers = [1, 2, 3, 4, 5];

const newNums = myNumbers.map((num) => {
    return num + 10;
});

console.log(newNums);
```

The result is:

```
[11, 12, 13, 14, 15]
```

Every element is transformed:

```
1 → 11
2 → 12
3 → 13
4 → 14
5 → 15
```

So the basic idea is:

```
map() → "What should each element become?"
```

### **`map()` vs `filter()`**

The easiest way to distinguish them is:

```
filter() → selects elements
map()    → transforms elements
```

### **Method Chaining**

One of the powerful features of these array methods is that you can **chain multiple methods together**.

For example:

```jsx
const myNumbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const newNums = myNumbers
    .map((num) => num * 10)
    .map((num) => num + 1)
    .filter((num) => num >= 40);

console.log(newNums);
```

Here, three operations are performed one after another.

First:

```jsx
.map((num) => num * 10)
```

produces:

```
[10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
```

The result of the first `map()` is passed to the second `map()`.

The second operation:

```jsx
.map((num) => num + 1)
```

produces:

```
[11, 21, 31, 41, 51, 61, 71, 81, 91, 101]
```

Then `filter()` receives this new array:

```jsx
.filter((num) => num >= 40)
```

and keeps only values greater than or equal to `40`.

Final result:

```
[41, 51, 61, 71, 81, 91, 101]
```

---

## **`reduce()`**

The reduce() in JavaScript is used to combine all elements of an array into a single value by applying a callback function to each element. The reduce() method accepts three parameters, they are:

- **Accumulator:** stores the result after each iteration
- **currentValue:** the current element being processed
- **currentIndex:** index of the current element

For example, suppose we have:

```jsx
const numbers = [1, 2, 3, 4];
```

and we want their total:

```
1 + 2 + 3 + 4 = 10
```

`reduce()` is suitable for this because multiple array values are being combined into one value.

The basic structure is:

```jsx
const total = numbers.reduce((accumulator, currentValue) => {
    return accumulator + currentValue;
}, 0);
```

Here, two important values are provided to the callback:

```
accumulator → the accumulated/result value so far
currentValue → the current array element
```

### **Initial Value in `reduce()`**

The second argument passed to `reduce()` is the **initial value**.

```jsx
numbers.reduce((acc, curr) => {
    return acc + curr;
}, 0);
```

Here:

```
0 → initial value of accumulator
```

Therefore, the first calculation is:

```
0 + 1 = 1, then
1 + 2 = 3, then
3 + 3 = 6, then
6 + 4 = 10,
Finally
10
```

### **Understanding the Accumulator**

The most important part of `reduce()` is understanding the **accumulator**.

Suppose:

```jsx
const numbers = [1, 2, 3];

const total = numbers.reduce((acc, curr) => {
    return acc + curr;
}, 0);
```

The first time the callback runs:

```
acc  → 0
curr → 1

0 + 1 = 1
```

The returned `1` becomes the accumulator for the next iteration.

Second iteration:

```
acc  → 1
curr → 2

1 + 2 = 3
```

Now `3` becomes the accumulator.

Third iteration:

```
acc  → 3
curr → 3

3 + 3 = 6
```

Therefore:

```
Final result → 6
```

The accumulator continuously carries the result from the previous iteration into the next iteration.

### **`reduce()` Example**

```jsx
const numbers = [1, 2, 3];

const total = numbers.reduce((acc, curr) => {
    return acc + curr;
}, 0);

console.log(total);
```

Execution:

```
Initial accumulator = 0

0 + 1 = 1
1 + 2 = 3
3 + 3 = 6

Final result = 6
```

### **`reduce()` with Arrow Function**

The same code can be written more concisely:

```jsx
const total = numbers.reduce((acc, curr) => acc + curr, 0);
```

Here the arrow function has an implicit return.

This is essentially the same as:

```jsx
const total = numbers.reduce((acc, curr) => {
    return acc + curr;
}, 0);
```

### **`reduce()` for a Shopping Cart**

`reduce()` is especially useful for calculating a shopping cart total.

For example:

```jsx
const shoppingCart = [
    { item: "JS Course", price: 2999 },
    { item: "React Course", price: 999 },
    { item: "Mobile Development", price: 599 },
];
```

Suppose we want the total price.

```jsx
const priceToPay = shoppingCart.reduce((acc, item) => {
    return acc + item.price;
}, 0);

console.log(priceToPay);
```

Here, `item` represents each object in the array.

For each object:

```jsx
item.price
```

accesses its price.

The accumulator keeps adding those prices:

```
0 + 2999 = 2999
2999 + 999 = 3998
3998 + 599 = 4597
```

Therefore:

```
priceToPay → 4597
```

This is a very common real-world use of `reduce()`.

For example, when an e-commerce application receives a shopping cart from a database, it can use `reduce()` to calculate the total price of all the products.

### **`filter()`, `map()` and `reduce()` Combined**

These methods become particularly powerful when used together.

The handwritten college example demonstrates exactly this.

```jsx
const products = [
    500, 3000, 6000, 2000, 500,
    10000, 210, 4000, 70000, 2500
];
```

Suppose the requirement is:

1. Select products whose price is greater than `2000`.
2. Add `18% GST` to each selected product.
3. Calculate the final total.

This can be done using `filter()`, `map()`, and `reduce()`.

### **Step 1: Filter the Products**

```jsx
const result = products.filter((x) => {
    return x > 2000;
});

console.log(result);
```

The values greater than `2000` are:

```
[3000, 6000, 10000, 4000, 70000, 2500]
```

Notice that `2000` itself is not included because the condition is:

```jsx
x > 2000
```

not:

```jsx
x >= 2000
```

### **Step 2: Add 18% GST Using `map()`**

Now we have:

```
[3000, 6000, 10000, 4000, 70000, 2500]
```

We want to add 18% to each value.

```jsx
const gst = result.map((x) => {
    return x + x * 0.18;
});

console.log(gst);
```

The values become:

```
3000  → 3540
6000  → 7080
10000 → 11800
4000  → 4720
70000 → 82600
2500  → 2950
```

Therefore:

```
[3540, 7080, 11800, 4720, 82600, 2950]
```

### **Step 3: Calculate the Total Using `reduce()`**

Now we want to add all those values together:

```jsx
const checkout = gst.reduce((acc, val) => {
    return acc + val;
}, 0);

console.log(checkout);
```

The calculation is:

```
0 + 3540   = 3540
3540 + 7080 = 10620
10620 + 11800 = 22420
22420 + 4720 = 27140
27140 + 82600 = 109740
109740 + 2950 = 112690
```

Therefore:

```
checkout → 112690
```

### **Complete Combined Example**

```jsx
const products = [
    500, 3000, 6000, 2000, 500,
    10000, 210, 4000, 70000, 2500
];

const result = products.filter((x) => {
    return x > 2000;
});

const gst = result.map((x) => {
    return x + x * 0.18;
});

const checkout = gst.reduce((acc, val) => {
    return acc + val;
}, 0);

console.log(checkout);
```

This follows a very logical flow:

```
products
   ↓
filter()
   ↓
products > 2000
   ↓
map()
   ↓
add 18% GST
   ↓
reduce()
   ↓
calculate total
```

### **The Combined Operation Can Be Chained**

Since `filter()` returns an array and `map()` also returns an array, they can be directly chained.

```jsx
const checkout = products
    .filter((x) => x > 2000)
    .map((x) => x + x * 0.18)
    .reduce((acc, val) => acc + val, 0);

console.log(checkout);
```

The result is still:

```
112690
```

This is a good example of **method chaining**, where the result of one array method becomes the input for the next method.

### **`forEach()` vs `filter()` vs `map()` vs `reduce()`**

| **Method** | **Main Purpose** | **Returns** |
| --- | --- | --- |
| `forEach()` | Perform an operation on every element | `undefined` |
| `filter()` | Select elements based on a condition | New array |
| `map()` | Transform every element | New array |
| `reduce()` | Combine elements into one final value | Single value |

A simple way to remember them is:

```
forEach() → Do something for every element

filter()  → Select some elements

map()     → Transform every element

reduce()  → Combine elements into one value
```

### **Original Array Is Not Modified**

`filter()`, `map()`, and `reduce()` do not modify the original array simply by performing their normal operation. Instead, `filter()` and `map()` produce new arrays, while `reduce()` produces an accumulated result.

For example:

```jsx
const numbers = [1, 2, 3, 4, 5];

const filtered = numbers.filter(num => num > 3);
const mapped = numbers.map(num => num * 10);
```

The original array remains:

```
numbers → [1, 2, 3, 4, 5]
```

while:

```
filtered → [4, 5]

mapped → [10, 20, 30, 40, 50]
```

This is why these methods are commonly described as **non-mutating array methods** in this context: they don’t directly change the original array; instead, they produce a result.

### Note

The most important distinction to remember is that **`forEach()` does not return a useful array**, while **`filter()` and `map()` return new arrays**, and **`reduce()` returns one accumulated result**.

Also remember the difference between `filter()` and `map()`:

```jsx
filter(num => num > 5)
```

asks:

Which elements should I keep?

while:

```jsx
map(num => num * 10)
```

asks:

What should each element become?

And `reduce()` asks:

How can I combine all these elements into one final value?

---