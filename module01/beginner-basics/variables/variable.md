````markdown
# 📌 1.2 — Variables: var, let, const

---

## 🧠 Simple Explanation

A variable is a named box that holds a value. You give the box a name, put something in it, and can refer to it later by name.

---

## 🔍 Deep Explanation

JavaScript has three ways to declare variables, and they behave very differently.

```javascript
// ─── VAR ───────────────────────────────────────
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
person = {};            // ERROR: this tries to rebind the variable itself
````

> **Note:** `const` protects the reference, not the internal data of objects/arrays.

---

## 📊 Scope Comparison Table

| Feature       | var         | let       | const     |
| ------------- | ----------- | --------- | --------- |
| Scope         | function    | block     | block     |
| Hoisted       | YES (undef) | YES (TDZ) | YES (TDZ) |
| Re-declare    | YES         | NO        | NO        |
| Re-assign     | YES         | YES       | NO        |
| Global object | YES         | NO        | NO        |

---

## 📦 Block Scope

A block is anything inside `{}`.

```javascript
if (true) {
    var x = 10;   // leaks OUT of the block (function-scoped)
    let y = 20;   // stays INSIDE the block
    const z = 30; // stays INSIDE the block
}

console.log(x);  // 10 ← var leaked out
console.log(y);  // ReferenceError ← let did not leak
console.log(z);  // ReferenceError ← const did not leak
```

> **Warning:** `var` ignores block scope, which can lead to unexpected bugs.

---

## 📌 Function Scope

### Definition

Function scope means a variable declared inside a function (using `var`) can only be used within that function, not outside it.

---

### Example

```javascript
function test() {
    var message = "Hello";
    console.log(message); // ✅ works inside function
}

test();

console.log(message); // ❌ ReferenceError (outside function)
```

👉 `message` exists only inside the function, so it cannot be accessed outside.

---

## ⏳ Temporal Dead Zone (TDZ)

`let` and `const` are hoisted but NOT initialized. The time between the start of the scope and the declaration line is called the Temporal Dead Zone.

```javascript
console.log(a);  // undefined  ← var is hoisted and initialized
console.log(b);  // ReferenceError ← let is in TDZ

var a = 5;
let b = 10;
```

> **Warning:** Accessing `let` or `const` before declaration throws a ReferenceError.

---

## 🧠 Memory — Where Do Variables Live?

```
Stack Memory (fast, limited)      Heap Memory (slow, large)
─────────────────────────────     ─────────────────────────
Primitives stored directly:       Objects/arrays stored here:
  let x = 5       → [5]            let obj = {a:1} → [address → {a:1}]
  let name = "hi" → ["hi"]         let arr = [1,2] → [address → [1,2]]
  const b = true  → [true]
```

Primitives are stored by value on the stack. Objects/arrays are stored by reference in the heap.

---

## ⚠️ Common Mistakes

### Mistake 1: Using var in loops

```javascript
// BAD — var shares same variable
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3

// GOOD — let creates new binding
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2
```

---

### Mistake 2: Thinking const = immutable

```javascript
const arr = [1, 2, 3];
arr.push(4);        // ✅ works
arr = [1, 2, 3, 4]; // ❌ error
```

---

### Mistake 3: Re-declaring var

```javascript
var score = 100;
// ... later
var score = 0; // ❌ silently overwrites
```

---

### Mistake 4: Global var pollution

```javascript
var globalVar = "I leak to window object";
console.log(window.globalVar); // exists ❌

let safeVar = "I don't pollute global";
console.log(window.safeVar); // undefined ✅
```

---

## 💡 Real-world Rule

> **Best Practice:**

* Use `const` by default
* Use `let` when value changes
* Avoid `var`

---

## 🧪 Practice Questions

```javascript
// 1. What is the output?
let x = 1;
{
    let x = 2;
    console.log(x);
}
console.log(x);

// 2. Why does const arr = [] allow arr.push(1) but not arr = [1]?

// 3. What is the Temporal Dead Zone? Write code for it.

// 4. Difference between undefined and ReferenceError?

// 5. Why does var cause "3,3,3" bug?
```

---

## 🛠 Coding Assignment 1.2

### Task 1

```javascript
var a = 1;
let b = 2;
const c = 3;

function test() {
    var a = 10;
    let b = 20;
    console.log(a, b, c);
}
test();
console.log(a, b);
```

---

### Task 2

```javascript
const user = {
    name: "Alice",
    age: 25
};

// user = { name: "Bob" }; ❌
user.age = 26;          // ✅
```

---

### Task 3

```javascript
// Write a loop that prints:
// 0 1 2 3 4

// First using var
// Then using let
```

---

```
```
