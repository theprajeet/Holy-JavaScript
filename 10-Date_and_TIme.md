# Date and Time

## **Date Object**

The **Date** object in JavaScript is a built-in object used to work with **dates and time**. It allows us to create, store, manipulate, and format dates and times.

JavaScript stores every date internally as the **number of milliseconds elapsed since midnight of January 1, 1970 (UTC)**. This point in time is known as the **Unix Epoch** or **ECMAScript Epoch**.This numeric value is called a **Timestamp**.

### **Creating the Current Date and Time**

To create an object containing the **current date and time**, use the `Date` constructor.

**Syntax:**

```jsx
new Date()
```

**Example:**

```jsx
let myDate = new Date();

console.log(myDate);
// Output (example):
// 2026-08-03T09:00:25.123Z

// This is the complete Date object.
// It contains the date, time, timezone, etc.
// The default output is not very human-readable.

console.log(myDate.toDateString());
// Output:
// Mon Aug 03 2026
// Displays only the date in a readable format.

console.log(myDate.toLocaleString());
// Output:
// 3/8/2026, 2:30:25 pm
// Displays the date and time according to the user's locale.

console.log(typeof(myDate));
// Output:
// object
// Date is an object in JavaScript.
```

### **Why is `typeof(myDate)` an Object?**

Although a `Date` stores date and time values, it is actually an **instance of the built-in `Date` object**.

```jsx
typeof(myDate)
```

Output

```
object
```

This means JavaScript provides many built-in methods such as:

`toDateString()`  `toLocaleString()`  `getMonth()`  `getDay()`   `getTime()`    

which can be used on every `Date` object.

### **Creating a Specific Date Using Numbers**

A date can also be created by passing the **year, month, and day** to the `Date` constructor.

**Syntax:**

```jsx
new Date(year, monthIndex, day)
```

**Example:**

```jsx
let myCreatedDate = new Date(2023, 0, 23);

console.log(myCreatedDate.toDateString());

// Output:
// Mon Jan 23 2023
```

### **Note**

When creating a date using numbers, **months start from `0`**.

| **Month** | **Index** |
| --- | --- |
| January | 0 |
| February | 1 |
| March | 2 |
| April | 3 |
| May | 4 |
| June | 5 |
| July | 6 |
| August | 7 |
| September | 8 |
| October | 9 |
| November | 10 |
| December | 11 |

So,

```jsx
new Date(2023, 0, 23)
```

means

```
23 January 2023
```

### **Creating a Date Using a String**

JavaScript also allows creating dates using a **string**.

**Example:**

```jsx
let myCreatedDate = new Date("2023-01-14");

console.log(myCreatedDate.toLocaleString());

// Output:
// 14/1/2023, 12:00:00 am
```

### **Why can’t we write `"2023-00-14"`?**

When using the **string format (`YYYY-MM-DD`)**, months are **1-based**, not 0-based.

Valid months are:

```
01 → January
02 → February
...
12 → December
```

So, `"2023-01-14"` means `14 January 2023`  whereas `"2023-00-14"` is **invalid**, because 
month `00` does not exist.

**Remember:**

- `new Date(2023, 0, 14)` → Numeric format → Month starts from **0**
- `new Date("2023-01-14")` → String format → Month starts from **1**

---

## **Timestamp**

A **Timestamp** is the total number of **milliseconds** elapsed since **January 1, 1970 (UTC)**.

It is commonly used to compare dates and calculate time differences.

**Example:**

```jsx
let myTimeStamp = Date.now();

console.log(myTimeStamp);

// Output (example):
// 1785749425123
```

The output will be a large number because it represents the **total milliseconds since January 1, 1970**.

### **Getting the Timestamp of a Specific Date**

The `getTime()` method returns the timestamp of a particular `Date` object.

```jsx
let myCreatedDate = new Date("01-14-2023");

let myTimeStamp = Date.now();

console.log(myTimeStamp);
// Output (example):
// 1785749425123

console.log(myCreatedDate.getTime());
// Output:
// 1673654400000
```

Here,

- `Date.now()` returns the timestamp of the **current date and time**.
- `getTime()` returns the timestamp of the **specified date**.

### **Getting the Current Time in Seconds**

Since `Date.now()` returns **milliseconds**, divide it by `1000` to convert it into seconds.

```jsx
console.log(Math.floor(Date.now() / 1000));

// Output (example):
// 1785749425

let currentTime = new Date();

console.log(
    currentTime.getHours() + ":" +
    currentTime.getMinutes() + ":" +
    currentTime.getSeconds()
);
```

`Math.floor()` removes the decimal part and returns the timestamp in **whole seconds**.

### **Commonly Used Date Methods**

```jsx
let newDate = new Date();

console.log(newDate);
// Output (example):
// Mon Aug 03 2026 14:30:25 GMT+0530 (India Standard Time)
// Returns the complete Date object.

console.log(newDate.getMonth());
// Output:
// 7
// Returns the current month.
// Months are zero-indexed (January = 0).

console.log(newDate.getDay());
// Output:
// 1
// Returns the day of the week.
// Sunday = 0, Monday = 1, Tuesday = 2, ..., Saturday = 6.

console.log(
    newDate.toLocaleString('default', {
        weekday: "long"
    })
);
// Output:
// Monday
// Returns the full name of the current weekday.
```

### **Commonly Used Date Methods**

| **Method** | **Description** |
| --- | --- |
| `new Date()` | Creates a Date object containing the current date and time. |
| `toDateString()` | Returns only the date in a readable format. |
| `toLocaleString()` | Returns the date and time according to the user’s locale. |
| `Date.now()` | Returns the current timestamp in milliseconds. |
| `getTime()` | Returns the timestamp of a specific Date object. |
| `getMonth()` | Returns the month (0–11). |
| `getDay()` | Returns the day of the week (0–6). |
| `toLocaleString(locale, options)` | Formats the date and time according to the specified locale and formatting options. |

```jsx
let currentDate = new Date();

console.log(currentDate);
// Output (example):
// Mon Aug 03 2026 02:30:25 GMT+0530 (India Standard Time)
// Creates a Date object containing the current date and time.

console.log(currentDate.toDateString());
// Output:
// Mon Aug 03 2026
// Returns only the date in a readable format.

console.log(currentDate.toLocaleString());
// Output:
// 3/8/2026, 2:30:25 pm
// Returns the date and time according to the user's locale.

console.log(Date.now());
// Output (example):
// 1785749425123
// Returns the current timestamp in milliseconds.

console.log(currentDate.getTime());
// Output (example):
// 1785749425123
// Returns the timestamp of the Date object in milliseconds.

console.log(currentDate.getMonth());
// Output:
// 7
// Returns the current month (0 = January, 11 = December).

console.log(currentDate.getDay());
// Output:
// 1
// Returns the current day of the week (0 = Sunday, 6 = Saturday).

console.log(
    currentDate.toLocaleString("default", {
        weekday: "long",
    })
);
// Output:
// Monday
// Formats the date using the specified locale and formatting options.
```

---