# JavaScript Engine

A **JavaScript Engine** is responsible for executing JavaScript code. It acts as a bridge between the JavaScript code written by the developer and the computer’s CPU. Whenever a JavaScript program is executed, the engine reads the source code, verifies that it follows JavaScript syntax, converts it into internal representations, optimises frequently executed code, and finally generates machine code that the CPU can execute. Every JavaScript runtime has its own engine—for example, **Google Chrome, Microsoft Edge (Chromium), and Node.js use the V8 Engine**, **Mozilla Firefox uses SpiderMonkey**, **Safari uses JavaScriptCore (Nitro)**, and the older version of **Microsoft Edge used Chakra**.

#### **Overall Working of the JavaScript Engine**

The Main Parts of JavaScript’s Engine are

1. Parser
2. Interpreter
3. JIT Compiler
4. Garbage Collector

The complete execution flow of a JavaScript program is as follows:

```
                 JavaScript Code (.js)
                          │
                          ▼
                     JavaScript Engine
                          │
                          ▼
                    1. Parser
                          │
                          ▼
               Abstract Syntax Tree (AST)
                          │
                          ▼
                 2. Interpreter
             (Generates Bytecode from AST)
                          │
                          ▼
                 Executes the Bytecode
                          │
            Frequently Executed Code?
                   Yes              No
                    │                │
                    ▼                ▼
          3. JIT Compiler      Continue Executing
                    │
                    ▼
     Optimized Machine Code
                    │
                    ▼
             CPU Executes Code
                    │
                    ▼
             Output is Displayed
```

Let’s understand each stage in detail.

### **Step 1: JavaScript Source Code**

Everything begins with the JavaScript source code written by the developer. At this stage, the code is simply a JS file containing characters that humans can read and understand. However, the CPU cannot understand this because it can execute only machine instructions consisting of binary values (0s and 1s). Therefore, before the program can run, the JavaScript engine must process this source code and gradually convert it into a format that the computer can understand.

```jsx
let a = 10;
let b = 20;

console.log(a + b);
```

### **Step 2: Parser**

The first component that receives the source code is the **Parser**. Its primary responsibility is to verify that the code follows JavaScript’s syntax rules. If the parser encounters any invalid syntax, such as a missing value or an unmatched bracket, it immediately throws a **SyntaxError**, and execution stops before the program runs.

For example,

```jsx
let a = ;
```

Since the value after `=` is missing, the parser cannot understand the statement and produces a syntax error.

If the source code is syntactically correct, the parser converts it into an **Abstract Syntax Tree (AST)**.

### **What is an Abstract Syntax Tree (AST)?**

An **Abstract Syntax Tree (AST)** is a tree-like representation of the logical structure of a JavaScript program. Instead of storing the original text exactly as written by the developer, the AST organises the program into meaningful components such as variables, functions, operators, expressions, loops, and conditions. This structured representation is much easier for the JavaScript engine to analyse and process than plain text.

For example,

```jsx
let sum = 10 + 20;
```

can be represented conceptually as:

```
Assignment
│
├── Variable
│      │
│      └── sum
│
└── Addition
       │
       ├── 10
       └── 20
```

Instead of reading the original JavaScript text, the engine now understands the program as a series of logical operations:

- Create a variable named `sum`.
- Add the values `10` and `20`.
- Store the result in `sum`.

This structured representation simplifies the next stages of execution.

### **Step 3: Interpreter**

Once the parser creates the AST, it is passed to the **Interpreter**. Unlike traditional interpreters that execute source code line by line, modern JavaScript interpreters (such as V8’s Ignition) work on the AST. They traverse the AST node by node, following its tree structure to understand the program’s logic, and generate **bytecode**, which is a low-level intermediate representation of the program. As soon as the bytecode is generated, the interpreter begins executing it immediately, allowing the program to start running without waiting for the entire program to be compiled into machine code. While executing the bytecode, the JavaScript engine continuously monitors the program. It identifies frequently executed (hot) code, which can later be optimised by the Just-In-Time (JIT) compiler into machine code for faster execution. This approach provides both **fast startup** and **high performance**.

### **What is Bytecode?**

