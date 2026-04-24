🟢 BASIC
📌 1.2 — Variables: var, let, const
🧠 Simple Explanation

A variable is a named box that holds a value. You give the box a name, put something in it, and can refer to it later by name.

🔍 Deep Explanation

JS has three ways to declare variables and they behave very differently.

```// ─── VAR ───────────────────────────────────────
var age = 25;           // function-scoped
var age = 30;           // can be RE-DECLARED (no error!)
age = 35;               // can be re-assigned

// ─── LET ───────────────────────────────────────
let name = "Rahul";     // block-scoped
// let name = "Raj";    // ERROR: cannot re-declare in same scope
name = "Raj";           // re-assignment is fine

// ─── CONST ─────────────────────────────────────
const PI = 3.14159;     // block-scoped, cannot be re-assigned
// PI = 3;              // ERROR: Assignment to constant variable

// ⚠️ IMPORTANT: const with objects/arrays is TRICKY
const person = { name: "Rahul" };
person.name = "Raj";    // THIS WORKS! The binding is const, not the contents
person = {};            // ERROR: this tries to rebind the variable itself ```

## 📊 Scope Comparison Table

| Feature        | var            | let           | const         |
|---------------|----------------|---------------|---------------|
| Scope         | function       | block         | block         |
| Hoisted       | YES (undef)    | YES (TDZ)     | YES (TDZ)     |
| Re-declare    | YES            | NO            | NO            |
| Re-assign     | YES            | YES           | NO            |
| Global object | YES            | NO            | NO            |

🟡 INTERMEDIATE

📦 What is block scope?
// A block is anything between { }
```if (true) {
    var x = 10;   // leaks OUT of the block (function-scoped)
    let y = 20;   // stays INSIDE the block
    const z = 30; // stays INSIDE the block
}

console.log(x);  // 10 ← var leaked out
console.log(y);  // ReferenceError ← let did not leak
console.log(z);  // ReferenceError ← const did not leak```

## 📌 Function Scope

**Definition:**
Function scope means a variable declared inside a function (using `var`) can only be used within that function, not outside it.

---

### ✅ Example

```js
function test() {
    var message = "Hello";
    console.log(message); // ✅ works inside function
}

test();

console.log(message); // ❌ ReferenceError (outside function)
```

👉 `message` exists only inside the function, so it cannot be accessed outside.


⏳ What is the Temporal Dead Zone (TDZ)?

let and const are hoisted (moved to top of scope) but NOT initialized. The time between the start of the scope and the declaration line is the TDZ. Accessing a variable in the TDZ throws a ReferenceError.

```console.log(a);  // undefined  ← var is hoisted and initialized to undefined
console.log(b);  // ReferenceError ← let is in TDZ here

var a = 5;
let b = 10;```


🔴 ADVANCED

🧠 Memory — where do variables live?

Stack Memory (fast, limited)      Heap Memory (slow, large)
─────────────────────────────     ─────────────────────────
Primitives stored directly:       Objects/arrays stored here:
  let x = 5       → [5]            let obj = {a:1} → [address → {a:1}]
  let name = "hi" → ["hi"]         let arr = [1,2] → [address → [1,2]]
  const b = true  → [true]

Primitives are stored by value on the stack. Objects/arrays are stored by reference — the variable holds a memory address that points to the heap.

```⚠️ Common Mistakes & Where People Get Stuck

Mistake 1: Using var in loops (classic bug)
// BAD — var leaks, all callbacks share the same i
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3  ← NOT 0, 1, 2

// GOOD — let creates a new binding per iteration
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}

// Output: 0, 1, 2  ✓


Mistake 2: Thinking const = immutable
const arr = [1, 2, 3];
arr.push(4);        // This WORKS. arr is still [1, 2, 3, 4]
arr = [1, 2, 3, 4]; // This FAILS. You can't rebind const.


Mistake 3: Re-declaring var accidentally
var score = 100;
// ... 200 lines later ...
var score = 0;  // No error! Silently overwrites. Debugging nightmare.

// With let, this would be caught immediately as an error.
Mistake 4: Global var pollution
var globalVar = "I leak to window object";
console.log(window.globalVar); // "I leak to window object" — dangerous!

let safeVar = "I don't pollute global";
console.log(window.safeVar); // undefined — safe```

💡 Real-world Rule

In modern JavaScript: always use const by default. Use let only when you know the value will change. Never use var.

```🧪 Practice Questions
What is the output of this code and why?
let x = 1;
{
    let x = 2;
    console.log(x);
}
console.log(x);
2. Why does const arr = [] allow arr.push(1) but not arr = [1]?
3. What is the Temporal Dead Zone? Write code that triggers it.
4. What's the difference between undefined and ReferenceError when accessing undeclared variables?
5. Explain why var in a for loop causes the famous "3,3,3" bug.

```🛠 Coding Assignment 1.2
// Task 1: Predict the output BEFORE running, then verify
var a = 1;
let b = 2;
const c = 3;

function test() {
    var a = 10;
    let b = 20;
    console.log(a, b, c);
}
test();
console.log(a, b);```

```// Task 2: Fix this buggy code
const user = {
    name: "Alice",
    age: 25
};
// user = { name: "Bob" };   // why does this fail?
user.age = 26;               // why does this work?

// Task 3: Write a loop that correctly logs 0, 1, 2, 3, 4
// using var (hint: you'll need to think creatively)
// then rewrite it cleanly with let```