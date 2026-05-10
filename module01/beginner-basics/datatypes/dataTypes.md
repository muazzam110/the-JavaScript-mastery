# 1.3 — Data Types: Primitives & Reference Types

## 🧠 Simple Explanation

Think of primitives as individual sticky notes — each holds one value, and when you copy them you get a fresh copy. Objects are like shared whiteboards — multiple people (variables) can point to the same whiteboard and see each other's changes.

---

# 🔍 Deep Explanation

## The 7 Primitive Types:

```javascript
// 1. Number — integers AND decimals (no separate int/float)
let age = 25;
let price = 19.99;
let negative = -100;
let big = 1e6;          // 1,000,000 (scientific notation)

// Special number values:
console.log(Infinity);  // mathematical infinity
console.log(-Infinity);
console.log(NaN);       // "Not a Number" — result of invalid math

// ⚠️ Famous floating point problem
console.log(0.1 + 0.2); // 0.30000000000000004 ← NOT 0.3!
// Why? IEEE 754 binary float representation. 0.1 can't be represented exactly in binary.
// Fix: Math.round((0.1 + 0.2) * 100) / 100 === 0.3 ✓

// 2. String
let single = 'hello';
let double = "world";
let template = `Hello, ${age} years old`;  // Template literal (ES6)
let multiline = `Line 1
Line 2`;

// String is immutable — you can't change individual characters
let str = "hello";
str[0] = "H";       // silently fails in non-strict mode
console.log(str);   // still "hello"

// 3. Boolean
let isLoggedIn = true;
let hasError = false;

// 4. Undefined — variable declared but no value assigned
let x;
console.log(x);     // undefined

// 5. Null — intentional absence of value (you set this on purpose)
let user = null;    // "I know there's no user here"

// ⚠️ The famous null bug:
console.log(typeof null);  // "object" ← this is a 20-year-old bug in JS!
// null is NOT an object. typeof null === "object" is a historical mistake.

// 6. Symbol (ES6) — unique identifier, advanced use
const id1 = Symbol("id");
const id2 = Symbol("id");
console.log(id1 === id2);  // false — every Symbol is unique

// 7. BigInt (ES2020) — integers larger than Number can hold
const hugeNumber = 9007199254740991n;  // note the 'n' suffix
```

---

# The 1 Reference Type (Object):

```javascript
// Everything that is NOT a primitive is an Object
const obj = { name: "Rahul", age: 25 };  // plain object
const arr = [1, 2, 3];                    // array (is an object)
const fn  = function() {};                // function (is an object)
const dt  = new Date();                   // date (is an object)
const rx  = /hello/;                      // regex (is an object)
```

---

# The critical difference — value vs reference:

```javascript
// PRIMITIVES: copied by VALUE
let a = 5;
let b = a;     // b gets a COPY of the value
b = 10;
console.log(a); // 5 — a is unchanged

// OBJECTS: copied by REFERENCE (memory address)
let obj1 = { name: "Rahul" };
let obj2 = obj1;         // obj2 points to the SAME object
obj2.name = "Raj";
console.log(obj1.name);  // "Raj" — obj1 was ALSO changed!
// They both point to the same place in heap memory
```

---

# Type checking:

```javascript
typeof 42           // "number"
typeof "hello"      // "string"
typeof true         // "boolean"
typeof undefined    // "undefined"
typeof null         // "object"  ← bug!
typeof {}           // "object"
typeof []           // "object"  ← also object, not "array"!
typeof function(){} // "function"

// Better check for arrays:
Array.isArray([])   // true  ✓
Array.isArray({})   // false ✓

// Better null check:
value === null      // true if null ✓
```

---

# Type Coercion (where JS surprises everyone):

```javascript
// Implicit coercion — JS converts types automatically
console.log("5" + 3);    // "53"  ← number 3 becomes string "3"
console.log("5" - 3);    // 2     ← string "5" becomes number 5
console.log("5" * "3");  // 15    ← both become numbers
console.log(true + 1);   // 2     ← true becomes 1
console.log(false + 1);  // 1     ← false becomes 0
console.log(null + 1);   // 1     ← null becomes 0
console.log(undefined + 1); // NaN ← undefined becomes NaN

// == vs === (CRUCIAL)
console.log(5 == "5");   // true  ← == coerces types first
console.log(5 === "5");  // false ← === checks type AND value
console.log(null == undefined);  // true  ← special rule
console.log(null === undefined); // false

// Falsy values (evaluate to false in boolean context):
// false, 0, "", null, undefined, NaN
// Everything else is truthy
if (0)         console.log("runs"); // doesn't run
if ("")        console.log("runs"); // doesn't run
if ([])        console.log("runs"); // RUNS! Empty array is truthy
if ({})        console.log("runs"); // RUNS! Empty object is truthy
```

---

# ⚠️ Things People Get Stuck On

## NaN is not equal to itself:

```javascript
console.log(NaN === NaN);  // false — only value in JS not equal to itself

// Check for NaN:
Number.isNaN(NaN);   // true ✓
isNaN("hello");      // true (converts first — less reliable)
Number.isNaN("hello"); // false (doesn't convert — more reliable)
```

---

# null vs undefined:

```javascript
// undefined: JS assigned this automatically (uninitialized)
// null: YOU assigned this intentionally (empty value)

let a;           // undefined — JS set this
let b = null;    // null — you set this intentionally

// Practical difference:
function getUser() {
    // return undefined; // function has no return = undefined
    return null;  // intentionally saying "no user found"
}
```

---

# String immutability:

```javascript
let str = "hello";
let upper = str.toUpperCase();  // returns NEW string

console.log(str);    // "hello" — unchanged
console.log(upper);  // "HELLO" — new string
```

---

# 🧪 Practice Questions

1. What is the output of typeof null and why is it considered a bug?

2. What's the difference between == and ===? When would you use ==?

3. Why does [] + [] give "" and [] + {} give "[object Object]"?

4. List all falsy values in JavaScript.

5. What's the difference between Number.isNaN() and isNaN()?

---

# 🛠 Coding Assignment 1.3

```javascript
// Task 1: Predict output for each line
console.log(typeof null);
console.log(typeof []);
console.log(typeof function(){});
console.log(null == undefined);
console.log(null === undefined);
console.log(NaN === NaN);
console.log(0.1 + 0.2 === 0.3);

// Task 2: Fix this function that checks if a value is a real number
function isRealNumber(val) {
    return typeof val === "number"; // BUG: this returns true for NaN!
}
// Fix it so isRealNumber(NaN) returns false

// Task 3: Demonstrate the reference type problem and fix it
let original = { score: 100 };
let copy = original;
copy.score = 0;

console.log(original.score); // This will show 0 — why? How do you fix it?
```