**Bytecode** is a low-level intermediate representation of a JavaScript program that lies between JavaScript source code and machine code. It is much closer to machine language than JavaScript, but it is still independent of the computer’s hardware and cannot be executed directly by the CPU. Because bytecode is simpler than the source code, it can be generated quickly and executed immediately, allowing JavaScript programs to start running without the delay of generating fully optimised machine code. Later, if certain parts of the program execute repeatedly, the bytecode for those sections can be converted into optimised machine code by the JIT compiler.

### **Step 4: JIT (Just-In-Time) Compiler**

While the interpreter executes the bytecode, the JavaScript engine continuously monitors the program to identify **hot code**, which refers to functions, loops, or other sections that are executed repeatedly. Instead of interpreting the same bytecode again and again, the engine sends these frequently executed sections to the **Just-In-Time (JIT) Compiler**. The JIT compiler translates the bytecode into highly optimised machine code that is specific to the computer’s processor. Once this optimised machine code has been generated, future executions of that code bypass the interpreter and are executed directly by the CPU, resulting in significantly faster performance.

For example,

```jsx
for(let i = 0; i < 1000000; i++){
    total += i;
}
```

Since the loop executes one million times, it becomes **hot code**, making it worthwhile for the JIT compiler to optimise it.

#### **Why is it called “Just-In-Time”?**

The term **Just-In-Time (JIT)** refers to the fact that the compiler performs optimisation **while the program is running**, rather than compiling the entire application before execution begins. Only those portions of the program that prove to be performance-critical are compiled into optimised machine code, ensuring both fast startup and efficient execution.

### **Step 5: Machine Code Execution**

Once the JIT compiler generates optimised machine code, it is sent to the CPU for execution. Since the CPU understands only machine instructions, it can execute these instructions directly without any further translation.

For example,

```jsx
let x = 5;
let y = 10;

console.log(x + y);
```

Internally, the CPU performs operations similar to:

- Allocate memory for `x`.
- Store the value `5`.
- Allocate memory for `y`.
- Store the value `10`.
- Retrieve both values from memory.
- Perform the addition.
- Pass the result to `console.log()`.

Finally,

```
15
```

is displayed in the console.

---

### Step 6: Garbage Collector

The **Garbage Collector (GC)** is a component of the JavaScript runtime responsible for **automatically managing memory**. When JavaScript creates variables, objects, arrays, functions, and other values, memory is allocated to store them. As the program continues running, some of these values are no longer needed. If that unused memory were never released, the program would continuously consume more and more memory, eventually causing performance problems or a **memory leak**.

The Garbage Collector automatically identifies objects and values that are **no longer reachable or usable by the program** and reclaims the memory occupied by them. This allows JavaScript developers to use memory without manually allocating and freeing it as they would in languages such as C or C++.

The Garbage Collector generally works alongside the JavaScript engine’s **heap**, where dynamically created objects and other data are stored.

---

### **Complete Internal Working**

The JavaScript engine executes it in the following sequence:

1. The browser loads the JavaScript source code and passes it to the JavaScript engine.
2. The **Parser** verifies that the program follows JavaScript syntax rules. If any syntax error is found, execution stops immediately. Otherwise, the parser converts the source code into an **Abstract Syntax Tree (AST)** that represents the logical structure of the program.
3. The **Interpreter** traverses the AST, generates **bytecode**, and immediately begins executing the generated bytecode, allowing the program to start running without waiting for full compilation.
4. While executing the bytecode, the engine monitors the program to identify **hot code**. Frequently executed functions or loops are sent to the **JIT Compiler**, which converts their bytecode into optimized machine code.
5. The optimized machine code is executed directly by the CPU, eliminating the need to repeatedly interpret the same bytecode and significantly improving performance.
6. Finally, the program produces the expected output:

### **Complete Execution Flow**

```
                 JavaScript Code
                        │
                        ▼
              JavaScript Engine Starts
                        │
                        ▼
              Parser Checks Syntax
                        │
                        ▼
          Generates Abstract Syntax Tree
                        │
                        ▼
      Interpreter Traverses the AST
                        │
                        ▼
          Generates and Executes Bytecode
                        │
                        ▼
      Detects Frequently Executed (Hot) Code
                        │
                        ▼
          JIT Compiler Optimizes the Code
                        │
                        ▼
      Converts to Optimized Machine Code
                        │
                        ▼
            CPU Executes Machine Code
                        │
                        ▼
                 Output is Displayed
```

---