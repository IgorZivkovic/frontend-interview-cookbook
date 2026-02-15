# Frontend Interview Cookbook

[![Stars](https://img.shields.io/github/stars/IgorZivkovic/frontend-interview-cookbook?style=flat-square)](https://github.com/IgorZivkovic/frontend-interview-cookbook/stargazers)
[![Forks](https://img.shields.io/github/forks/IgorZivkovic/frontend-interview-cookbook?style=flat-square)](https://github.com/IgorZivkovic/frontend-interview-cookbook/network/members)
[![Issues](https://img.shields.io/github/issues/IgorZivkovic/frontend-interview-cookbook?style=flat-square)](https://github.com/IgorZivkovic/frontend-interview-cookbook/issues)
[![Last Commit](https://img.shields.io/github/last-commit/IgorZivkovic/frontend-interview-cookbook?style=flat-square)](https://github.com/IgorZivkovic/frontend-interview-cookbook/commits/main)
[![License](https://img.shields.io/github/license/IgorZivkovic/frontend-interview-cookbook?style=flat-square)](https://github.com/IgorZivkovic/frontend-interview-cookbook/blob/main/LICENSE)


## Frontend Interview Questions (Q&A)

> **The same core questions keep showing up in frontend interviews.**
> **This is the cheat sheet for answering them well.**

**What makes this different:**
- Answers at **interview depth** (not too shallow, not academic)
- Practical examples where they add interview value
- Common **pitfalls and red flags** interviewers look for
- Covers **JS, React, Node, Vue, Angular, HTML, CSS**

**No fluff. Just what helps you pass the interview.**

## Table of Contents

- [Frontend Interview Questions (Q&A)](#frontend-interview-questions-qa)
- [JavaScript Fundamentals](#javascript-fundamentals)
- [Markup (HTML5+)](#markup-html5)
- [Styling](#styling)
- [Node.js Fundamentals](#nodejs-fundamentals)
- [React Deep Dive](#react-deep-dive)
- [Vue Deep Dive](#vue-deep-dive)
- [Angular Deep Dive](#angular-deep-dive)
- [ES6 Deep Dive](#es6-deep-dive)
- [Bundlers & Compilers](#bundlers--compilers)
- [Threading & Data Streams](#threading--data-streams)
- [Networking & Processing](#networking--processing)
- [Libraries & State Management](#libraries--state-management)
- [Tools & Testing](#tools--testing)
- [Architecture & Patterns](#architecture--patterns)
- [Git](#git)
- [Agile, Kanban & Scrum](#agile-kanban--scrum)
- [SEO & Accessibility](#seo--accessibility)
- [Internationalization (i18n) & Theming](#internationalization-i18n--theming)
- [Performance Optimization & Proxying](#performance-optimization--proxying)
- [Angular vs React vs Vue Cheat Sheet](#angular-vs-react-vs-vue-cheat-sheet)
- [Bonus](#bonus)
- [Changelog](#changelog)
- [Author](#author)
- [License](#license)

## JavaScript Fundamentals

### Type system

#### Definition

JavaScript has a dynamic, weakly-typed (a.k.a. “loose”) type system. Variables are not bound to a type - instead, values carry one of the seven primitive types (undefined, null, boolean, number, bigint, string, symbol) or an object type (which includes arrays, dates, etc.). Functions are a callable object subtype. Type checks happen at runtime and you may freely assign any type of value to the same variable without a compile-time error.
#### Example

```
let x = 42;     // number
x = "answer";   // string
x = { value: 42 }; // object
console.log(typeof x); // "object"
```

#### Common Interview Questions

- **How many primitive types does JavaScript have and why are both null and undefined needed?** Seven primitives: undefined, null, boolean, number, bigint, string, symbol. undefined means “variable declared but no value assigned”; null is an intentional assignment of “no value”.
- **What is the difference between primitive values and objects with respect to mutability and comparison?** Primitives are immutable and compared by value (=== compares the actual data), whereas objects are mutable and compared by reference (=== checks if two variables point to the same object in memory).
- **How do typeof and instanceof work?** typeof returns a string indicating the operand’s type (with a quirk: typeof null is ‘object’). instanceof checks if an object is an instance of a constructor by traversing its prototype chain.
- **Explain why typeof null === "object" returns true. Is this a bug?** It is a long-standing quirk in JavaScript’s implementation stemming from the original 32-bit tagged representation of values. null has its type tag set to 0, which was interpreted as “object”. It’s officially recognized as a historical bug but kept for backward compatibility.
- **What is the difference between null and undefined?** undefined indicates a variable has been declared but not assigned a value and is the default return value of functions. null is an assignment value representing the intentional absence of any object.

### Type coercion

#### Definition

Type coercion is JavaScript’s conversion of a value from one type to another. It can be implicit (done automatically, e.g., in ==, arithmetic, or string concatenation) or explicit (developer-initiated with helpers like Number(), String(), Boolean()).
#### Example

```
console.log(5 + "10");   // "510" (number → string, concatenation)
console.log("5" * 2);    // 10    (string → number)
console.log(5 == "5");   // true  (loose equality after coercion)
console.log(5 === "5");  // false (strict equality, no coercion)

// Special cases
console.log(null == undefined); // true (special == rule)
console.log(null == 0);         // false
```

#### Common Interview Questions

- **What is the difference between implicit and explicit type coercion?** Implicit coercion happens automatically during an operation ("5" + 2 → "52"). Explicit coercion uses helpers like Number("5") or String(42). Implicit can be surprising; explicit is intentional.
- **How does == differ from ===?** == (loose equality) follows ECMAScript coercion rules and may apply special cases (e.g., null == undefined is true, but null == 0 is false).
  === (strict equality) compares both value and type with no coercion.
- **What happens when the + operator is used with mixed types?** If either operand is a string, + performs string concatenation. Otherwise, it coerces operands to numbers.
  "3" + 2 → "32"
  2 + "3" → "23"
  2 + 3 → 5

### Prototypical inheritance

#### Definition

JavaScript uses prototype-based inheritance: every object has an internal link ([[Prototype]], accessible via Object.getPrototypeOf(obj) or __proto__) to another object called its prototype. When you access a property or method that isn’t found on the object itself, the engine follows this link up the prototype chain until it’s found (or until it reaches null). Functions double as constructor functions, setting their prototype object as the prototype of the instances they create.
#### Example

```
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function () {
  return `${this.name} makes a noise`;
};

const dog = new Animal("Rex");
console.log(dog.speak()); // "Rex makes a noise"

// Modern best practice:
console.log(Object.getPrototypeOf(dog) === Animal.prototype); // true

// Legacy (still works, but discouraged):
console.log(dog.__proto__ === Animal.prototype); // true
```

#### Common Interview Questions

- **How does property lookup work in the prototype chain?** The engine first checks the object itself. If the key is absent, it looks at the object’s [[Prototype]], repeating the process up the chain until it finds the property or hits null. The first match stops the search.
- **What is the difference between Object.create() and using new with a constructor function?** Object.create(proto) directly creates a new object with proto as its prototype and lets you define properties via a second argument. Using new Constructor() runs the constructor function (allowing initialization logic) and sets the resulting object’s prototype to Constructor.prototype automatically.
- **Why might you place shared methods on the constructor’s prototype instead of inside the constructor function itself?** Methods placed on Constructor.prototype are shared by all instances, saving memory and ensuring behavior consistency. If you define the same function inside the constructor, each instance gets its own copy, which is inefficient.
- **What are the different ways to create objects?**
  - Object literals: `const obj = {};`
  - Constructor functions: `function Car() {}; const myCar = new Car();`.
  - `Object.create(proto)`: creates an object with a specified prototype.
  - ES6 classes: syntactic sugar over prototypes: `class Car {}; const myCar = new Car();`.

### Pass by value/reference

#### Definition

In JavaScript all function arguments are passed by value.

- For primitive variables the actual data is copied, so the callee can’t affect the caller’s variable.
- For object variables the reference value (a pointer) is copied. The callee can mutate the same underlying object through that reference, but reassigning the parameter only changes the local copy of the reference, leaving the caller’s variable untouched.
#### Example

```
function demo(num, obj) {
  num += 1;            // local change
  obj.count += 1;      // mutates shared object
  obj = { count: 0 };  // re‑binds local reference
}
let n   = 5;
let box = { count: 5 };
demo(n, box);
console.log(n);   // 5  (unchanged)
console.log(box); // { count: 6 } (mutated)
```

#### Common Interview Questions

- **What actually gets copied when you pass an object to a function in JavaScript?** The variable’s reference value - a pointer to the object - is copied. Both caller and callee now hold separate references to the same object in memory.
- **Why does reassigning an object parameter inside a function not affect the original object?** Reassignment simply changes the local parameter to point elsewhere; it doesn’t modify the original reference held by the caller.
- **How can you protect an object argument from unintended mutation inside a function?** Pass a defensive copy: demo(structuredClone(obj)), demo({...obj}), or deep-clone with libraries/JSON.parse(JSON.stringify(obj)). Mutations then affect only the clone, not the original.

### This keyword

#### Definition

The this keyword in JavaScript refers to the execution context of a function and is determined by how the function is called, not where it's defined. JavaScript has four main binding rules that determine what this references, plus special behavior for arrow functions. Understanding these rules is crucial for predicting function behavior in different calling contexts.
#### Example

```
// 1. Default binding (standalone function call)
function standAlone() {
    console.log(this); // Window (or undefined in strict mode)
}
standAlone();

// 2. Implicit binding (method call)
const obj = {
    name: "Alice",
    greet() {
        console.log(`Hello, ${this.name}`); // "Hello, Alice"
    }
};
obj.greet();

// 3. Explicit binding (call/apply/bind)
function introduce() {
    console.log(`I'm ${this.name}`);
}
const person = { name: "Bob" };
introduce.call(person); // "I'm Bob"

// 4. new binding (constructor call)
function Person(name) {
    this.name = name;
}
const john = new Person("John");
console.log(john.name); // "John"

// 5. Arrow functions (lexical this)
const arrowObj = {
    name: "Carol",
    greet: () => {
        console.log(`Hello, ${this.name}`); // "Hello, undefined" (captures outer this)
    }
};
arrowObj.greet();
```

#### Common Interview Questions

- **Explain the this keyword and its binding rules.** this is determined by how a function is called. Default binding: in a standalone function this refers to the global object (or undefined in strict mode). Implicit binding: when a function is called as a method, this refers to the object. Explicit binding: use call, apply or bind to set this. new binding: when a function is invoked with new, this refers to the new object being constructed. Arrow functions don't have their own this and instead capture the this of the enclosing context.
- **What is the difference between arrow functions and regular functions regarding this?** Arrow functions inherit this from their surrounding (lexical) scope at the time they are defined, while regular functions have their own this binding determined by how they are called.
- **How does strict mode affect this binding?** In strict mode, default binding sets this to undefined instead of the global object, preventing accidental pollution of the global scope.
- **What is the precedence order of the this binding rules?** new binding > explicit binding (call/apply/bind) > implicit binding (method call) > default binding (standalone function).
- **Why might this be undefined or unexpected in callback functions?** When passing a method as a callback, it loses its original context and this defaults to the global object or undefined. Solutions include using bind(), arrow functions, or passing wrapper functions.

### Object configuration

#### Definition

Object configuration is the practice of controlling how an object and its properties behave by using property descriptors and extensibility helpers. With Object.defineProperty/defineProperties you set the attributes value, writable, enumerable, and configurable, and with helpers such as Object.preventExtensions, Object.seal, and Object.freeze you restrict whether new properties can be added, existing ones removed, or values modified.
#### Example

```
const user = {};
Object.defineProperty(user, 'id', {
  value: 123,
  writable: false,
  enumerable: false,
  configurable: false
});
console.log(user.id);           // 123
user.id = 456;                  // ignored in strict mode
console.log(Object.keys(user)); // []
Object.freeze(user);            // object now fully immutable
```

#### Common Interview Questions

- **What does the configurable attribute of a property descriptor control?** If configurable: false, the property cannot be deleted and its descriptor (other than writable from true to false) cannot be changed.
- **Compare Object.seal and Object.freeze.** Object.seal(obj) stops adding or deleting properties but leaves existing properties’ writability intact. Object.freeze(obj) does the same and sets every existing data property’s writable attribute to false, making the entire object immutable.
- **How would you create a property that is read-only and hidden from enumeration?** Use Object.defineProperty with { writable: false, enumerable: false }. Example: Object.defineProperty(obj, 'secret', { value: 42, writable: false, enumerable: false, configurable: true });

### Property getters/setters

#### Definition

JavaScript supports accessor properties - functions that run when a property is read (getter) or assigned (setter). They are declared with the get and set keywords (shorthand) or via Object.defineProperty. Accessor properties store no value themselves; they compute or validate on access while appearing like “regular” fields to callers.
#### Example

```
const account = {
  _balance: 0,
  get balance() {
    return this._balance.toFixed(2);
  },
  set balance(amount) {
    if (amount < 0) throw Error('negative');
    this._balance = Number(amount);
  }
};
account.balance = 50;
console.log(account.balance); // "50.00"
```

#### Common Interview Questions

- **What is the practical difference between a data property and an accessor property?** A data property holds a value directly (value, writable flags), whereas an accessor property defines get/set functions and stores no value - logic runs on every read or write.
- **Why might you choose Object.defineProperty over the get/set shorthand?** Object.defineProperty lets you also set descriptor flags (enumerable, configurable) and attach getters/setters to an object after creation, useful for libraries or meta-programming.
- **Can a property have both a value and a get/set in its descriptor?** No. A property descriptor is either a data descriptor (value, writable) or an accessor descriptor (get, set). Mixing them throws a TypeError.
### Higher-order functions

#### Definition

A higher-order function (HOF) is any function that takes one or more functions as arguments, returns a function, or both. HOFs enable functional patterns such as callbacks, currying, composition, and lazy evaluation.
#### Example

```
const once = (fn) => {
  let called = false;
  return (...args) => {
    if (!called) {
      called = true;
      return fn(...args);
    }
  };
};
const logOnce = once(console.log);
logOnce("Hello"); // prints "Hello"
logOnce("Hi");    // does nothing
```

#### Common Interview Questions

- **Name three built-in array methods that are higher-order functions and explain why.** Array.prototype.map, filter, and reduce each accept a callback function that they invoke on every element; therefore they meet the definition of HOFs.
- **What is the difference between a callback and a higher-order function?** A callback is the function passed to another function. The function receiving (or returning) the callback is the higher-order function.
- **How do higher-order functions improve code reusability?** They encapsulate common control-flow or data-processing patterns, letting you pass custom logic as callbacks. This separates “what to do” (the callback) from “how to do it” (the HOF), leading to concise, composable, and reusable code.

### Recursion

#### Definition

Recursion is a programming technique in which a function calls itself to solve a problem that can be broken down into similar sub-problems. A recursive algorithm needs a base case (to stop recursing) and a recursive case (that reduces the problem size and calls itself).
#### Example

```
function factorial(n) {
  if (n === 0) return 1;        // base case
  return n * factorial(n - 1);  // recursive case
}
console.log(factorial(5)); // 120
```

#### Common Interview Questions

- **What two elements must every recursive function have?** A base case that returns without further self-calls, preventing infinite recursion, and a recursive case that makes at least one call to itself with a “smaller” or simpler input.
- **Why can deep recursion cause a “Maximum call stack size exceeded” error in JavaScript, and how can you avoid it?** Each recursive call adds a frame to the call stack. If the depth is too large (common with large datasets or missing base cases), the stack overflows. Solutions: convert to an iterative loop, use tail-call optimization (in engines that support it), or implement recursion manually with your own stack/queue.
- **Explain tail recursion and its potential benefit.** A function is tail-recursive when its recursive call is the last operation it performs. This allows the engine (if optimized) to reuse the same stack frame instead of growing the stack.
  Example:
  ```js
  function factorial(n, acc = 1) {
    if (n === 0) return acc;        // base case
    return factorial(n - 1, n * acc); // tail call
  }
  ```
  ECMAScript specifies Tail Call Optimization (TCO), but most JavaScript engines (e.g., V8 in Chrome/Node) do not implement it, mainly for debugging/stack-trace reasons. Safari’s JavaScriptCore supports it.

### Closures & scope

#### Definition

Scope is the set of rules that determines where identifiers (variables, functions) are visible. JavaScript uses lexical (static) scope: visibility is decided by where code is written, not where it runs.

A closure is a function that keeps access to its lexical environment - all the variables that were in scope when the function was created - even after the outer function has finished executing. This lets inner functions remember and interact with private data.
#### Example

```
let planet = "Earth";
function showScopes() {
  let continent = "Europe";
  if (true) {
    let country = "Serbia";
    console.log("Inside block:", planet, continent, country);
  }
  console.log("Inside function:", planet, continent);

  // country is block-scoped  -  not accessible here
  // console.log(country); // ReferenceError
}
showScopes();
console.log("Global only:", planet);
```

Closures capture variables:

```
function makeCounter() {
  let count = 0;
  return () => ++count;
}
const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

#### Common Interview Questions

- **What practical problem do closures solve?** Give one real-world use-case. Closures enable data privacy and stateful functions. Example: a module that exposes increment and get methods but hides the internal count variable, preventing external tampering.
  Contrast var, let, and const in terms of scope. var is function-scoped and hoisted; let and const are block-scoped (limited to {}) and are not initialized until their declaration is evaluated (temporal dead zone). This makes let/const safer for loops/closures because each iteration can capture a unique binding.
- **How can poorly managed closures cause memory leaks, and how do you prevent them?** If a closure holds references to large objects (e.g., DOM nodes) that are no longer needed, the garbage collector cannot reclaim them until the closure is released. Mitigation: null out unused references, remove event listeners, or structure code so closures don’t outlive their useful scope.
- **How do closures behave in loops with var vs let?** With var, closures share the same binding and often read the final value; with let, each iteration gets a new binding. Use let or an IIFE to capture the current value.

### Callbacks

#### Definition

A callback is a function passed as an argument to another function so that it can be invoked at a later time - after a task finishes, at every iteration, in response to an event, etc. Callbacks enable flexible control-flow, especially for asynchronous operations such as timers, I/O, and network requests.
#### Example

```
function readFile(path, callback) {
  setTimeout(() => {
    if (path === 'data.txt') callback(null, 'file contents');
    else callback(new Error('Not found'));
  }, 100);
}
readFile('data.txt', (err, data) => {
  if (err) return console.error(err);
  console.log('Got:', data);
});
```

#### Common Interview Questions

- **What is the difference between a synchronous and an asynchronous callback?** A synchronous callback (e.g., Array.prototype.map) is executed immediately in the same call stack. An asynchronous callback (e.g., setTimeout, event listeners) is queued to run later via the event loop, allowing the current call stack to complete first.
- **Why can heavy use of nested callbacks lead to “callback hell,” and how can it be mitigated?** Deeply nested callbacks are hard to read, test, and handle errors in (“pyramid of doom”). Solutions include breaking functions into smaller named callbacks, using Promises/async-await, or employing utilities like Promise.all for parallel work.
- **What is the “error-first” (Node-style) callback convention and why is it useful?** A callback receives the signature (error, result). On success error is null; on failure it holds an Error object. This unifies success and failure paths, enables early returns on errors, and standardizes APIs across the Node.js ecosystem.

### Chainable methods

#### Definition

Method chaining is a design pattern in which each method returns an object that exposes more methods, allowing multiple operations to be performed in a single, fluent statement (e.g., obj.doA().doB().doC()). Most chains return this (the same instance) or a new wrapper object so the chain can continue.
#### Example

```
class Counter {
  #value = 0;
  inc(n = 1) { this.#value += n; return this; }
  dec(n = 1) { this.#value -= n; return this; }
  log()      { console.log(this.#value); return this; }
}
new Counter().inc(3).dec().log(); // 2
```

#### Common Interview Questions

- **How do you make a method chainable?** Ensure the method returns an object with the next callable methods - most commonly return this; inside class methods so the same instance is passed along.
- **What are the benefits and potential drawbacks of method chaining?** Benefits: concise, readable “fluent” API, fewer temporary variables. Drawbacks: harder debugging if a link returns undefined, risk of hidden side-effects when chaining mutating methods, and chains can become hard to break across lines for error handling.
- **When would you return a new object instead of this in a chain?** If you want an immutable API (e.g., many functional libraries), each call returns a fresh object with the updated state, leaving previous links unchanged. This avoids shared-state bugs at the cost of extra allocations.

### Array methods

#### Definition

JavaScript arrays inherit a rich set of prototype methods that let you add/remove elements, iterate, transform, query, and aggregate data. Methods fall into two broad camps: mutating (change the original array, e.g., push, pop, splice) and non-mutating (return a new array or value, e.g., map, filter, slice).
#### Example

```
const nums = [1, 2, 3, 4, 5];
// Non‑mutating chain
const squaredEvens = nums
  .filter(n => n % 2 === 0) // [2, 4]
  .map(n => n ** 2);        // [4, 16]
console.log(nums);         // [1, 2, 3, 4, 5]
console.log(squaredEvens); // [4, 16]
// Mutating method
nums.splice(2, 1);         // remove element at index 2
console.log(nums);         // [1, 2, 4, 5]
```

#### Common Interview Questions

- **Compare map, forEach, and reduce.**
  - forEach executes a callback for side-effects and always returns undefined.
  - map iterates and returns a new array of the callback’s return values, keeping length 1-to-1.
  - reduce collapses the array into a single accumulated value (number, object, string, etc.) by repeatedly applying a reducer function.
- **Name four array methods that do not mutate the original array and four that do.**
  - Non-mutating: map, filter, slice, concat, toSorted, toSpliced (modern).
  - Mutating: push, pop, shift, unshift, splice, sort, reverse, fill.
- **Why might Array.prototype.find be preferred over filter when you only need one match?** find stops iterating as soon as it locates the first element that satisfies the predicate, returning that single value (or undefined). This is more efficient and expressive than filter, which scans the whole array and returns a new array - even if you only wanted one element.

### Currying

#### Definition

Currying is the technique of transforming a function that takes multiple arguments at once (f(a, b, c)) into a sequence of nested, unary functions (f(a)(b)(c)), each capturing one argument and returning another function until all arguments are provided. The final call returns the result. Currying is useful for partial application, function composition, and creating specialized helpers.
#### Example

```
const curriedAdd = a => b => c => a + b + c;
const addFive = curriedAdd(2)(3);
console.log(addFive(4)); // 9
```

#### Common Interview Questions

- **How does currying differ from partial application?** Currying always transforms a multi-argument function into a chain of unary functions. Partial application fixes one or more arguments of any position and returns a new function expecting the rest. Currying enables partial application, but they are not the same.
- **Give a practical use-case for currying in frontend development.** Event handler factories: `const withLogging = eventName => handler => e => { console.log(eventName); handler(e); };`. You can create `onClick = withLogging('click')(handleClick)` and reuse the logging wrapper for many events by pre-filling eventName.
- **Why is Function.prototype.bind not full currying, yet often used similarly?** bind returns a new function with a fixed this value and/or any subset of leading arguments pre-set. It performs partial application, but the returned function still accepts multiple parameters at once; it doesn’t automatically break them into unary calls as true currying does.

### Memoization

#### Definition

Memoization is an optimization technique where a function caches the result of expensive computations keyed by its argument values. On subsequent calls with the same inputs, the function returns the cached value instead of recalculating, improving performance for pure or mostly-pure functions.
#### Example

```
const memoize = (fn) => {
  const cache = new Map();
  return (...args) => {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
};
const fib = memoize(function f(n) {
  return n < 2 ? n : f(n - 1) + f(n - 2);
});
console.log(fib(40));
```

#### Common Interview Questions

- **When does memoization not help?** If the function is impure (relies on mutable state or side-effects) or rarely called with the same inputs, caching adds overhead without benefit.
- **How does memoization differ from general caching?** Memoization is usually function-scoped and automatic - tied to specific argument sets - whereas caching is broader (API responses, files) and may be managed manually with eviction, TTLs, etc.
- **What pitfalls arise when memoizing functions that accept objects or arrays?** Object/array arguments are compared by reference, not by deep content. Two identical literals {x:1} and {x:1} have different references, producing separate cache entries. Solutions include: use stable references, serialize arguments into a key, or implement a custom deep-hash function.

### Regular expressions (RegExp)

#### Definition

A regular expression (RegExp) is a concise pattern-language for matching, searching, and replacing text. In JavaScript you create them with literals (/pattern/flags) or the RegExp constructor, and use them with string methods (match, replace, search, split) or RegExp methods (test, exec). Common flags include i (ignore case), g (global – find all matches), m (multiline anchors), and u (Unicode).
#### Example

```
const yearRE = /\b(\d{4})\b/g;
const str = "2019 was wet, 2025 is sunny";
console.log(str.match(yearRE)); // ["2019", "2025"]
```

#### Common Interview Questions

- **Explain the difference between test and exec.** Both belong to a RegExp instance. test(str) returns a boolean (was there a match?). exec(str) returns the match object (array of matched text plus captured groups) or null. When the regex has the g or y flag, successive calls to exec continue from the previous match thanks to the regex’s internal lastIndex pointer.
- **Why might /foo/gi.test("FOO") return false on the second call?** With the g flag, test also advances lastIndex. After the first successful match lastIndex sits at the end of the string; the next test starts there and fails. Reset lastIndex = 0 or omit g when you need a simple boolean check.
- **How do lookaheads and lookbehinds help build zero-width assertions?** Give an example. Lookaheads (?= ... ) and lookbehinds (?<= ... ) assert that a pattern must (positive) or must not (negative) appear next to the current position without consuming it. Example: /\d+(?=%)/ matches the number before a percent sign in "Price went up 15% last year" → "15". Another example using a lookbehind: /\d+(?<!\.)/ matches a number that is not preceded by a dot. This would match the 5 in version 5.3 but not the 3 (because the 3 is preceded by a dot).
### ES6 / ES6+ (full feature set)

#### Definition

ES6 (ECMAScript 2015) and ES6 + refer to the annual editions of the ECMAScript specification starting with 2015. Key additions include:

- Block scope & bindings: let, const
- Arrow functions & lexical this
- Template literals with interpolation and tagged templates
- Destructuring (arrays, objects) & default / rest / spread syntax
- Enhanced object literals (shorthand props, computed keys, method syntax)
- Classes and extends / super (plus private #fields, static blocks in later years)
- Modules (import / export, plus top-level await from ES2022)
- Promises, async/await (ES2017), Generators / yield, Iterators / for…of
- Collections & primitives: Map, Set, WeakMap, WeakSet, Symbol, BigInt
- Reflect & Proxy meta-programming APIs
- New operators: exponentiation `**`, optional chaining `?.`, nullish coalescing `??`, and logical assignment `&&=`, `||=`, `??=`.
#### Example

```
import fetchData from './api.js';
const getUser = async (id) => {
  const { data } = await fetchData(`/users/${id}`);
  return {
    id,
    ...data,
    fullName: `${data.first} ${data.last}`
  };
};
getUser(1).then(console.log);
```

#### Common Interview Questions

- **Why are let and const preferable to var, and when would you still choose let over const?** let/const are block-scoped, eliminating hoisting-gotchas and accidental globals. const signals no reassignment (safer defaults). Use let when the variable truly needs reassignment (e.g., loop counters or re-binding in algorithms).
- **Explain how the spread (...) operator differs when used in array vs. object literals and give one practical use-case for each.** In arrays, spread flattens elements: newArr = [...arr1, ...arr2] → concatenation. In objects, it copies enumerable own properties: clone = {...obj} or merges: {...defaults, ...overrides}. Practical cases: array dedup with new Set([...arr]); merging props into React component state {...state, updated}.
- **What problem did async/await solve over plain Promises, and how does it work under the hood?** async/await lets asynchronous code look synchronous, improving readability and error handling (try/catch). Under the hood the async function returns a Promise; each await pauses execution until the awaited value resolves, then resumes by chaining .then behind the scenes.

### Event loop

#### Definition

The event loop is the runtime mechanism that allows JavaScript (single-threaded) to handle non-blocking operations. It repeatedly takes the next task from the task queue (a.k.a. macro-task queue: setTimeout, I/O, UI events) when the call stack is empty, executes it, then processes all micro-tasks created during that turn (Promise reactions, queueMicrotask, MutationObserver) before repainting and starting the next loop tick.
#### Example

```
console.log('sync');       // (1)
setTimeout(() => console.log('timeout'), 0); // macro‑task
Promise.resolve().then(() => console.log('promise')); // micro‑task
console.log('end');        // (2)
// Output: sync → end → promise → timeout
```

#### Common Interview Questions

- **Why does a resolved Promise callback run before a setTimeout(..., 0) callback?** Because Promise reactions are micro-tasks and are flushed immediately after the current script finishes, before the event loop picks the next macro-task such as the timer.
- **What happens if a micro-task continuously enqueues more micro-tasks?** The event loop will keep executing them in the same tick, potentially starving rendering and I/O because the browser/Node cannot proceed to the next macro-task until the micro-task queue is empty.
- **How do requestAnimationFrame and requestIdleCallback fit into the event loop?** requestAnimationFrame callbacks run before repaint in the render phase of a tick, ideal for visual updates. requestIdleCallback runs after rendering when the loop is otherwise idle, letting you schedule low-priority work without jank.
- **What is the difference between window.load and DOMContentLoaded events?** DOMContentLoaded fires when the initial HTML document has been completely loaded and parsed (without waiting for stylesheets/images). window.load fires when the entire page, including all dependent resources, has finished loading.

### Iterators

#### Definition

An iterator is an object that implements the iterator protocol: it has a next() method that returns an object with { value, done }. If done is false the value is the next item; when done becomes true, the iteration ends. Objects that expose an iterator via a Symbol.iterator method are iterables and work with language constructs like for…of, spread (...), array destructuring, and Array.from().
#### Example

```
function count(n) {
  return {
    [Symbol.iterator]() {
      let i = 0;
      return {
        next() {
          i++;
          return { value: i, done: i > n };
        }
      };
    }
  };
}
for (const num of count(3)) console.log(num); // 1 2 3
```

#### Common Interview Questions

- **How do iterables and iterators differ?** An iterable is any object with a Symbol.iterator method that returns an iterator. The iterator performs the actual traversal via its next() calls; the iterable is just the container that can produce one.
- **Why does for…of skip properties added with Array.prototype but iterate array elements?** for…of uses the array’s built-in iterator, which yields index values only; prototype properties are not part of that iteration sequence.
- **What advantages do generator functions offer when creating custom iterators?** Generators (function*) automatically keep state and produce iterators for you. yield replaces manual bookkeeping of value/done, making complex sequences (e.g., infinite streams, async flows with for await…of) easier to implement and read.

### Immutable vs mutable data

#### Definition

Immutable data cannot be changed after it’s created; any “update” produces a new value. Mutable data can be altered in-place. In JavaScript, all primitive values (undefined, null, boolean, number, bigint, string, symbol) are inherently immutable, while objects (including arrays, functions, dates, maps, sets) are mutable unless you deliberately freeze or clone them.
#### Example

```
// Immutable primitive
let a = 'Hi';
a[0] = 'h';  // ignored
a = 'Hello'; // creates a new string
// Mutable object
const user = { name: 'Ann' };
user.name = 'Ana';
// Forcing immutability
const frozen = Object.freeze({ year: 2025 });
frozen.year = 2030; // ignored in strict mode
```

#### Common Interview Questions

- **Which built-in JavaScript types are immutable and why does it matter?** All primitives are immutable: you can’t change a number, boolean, or the characters inside a string - only reassign the variable. This guarantees the value won’t be altered elsewhere, making primitives naturally safe to share across logic.
- **How can you create an immutable (read-only) object or array?** Use Object.freeze(obj) (or Object.seal plus writable:false) to lock an object. For deep structures, recursively freeze or use libraries like Immer, Immutable.js, or structural-sharing helpers ({ ...state, updatedKey }) that return a fresh object instead of mutating.
- **Why is immutability beneficial in UI frameworks such as React or state libraries like Redux/MobX?** Immutable updates make change detection simple (shallow reference checks), enable time-travel debugging, and prevent accidental side-effects.
  Note: Redux enforces immutability as a core principle. MobX, on the other hand, often uses observable mutable state with transparent tracking. While immutability is not required in MobX, it can still be applied if a project prefers immutable patterns.

### Pure functions

#### Definition

A pure function is a function that, for the same input arguments, always returns the same output and has no observable side-effects (doesn’t modify external state, perform I/O, mutate its parameters, log, etc.). Its behavior is entirely determined by its inputs, which makes it predictable, testable, and cache-friendly (memoizable).
#### Example

```
const add = (a, b) => a + b;
let counter = 0;
const increment = () => ++counter; // impure – relies on external state
```

#### Common Interview Questions

- **Why are pure functions easier to test than impure functions?** They rely solely on input parameters and return a value without side-effects, so you can assert “given X expect Y” without setting up or mocking global state, timers, I/O, or random data.
- **Explain how pure functions enable memoization and give a practical benefit.** Because a pure function’s output depends only on its inputs, you can safely cache results keyed by arguments. Repeated calls with identical inputs bypass expensive recalculation - useful for CPU-heavy tasks like formatting large datasets in UI rendering.
- **Can a function that throws errors be considered pure?** Yes - throwing is not a side-effect if the error depends solely on the inputs (e.g., sqrt(-1) throws). The key is that no external state is mutated and identical inputs always either return the same value or throw the same error.

### Hoisting

#### Definition

Hoisting is JavaScript’s compilation-time behavior that registers declarations before code execution.

- var declarations are hoisted and automatically initialized to undefined.
- let / const declarations are hoisted without initialization; accessing them before the declaration line triggers a Temporal Dead Zone (TDZ) ReferenceError.
- Function declarations are hoisted with their full body, so they’re callable earlier in the scope.
- Function expressions / arrow functions behave like variables: only the variable name is hoisted, not the value.
#### Example

```
console.log(foo); // undefined
console.log(bar); // ReferenceError (TDZ)
speak();          // "Hi!"
var foo = 1;
let bar = 2;
function speak() { console.log('Hi!'); }
```

#### Common Interview Questions

- **What is hoisted for each of these: var, let, const, function declaration, function expression?**
  - var: name + implicit undefined initialization.
  - let / const: name only; no init until definition line (TDZ).
  - Function declaration: name and executable body.
  - Function expression / arrow: variable name only; value assigned at runtime.
- **Explain the Temporal Dead Zone (TDZ).** The TDZ is the period between entering a scope and executing a let/const declaration. During this window the identifier exists but is uninitialized; any access throws a ReferenceError, enforcing safer ordering.
- **Why does calling a function defined with const myFn = () => {} before its declaration fail, whereas calling a function myFn() {} succeeds?** With const, only the variable binding is hoisted - its value isn’t set yet (TDZ), so invoking it before assignment triggers a ReferenceError. A function declaration’s body is hoisted, so it’s fully defined and callable from the top of its scope.

### Immediately-Invoked Function Expression (IIFE)

#### Definition

An IIFE is a function expression that executes immediately after it is created. It encloses variables in a private scope and avoids polluting the surrounding (usually global) namespace. Syntax: wrap a function in parentheses to turn it into an expression, then append () to invoke it right away.
#### Example

```
(function () {
  const secret = 'cookie';
  console.log('Inside IIFE');
})();
console.log(typeof secret); // "undefined"
```

#### Common Interview Questions

- **Why were IIFEs popular before ES6 modules and let/const?** They offered a simple way to create block-like scope for variables using only var, keeping globals clean in browsers where every script shared one global object.
- **Write the minimal ES6 arrow-syntax IIFE that logs "Hello".**
  ```js
  (() => console.log('Hello'))();
  ```
  Parentheses turn the arrow function into an expression; the trailing () invokes it.
- **Can an IIFE return a value? Show a use-case.** Yes. Common pattern for singletons/config objects:
  ```js
  const config = (() => { const apiKey = 'XYZ'; return { apiKey, timeout: 5000 }; })();
  ```
  config now holds the returned object, while apiKey remains private.

### Shallow vs deep copy

#### Definition

A shallow copy duplicates the top-level properties of an object or array. If any of those properties are themselves objects, the references are copied, so the original and the copy still share nested data.

A deep copy duplicates the entire structure recursively, producing a completely independent clone - changes to any level of the clone never affect the source.
#### Example

```
const original = { id: 1, profile: { name: 'Ana' } };
const shallow = { ...original };
shallow.profile.name = 'Marko';
console.log(original.profile.name); // 'Marko'
// Deep copy using structuredClone (modern)
const deep = structuredClone(original);
deep.profile.name = 'Mila';
console.log(original.profile.name); // 'Marko'
```

#### Common Interview Questions

- **Why does Object.assign or the spread operator { ...obj } not create a true deep copy?** They only copy enumerable own properties one level deep; nested objects are copied by reference. Mutating a nested field in the copy mutates the same object inside the original.
- **Give two ways to deep-clone an object in modern JavaScript and list one trade-off for each.**
  - `structuredClone(obj)` - handles most data types (even Map, Set, Date), but not functions or DOM nodes; requires modern runtime (Node 17+, modern browsers).
  - `JSON.parse(JSON.stringify(obj))` - simple and widely supported but loses functions, undefined, symbols, circular references, and date objects turn into strings.
- **When is a shallow copy sufficient, and when must you choose a deep copy?** Shallow copy is fine when the structure contains only primitives or you intentionally want shared nested objects. Deep copy is required when you need full isolation - e.g., producing immutable Redux state snapshots or cloning configuration objects that the callee should not mutate.

### DOM manipulation

#### Definition

DOM manipulation is the act of reading or changing the browser’s Document Object Model - the live tree of nodes representing HTML and XML documents - via JavaScript APIs. Core operations include selecting nodes (querySelector), creating/inserting/removing nodes (createElement, append, remove), modifying attributes and classes (setAttribute, classList), changing inline styles (style), and reacting to events (addEventListener).
#### Example

```
<!-- HTML -->
<ul id="list"></ul>
<script>
  const list = document.getElementById('list');
  const item = document.createElement('li');
  item.textContent = 'Dynamically added';
  item.classList.add('highlight');
  list.append(item);
</script>
```

#### Common Interview Questions

- **What is the difference between innerHTML and textContent, and when would you use each?** innerHTML parses and inserts markup, so it can create nested elements but is vulnerable to XSS if unsafe input is injected. textContent inserts plain text only (no parsing), making it faster and safer when you don’t need HTML.
- **Compare append/prepend with appendChild.** `append`/`prepend` (modern) can accept multiple arguments and strings, and return nothing; `appendChild` accepts one Node, returns that node, and errors on strings. Both move existing nodes instead of cloning them.
- **Why is documentFragment useful for performance?** A DocumentFragment is a lightweight, in-memory container. Building complex markup inside it avoids multiple reflows/repaints; once finished you insert the fragment in one operation, letting the browser render just once.
- **What are data attributes?** Data attributes (data-) allow storing custom data on standard HTML elements. They can be accessed in JavaScript via element.dataset..
- **What is the difference between the nodeValue and textContent properties?** nodeValue: For text nodes, it returns the text content. For element nodes, it returns null.
  textContent: Returns the concatenated text of all descendants, including `<script>` and `<style>` content. It is more performant than innerText as it doesn't require layout calculations.

## Markup (HTML5+)

#### Definition

HTML5 + refers to HTML5 (2014 Recommendation) plus the ongoing “Living Standard” additions that browsers have adopted since - bringing new semantic elements (`<header>`, `<article>`, `<nav>` …), richer media & graphics (`<video>`, `<canvas>`, WebGL), modern forms & validation (`type="email"`, required, pattern), responsive images (`<picture>`, srcset, sizes), interactive controls (`<dialog>`, `<details>`/`<summary>`), and hooks for Web Components (`<template>`, `<slot>`, custom elements).
#### Example

```
<header>
  <h1>JS Fundamentals</h1>
  <nav>
    <a href="#home">Home</a>
    <a href="#topics">Topics</a>
  </nav>
</header>
<picture>
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="cover">
</picture>
<dialog id="modal">
  <p>Hello, world!</p>
  <button onclick="document.getElementById('modal').close()">Close</button>
</dialog>
```

#### Common Interview Questions

- **What is the purpose of the `<!DOCTYPE html>` declaration?** It tells the browser the version of HTML in use (HTML5) and ensures standards‑mode rendering for consistent cross‑browser behavior.
- **What are the benefits of using semantic HTML5 elements instead of generic tags?** They improve accessibility, SEO and maintainability by giving meaning to page regions (e.g., `<header>`, `<nav>`, `<main>`, `<footer>`). Screen readers and search engines can better understand the document structure.
- **How do you implement responsive images in HTML5?** For art direction (different crops or image compositions at different breakpoints): Use the `<picture>` element with multiple `<source>` elements and media queries.
  For resolution switching (serving the same image at different resolutions/sizes): Use the srcset and sizes attributes directly on an `<img>` element, which is often simpler and sufficient for many use cases.
  The browser chooses the appropriate image based on device resolution, viewport size, and supported formats. Fallback to an `<img>` tag ensures support in non-compliant browsers.
- **What is a Web Component and why are `<template>` and `<slot>` used?** Web Components are reusable custom elements encapsulating markup, style and behaviour. The `<template>` tag defines inert markup that can be cloned, and `<slot>` defines insertion points for children, enabling composability.
- **Describe the difference between the `defer` and `async` attributes in the `<script>` tag.**
  - Async: fetching scripts is asynchronous and does not block HTML parsing; execution runs as soon as the script is fetched; order is not guaranteed.
  - Defer: fetching is asynchronous and does not block parsing; execution runs after the HTML has been fully parsed; scripts maintain execution order.

### EJS

#### Definition

EJS (Embedded JavaScript) is a server‑side templating engine for Node.js. EJS lets you embed JavaScript into HTML using tags: `<%= %>` for HTML‑escaped output, `<%- %>` for unescaped raw HTML, `<% code %>` for running arbitrary logic and `<% include file %>` for partials. It renders templates on the server and sends HTML to the client.
#### Example

```
<!-- profile.ejs -->
<h2><%= user.name %></h2>
<p>Age: <%= user.age %></p>
<p>Bio: <%- user.bio %></p>
<%- include('footer') %>

// Express server
const express = require('express');
const app = express();
app.set('view engine', 'ejs');
app.get('/profile/:id', (req, res) => {
  const user = { name: 'Ana', age: 30, bio: '<b>Developer</b>' };
  res.render('profile', { user });
});
app.listen(3000);
```

#### Common Interview Questions

- **How does EJS differ from JSX or Handlebars?** EJS is string-based server-side templating: templates compile to functions that return HTML strings rendered before they hit the browser. JSX is a syntax extension for React that compiles to component calls and usually runs in the browser (or during build). Handlebars is logic-less; it restricts you to helpers, whereas EJS lets you run arbitrary JavaScript inside `<% %>`.
- **Explain the security implications of `<%- %>` vs. `<%= %>` in EJS.** `<%= %>` escapes output by default, preventing XSS when rendering user-provided data. `<%- %>` outputs raw, unescaped HTML. Use it only for **trusted, sanitized content** (e.g., Markdown that you’ve cleaned server-side). Using `<%- %>` with untrusted data creates a serious XSS risk.
- **What performance optimizations are available when using EJS in production?**
  - Template caching: app.set('view cache', true) or ejs.renderFile(path, data, {cache:true}) stores the compiled function in memory.
  - Pre-compilation: run ejs-cli to compile templates to JS bundles during build time.
  - Minify HTML after render to reduce payload size, and avoid heavy logic in templates - prepare data beforehand in controllers or middleware.

### Underscore

#### Definition

Underscore.js is a compact utility library that provides more than 100 functional helpers for working with arrays, objects, functions, and collections - such as _.map, _.filter, _.reduce, _.clone, _.debounce, and _.template. It emphasizes functional style, immutability (most helpers return new values), and works in both browser and Node environments without extending native prototypes.
#### Example

```
const users = [
  { name: 'Ana', age: 30 },
  { name: 'Marko', age: 25 },
  { name: 'Jelena', age: 35 }
];

const avgAge = _.chain(users)
  .map('age')                 // modern Underscore/Lodash replacement for pluck
  .reduce((sum, n) => sum + n, 0)
  .value() / users.length;    // division done manually (no _.divide in core)

console.log(avgAge); // 30
```

#### Common Interview Questions

- **How does _.map differ from native Array.prototype.map?** _.map works on both arrays and objects (iterates keys), accepts an optional context, and handles null/undefined gracefully. Native map works only on arrays or array-like objects.
- **What benefit does _.chain provide, and when should you call .value()?** _.chain(obj) wraps the value to enable fluent chaining. Each method call returns the wrapper. You call .value() at the end to unwrap the final result.
- **Compare _.debounce and _.throttle; give a use-case for each.** `_.debounce(fn, wait)` delays execution until no calls happen for wait ms - ideal for search-as-you-type. `_.throttle(fn, wait)` ensures fn runs at most once per wait ms - useful for scroll listeners.

## Styling

This section summarizes modern styling techniques and frameworks, including CSS3+, preprocessors, Bootstrap and Material Design.

### CSS3+

#### Definition

CSS3 + encompasses the modular CSS3 specification (2009 – 2016) and the continuous “Level 4+” additions standardized by the W3C Working Groups. Major modern features include:
- Layout - Flexbox, Grid, multi-column, position: sticky.
- Visual effects - 2-D/3-D transforms, transitions, animations, filters, blend modes.
- Responsive design - media queries level 4, container queries, @supports feature queries.
- Typography & UI - web fonts (@font-face), variable fonts, :focus-visible, logical properties.
- Variables & math - custom properties (--var), calc(), clamp(), color functions (color-mix(), hsl()), @property for typed variables.
- Selectors level 4+ - :is(), :where(), :not(), relational selectors (:has()), case-insensitive attribute flags.
All modern browsers ship evergreen updates, so “CSS3+” usually means “everything beyond CSS 2.1 that ships today.”
#### Common Interview Questions

- **Compare Flexbox and CSS Grid. When would you pick one over the other?** Flexbox is one-dimensional (row or column) and excels at aligning items along a single axis (nav bars, toolbars, centering). Grid is two-dimensional (rows and columns simultaneously) and is suited for full page/section layouts or complex card galleries. You can nest them - use Grid for macro layout, Flexbox for micro alignment inside each cell.
- **Explain the CSS Box Model. What is box‑sizing: border‑box?** Every element is a box consisting of content → padding → border → margin. In the default content-box model, the width and height properties include only the content; in border‑box the width/height include content, padding and border, making sizing more intuitive.
- **What advantages do CSS custom properties (--vars) provide over pre-processor variables?** They are live in the cascade: values can change at runtime (e.g., theming with data-theme), participate in inheritance, and work with calc()/color functions. Pre-processor variables (Sass/LESS) are compile-time only, so they can’t react to DOM state or media queries without recompiling.
- **Explain the specificity hierarchy and two ways to override a highly specific rule without !important.** Specificity is calculated by (a) inline styles, (b) IDs, (c) classes/attributes/pseudo-classes, (d) elements/pseudo-elements. A later rule with a higher numeric specificity wins. To override without !important:
  - Increase specificity gently - e.g., prepend a class or use :where() to zero out earlier selectors (.new .btn outranks .btn).
  - Place the override later in the stylesheet with equal specificity (source-order wins if the numeric score ties).
- **What is CSS specificity and how is it calculated?** CSS specificity is a weight-based system that determines which CSS rule is applied when multiple rules target the same element.
  Specificity is calculated using a four-level ranking system (a, b, c, d), not a base-10 number:
  - Inline styles (style attribute) - highest priority
  - ID selectors (e.g., #nav)
  - Class selectors, attribute selectors, and pseudo-classes (e.g., .button, :hover)
  - Element selectors and pseudo-elements (e.g., div, ::before) - lowest priority
  - The universal selector (*) and combinators (like > or +) do not add to specificity.
  - When comparing selectors, the browser checks each level in order - a higher value in an earlier level wins.
  - Example: #nav .list li a:hover contains 1 ID, 2 classes/pseudo-classes, and 3 elements, giving it a specificity score of (0,1,2,3).
- **What is the difference between display: none, visibility: hidden and opacity: 0?** display: none: the element is removed from the document flow, takes no space and causes reflow.
  visibility: hidden: the element is hidden but still occupies space; not interactive.
  opacity: 0: the element is fully transparent but still takes up space and is interactive.
- **How does z-index work and what is a stacking context?** z-index controls the stacking order of positioned elements. Its effect is constrained by the stacking context, which is formed by the root element, positioned elements with a z-index, opacity less than 1 and other properties. A child’s z-index only has meaning within its parent’s stacking context.
- **How would you create a responsive website?** Use the viewport meta tag (), fluid layouts (percentages, vw/vh units), media queries (@media (max-width: 768px) { … }), flexible images (max-width: 100%; height: auto), and modern layout techniques like CSS Grid and Flexbox.
- **How can you center a div horizontally and vertically?** Flexbox: display: flex; justify-content: center; align-items: center; height: 100vh;.
  - Grid: display: grid; place-items: center; height: 100vh;.
  - Legacy (transform): position the child absolutely at 50%/50% and translate it by -50%.
- **Explain CSS pseudo‑elements and pseudo‑classes. Give examples.** A pseudo‑class defines a special state of an element (:hover, :focus, :first-child) while a pseudo‑element targets a part of an element (::before, ::after, ::first-line, ::selection).

### CSS Preprocessors (SCSS/Sass/LESS)

#### Definition

CSS preprocessors extend vanilla CSS with features such as variables, nesting, mixins, functions, loops, and modular imports. They compile to plain CSS that browsers understand.
- Sass (indented) and SCSS (syntax similar to CSS) share the same engine.
- LESS offers a comparable feature set but uses slightly different syntax (@var vs $var, mixin-as-class conventions). Preprocessors improve maintainability and enable DRY patterns, especially in large code-bases, design-system tokens, and themeable UIs.
#### Common Interview Questions

- **Contrast Sass/SCSS with LESS - why might a team choose one over the other?** Sass offers two syntaxes (indented & SCSS), richer math, first-class modules (@use, @forward), and a mature ecosystem (Dart Sass). LESS integrates into the Node toolchain naturally and treats mixins as first-class rulesets. Choice often depends on toolchain preference, existing code, and required features such as modules or built-in color functions.
- **Explain the difference between a mixin, an extend, and a function in Sass.** A mixin injects declarations every time it’s @included (can accept arguments). @extend merges selectors to share a ruleset without duplicating declarations (lighter CSS but tighter coupling). A function returns a single computed value (e.g., scale($size, 1.2)) and is used in property values or other mixins.
- **Why were @import statements deprecated in Sass, and how do the newer @use / @forward directives improve performance?** @import inlined files repeatedly, produced global namespaces, and caused duplicated CSS. @use loads a module once, scopes its members under a namespace (or as *), and supports configurable variables; @forward re-exports parts of a module - together they deliver faster compilation and predictable symbol resolution.
- **What is BEM?** Block‑Element‑Modifier is a naming convention that makes CSS more maintainable. A block is a standalone entity (e.g., .menu); an element is a part of a block (.menu__item); a modifier changes appearance (.menu–hidden or .menu__item–active).

### Bootstrap

#### Definition

Bootstrap is a popular open-source CSS & JS framework created by Twitter. It ships ready-made responsive grid, utility classes, and interactive components (modals, dropdowns, offcanvas, toasts) powered by vanilla JavaScript and Popper. Using a mobile-first 12-column flexbox grid, Bootstrap accelerates prototyping and ensures visual consistency across browsers without writing custom CSS from scratch.
#### Common Interview Questions

- **Explain Bootstrap’s grid breakpoints and how the class naming works (e.g., col-12, col-md-6).** Bootstrap defines five default breakpoints (xs <576 px, sm ≥576, md ≥768, lg ≥992, xl ≥1200, plus xxl ≥1400). Classes follow the pattern `col-{breakpoint?}-{span}`: omit the breakpoint for extra-small; specify one to apply from that width upward. Example col-md-6 gives half-width from 768 px+, while remaining full-width below.
- **What advantages do Bootstrap utility classes (.d-flex, .text-center, .mt-3, etc.) provide over writing custom CSS?** They enable rapid, consistent styling directly in markup, reduce context-switching between HTML/CSS, and leverage a well-tested design system. Utilities are generated via Sass maps, so you can customize scales and colors while keeping bundle size predictable.
- **How would you customize Bootstrap’s default theme colors without editing the core files?** Install Bootstrap’s Sass source, then override Sass variables before importing:
  ```scss
  // custom.scss
  $primary: #6f42c1; // brand purple
  $font-family-base: 'Inter', sans-serif;
  @import "bootstrap"; // compiles with new variables
  ```
  Compile with Dart Sass or the official build tools. Alternatively, use CSS variables in v5 (:root { --bs-primary: #6f42c1; }) for runtime theming without recompilation.

### Material (Material Design & MUI)

#### Definition

Material Design is Google’s opinionated design system that defines visual, motion, and interaction guidelines. Material UI (MUI) is a popular React component library that implements those guidelines with ready-made, accessible components, a CSS-in-JS theming system, and utility hooks (e.g., useTheme). MUI uses the “theme → overrides → props” pattern, supports dark mode, responsive breakpoints, and tree-shakable component imports (@mui/material/Button).
#### Common Interview Questions

- **How does MUI’s theming system differ from plain CSS variables or Sass themes?** MUI’s createTheme() builds a JS object consumed by every styled component via React context - changes propagate automatically, and you can access the theme in JS (useTheme) for dynamic styling or logic. CSS variables/Sass themes operate at the stylesheet layer and don’t provide runtime access in JavaScript.
- **Explain the pros and cons of MUI’s “styled API” (@mui/styled-engine) compared to the sx prop.** styled() generates re-usable, themed components with full CSS-in-JS power; great for design-system atoms but heavier in bundle size. The sx prop is terse, tree-shakable, evaluated at runtime, and ideal for one-off overrides, but mixing too many sx styles in JSX can hurt readability.
- **What accessibility features are built into MUI components, and how would you audit them?** Components ship with proper ARIA roles/attributes (e.g., role="button" on IconButton, aria-label support). Keyboard navigation and focus rings follow WCAG. Audit with Lighthouse, axe-core, or @testing-library/jest-dom to ensure props like aria-label, color contrast, and focus order remain valid after customization.

## Node.js Fundamentals

### Node.js Single-Threaded Nature & Concurrency Model

#### Definition

Node.js runs JavaScript in a single main thread, meaning only one piece of JavaScript code is executed at any given moment. However, this does not make Node.js “single-tasking.” Concurrency is achieved through an event-driven, non-blocking I/O model powered by the event loop and the libuv library. Long-running or blocking operations (such as file system access, DNS lookups, compression, or cryptography) are delegated to the operating system or to a background thread pool, while the main thread continues executing other callbacks. This design allows Node.js to efficiently handle a large number of concurrent I/O-bound operations without the overhead of managing one thread per request.
#### Example

```
console.log("Start");

setTimeout(() => {
  console.log("Timer finished");
}, 1000);

Promise.resolve().then(() => {
  console.log("Promise resolved");
});

console.log("End");
```

Output order:
Start
End
Promise resolved
Timer finished
Although JavaScript runs on a single thread, the timer and promise callbacks are executed later by the event loop, demonstrating how Node.js interleaves multiple asynchronous tasks without blocking the main execution flow.

#### Common Interview Questions

- **Is Node.js single-threaded or multi-threaded?** Node.js executes JavaScript on a single main thread. However, it is not limited to a single core for all work: I/O operations are handled asynchronously by the operating system and by libuv’s internal thread pool, and additional threads can be created explicitly using Worker Threads. So the execution model is single-threaded for JavaScript, but multi-threaded under the hood.
- **How can Node.js handle thousands of concurrent requests if it is single-threaded?** It uses non-blocking I/O and an event-driven architecture. Instead of waiting for an operation (like disk or network access) to finish, Node.js registers a callback and continues processing other events. When the operation completes, its callback is placed in the event loop queue and executed later. This allows a single thread to multiplex many concurrent connections efficiently.
- **What kind of tasks does Node.js handle best: CPU-bound or I/O-bound?** Node.js excels at I/O-bound workloads (network servers, APIs, real-time systems), where most of the time is spent waiting for external resources. CPU-bound tasks can block the event loop and degrade performance unless they are offloaded to Worker Threads or separate processes.
- **What happens if a CPU-intensive function runs on the main thread?** It blocks the event loop. While the computation is running, no other callbacks can be processed, causing all incoming requests and timers to wait. This is why heavy computation in Node.js should be moved to Worker Threads or separate processes.
- **How is concurrency different from parallelism in Node.js?** Concurrency means the ability to manage multiple tasks at the same time (interleaving their execution), which Node.js achieves via the event loop. Parallelism means executing tasks simultaneously on multiple CPU cores, which in Node.js requires clustering, child processes, or Worker Threads.
- **How does this model differ from traditional multi-threaded servers (e.g., Java, .NET)?** Traditional servers often use one thread per request, relying on the OS scheduler for concurrency, which can lead to high memory and context-switching overhead. Node.js uses a small number of threads and an event loop to handle many requests asynchronously, trading thread-level parallelism for scalability and lower overhead in I/O-heavy scenarios.

### Node.js Event Loop & Phases

#### Definition

The Node.js Event Loop is the core mechanism that enables non-blocking, asynchronous execution on a single JavaScript thread. It is implemented in the libuv library and runs in a continuous loop, processing different categories of callbacks in well-defined phases. Each phase has its own queue of tasks, and the event loop iterates through these phases in order, executing the callbacks whose time or I/O conditions are satisfied.
The main phases are:
Timers – executes callbacks scheduled by setTimeout and setInterval.
Pending Callbacks – executes I/O callbacks deferred from the previous cycle.
Idle, Prepare – internal use by libuv.
Poll – retrieves new I/O events, executes their callbacks, and may block waiting for I/O.
Check – executes setImmediate callbacks.
Close Callbacks – executes cleanup callbacks such as socket.on('close').
In addition to these macro-task phases, Node.js processes microtasks (Promise callbacks, queueMicrotask, and process.nextTick) very frequently during execution. process.nextTick runs before Promise microtasks and can starve the event loop if misused.
#### Example

```
const fs = require('fs');

console.log('Start');

setTimeout(() => {
  console.log('setTimeout');
}, 0);

setImmediate(() => {
  console.log('setImmediate');
});

fs.readFile(__filename, () => {
  console.log('I/O callback');
});

Promise.resolve().then(() => {
  console.log('Promise');
});

process.nextTick(() => {
  console.log('nextTick');
});

console.log('End');
```

Typical output order:
Start
End
nextTick
Promise
I/O callback
setImmediate
setTimeout

Note: The relative order of setImmediate and setTimeout(…, 0) can vary when scheduled from top-level code. When scheduled from inside an I/O callback, setImmediate usually fires before setTimeout(…, 0).

Explanation:
Synchronous code runs first.
process.nextTick runs before Promise microtasks.
Promise callbacks run before the next event loop phase.
I/O callbacks run in the Poll phase.
setImmediate runs in the Check phase.
setTimeout runs in the Timers phase (if its delay has elapsed).

#### Common Interview Questions

- **What is the role of the event loop in Node.js?** It coordinates the execution of asynchronous callbacks by cycling through its phases and executing queued tasks when the call stack is empty. This allows Node.js to handle many concurrent I/O operations without blocking the main thread.
- **What is the difference between setTimeout(fn, 0) and setImmediate(fn)?** setTimeout schedules the callback in the Timers phase after at least the given delay.
  setImmediate schedules the callback in the Check phase, which runs immediately after the Poll phase.
  When scheduled from an I/O callback, setImmediate usually fires before setTimeout(…, 0).
- **How do microtasks differ from macrotasks in Node.js?** Microtasks (Promises, queueMicrotask, process.nextTick) are executed immediately after the current operation and before the event loop moves to the next phase. Macrotasks (timers, I/O, immediates) are processed in their respective phases. process.nextTick has higher priority than Promise microtasks and can starve the event loop if misused.
- **Why can excessive use of process.nextTick be dangerous?** Because nextTick callbacks are executed before the event loop continues, recursively scheduling nextTick can prevent the loop from reaching I/O and timer phases, effectively blocking the application.
- **In which phase are file system callbacks executed?** File system callbacks are executed in the Poll phase after the underlying I/O operation completes.
- **How does the Node.js event loop differ from the browser event loop?** Both follow the same single-threaded, event-driven principle, but Node.js has additional phases (Poll, Check, Close), a built-in thread pool for offloading work, and a special microtask queue with process.nextTick that runs before Promise callbacks.

### libuv & Thread Pool

#### Definition

libuv is the C library that underpins Node.js’s asynchronous I/O and event loop. It provides a cross-platform abstraction over operating system facilities such as epoll (Linux), kqueue (macOS), IOCP (Windows), and implements the event loop, timers, networking, and file system operations.
While JavaScript execution runs on a single main thread, libuv maintains an internal thread pool (default size: 4 threads) to offload operations that cannot be performed in a truly non-blocking way by the OS. These include:
- File system operations (fs)
- DNS lookups (some cases)
- Cryptographic operations (crypto, TLS)
- Compression (zlib)
- Certain user-defined native addons

When such a task is scheduled, Node.js submits it to the libuv thread pool. Once a worker thread completes the task, the result is queued back to the event loop, and the corresponding callback or Promise resolution is executed on the main thread.
This architecture allows Node.js to remain responsive while performing expensive or blocking work in parallel.
#### Example

```
const crypto = require('crypto');

console.time('hash');

crypto.pbkdf2('password', 'salt', 100000, 64, 'sha512', () => {
  console.timeEnd('hash');
});
```

If you run this several times in parallel:

```
for (let i = 0; i < 8; i++) {
  crypto.pbkdf2('password', 'salt', 100000, 64, 'sha512', () => {
    console.log(`Hash ${i} done`);
  });
}
```

Only four operations will run simultaneously by default, because the libuv thread pool has four worker threads. The remaining tasks wait in a queue until a thread becomes free.
You can change the pool size:

```
UV_THREADPOOL_SIZE=8 node app.js
```

#### Common Interview Questions

- **What is libuv and why is it critical to Node.js?** libuv implements the event loop and provides asynchronous, cross-platform access to the file system, networking, timers, and system threads. Without libuv, Node.js would not be able to offer its non-blocking I/O model or consistent behavior across operating systems.
- **Why does Node.js need a thread pool if it is “non-blocking”?** Some operations (like disk I/O, encryption, compression) cannot be made fully non-blocking using the OS event APIs. libuv offloads these tasks to worker threads so that the main JavaScript thread is not blocked, preserving responsiveness.
- **Which operations use the libuv thread pool?** Typical examples are fs module calls, crypto operations, zlib compression, and certain DNS lookups. Pure network I/O (sockets) usually relies on the OS’s asynchronous APIs and does not consume thread-pool threads.
- **What is the default size of the libuv thread pool and how can it be changed?** The default size is 4. It can be configured via the environment variable UV_THREADPOOL_SIZE before starting the Node.js process.
- **What happens if all thread pool workers are busy?** Additional tasks are queued and must wait until a worker thread becomes available. This can increase latency and is a common performance bottleneck when many CPU-intensive or fs/crypto operations run concurrently.
- **How do Worker Threads differ from the libuv thread pool?** The libuv thread pool is internal and used automatically for specific native operations. Worker Threads, on the other hand, are explicit JavaScript threads that you create and manage yourself, allowing you to run your own JS code in parallel and communicate via message passing or shared memory.

### Event Emitters

#### Definition

Event Emitters are a core pattern in Node.js for implementing event-driven, asynchronous communication. The EventEmitter class (from the events module) provides a simple publish–subscribe mechanism where objects can emit named events and other parts of the application can subscribe to them with listener functions.
An emitter maintains an internal mapping of event names to listener arrays. When an event is emitted via emit(eventName, ...args), all registered listeners for that event are synchronously invoked in the order they were added. This pattern underlies many built-in Node.js APIs, including streams, HTTP servers, sockets, and process-level events.
#### Example

```
const EventEmitter = require('events');

class OrderService extends EventEmitter {}

const orders = new OrderService();

orders.on('created', (orderId) => {
  console.log(`Order ${orderId} created`);
});

orders.once('created', () => {
  console.log('First order only');
});

orders.emit('created', 42);
orders.emit('created', 43);
```

Output:
Order 42 created
First order only

### Order 43 created

#### Common Interview Questions

- **What is the EventEmitter pattern and where is it used in Node.js?** It is an implementation of the observer (publish–subscribe) pattern. Many Node.js core modules are event emitters: streams emit data and end, HTTP servers emit request, sockets emit connect and close, and the process object emits lifecycle and error events.
- **What is the difference between on and once?** on registers a persistent listener that is called every time the event is emitted.
  once registers a one-time listener that is automatically removed after the first invocation.
- **Are EventEmitter listeners executed asynchronously?** No. Listener functions are invoked synchronously during the emit call, in the same tick of the event loop. If a listener performs heavy computation, it will block subsequent listeners and the event loop.
- **How do you remove an event listener?** Use off(event, handler) (or the older removeListener) to remove a specific listener, or removeAllListeners(event) to clear all listeners for an event.
- **What is the “MaxListenersExceededWarning” and why does it exist?** Node.js warns when more than 10 listeners are attached to the same event by default. This helps detect potential memory leaks where listeners are added but never removed. The limit can be changed with emitter.setMaxListeners(n).
- **How do Event Emitters relate to Streams?** Streams are built on top of EventEmitter. They emit events such as data, end, error, and close, using the same subscription and emission mechanism.

### Streams

#### Definition

Streams are an abstraction in Node.js for working with streaming data - data that is read or written sequentially over time instead of being loaded entirely into memory at once. They are built on top of the EventEmitter pattern and are fundamental for handling I/O efficiently, especially for large files, network sockets, and real-time data processing.
Node.js defines four main stream types:
Readable – source of data (fs.createReadStream, HTTP request)
Writable – destination for data (fs.createWriteStream, HTTP response)
Duplex – both readable and writable (TCP sockets)
Transform – duplex streams that modify data (compression, encryption, parsing)
Streams use internal buffering and a mechanism called backpressure to prevent a fast producer from overwhelming a slow consumer.
#### Example

```
const fs = require('fs');

const readable = fs.createReadStream('bigfile.txt');
const writable = fs.createWriteStream('copy.txt');

readable.pipe(writable);
```

This copies a large file without loading it fully into memory. Data flows chunk by chunk, and the writable stream signals when it is ready to receive more.
Transform stream example:

```
const { Transform } = require('stream');

const upper = new Transform({
  transform(chunk, encoding, callback) {
    callback(null, chunk.toString().toUpperCase());
  }
});

process.stdin.pipe(upper).pipe(process.stdout);
```

#### Common Interview Questions

- **Why are streams preferred over reading an entire file with fs.readFile for large data?** Streams process data incrementally, keeping memory usage low and enabling processing to start before the entire payload is available. fs.readFile loads the full content into memory, which is inefficient and risky for large files.
- **What is backpressure and why is it important?** Backpressure is the mechanism by which a writable stream signals that its internal buffer is full and the readable stream should slow down. It prevents memory overflow and keeps producer and consumer speeds balanced.
- **What does pipe() do internally?** pipe() connects a readable stream to a writable stream, forwarding data events, managing backpressure, and automatically handling end and error propagation.
- **What is the difference between a Duplex and a Transform stream?** Both are readable and writable. A Duplex stream treats input and output as independent (e.g., a TCP socket). A Transform stream modifies the data, so its output is a transformed version of its input (e.g., gzip, encryption, JSON parsing).
- **Are streams synchronous or asynchronous?** Stream events and callbacks are asynchronous and integrated with the event loop. However, individual chunk processing inside a transform can block if heavy CPU work is done synchronously.
- **How are streams related to Event Emitters?** All streams inherit from EventEmitter and use events such as data, end, error, finish, and close to signal state changes and data availability.

### Buffers vs Strings

#### Definition

In Node.js, a String represents text encoded in a specific character set (usually UTF-8) and is immutable. A Buffer represents a fixed-size chunk of raw binary memory, used to work directly with bytes. Buffers are essential for handling binary data such as files, network packets, images, audio, and encrypted content, where no character encoding should be assumed.
Strings are suitable for human-readable text and high-level manipulation. Buffers are designed for low-level I/O and performance-critical operations, and they map closely to memory allocated outside the V8 heap.

#### Example

```
const text = "Hello";
const buf = Buffer.from("Hello");

console.log(text.length); // 5 (characters)
console.log(buf.length);  // 5 (bytes in UTF-8 here)

// Binary modification
buf[0] = 0x48; // 'H'
console.log(buf.toString()); // "Hello"
```

Reading a file as a Buffer vs as a String:

```
const fs = require('fs');

const binary = fs.readFileSync('image.png');          // Buffer
const textFile = fs.readFileSync('note.txt', 'utf8'); // String
```

#### Common Interview Questions

- **Why does Node.js need Buffers when JavaScript already has Strings?** Strings are for encoded text and are immutable. Buffers represent raw bytes and allow efficient binary I/O, partial reads, slicing without copying, and direct interaction with OS-level data. Network protocols and file systems operate on bytes, not characters.
- **What is the difference between Buffer and Uint8Array?** Buffer is a Node.js-specific subclass of Uint8Array with additional methods for encoding/decoding (toString, write, equals, etc.) and integration with Node APIs. Both represent byte arrays, but Buffer is optimized for Node’s I/O ecosystem.
- **Are Buffers mutable?** Yes. Buffer contents can be modified in place, unlike Strings which are immutable. This allows efficient transformations without reallocating memory.
- **What is the difference between Buffer.from(), Buffer.alloc(), and Buffer.allocUnsafe()?** Buffer.from(data) creates a buffer from existing data.
  Buffer.alloc(size) allocates zero-filled memory (safe but slower).
  Buffer.allocUnsafe(size) allocates uninitialized memory (faster but must be filled before use to avoid leaking old data).
- **Why is encoding important when converting between Buffers and Strings?** Because the same sequence of bytes can represent different characters under different encodings (utf8, ascii, latin1, base64, hex). Incorrect encoding leads to corrupted or misinterpreted data.
- **How are Buffers used together with Streams?** Streams typically emit and consume Buffers for binary data. Transform streams often operate on Buffer chunks, modifying or re-encoding them before passing them downstream.
### Modules: CommonJS vs ES Modules

#### Definition

Node.js supports two module systems:
CommonJS (CJS) – the original Node.js module system, using require() to import and module.exports / exports to export. Modules are loaded synchronously and resolved at runtime.
ECMAScript Modules (ESM) – the standardized JavaScript module system, using import / export syntax. Modules are statically analyzable, support top-level await, and are resolved using URLs and file extensions.
The module type is determined by file extension (.cjs, .mjs) or by the "type" field in package.json.
#### Example

```
// math.cjs
module.exports.add = (a, b) => a + b;

// app.cjs
const { add } = require('./math.cjs');
console.log(add(2, 3));
```

ES Modules:

```
// math.mjs
export const add = (a, b) => a + b;

// app.mjs
import { add } from './math.mjs';
console.log(add(2, 3));
```

Mixed environment with "type": "module":

```
{
  "type": "module"
}

// app.js (now treated as ESM)
import fs from 'fs';
Dynamic import (works in both):
const mod = await import('./math.mjs');
```

#### Common Interview Questions

- **What are the main differences between CommonJS and ES Modules?** CommonJS uses synchronous require and module.exports, is resolved at runtime, and is the traditional Node.js format. ES Modules use static import/export, are resolved before execution, support tree-shaking, top-level await, and are the standard across browsers and Node.
- **How does Node.js decide whether a file is treated as CommonJS or ESM?** By file extension (.cjs → CommonJS, .mjs → ESM) or by the "type" field in package.json ("module" means .js files are ESM; "commonjs" or absence means CJS).
- **Can you import a CommonJS module from an ES Module and vice versa?** Yes, but with interop rules. ESM can import CJS via default import (import pkg from 'pkg'). CJS can load ESM only via dynamic import(), not require().
- **Why are ES Modules considered better for tooling and optimization?** Because their static structure allows bundlers and runtimes to perform tree-shaking, dead-code elimination, and dependency analysis at build time.
- **What is the difference between module.exports and exports?** exports is initially a reference to module.exports. Reassigning exports breaks that link; only module.exports is actually returned by require().
- **Why do ES Modules require explicit file extensions in Node.js imports?** Because ESM resolution follows URL semantics, not CommonJS’s extension-guessing algorithm, making resolution deterministic and compatible with browser module loading.

### Process & Global Objects

#### Definition

In Node.js, the global execution environment provides a set of objects that are available in every module without importing them. The most important is the process object, which represents the currently running Node.js process and exposes information and control over its environment, lifecycle, and resources. Unlike browsers, where the global object is window, Node.js uses global, and many platform-specific APIs (Buffer, setTimeout, clearImmediate, etc.) are attached to it.
The process object is an instance of EventEmitter and provides access to:
- Environment variables (process.env)
- Command-line arguments (process.argv)
- Process ID and parent process (process.pid, process.ppid)
- Exit codes and termination (process.exit)
- Current working directory (process.cwd)
- Runtime events (exit, uncaughtException, unhandledRejection, SIGINT, ...)
#### Example

```
console.log(process.env.NODE_ENV);
console.log(process.argv);

process.on('SIGINT', () => {
  console.log('Gracefully shutting down...');
  process.exit(0);
});
```

Using global vs module scope:

```
global.appName = 'Cookbook';

console.log(global.appName); // 'Cookbook'
```

#### Common Interview Questions

- **What is the difference between global in Node.js and window in browsers?** Both represent the global object, but window also represents the DOM and browser APIs, while global only contains Node.js runtime objects. Variables declared with var at the top level are not attached to global in Node modules due to module scoping.
- **What information can you get from process.env and how is it typically used?** process.env exposes environment variables. It is commonly used for configuration (NODE_ENV, database URLs, API keys, feature flags) and to separate development, staging, and production behavior without changing code.
- **How do you pass arguments to a Node.js script and read them?** Arguments are passed after the script name: node app.js arg1 arg2. They are available in process.argv as an array, where index 0 is the Node binary and index 1 is the script path.
- **What is the difference between process.exit() and letting the event loop finish naturally?** process.exit() terminates the process immediately, even if there are pending I/O operations, which can lead to data loss or incomplete work. Letting the event loop drain allows all pending callbacks and handles to complete gracefully.
- **Why is process an EventEmitter and what events does it emit?** Because the process lifecycle is event-driven. It emits events such as exit, beforeExit, uncaughtException, unhandledRejection, and OS signals like SIGTERM and SIGINT, allowing applications to perform cleanup and graceful shutdown.
- **What are some other commonly used global objects in Node.js?** Buffer (binary data), setTimeout / setImmediate, clearTimeout / clearImmediate, __dirname, __filename, and console. Unlike browsers, there is no document or window in Node.js.

### Async Patterns in Node

#### Definition

Asynchronous programming in Node.js is built around non-blocking operations and callback-based APIs, which evolved into Promises and later into async / await. These patterns allow Node.js to perform I/O and long-running tasks without blocking the event loop, enabling high concurrency on a single JavaScript thread.
The three main async patterns in Node.js are:
Callbacks – the original pattern, often following the error-first convention (err, result).
Promises – represent a value that will be available in the future and allow chaining via .then() / .catch().
async / await – syntactic sugar over Promises that enables writing asynchronous code in a synchronous style using try / catch for error handling.
#### Example

```js
// Callback style:
fs.readFile('data.txt', (err, data) => {
  if (err) return console.error(err);
  console.log(data.toString());
});

// Promise style:
fs.promises.readFile('data.txt')
  .then(data => console.log(data.toString()))
  .catch(console.error);

// async / await:
async function read() {
  try {
    const data = await fs.promises.readFile('data.txt');
    console.log(data.toString());
  } catch (err) {
    console.error(err);
  }
}
read();
```

#### Common Interview Questions

- **What is the “error-first” callback convention in Node.js?** Callbacks receive arguments in the form (error, result). On success, error is null; on failure, it contains an Error object. This standardizes error handling and allows early returns when an error occurs.
- **How do Promises improve over nested callbacks?** They flatten control flow, support chaining, centralize error handling via .catch(), and enable composition with utilities like Promise.all, Promise.race, and Promise.allSettled.
- **What advantages does async/await provide over raw Promise chains?** It makes asynchronous code read like synchronous code, simplifies error handling with try/catch, and avoids deeply nested .then() chains, improving readability and maintainability.
- **How do you run asynchronous operations in parallel in Node.js?** By starting them without awaiting immediately and then awaiting them together using Promise.all or related combinators.
- **What happens if you forget to await a Promise inside an async function?** The function continues execution and returns a pending Promise. Errors may become unhandled rejections if not later awaited or caught.
- **How should errors be propagated in async/await code?** Throw the error inside the async function or let it bubble up; the returned Promise will be rejected and can be handled by a surrounding try/catch or by a .catch() on the caller.
### File System - Sync vs Async vs Promises

#### Definition

The Node.js fs module provides APIs to interact with the file system using three different styles:
- Synchronous (fs.*) - blocking operations that halt the event loop until completion.
- Asynchronous Callback-based (fs.*) - non-blocking operations using the error-first callback pattern.
- Promise-based (fs/promises) - non-blocking operations that return Promises and integrate naturally with async/await.

All three ultimately perform the same underlying I/O work, but differ in how they affect the event loop and how results are consumed.
#### Example

```js
// Synchronous (blocking):
const fs = require('fs');
const data = fs.readFileSync('file.txt', 'utf8');
console.log(data);

// Asynchronous with callback:
const fs = require('fs');
fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});

// Promise-based:
const fs = require('fs/promises');
async function read() {
  const data = await fs.readFile('file.txt', 'utf8');
  console.log(data);
}
read();
```

#### Common Interview Questions

- **Why are synchronous file system calls discouraged in Node.js servers?** Because they block the event loop. While a synchronous operation is running, no other requests, timers, or callbacks can be processed, causing throughput collapse under load.
- **When is it acceptable to use synchronous fs methods?** During startup scripts, build tools, CLI utilities, or one-off configuration loading where blocking is acceptable and simplicity is preferred over concurrency.
- **What is the difference between fs.readFile and fs.createReadStream?** readFile loads the entire file into memory at once. Streams read the file chunk by chunk, enabling lower memory usage and backpressure handling for large files.
- **Why was fs/promises introduced?** To provide a native Promise-based API without manual promisification, enabling clean async / await usage and consistent error handling.
- **Are fs operations truly asynchronous at the OS level?** On most platforms, disk I/O is blocking. Node.js offloads these operations to the libuv thread pool, making them appear asynchronous to JavaScript.
- **How do errors propagate differently between callback and Promise fs APIs?** In callback style, errors are passed as the first argument and must be handled manually. In Promise style, errors reject the Promise and are caught with try/catch or .catch().

### Error Handling in Node.js

#### Definition

Error handling in Node.js spans both synchronous and asynchronous execution and is critical for building stable, long-running server processes. Errors can originate from thrown exceptions, rejected Promises, callback failures, or system-level failures (I/O, memory, signals). Because Node.js is event-driven and single-threaded, an unhandled error can crash the entire process if not properly intercepted.
Node.js provides several layers of error handling:
Synchronous errors – caught with try/catch.
Callback errors – passed via the error-first convention.
Promise rejections – handled with .catch() or try/catch in async/await.
Process-level events – uncaughtException, unhandledRejection, and OS signals for last-resort handling and graceful shutdown.
#### Example

```js
// Synchronous:
try {
  JSON.parse('invalid');
} catch (err) {
  console.error('Parse failed:', err.message);
}

// Async / Promise:
async function load() {
  try {
    const data = await fs.promises.readFile('missing.txt');
  } catch (err) {
    console.error('File error:', err.code);
  }
}

// Process-level safety net:
process.on('unhandledRejection', (reason) => {
  console.error('Unhandled rejection:', reason);
});

process.on('uncaughtException', (err) => {
  console.error('Uncaught exception:', err);
  process.exit(1); // fail fast
});
```

#### Common Interview Questions

- **What is the difference between uncaughtException and unhandledRejection?** uncaughtException fires when a synchronous exception escapes the call stack.
  unhandledRejection fires when a Promise is rejected without a .catch() handler. Both indicate programming errors and usually require process termination after logging.
- **Why is it dangerous to continue running after an uncaughtException?** The application state may be corrupted (open handles, partial writes, inconsistent memory). Node.js documentation recommends logging the error and performing a controlled shutdown rather than attempting to continue.
- **How should errors be handled in async/await code?** Wrap await calls in try/catch blocks or let the error propagate to a higher-level handler. Always ensure rejected Promises are either awaited or returned so they can be caught.
- **What is the recommended pattern for error handling in Express / HTTP servers?** Centralized error-handling middleware that receives (err, req, res, next) and converts internal errors into sanitized HTTP responses while logging full stack traces server-side.
- **What are “operational errors” vs “programmer errors”?** Operational errors are expected runtime issues (network timeouts, invalid input, missing files) and should be handled gracefully.
  Programmer errors are bugs (null dereference, logic flaws, uncaught exceptions) and usually require process restart after logging.
- **How do you implement graceful shutdown on fatal errors or signals?** Listen for SIGTERM, SIGINT, uncaughtException, and unhandledRejection, stop accepting new requests, close open connections, flush logs, and then exit the process with a non-zero code.

### Child Processes

#### Definition

Node.js allows creating and controlling operating system processes via the child_process module. Child processes run in separate OS-level processes with their own memory and event loops, enabling true parallelism and isolation from the main Node.js process. They are commonly used to execute shell commands, run other programs, or offload CPU-intensive work.
The main APIs are:
spawn – launches a process and streams its output (best for long-running processes and large data).
exec – runs a command in a shell and buffers the entire output (convenient but memory-heavy).
execFile – like exec, but runs a binary directly without a shell.
fork – a special case of spawn for launching another Node.js process with a built-in IPC (inter-process communication) channel.
#### Example

```js
// Running a shell command:
const { exec } = require('child_process');
exec('ls -l', (err, stdout, stderr) => {
  if (err) return console.error(err);
  console.log(stdout);
});

// Spawning a long-running process with streams:
const { spawn } = require('child_process');
const ping = spawn('ping', ['google.com']);
ping.stdout.on('data', data => {
  console.log(`Output: ${data}`);
});

// Forking another Node.js process:
const { fork } = require('child_process');
const worker = fork('./worker.js');
worker.send({ task: 'start' });
worker.on('message', msg => console.log('From worker:', msg));
```

#### Common Interview Questions

- **What is the difference between spawn and exec?** spawn streams data and is suitable for large or continuous output.
  exec buffers the entire output in memory and is simpler for small commands but can cause memory issues for large results.
- **When would you use fork instead of spawn?** When starting another Node.js process and needing structured communication via IPC. fork sets up a message channel and allows sending JavaScript objects between processes.
- **Do child processes share memory with the parent process?** No. Each child process has its own memory space. Communication happens via IPC or standard streams, which makes child processes safer but slower to communicate with than threads.
- **How do child processes help with CPU-bound tasks in Node.js?** They allow CPU-intensive work to run in parallel on separate cores, preventing the main event loop from being blocked.
- **What is the security implication of using exec with user input?** Since exec runs a shell, unsanitized input can lead to command injection. spawn or execFile should be preferred for safer argument handling.

### Clustering & the Cluster Module

#### Definition

Clustering in Node.js is a technique for running multiple instances of a Node.js application across CPU cores while sharing the same server port. The built-in cluster module creates a master (primary) process that spawns multiple worker processes, each running its own event loop and JavaScript thread. Incoming connections are distributed among workers by the operating system or by the cluster’s internal load balancer.
This model allows Node.js to utilize multi-core systems for handling large numbers of concurrent requests, while preserving the single-threaded programming model inside each worker.
#### Example

```
const cluster = require('cluster');
const http = require('http');
const os = require('os');

if (cluster.isPrimary) {
  const cpuCount = os.cpus().length;

  for (let i = 0; i < cpuCount; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker) => {
    console.log(`Worker ${worker.process.pid} died. Restarting...`);
    cluster.fork();
  });
} else {
  http.createServer((req, res) => {
    res.end(`Handled by worker ${process.pid}`);
  }).listen(3000);
}
```

Each worker runs on a separate core and handles a subset of incoming requests.

#### Common Interview Questions

- **Why is clustering needed if Node.js already supports asynchronous I/O?** Asynchronous I/O enables high concurrency but does not provide CPU parallelism. A single Node.js process can use only one core for JavaScript execution. Clustering allows multiple processes to run in parallel across cores, increasing throughput for CPU-bound and high-load servers.
- **How does the cluster module distribute incoming connections?** By default, the master process uses a round-robin strategy (on most platforms) to distribute sockets to workers. On some systems, the OS may handle the load balancing directly.
- **What happens if a worker process crashes?** The master process can listen for the exit event and spawn a new worker, enabling fault tolerance and zero-downtime recovery patterns.
- **How is state handled in a clustered Node.js application?** Workers do not share memory. Any shared state must be stored in an external system (Redis, database, message broker) or synchronized via IPC, otherwise requests handled by different workers will see inconsistent in-memory data.
- **How does clustering differ from using a reverse proxy like Nginx with multiple Node processes?** Both approaches enable multi-process scaling. The cluster module performs load balancing inside Node.js, while Nginx or similar proxies handle it externally. External load balancers provide more control, observability, and cross-host scaling, while clustering is simpler for single-machine setups.
- **Why is the cluster module less common in modern containerized deployments?** In Docker/Kubernetes environments, scaling is usually handled by running multiple container instances and letting the orchestrator or load balancer distribute traffic, making in-process clustering unnecessary or even undesirable.

### Worker Threads

#### Definition

Worker Threads provide a way to run JavaScript in parallel across multiple threads within a single Node.js process. Unlike the libuv thread pool (which is used internally for native operations), Worker Threads allow you to execute your own JavaScript code on separate threads, each with its own event loop, call stack, and memory heap.
Workers communicate with the main thread via message passing (postMessage) and can optionally share memory using SharedArrayBuffer and Atomics. This makes Worker Threads suitable for CPU-intensive tasks such as image processing, encryption, data compression, or complex calculations that would otherwise block the event loop.
#### Example

```js
// Main thread:
const { Worker } = require('worker_threads');
const worker = new Worker('./worker.js');
worker.postMessage(10);
worker.on('message', result => {
  console.log('Result from worker:', result);
});

// Worker file (worker.js):
const { parentPort } = require('worker_threads');
parentPort.on('message', (n) => {
  const result = fib(n); // CPU-intensive
  parentPort.postMessage(result);
});
function fib(n) {
  return n < 2 ? n : fib(n - 1) + fib(n - 2);
}
```

#### Common Interview Questions

- **How do Worker Threads differ from child processes?** Worker Threads run in the same process and can share memory, resulting in faster communication. Child processes are isolated OS processes with separate memory and communicate via IPC, which is slower but offers stronger isolation.
- **Do Worker Threads share the same event loop as the main thread?** No. Each worker has its own event loop and JavaScript execution context, allowing true parallel execution of JS code.
- **When should you prefer Worker Threads over the libuv thread pool?** When you need to parallelize custom JavaScript CPU-bound logic. The libuv thread pool is used automatically for certain native operations, but it cannot execute user-defined JS code.
- **How is data passed between threads?** By copying via structured cloning in postMessage, or by sharing memory using SharedArrayBuffer and coordinating with Atomics for synchronization.
- **What are the trade-offs of using Worker Threads?** They add complexity (thread management, synchronization), incur some startup overhead, and are best suited for long-running or heavy tasks. For simple I/O-bound workloads, the event loop alone is sufficient.

### HTTP & HTTPS Core Modules

#### Definition

The http and https modules are Node.js’s low-level APIs for building web servers and clients. They provide direct access to the HTTP protocol, allowing you to create servers, handle requests and responses, manage headers, status codes, streaming bodies, and work with TLS (in the case of https) without any framework abstraction.
These modules operate on top of streams: incoming requests are readable streams, and responses are writable streams. This design enables efficient handling of large payloads, backpressure, and real-time data transfer.
#### Example

```js
// Basic HTTP server:
const http = require('http');
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello from Node\n');
});
server.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});

// HTTPS server:
const https = require('https');
const fs = require('fs');
const options = {
  key: fs.readFileSync('key.pem'),
  cert: fs.readFileSync('cert.pem')
};
https.createServer(options, (req, res) => {
  res.end('Secure response');
}).listen(443);
```

#### Common Interview Questions

- **What is the difference between the http and https modules?** http creates servers over plain TCP. https adds TLS encryption on top of HTTP, requiring certificates and keys, and is used for secure communication.
- **Why are request and response objects streams?** Because HTTP bodies can be large or infinite (file uploads, downloads, streaming). Using streams allows chunked processing, backpressure handling, and low memory usage.
- **How do you read the body of an incoming HTTP request in Node.js core?** By listening to the data and end events on the request stream and accumulating the chunks, or by piping the stream into a writable or transform stream.
- **What is the difference between res.write() and res.end()?** res.write() sends a chunk of the response body but keeps the stream open. res.end() sends the final chunk and signals that the response is complete.
- **How does Node.js handle keep-alive connections?** The HTTP agent manages connection pooling and reuse. With keep-alive enabled, multiple requests can be sent over the same TCP connection, reducing latency and overhead.
- **Why do frameworks like Express exist if Node.js has http already?** The core modules are low-level and verbose. Frameworks add routing, middleware, body parsing, error handling, and abstractions that simplify building large applications while still using http/https under the hood.

### package.json Fields Relevant for Interviews

#### Definition

package.json is the manifest file of a Node.js project. It describes metadata, dependencies, entry points, scripts, and runtime constraints. In interviews, focus is usually on the fields that affect module resolution, execution, packaging, and environment compatibility rather than on descriptive metadata.
Key fields that are commonly discussed:
- name, version
- main, module, exports
- type
- scripts
- dependencies, devDependencies, peerDependencies, optionalDependencies
- engines
- bin

#### Example

```
{
  "name": "my-app",
  "version": "1.0.0",
  "type": "module",
  "main": "./dist/index.cjs",
  "module": "./dist/index.js",
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  },
  "scripts": {
    "start": "node dist/index.js",
    "dev": "nodemon src/index.ts"
  },
  "engines": {
    "node": ">=18"
  }
}
```

#### Common Interview Questions

- **What is the purpose of the main field?** It defines the default entry file when the package is imported using require() (CommonJS). If omitted, Node falls back to index.js.
- **What does the type field control?** It determines the default module system: "type": "module" → .js files are treated as ES Modules. No type or "type": "commonjs" → .js files are treated as CommonJS.
- **How does the exports field differ from main?** exports provides fine-grained, explicit control over which files are publicly accessible and supports conditional exports (import vs require, browser vs node). It is the modern, recommended alternative to exposing the entire package structure.
- **What is the difference between dependencies and devDependencies?** dependencies are required at runtime in production.
  devDependencies are only needed for development and build tooling (TypeScript, bundlers, linters, test frameworks).
- **What are peerDependencies used for?** They express compatibility requirements with a host package (e.g., a React plugin declaring it works with React 17–18). They are not installed automatically and must be provided by the consuming project.
- **What is the bin field used for?** It exposes executables when the package is installed globally or via npx, enabling CLI tools.
- **Why is the engines field important?** It declares the supported Node.js versions and allows package managers and CI systems to warn or fail when the runtime does not meet the required version.
### Package Managers: npm vs pnpm vs yarn (2026 perspective)

#### Definition

Package managers are tools that install, resolve, and manage JavaScript dependencies based on package.json and a lockfile. In the Node.js ecosystem, the three dominant managers are npm, yarn, and pnpm. By 2026, all three are mature, stable, and production-ready, but they differ in architecture, performance, disk usage, and ecosystem conventions.
#### Example

```sh
Basic commands (conceptually equivalent):
npm install
yarn install
pnpm install

All produce:
node_modules/
a lockfile (package-lock.json, yarn.lock, pnpm-lock.yaml)
```

#### Common Interview Questions

- **What is the main architectural difference between npm/yarn and pnpm?** npm and yarn create a flat node_modules tree with duplicated packages.
  pnpm uses a content-addressable global store and hard links, so each package version is stored once on disk and shared across projects, dramatically reducing disk usage and install time.
- **Why is pnpm considered faster and more deterministic?** Because it:
  - Avoids duplicate downloads via a global store.
  - Enforces strict dependency boundaries (no phantom dependencies).
  - Uses a single lockfile format that fully describes the dependency graph.
- **What are “phantom dependencies” and how does pnpm prevent them?** In npm/yarn, a package may accidentally import a dependency that exists higher in the tree but is not listed in its own package.json. pnpm’s strict node_modules structure prevents this, forcing every package to declare its dependencies explicitly.
- **Why did many large monorepos move to pnpm by 2024–2026?**
  - Massive disk savings.
  - Faster CI installs.
  - Better workspace handling.
  - Deterministic builds.
  - Stricter dependency correctness.
- **What is Yarn Berry (v2+) and how does it differ from Yarn Classic?** Yarn Berry introduced Plug’n’Play (PnP), removing node_modules entirely and resolving packages via a virtual filesystem. This improves performance but can cause tooling compatibility issues, which is why many teams either stay on Yarn Classic or migrate to pnpm instead.
- **In 2026, when would you still choose npm?**
  - Simplicity and zero setup.
  - Maximum compatibility with legacy tooling.
  - Default in Node.js installations.
  - Small projects where install speed and disk efficiency are not critical.
- **In one sentence: how would you summarize the 2026 landscape?** npm = universal baseline, yarn = legacy + PnP niche, pnpm = modern default for serious projects and monorepos.

### Performance Hooks & AsyncLocalStorage

#### Definition

Node.js provides advanced runtime introspection and context-propagation APIs through the perf_hooks and async_hooks modules. These are used for profiling, tracing, request correlation, and performance monitoring in production systems.
Two key modern tools frequently appearing in senior-level interviews:
Performance Hooks (perf_hooks) – high-resolution timing and performance measurement.
AsyncLocalStorage (async_hooks) – per-request context storage across async boundaries (similar to ThreadLocal in Java / CLS in .NET).
#### Common Interview Questions

- **What problem does AsyncLocalStorage solve?** It preserves contextual data (request ID, user, transaction, trace ID) across asynchronous calls without manually passing parameters through every function.
- **How is AsyncLocalStorage different from global variables?** Globals are shared across all requests and cause race conditions. AsyncLocalStorage creates an isolated context per async execution chain, safe for concurrent requests.
- **What are typical real-world uses of AsyncLocalStorage?** Distributed tracing, structured logging, APM correlation, per-request security context, multi-tenant isolation.
- **How does AsyncLocalStorage work internally?** It builds on the async_hooks API, tracking async resource lifecycles and restoring the correct store when execution resumes after awaits, timers, I/O, or Promises.
- **When should you avoid AsyncLocalStorage?** In extremely high-throughput, low-latency paths where the overhead of async hooks may become measurable, or in simple services where explicit parameter passing is clearer.
- **What is the difference between performance.now() and Date.now()?** performance.now() provides monotonic, high-resolution timestamps (sub-millisecond, unaffected by system clock changes). Date.now() is wall-clock time with lower precision.
- **Why are performance hooks important in production Node systems?** They enable:
  Precise latency measurement
  Event loop delay tracking
  Garbage collection profiling
  Custom APM instrumentation
  Bottleneck detection in async pipelines

## React Deep Dive
This section explores advanced React concepts beyond basic hooks and components.

### State vs Props

#### Definition

State and props are the two fundamental data sources for React components, but their relationship and usage patterns have evolved significantly across React versions, particularly with the introduction of hooks and modern state management patterns.
#### Evolution of State Management

- React 0.14-16.7: State only available in class components via this.state and this.setState()
- React 16.8 (2019): useState hook brings state to functional components
- React 18 (2022): Automatic batching, concurrent state updates with startTransition
- React 19 (2024): Enhanced state patterns with useOptimistic and form state hooks
#### Props Evolution

- Early React: Props primarily for parent-child communication
- React 16.3 (2018): New Context API enables prop drilling avoidance
- Modern React: Props combined with composition patterns and state management libraries

#### Common Interview Questions

- **How has the relationship between state and props evolved since early React versions?** Early React had a clear separation: props for external data flow, state for internal management. Modern React blurs these lines with context, state management libraries, and patterns like lifting state up, making data flow more flexible but requiring clearer architectural decisions.
- **What are the key differences in state management between class and functional components?** Class components use this.setState() which merges updates, while functional components use state setters that replace values. Functional components also enable more granular state through multiple useState calls and better encapsulation with custom hooks.
- **How do React 18's concurrent features affect state updates?** startTransition allows marking state updates as non-urgent, enabling React to interrupt rendering and keep the UI responsive during heavy state transitions.
- **When should state be lifted up to a parent component versus kept local?** Lift state up when multiple components need to share and synchronize the same data. Keep state local when it's only relevant to a single component or its immediate children. Modern state management libraries often provide alternative patterns via context or global stores.
- **How has the "props down, events up" pattern evolved with hooks and context?** While the fundamental pattern remains, context API and state management libraries now provide alternatives to prop drilling. However, explicit props are still preferred for component reusability and clarity.
- **What are the performance implications of passing new object/function references as props?** Passing new references can cause unnecessary re-renders. Solutions include useMemo/useCallback for stabilization, or using state management that provides stable references.
- **How do modern React patterns like Server Components affect state and props?** Server Components can pass data directly to Client Components as serializable props, but cannot use state or effects. This encourages moving state management to the appropriate component type (server vs client).

### React Component Communication Patterns

#### Definition

React emphasizes unidirectional data flow and explicit communication patterns. Components communicate through props, callbacks, and shared state, with Context API and custom hooks providing solutions for complex scenarios. Understanding React's composition model is key to effective component communication.
#### Common Interview Questions

- **What are the main component communication patterns in React and when should you use each?** Use Props for parent-to-child data flow, Callback Functions for child-to-parent communication, Context API for sharing data across many components, Lifting State Up for sibling communication, Custom Hooks for logic sharing, and State Management Libraries (Redux, Zustand) for complex global state.
- **How do you choose between Context API and prop drilling?** Use prop drilling for 2-3 levels of component hierarchy where the data flow is clear. Use Context API when you have deeply nested components that need the same data, or when intermediate components don't need the data (prop drilling becomes messy).
- **What is the difference between lifting state up and using Context API?** Lifting state up moves shared state to the nearest common ancestor, making data flow explicit but potentially causing prop drilling. Context API allows components to access state directly without passing props through every level, but can make data flow less transparent.
- **How do custom hooks help with component communication?** Custom hooks encapsulate and share stateful logic between components, allowing multiple components to use the same logic without sharing the actual state instance. Each component gets its own isolated state from the hook.
- **When should you upgrade from local state to a state management library?** Move to state management when: multiple unrelated components need the same data, you need time-travel debugging, state logic becomes complex and hard to test, or when prop drilling and context become unmanageable.
#### Communication Patterns Reference
1. Props (Parent -> Child)
   - Step 1: Parent passes data to child via props: `<Child data={value} />`.
   - Step 2: Child receives props as function parameters: function Child({ data }).
   - Step 3: Child uses props in rendering and logic; props are read-only.
   - Note: Props changes trigger re-renders. Use React.memo() to optimize if props haven't changed.
2. Callback Functions (Child -> Parent)
   - Step 1: Parent passes callback function as prop: `<Child onUpdate={handleUpdate} />`.
   - Step 2: Child calls callback with data: props.onUpdate(newData).
   - Step 3: Parent handles the callback and updates its state accordingly.
   - Note: Use useCallback() to prevent unnecessary re-renders of child components.
3. Lifting State Up (Sibling communication)
   - Step 1: Identify the nearest common parent of components that need shared state.
   - Step 2: Move the state to the common parent component.
   - Step 3: Pass state and callbacks down as props to child components.
   - Note: This makes data flow explicit but can lead to prop drilling in deep hierarchies.
4. Context API (Any components)
   - Step 1: Create context: const MyContext = createContext().
   - Step 2: Wrap components with Provider: `<MyContext.Provider value={data}>`.
   - Step 3: Consume context in children: const data = useContext(MyContext).
   - Note: Context value changes trigger re-renders in all consuming components. Optimize with memoization.
5. useReducer + Context (Global state)
   - Step 1: Create reducer function and initial state.
   - Step 2: Use useReducer hook in parent component.
   - Step 3: Provide dispatch and state via Context to child components.
   - Note: Similar to Redux pattern but built-in. Good for complex state transitions.
6. Custom Hooks (Logic sharing)
   - Step 1: Create hook that uses built-in hooks: function useCustomHook() { ... }.
   - Step 2: Multiple components use the same custom hook.
   - Step 3: Each component gets isolated state instance from the hook.
   - Note: Hooks share logic but not state. For shared state, combine with Context or state management.
7. Forward Refs (Parent accessing Child)
   - Step 1: Wrap child component with forwardRef().
   - Step 2: Child accepts ref as second parameter and attaches to DOM element.
   - Step 3: Parent creates ref and passes to child: `<Child ref={childRef} />`.
   - Note: Use sparingly as it breaks component encapsulation. Prefer declarative props when possible.
8. State Management Libraries (Global)
   - Step 1: Choose library (Redux Toolkit, Zustand, Jotai) and set up store.
   - Step 2: Create slices/stores with state and actions.
   - Step 3: Components use hooks to access state and dispatch actions.
   - Note: Redux Toolkit is recommended for Redux; Zustand is simpler for most use cases.
### Built-in hooks

#### Definition

Hooks are functions that enable functional components to use React features like state, lifecycle, and context that were previously only available in class components. Introduced in React 16.8, hooks represent a fundamental shift in React's architecture towards functional programming patterns.
#### Version Evolution Timeline

- React 16.8 (2019): Basic Hooks - useState, useEffect, useContext, useReducer, useCallback, useMemo, useRef, useImperativeHandle, useLayoutEffect
- React 18 (2022): Concurrent Hooks - useId, useSyncExternalStore, useTransition, useDeferredValue
- React 19 (2024): Action Hooks - use hook, useOptimistic, useFormState, useFormStatus
- React 19.2 (2025): useEffectEvent for separating event logic from effects
- Note on React 19 features: While these hooks are documented in React RFCs and available in Canary releases, production adoption may vary. Always check the official React documentation for stable release status and consider progressive adoption strategies for mission-critical applications.
#### Common Interview Questions

- **How did hooks change React development patterns since their introduction in 16.8?** Hooks enabled functional components to fully replace class components, introduced better code reuse through custom hooks, simplified complex component logic, and paved the way for concurrent features.
- **What problem do concurrent hooks (useTransition, useDeferredValue) solve in React 18?** They enable non-blocking user interfaces by allowing React to interrupt rendering, prioritize urgent updates (like typing), and defer non-urgent updates (like rendering large lists), improving perceived performance.
- **How does the use hook in React 19 differ from traditional async patterns?** The use hook allows components to consume Promises and Context directly in render, simplifying data fetching patterns and enabling more natural async/await syntax in components.
- **What does useEffectEvent solve?** It lets you move event-like logic out of effects while still reading the latest props/state, reducing stale-closure bugs without re-running the effect.
- **When would you choose useLayoutEffect over useEffect?** useLayoutEffect runs synchronously after DOM mutations but before paint, making it suitable for measurements or DOM manipulations that must happen before the browser repaints. useEffect runs asynchronously after paint.
- **How do you run code when a component mounts and unmounts?** Use the dependency array for mount timing and return a cleanup function for unmount. Empty array [] runs once after mount; return function handles unmount cleanup.
- **What common operations require cleanup on unmount?** Timers/Intervals  -  clearTimeout, clearInterval
  Subscriptions  -  unsubscribe, websocket.close
  Event Listeners  -  removeEventListener
  API Requests  -  AbortController.abort
  DOM References  -  document.removeEventListener
- **What's the difference between empty dependency array and no dependency array in useEffect?** Empty array [] runs once after mount. No array runs after every render. Missing dependency array is a common source of infinite loops.
- **When would you need to use useRef with useEffect?** When you need to access the latest state/value in a cleanup function without causing re-renders.
- **What's the difference between useMemo and useCallback, and when should each be used?** useMemo memoizes computed values to avoid expensive recalculations. useCallback memoizes function references to prevent unnecessary re-renders in child components. Use useMemo for expensive computations, useCallback for function props passed to optimized children.
- **How have hooks evolved to handle common patterns like forms and optimistic updates?** React 19 introduced useFormState, useFormStatus, and useOptimistic to provide built-in solutions for patterns that previously required external libraries or complex custom hook implementations.
- **What are the Rules of Hooks and why are they necessary?** Hooks must be called at the top level (not in loops/conditions) and only from React functions. These rules ensure hooks are called in the same order each render, which is essential for React to correctly preserve state between re-renders.
  Key Lifecycle Patterns:
  Mount  -  useEffect(fn, [])
  Update  -  useEffect(fn, [dep])
  Unmount  -  useEffect(() => { return cleanup }, [])
  Every Render  -  useEffect(fn) (use sparingly)

### React Component Lifecycle

#### Definition

The React component lifecycle refers to the series of phases a component goes through from creation to removal from the DOM. In functional components, lifecycle management is handled primarily through useEffect and other hooks, replacing the class component lifecycle methods (componentDidMount, componentDidUpdate, componentWillUnmount). Understanding lifecycle phases is crucial for proper resource management, performance optimization, and preventing memory leaks.
#### Lifecycle Phases

- **Mounting** - Component is created and inserted into DOM
- **Updating** - Component re-renders due to state or prop changes
- **Unmounting** - Component is removed from DOM
- **Error Handling** - Catching errors in child components

#### Common Interview Questions

- **What happens during component mount and unmount?** Mount Phase  -  DOM elements created and inserted, initial state established, useEffect with empty dependency array runs. Common operations: API calls, analytics, subscriptions.
  Unmount Phase  -  Component removed from DOM, cleanup functions run, state and effects destroyed. Common operations: cleanup, resource disposal.
- **How do you temporarily hide UI without unmounting it?** Use `<Activity>` (React 19.2+) to hide and restore a subtree while preserving its internal state, useful for tabs and transitions.
- **How do you prevent memory leaks in React components?** Always clean up intervals and timeouts
  Cancel ongoing API requests with AbortController
  Remove event listeners properly
  Unsubscribe from observables and stores
  Clear any external references in cleanup functions

### Context & Providers

#### Definition

Context provides a way to pass data through the component tree without having to pass props down manually at every level. The Context API has undergone significant evolution, from an experimental feature to a core React pattern for state distribution.
#### Version Evolution Timeline

- React 0.14 (2015): Experimental context (unstable, undocumented)
- React 16.3 (2018): New Context API - stable, documented API with createContext, Provider, Consumer
- React 16.8 (2019): useContext hook simplifies context consumption
- React 18 (2022): Concurrent rendering compatibility
- React 19 (2024): use hook can consume context, better performance patterns
#### Common Interview Questions

- **How has the Context API evolved from its experimental beginnings to the current stable API?** The API moved from an unstable, type-unsafe pattern using contextTypes to a first-class, type-safe API with createContext, explicit Providers, and multiple consumption methods (Consumer render prop, useContext hook).
- **What performance improvements were introduced in React 16.3+ context compared to the legacy context?** The new Context API uses reference identity for value propagation, enabling better optimizations and more predictable re-render behavior compared to the legacy context's experimental implementation.
- **How does the useContext hook (16.8+) improve upon the Consumer render prop pattern?** useContext provides a more ergonomic, hook-based consumption method that avoids nested render props, improves code readability, and works better with TypeScript and custom hooks.
- **What are common performance pitfalls with Context, and how have modern patterns addressed them?** Pitfall: Providing a new object value on every render causes all consumers to re-render. Solutions: useMemo for object stabilization, splitting context into state/dispatch, using selectors with libraries like use-context-selector.
- **How does context work with React 18's concurrent features?** Context values are properly propagated through concurrent renders, and context consumers respect suspense boundaries and transition priorities, maintaining consistent state across interrupted renders.
- **What's the difference between using context and prop drilling?** When is each appropriate?
  Context is ideal for truly global data (theme, auth, localization) or deep prop drilling. Explicit props are better for component composition and when data flow needs to be clear and traceable.
- **How has the role of context evolved with the rise of state management libraries?** Context shifted from being a potential state management solution to primarily a dependency injection mechanism. Modern patterns often combine context with libraries like Redux Toolkit or Zustand for complex state needs.
- **What are the limitations of context compared to dedicated state management solutions?** Context triggers re-renders for all consumers when value changes (no selective subscriptions), lacks built-in devtools, middleware, or performance optimizations that state libraries provide.

### Error Boundaries

#### Definition

Error Boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed. This concept has evolved from a class-component-only feature to more flexible modern patterns.
#### Version Evolution Timeline

- React 16 (2017): Error Boundaries introduced as class components with componentDidCatch
- React 16.2+: Community libraries emerge for functional component error boundaries
- React 18 (2022): Improved error recovery and development error overlay
- React 19 (2024): Continued class-component requirement but better error messaging
#### Common Interview Questions

- **Why are Error Boundaries still implemented as class components in modern React?** Error Boundaries require lifecycle methods (componentDidCatch) that hook into React's internal error handling mechanism. As of React 19, there's no hook equivalent because error catching needs to happen during the render phase lifecycle.
- **How has error handling evolved with community solutions despite the class component requirement?** Libraries like react-error-boundary provide functional-style APIs that wrap the class component implementation, offering better developer experience with features like error resetting, recovery callbacks, and more flexible fallback UIs.
- **What types of errors do Error Boundaries catch versus what they miss?** Caught: Errors during render, in lifecycle methods, in constructors of child components. Not Caught: Event handlers, asynchronous code (setTimeout, promises), server-side rendering errors, errors in the error boundary itself.
- **How do you handle errors that Error Boundaries can't catch?** Use window.onerror for global errors, window.addEventListener('unhandledrejection') for promise rejections, and try/catch blocks in event handlers and async operations.
- **What improvements did React 18 bring to error handling?** Better error messages in development, improved component stacks for debugging, and more resilient error recovery in concurrent rendering mode.
- **How would you implement error boundaries for a large application?** Use a hierarchical approach: a top-level boundary for critical errors, feature-level boundaries for sections of the app, and component-level boundaries for isolated features. Each level can have different fallback strategies.
- **What's the difference between getDerivedStateFromError and componentDidCatch?** getDerivedStateFromError is static and used to render a fallback UI. componentDidCatch is for side effects like logging. Both are called during the "commit" phase when the error is detected.

### Render Props / HOCs / Composite components

#### Definition

These were the primary composition patterns before hooks revolutionized React component reuse. Understanding their evolution shows how React's composition model progressed from complex patterns to simpler hook-based solutions.
#### Version Evolution Timeline

- React 0.14-15 (2015-2016): HOCs dominate - the primary reuse pattern (influenced by React-Redux connect)
- React 16+ (2017): Render Props gain popularity as an alternative to HOCs
- React 16.3 (2018): Context API enables compound components without prop drilling
- React 16.8 (2019): Custom hooks largely replace both HOCs and render props for logic reuse
- React 18+ (2022): Patterns coexist with hooks, each serving specific use cases
#### Common Interview Questions

- **How did the introduction of hooks change the prevalence of HOCs and render props?** Hooks provided a simpler, more direct way to reuse stateful logic, making complex HOC chains and render prop nesting largely unnecessary for logic reuse. However, each pattern still has specific use cases.
- **What are the modern use cases where HOCs or render props might still be preferable to hooks?** HOCs are still useful for component instrumentation (logging, analytics wrappers). Render props work well for UI components that need flexible rendering. Compound components excel for building component libraries with complex internal state.
- **How did the new Context API (16.3+) enable better compound component patterns?** Context provided a clean way for compound components to share internal state without prop drilling, making the pattern more practical and maintainable compared to previous React.cloneElement approaches.
- **What were the main problems with HOCs that hooks solved?** "Wrapper hell" (deep component nesting), name collisions, ref forwarding issues, and the complexity of composing multiple HOCs together. Hooks provide flat composition without changing component hierarchy.
- **How do you decide between custom hooks, compound components, or render props today?** Use custom hooks for stateful logic reuse, compound components for building complex UI components with internal state, and render props when you need maximum rendering flexibility. Often, a combination works best.
- **What performance considerations differ between these patterns?** HOCs create new component classes on each render. Render props create new function references. Compound components with context can cause unnecessary re-renders if not optimized. Hooks have the most predictable performance characteristics.
- **How has TypeScript support evolved across these patterns?** HOCs were notoriously difficult to type correctly. Render props and hooks have much better TypeScript inference. Modern patterns prioritize type safety from the ground up.

### React Hook Form

#### Definition

React Hook Form emerged as a performance-focused alternative to existing form libraries, leveraging uncontrolled inputs and native HTML validation to minimize re-renders. Its evolution reflects the React ecosystem's shift toward performance optimization and developer experience.
#### Version Evolution Timeline

- v6 (2019-2020): Initial release focusing on performance with uncontrolled inputs
- v7 (2021): TypeScript rewrite, improved TypeScript support, better DX
- v7.20+ (2022): useFieldArray improvements, useFormContext enhancements
- v7.30+ (2023): Strictly typed formState, better validation integration
- v7.40+ (2024): React 19 compatibility, enhanced useFormState integration
#### Common Interview Questions

- **How has React Hook Form's performance story evolved across versions?** Early versions focused heavily on uncontrolled inputs for minimal re-renders. Recent versions maintain performance while adding features like real-time validation, without sacrificing the core performance benefits.
- **What significant improvements did v7 bring compared to v6?** Complete TypeScript rewrite providing excellent type inference, better developer experience with improved error messages, enhanced useFieldArray for dynamic forms, and more flexible validation strategies.
- **How does React Hook Form integrate with modern validation libraries like Zod and Yup?** Through resolver adapters (yupResolver, zodResolver) that allow using schema-based validation while maintaining RHF's performance characteristics. The library converts schema errors to RHF's error format.
- **What's the evolution of the Controller component and when should it be used?** Initially for integrating controlled components, now also handles complex UI libraries. Use Controller for third-party components that don't expose refs, or when you need fine-grained control over re-renders.
- **How has React Hook Form adapted to React 18+ concurrent features?** Better handling of concurrent renders, proper cleanup of effects, and compatibility with concurrent mode patterns. The library respects React's rendering priorities.
- **What are the main differences between React Hook Form's approach and Formik's?** RHF uses uncontrolled inputs with refs (performance), while Formik uses controlled inputs (familiarity). RHF leverages native HTML validation; Formik implements custom validation. RHF has zero re-renders per keystroke; Formik re-renders on every change.
- **How does useFormContext enable better form composition in complex applications?** Allows deeply nested form components to access form methods without prop drilling, enabling better component separation and reuse in large form architectures.
- **What performance optimizations has React Hook Form maintained throughout its evolution?** Minimal re-renders through uncontrolled inputs, optimized validation execution, smart subscription management to formState, and efficient dirty field tracking.

### Formik

#### Definition

Formik was one of the first comprehensive form libraries for React, pioneering the controlled component approach to form management. Its evolution reflects the React ecosystem's journey from class-based forms to hooks, and eventually to more performance-focused alternatives.
#### Version Evolution Timeline

- v1 (2017): Initial release with render props and HOC patterns
- v2 (2019): Hook support added with useFormik, TypeScript improvements
- v2.1+ (2020): useField hook, better TypeScript support
- Maintenance Mode (2021+): Development slows as React Hook Form gains popularity
- Current State: Stable but largely in maintenance, with React Hook Form as the modern alternative
#### Common Interview Questions

- **How did Formik's architecture evolve from render props to hooks?** Formik started with a render props pattern (v1) which was familiar to React developers at the time. With the introduction of hooks in React 16.8, Formik added useFormik and useField hooks (v2) to provide a more modern API while maintaining backward compatibility.
- **What were Formik's main contributions to the React form ecosystem?** Formik popularized the concept of a comprehensive form library, introduced seamless Yup integration, demonstrated effective validation patterns, and provided a blueprint for form state management that influenced later libraries.
- **Why did React Hook Form largely replace Formik in modern applications?** RHF's uncontrolled input approach provided significantly better performance for large forms, zero re-renders on keystrokes, and smaller bundle size. Formik's controlled component approach caused performance issues with complex forms.
- **What is the purpose of FastField and how does it optimize performance?** FastField is a performance-optimized version of Field that only re-renders when its specific field value changes, unlike regular Field which re-renders on any form change. This helps prevent unnecessary re-renders in large forms.
- **How does Formik's validation approach differ from React Hook Form's?** Formik typically uses schema-based validation (Yup) and runs validation on every change/blur/submit. RHF leverages HTML5 validation and native browser APIs, with more granular control over validation timing.
- **What are Formik's strengths compared to React Hook Form?** More intuitive for developers familiar with controlled components, excellent Yup integration, simpler mental model for basic forms, and better support for complex conditional logic in some cases.
- **How did Formik handle the transition to TypeScript?** Formik v2 included significant TypeScript improvements with better type inference for form values, field names, and error types, though it never achieved the same level of type safety as RHF v7+.
- **What is Formik's current status in the React ecosystem?** Largely in maintenance mode with minimal active development. It's still used in many legacy codebases but new projects typically choose React Hook Form for better performance and active maintenance.

### React Router

#### Definition

React Router has been the standard routing solution for React applications since the early days of the ecosystem. Its evolution mirrors React's own journey from class components to hooks, and from simple SPA routing to full-stack application patterns.
#### Version Evolution Timeline

- v1-2 (2015-2016): Early versions with basic routing, mixin-based patterns
- v3 (2016): Stable API with configuration-based routing
- v4 (2017): Declarative routing revolution - components as routes
- v5 (2019): Stability release with hooks support
- v6 (2021): Modern rewrite with data APIs, improved nested routing
- v6.4+ (2022): Data routers, loaders, actions (Remix-inspired patterns)
- v6.8+ (2023): Future flags for v7 features, better TypeScript
#### Common Interview Questions

- **How did React Router's architecture change from v3 to v4?** v3 used a configuration-based approach with a central route config. v4 introduced a declarative, component-based approach where routes are components that render based on the current location, enabling more dynamic routing patterns.
- **What are the key differences between React Router v5 and v6?** v6 uses element prop instead of component, Routes instead of Switch, relative paths by default, required Outlet for nested routes, and new hooks like useNavigate instead of useHistory.
- **How do data routers (v6.4+) change the way we think about routing?** Data routers move routing configuration outside components, enable data loading before navigation, provide built-in form handling with actions, and support error boundaries at the route level - similar to Remix's approach.
- **What problem do loaders and actions solve in modern React Router?** Loaders enable data fetching before a component renders, eliminating loading states within components. Actions handle form submissions at the route level, providing a more integrated approach to data mutations.
- **How has React Router's approach to code splitting evolved?** Early versions required manual code splitting. v6+ integrates well with React.lazy and Suspense, and data routers can automatically handle loading states during lazy route transitions.
- **What's the difference between useNavigate and the old useHistory?** useNavigate returns a function that can be called with a path or numerical delta, while useHistory returned an object with methods like push, replace, and goBack. useNavigate is more concise and consistent.
- **How does React Router handle authentication and protected routes in v6?** Through loader functions that can redirect or throw errors for unauthorized access, combined with route-level error boundaries to handle authentication failures gracefully.
- **What are the performance implications of React Router's evolution?** v6's relative paths and improved matching algorithm provide better performance. Data routers can reduce client-side JavaScript by loading data more efficiently and enabling better server integration patterns.

### React Query

#### Definition

TanStack Query (formerly React Query) revolutionized server state management in React applications by introducing sophisticated caching, background synchronization, and mutation handling. Its evolution reflects the React ecosystem's shift from client-state-centric solutions (like Redux) to specialized server-state management.
#### Version Evolution Timeline

- v1 (2019): Initial release with basic query caching and mutations
- v2 (2020): Improved TypeScript support, infinite queries, focus on DX
- v3 (2021): Major API overhaul - better defaults, infinite query improvements
- v4 (2022): Renamed to TanStack Query, framework-agnostic core
- v5 (2023): Modernized API - removed deprecated features, better error handling
- v5.8+ (2024): Suspense integration, streaming support
#### Common Interview Questions

- **How has TanStack Query's caching strategy evolved across versions?** Early versions used simple cache keys. v3+ introduced hierarchical cache keys with arrays, enabling more precise cache management. v5 added background synchronization improvements and smarter cache invalidation.
- **What problem does TanStack Query solve that traditional useState/useEffect patterns struggle with?** It handles complex server-state concerns like caching, background updates, pagination, deduplication, error retries, and optimistic updates that are tedious to implement manually.
- **How has the mutation API evolved from v1 to v5?** v1 had basic mutation callbacks. v3 added optimistic update patterns with rollback. v5 improved error handling and integrated better with Suspense. The API moved from simple functions to a comprehensive object configuration.
- **What's the difference between useQuery and useSuspenseQuery in v5?** useQuery provides traditional loading states via isLoading/isError props. useSuspenseQuery throws promises for Suspense boundaries to catch, enabling more declarative loading states.
- **How does TanStack Query handle real-time data synchronization?** Through background refetching (window focus, network reconnect), staleTime configurations, and WebSocket integrations that update cache directly, ensuring UI consistency with server state.
- **What are the key differences between client state (Redux/Zustand) and server state (TanStack Query)?** Client state is synchronous, client-owned, and persistent. Server state is asynchronous, shared with server, and needs caching, synchronization, and mutation handling.
- **How has TypeScript support evolved across versions?** v1 had basic TypeScript support. v3 significantly improved type inference. v5 provides excellent type safety with query key inference and mutation type propagation.
- **What performance optimizations does TanStack Query provide?** Request deduplication, smart background refetching, garbage collection, pagination/infinite query optimizations, and efficient cache updates through structural sharing.

### SSR & Hydration (Server-Side Rendering)

#### Definition

Server-Side Rendering has evolved from basic string rendering to sophisticated hybrid approaches that balance performance, SEO, and developer experience. The hydration process has similarly advanced from simple DOM adoption to intelligent partial and progressive hydration strategies.
#### SSR Evolution Timeline

- React 0.14 (2015): Basic ReactDOMServer.renderToString() introduced
- React 15-16 (2016-2017): ReactDOM.hydrate() replaces render() for SSR
- Next.js 1-8 (2016-2019): Framework-level SSR abstractions
- React 18 (2022): Streaming SSR with renderToPipeableStream()
- React 19.2 (2025): Resume/prerender APIs for partial prerendering and Node Web Streams support
- Next.js 13+ (2022): App Router, React Server Components, partial hydration
- Modern Era (2023+): Islands architecture, resumability, fine-grained hydration
#### Common Interview Questions

- **How has SSR evolved from basic string rendering to modern streaming approaches?** Early SSR blocked until entire page was ready. Modern streaming SSR sends the shell immediately and streams components as they resolve, significantly improving Time to First Byte (TTFB) and perceived performance.
- **What problem does React 18's streaming SSR solve?** It eliminates the "waterfall" problem where servers wait for all data before sending HTML. Now, the shell can be sent immediately while dynamic content streams in later.
- **What do the React 19.2 resume/prerender APIs enable?** They let you pause a prerender and later resume it to a stream (`resume*`), which supports partial prerendering and more flexible streaming/hydration workflows on both Web Streams and Node streams.
- **How do React Server Components change the SSR landscape?** RSCs execute on the server only, producing zero client JavaScript. They enable finer-grained loading states, reduce bundle size, and allow direct data access without API endpoints.
- **What is cacheSignal in React Server Components?** It's a signal that lets RSCs detect when a cache lifetime ends, helping frameworks coordinate cache invalidation and refresh behavior.
- **What's the difference between hydration in React 16 vs React 18?** React 16 hydration was all-or-nothing. React 18 supports selective hydration, where interactive parts can hydrate independently from static content, improving perceived performance.
- **How does Next.js's App Router improve upon the Pages Router for SSR?** App Router uses React Server Components by default, enables nested layouts with individual loading states, supports parallel data fetching, and provides better error boundaries.
- **What are hydration mismatches and how can they be prevented?** Mismatches occur when server-rendered HTML differs from client-rendered HTML. Prevention: Avoid browser-specific APIs during render, use useEffect for side effects, and ensure consistent data between server and client.
- **What is "islands architecture" and how does it relate to hydration?** Islands architecture hydrates only interactive "islands" in a sea of static HTML. This reduces JavaScript payload and improves performance by avoiding full-page hydration.
- **How do modern frameworks handle the transition from SSR to SPA?** They use hybrid approaches: SSR for initial load, then client-side navigation (SPA) for subsequent page changes, providing both SEO benefits and smooth user experience.

### React Ecosystem Frameworks (Next.js, Remix)

#### Definition

The React ecosystem has evolved from library-only solutions to full-stack frameworks that provide opinionated, production-ready architectures. Next.js and Remix represent two different philosophical approaches to solving the same problems: performance, SEO, developer experience, and production readiness.
#### Framework Evolution Timeline

- Pre-Framework Era (2015-2016): Manual React + Router + Webpack setups
- Next.js 1-3 (2016-2018): SSR-focused framework emerges
- Gatsby Era (2018-2020): SSG-focused alternative gains popularity
- Next.js 9-12 (2019-2021): API routes, incremental static regeneration
- Remix Launch (2021): Web standards-focused framework
- Next.js 13+ (2022): App Router, React Server Components
- Modern Era (2023+): Full-stack React dominance, meta-frameworks
#### Common Interview Questions

- **How has the React ecosystem evolved from library to framework approach?** Early React required assembling many pieces manually. Modern frameworks provide integrated solutions for routing, data fetching, rendering, and deployment, reducing configuration complexity.
- **What are the key philosophical differences between Next.js and Remix?** Next.js focuses on incremental adoption and rendering flexibility. Remix emphasizes web standards, progressive enhancement, and nested routing. Both are converging on similar patterns.
- **How does the App Router in Next.js 13+ differ from the Pages Router?** App Router uses React Server Components by default, enables nested layouts, supports parallel data fetching, and provides more fine-grained loading states through the file system.
- **What problem do meta-frameworks solve that vanilla React doesn't?** They provide production-ready solutions for SSR/SSG, image optimization, font loading, performance monitoring, and deployment that would require significant manual configuration.
- **How has data fetching evolved across React frameworks?** From manual useEffect calls to getServerSideProps/getStaticProps in Next.js Pages Router, to Server Components and Server Actions in App Router, to loaders/actions in Remix.
- **What's the future of React frameworks with React Server Components?** RSCs enable zero-bundle-size components, finer-grained loading states, and direct database access from components, fundamentally changing how we think about React architecture.
- **When would you choose a meta-framework vs vanilla React + Vite?** Meta-frameworks for production applications needing SEO, performance, and complex features. Vanilla React + Vite for SPAs, internal tools, or when you need maximum control.
- **How do modern frameworks handle the trade-offs between developer experience and performance?** They use compiler optimizations, intelligent bundling, and runtime analysis to provide great DX without sacrificing performance, through features like automatic code splitting and image optimization.

### Axios & Data Fetching

#### Definition

The evolution of data fetching in React applications mirrors the broader JavaScript ecosystem's journey from XMLHttpRequest to modern fetch API and specialized HTTP clients. Axios emerged as a feature-rich alternative to native fetch, while recent trends show a move toward simpler, more integrated solutions.
#### Data Fetching Evolution Timeline

- jQuery Era (2010-2015): $.ajax() dominated frontend HTTP requests
- Native Fetch API (2015): Standards-based alternative to XMLHttpRequest
- Axios Rise (2016-2018): Feature-rich HTTP client gaining popularity
- React Query Era (2019+): Specialized server state management reduces Axios usage
- Modern Patterns (2022+): Framework-integrated fetching (Next.js, Remix), React Server Components
#### Common Interview Questions

- **How has data fetching evolved from jQuery AJAX to modern patterns?** jQuery provided a unified API but was heavy. Native fetch emerged as a standards-based solution. Axios added convenience features. Modern patterns integrate fetching with state management and framework routing.
- **What are the key differences between Axios and native fetch?** Axios has automatic JSON transformation, request/response interceptors, timeout configuration, request cancellation, and better error handling. Fetch is more lightweight but requires more boilerplate.
- **How did AbortController change request cancellation in modern JavaScript?** AbortController provided a standards-based way to cancel requests, replacing library-specific solutions like Axios CancelToken. It works consistently across fetch and Axios.
- **What role does Axios play in modern React applications with tools like React Query?** Axios serves as the HTTP transport layer, while React Query handles caching, background updates, and state management. They complement each other rather than compete.
- **How do framework-integrated data fetching solutions (Next.js, Remix) change the game?** They move data fetching closer to the component, enable server-side rendering with data, provide built-in loading states, and reduce client-side JavaScript through Server Components.
- **What are the performance implications of different data fetching strategies?** Client-side fetching increases Time to Interactive but provides better subsequent navigation. Server-side fetching improves initial load and SEO but requires server resources. Hybrid approaches provide the best balance.
- **How has error handling evolved across data fetching approaches?** From jQuery's success/error callbacks to promise-based try/catch, to React Query's built-in error states, to framework-level error boundaries and Suspense fallbacks.
- **What's the current recommended approach for data fetching in new React projects?** For SPAs: React Query/TanStack Query with Axios/fetch. For full-stack: Next.js App Router with Server Components. The choice depends on project requirements and team preferences.

### Custom Hooks

#### Definition

Custom hooks represent one of the most significant evolutionary steps in React's composition model. They emerged as the primary pattern for logic reuse after the introduction of hooks in React 16.8, fundamentally changing how developers share functionality across components.
#### Evolution Timeline

- Pre-Hooks Era (2015-2018): Logic reuse via HOCs, render props, mixins (deprecated)
- React 16.8 (2019): Hooks introduced - custom hooks become possible
- Early Adoption (2019-2020): Community discovers custom hook patterns
- Maturity (2021-2022): Best practices emerge, hook libraries proliferate
- Modern Era (2023+): Custom hooks as standard practice, framework-integrated hooks
#### Common Interview Questions

- **How did custom hooks solve problems that HOCs and render props couldn't?** Custom hooks avoid wrapper hell, provide better TypeScript support, enable easier composition, and don't interfere with the component hierarchy. They're just functions, not components.
- **What's the evolution of custom hook testing practices?** Early testing required wrapper components. Modern testing uses @testing-library/react-hooks which provides renderHook and act utilities specifically for testing hooks in isolation.
- **How do custom hooks work with React's rules of hooks?** Custom hooks must follow the same rules: only call hooks at the top level, not conditionally. The linter can verify this because custom hooks must start with "use".
- **What's the difference between a custom hook and a regular utility function?** Custom hooks can use other hooks, have access to React's lifecycle, and can cause re-renders. Utility functions are pure and don't interact with React's internals.
- **How has TypeScript support for custom hooks evolved?** Early hook typing was basic. Modern TypeScript provides excellent inference for hook return types, generic constraints, and proper typing of complex hook patterns.
- **What are common pitfalls when creating custom hooks?** Forgetting dependency arrays, not handling cleanup, over-abstraction, creating hooks that are too specific, and not considering re-render optimization.
- **How do custom hooks integrate with React 18's concurrent features?** They can use useTransition, useDeferredValue, and other concurrent features to build hooks that are aware of React's rendering priorities and can avoid blocking the main thread.
- **What's the future of custom hooks with React Server Components?** Custom hooks that rely on browser APIs won't work in Server Components. The trend is toward separating client-side and server-side logic, with hooks specifically designed for each environment.

### Virtual DOM

#### Definition

The Virtual DOM is React's abstraction layer that sits between component updates and actual browser DOM manipulations. Its implementation has evolved significantly across React versions, from a simple performance optimization to a sophisticated reconciliation engine enabling advanced features like concurrent rendering.
#### Virtual DOM Evolution Timeline

React 0.3 (2013): Initial VDOM implementation - basic tree diffing
React 15 (2016): Stack Reconciler - recursive tree traversal
React 16 (2017): Fiber Architecture - incremental rendering, time slicing
React 18 (2022): Concurrent Features - interruptible rendering, priorities
Future: Compiler optimizations - potentially reducing VDOM overhead
#### Common Interview Questions

- **How has the Virtual DOM's role evolved from React's early versions to today?** Initially, VDOM was primarily a performance optimization to batch DOM updates. Today, it's a sophisticated scheduling system that enables concurrent rendering, suspense, and priority-based updates.
- **What problem did the Fiber architecture (React 16) solve that the Stack Reconciler couldn't?** Stack Reconciler used recursive traversal that couldn't be interrupted, causing jank on large updates. Fiber introduced incremental, interruptible rendering that can pause work for high-priority updates.
- **How do React 18's concurrent features change the VDOM reconciliation process?** Concurrent features introduce priority levels to updates. High-priority work (like user input) can interrupt low-priority work (like rendering offscreen content), making the UI more responsive.
- **How does React's Virtual DOM implementation differ from that of other frameworks, like Vue?** While both use a virtual DOM, the key difference is in the reconciliation algorithm. React's Fiber architecture, introduced in v16, enables features like concurrent rendering and interruptible updates. Vue's reactivity system is more fine-grained by default, allowing it to more precisely know which components need to re-render without a full tree comparison in many cases.
- **What's the significance of the key prop in VDOM diffing, and how has its importance evolved?** Keys help React identify which items have changed, been added, or removed. Their importance increased with dynamic lists and reordering capabilities. Modern React uses keys for more efficient reconciliation and state preservation.
- **How might future React versions reduce or eliminate VDOM overhead?** Through compiler optimizations like React Forget (auto-memoization), improved tree diffing algorithms, and potentially more compile-time optimizations that reduce runtime work.
- **What's the difference between VDOM reconciliation and actual DOM manipulation performance?** VDOM reconciliation is JavaScript execution (generally fast). DOM manipulation is browser-native (potentially slow). The VDOM optimizes by minimizing expensive DOM operations through batching and diffing.
- **How does Suspense integrate with the VDOM and reconciliation process?** Suspense allows React to "pause" rendering of components that are waiting for data, showing fallback UI instead. The VDOM tracks which parts of the tree are suspended and resumes them when data is ready.

### Lazy Loading & Code Splitting

#### Definition

Code splitting has evolved from manual script management to sophisticated framework-integrated patterns that automatically optimize bundle delivery. This evolution reflects the JavaScript ecosystem's growing complexity and the need for performance optimization in modern web applications.
#### Code Splitting Evolution Timeline

Early Web (2000s): Manual script tags with dependency management
Require.js Era (2010-2014): AMD modules with runtime loading
Webpack 1-2 (2014-2016): Static code splitting with require.ensure
Webpack 3+ (2017): Dynamic import() syntax support
React 16.6 (2018): React.lazy() and Suspense for components
Modern Era (2020+): Framework-level splitting (Next.js, Vite), React Server Components
#### Common Interview Questions

- **How has code splitting evolved from manual script tags to modern frameworks?** We've moved from manual dependency management to build-tool automation, then to framework-level optimizations, and now to hybrid server-client splitting strategies.
- **What's the difference between static and dynamic code splitting?** Static splitting happens at build time (route-based chunks). Dynamic splitting happens at runtime based on user interactions or conditions.
- **How does React.lazy() differ from direct dynamic imports?** React.lazy() wraps dynamic imports with React component semantics, integrates with Suspense for loading states, and works with React's reconciliation process.
- **What problems did Suspense solve for code splitting?** Suspense provides a declarative way to handle loading states, avoids callback hell, enables better error boundaries, and allows for more sophisticated loading patterns.
- **How do modern frameworks like Next.js change code splitting strategies?** Next.js automates route-based splitting, provides built-in loading states, enables hybrid rendering, and optimizes chunk delivery through framework intelligence.
- **What's the impact of React Server Components on code splitting?** Server Components have zero client bundle size, fundamentally changing the calculus of what needs to be code-split. The focus shifts to splitting client components and managing hydration.
- **How do you decide what to code split in a modern React application?** Split routes, heavy components, below-the-fold content, features behind authentication, and third-party libraries. Use analytics to identify slow-loading components.
- **What are common pitfalls with code splitting and how have solutions evolved?** Early pitfalls: Flash of loading content, poor error handling. Modern solutions: Skeleton screens, error boundaries, preloading strategies, and progressive hydration.

### Functional Components

#### Definition

A functional component is a JavaScript function that returns JSX. The evolution of functional components mirrors React's shift from class-based to function-based architecture, with each major version adding significant capabilities.
#### Version Evolution Timeline

- React 0.14 (2015): Functional components introduced as "stateless functional components" - could only receive props and return JSX, no state or lifecycle.
- React 16.8 (2019): Hooks introduced - functional components gained full capability to manage state, side effects, and lifecycle via useState, useEffect, etc.
- React 18 (2022): Concurrent Features - functional components work seamlessly with concurrent rendering, transitions, and Suspense.
- React 19 (2024): Actions and Optimistic Updates - new hooks like useOptimistic and useFormState further enhance functional components.
#### Common Interview Questions

- **How has the role of functional components evolved across React versions?** Functional components progressed from simple presentational components (pre-16.8) to fully-featured components with hooks (16.8+), and now to advanced patterns with concurrent features and new APIs (18+).
- **What capabilities were added to functional components in React 16.8 vs React 18 vs React 19?** 16.8: State (useState), effects (useEffect), context (useContext), refs (useRef)
  18: Concurrent features (useTransition, useDeferredValue), useId, useSyncExternalStore
  19: use hook, useOptimistic, useFormState, better Suspense integration
- **Why did React shift from class components to functional components as the preferred approach?** Functional components with hooks provide simpler code structure, better composition, avoidance of this binding issues, improved tree-shaking, and better alignment with React's future direction including concurrent rendering.
- **Can functional components handle all use cases that class components could?** Yes, since React 16.8. The only exception was error boundaries, but community solutions and patterns now cover this gap. Functional components are now considered the modern standard.
- **How do functional components work with React's concurrent features?** Functional components seamlessly integrate with concurrent rendering through hooks like useTransition and useDeferredValue, allowing non-blocking state updates and improved user experience during heavy rendering.

### Controlled vs Uncontrolled Components

#### Definition

The debate between controlled and uncontrolled components represents a fundamental design decision in React forms that has evolved with the ecosystem's growing sophistication. This choice reflects the balance between React's declarative philosophy and practical performance considerations.
#### Evolution Timeline

- React 0.14 (2015): Controlled components introduced as React's declarative approach
- Early React: Uncontrolled components common for simple forms and file inputs
- React 16.8 (2019): Hooks make controlled components more ergonomic
- Form Library Era (2019+): React Hook Form popularizes uncontrolled approach for performance
- Modern Era (2022+): Hybrid approaches, framework-integrated solutions
#### Common Interview Questions

- **How has the controlled vs uncontrolled debate evolved with React's maturity?** Early React strongly favored controlled components for their declarative nature. As applications grew more complex, uncontrolled components gained popularity for performance. Modern libraries like React Hook Form show that both approaches have their place.
- **What performance issues led to the resurgence of uncontrolled components?** Large forms with controlled components caused re-renders on every keystroke, leading to janky user experiences. Uncontrolled components with refs avoid these re-renders while still providing React integration.
- **How do modern form libraries like React Hook Form bridge the controlled/uncontrolled gap?** They use uncontrolled inputs for performance but provide controlled-like APIs through refs and state management, offering the best of both worlds.
- **What's the impact of React Server Components on form patterns?** Server Components enable forms that submit directly to server actions, reducing client-side JavaScript and blurring the line between controlled and uncontrolled approaches.
- **When are controlled components still preferable despite performance concerns?** For real-time validation, complex conditional logic, immediate UI feedback, and when integrating with state management systems that require full control over form state.
- **How do React 19's new hooks like useActionState change form handling?** They provide built-in support for server actions with loading states and error handling, making it easier to build forms that work well with both client and server rendering.
- **What are the accessibility considerations for each approach?** Controlled components make it easier to implement real-time ARIA live regions for validation feedback. Uncontrolled components require more manual accessibility implementation but can be made accessible with proper patterns.
- **How should developers choose between approaches in modern React applications?** Use controlled components for simple forms with real-time needs. Use uncontrolled approaches (with libraries) for complex, performance-sensitive forms. Consider hybrid approaches for the best balance.

### Class Components

#### Definition

A class component is an ES6 class that extends React.Component (or React.PureComponent) and implements a render() method returning JSX. Before React 16.8, class components were the only way to use local state, lifecycle methods, error boundaries, and refs.
#### Evolution Context

- React 0.13 (2015): Class components introduced as primary component type
- React 16.8 (2019): Hooks introduced - functional components gained parity
- React 18+ (2022+): Functional components become recommended approach
- Current Status: Class components are legacy patterns primarily used for maintaining existing codebases
#### Modern Usage Perspective

- While class components are no longer the recommended approach for new development, understanding them remains valuable for:
- Maintaining legacy codebases (many enterprise applications still use class components)
#### Understanding React's evolution and architectural decisions

Interview contexts where knowledge of lifecycle methods and historical patterns is tested
#### Common Interview Questions

- **When would you choose class components over functional components today?** Primarily for maintaining existing codebases or in rare cases where error boundaries are needed without external libraries. For new development, functional components with hooks are recommended.
- **What are the key differences in lifecycle management between class and functional components?** Class components use discrete lifecycle methods (componentDidMount, componentDidUpdate, componentWillUnmount), while functional components use useEffect with dependency arrays to achieve similar behavior in a more declarative way.
- **Why did the React team shift towards functional components with hooks?** Functional components provide simpler code structure, better composition through custom hooks, avoidance of this binding issues, improved performance through better optimizations, and better alignment with React's concurrent features.
- **Can functional components completely replace class components?** Yes, for virtually all use cases. The only native capability unique to class components was error boundaries via componentDidCatch, but community libraries like react-error-boundary provide this functionality for functional components.
- **What should developers know about class components when working with legacy code?** Understanding lifecycle methods, this binding patterns, legacy context API, and how to gradually migrate class components to functional components using equivalent hook patterns.
- **How do PureComponent and React.memo differ?** PureComponent is a class component that implements shallow prop/state comparison in shouldComponentUpdate, while React.memo is the functional component equivalent that memoizes based on prop changes.

### React Portals

#### Definition

React Portals enable rendering React components into DOM nodes outside their parent hierarchy while preserving React's component relationships. This feature has evolved from a solution for specific UI challenges to a fundamental tool for building accessible, well-structured applications.
#### Evolution Timeline

- React 16 (2017): Portals introduced as stable API with ReactDOM.createPortal()
- Early Use Cases: Primarily for modals, tooltips, and overlays
- React 17 (2020): Event delegation changes improve portal behavior
- Modern Era (2022+): Portals for microfrontends, accessibility, and complex layouts
- React 19 (2024): Potential portal enhancements with new features
#### Common Interview Questions

- **How have portals evolved from React 16 to modern versions?** Portals started as a basic escape hatch for DOM rendering. Modern usage includes accessibility patterns, microfrontend integration, SSR compatibility, and complex event handling scenarios.
- **What problem do portals solve that couldn't be handled with CSS positioning?** While CSS can visually position elements anywhere, portals solve React-specific issues: context preservation, event bubbling through React tree, and component hierarchy maintenance despite DOM location.
- **How do portals handle React context and event bubbling differently from regular components?** Portals render to different DOM locations but maintain their position in the React component tree. Context flows normally, and events bubble through React's synthetic event system, not the DOM hierarchy.
- **What are the accessibility considerations when using portals for modals?** Must manage focus trapping, ARIA attributes, keyboard navigation (Escape to close, Tab trapping), screen reader announcements, and ensuring the underlying page is properly hidden.
- **How do portals work with server-side rendering?** Portals are client-side only. SSR must render fallback content or nothing, then hydrate appropriately on the client. This requires careful handling to avoid hydration mismatches.
- **What are modern use cases for portals beyond modals and tooltips?** Microfrontend integration, third-party widget embedding, notification systems, complex layout management, and rendering into iframes or shadow DOM.
- **How can portals be used in microfrontend architectures?** Portals allow microfrontends to render components into designated slots in the host application while maintaining their own state management and lifecycle.
- **What performance considerations should be made when using portals?** Portal creation/cleanup overhead, memory leaks from improper cleanup, excessive re-renders affecting portal content, and impact on React's reconciliation process.

## Vue Deep Dive

### Vue 2 vs Vue 3: The Composition API Revolution

#### Definition

Vue 3 represents a significant evolution from Vue 2, introducing a new, flexible paradigm for organizing component logic while maintaining full backwards compatibility. The most transformative addition is the Composition API, a set of additive, function-based APIs that allow logic to be defined and reused more effectively than the Options API of Vue 2. Vue 3 is built from the ground up with better performance, first-class TypeScript support with stronger typing and inference, and a smaller core runtime.
#### Common Interview Questions

- **What are the primary advantages of Vue 3 over Vue 2?** Key advantages include: the Composition API for better logic reuse and organization, superior performance through a new reactivity system (using Proxies) and a more efficient virtual DOM, first-class TypeScript support with stronger typing and inference, and a smaller core runtime with improved tree-shaking. (Note: The overall bundle size of a full application using Vue Router and Pinia may be comparable to Vue 2, but the core is more optimized.)
- **Explain the fundamental difference between the Options API and the Composition API.** The Options API organizes code by options (data, methods, computed, lifecycle hooks), which can fragment related logic across different sections. The Composition API uses a setup function where all related logic (state, methods, computed properties) can be colocated, making complex components more readable and maintainable.
- **Is the Composition API a replacement for the Options API?** No, it is an additive alternative. The Options API is still fully supported and is often the best choice for simple components. The Composition API shines in complex components and for creating reusable composables.
- **What problem does the Composition API solve that the Options API struggled with?** It solves two main problems:
  Logic Fragmentation: In the Options API, code for a single feature (e.g., user authentication) could be split across data, methods, and computed, making it hard to follow.
  Logic Reuse: While Vue 2 offered mixins for reuse, they could lead to naming collisions and unclear property origins. Composables provide a clean, scalable way to extract and reuse reactive logic.
- **How does Vue 3's reactivity system differ from Vue 2's?** Vue 2 used Object.defineProperty to make data reactive, which could not detect property addition/deletion and had performance limitations with large objects. Vue 3 uses JavaScript Proxies, enabling it to track any property changes (including additions and deletions) and providing better performance for reactive objects.
- **What is the mental model for migrating a component from the Options API to the Composition API?** The mental model shifts from "filling out options in a configuration object" to "writing a function that sets up and returns reactive state." You move from declaring data, methods, and computed as separate options to defining variables and functions inside the setup function and returning what the template needs access to.
- **When would you recommend using the Options API for a new project?** The Options API is still a valid choice for low-complexity components, for developers new to Vue who find its structure easier to learn, or in codebases where the team is already proficient with it and the components are simple enough not to benefit from the Composition API's advantages.

### The Vue Instance & Application Lifecycle

#### Definition

In Vue 2, the Vue instance is the root of every application, created with new Vue(). In Vue 3, this is replaced by the application instance, created with createApp(). This change allows for better isolation between multiple Vue applications on the same page. The lifecycle of a Vue component refers to the series of phases it goes through from creation to destruction, with specific hooks available to execute code at each stage.
#### Common Interview Questions

- **What is the key difference between creating a Vue 2 instance and a Vue 3 application?** In Vue 2, you use new Vue() to create a root instance, which is a singleton pattern that can cause conflicts when multiple apps exist on a page. In Vue 3, you use const app = createApp(App) to create a context-isolated application instance, enabling multiple Vue apps to coexist independently.
- **Explain the purpose of the mount method in both Vue 2 and Vue 3.** The mount method attaches the Vue application/instance to a DOM element, initiating the compilation and rendering process. In Vue 2, it's called on the instance: new Vue({...}).mount('#app'). In Vue 3, it's called on the application instance: createApp(App).mount('#app').
- **What are the main lifecycle phases of a Vue component?** The main phases are: Creation (initial setup), Mounting (DOM insertion), Updating (re-rendering due to data changes), and Destruction (teardown and cleanup).
- **Name the most commonly used lifecycle hooks and their execution order.** The core hooks in execution order are: beforeCreate, created, beforeMount, mounted, beforeUpdate, updated, beforeUnmount (Vue 3) / beforeDestroy (Vue 2), and unmounted (Vue 3) / destroyed (Vue 2).
- **In which lifecycle hook are component data and events initialized?** Data observation and events are initialized at the end of the beforeCreate hook and are fully available within the created hook.
- **Why is the mounted hook crucial for DOM-dependent operations?** The mounted hook is called after the component's DOM has been rendered for the first time. This makes it the safest place to perform operations that require access to or manipulation of the rendered DOM elements.
- **What is the difference between beforeUnmount and unmounted?** beforeUnmount is called just before the component is torn down, when the component is still fully functional. This is the ideal place to clean up manually added event listeners or cancel network requests. unmounted is called after the component has been destroyed and its DOM elements have been removed.
- **How has the lifecycle hook naming changed from Vue 2 to Vue 3 for destruction?** Vue 2 used beforeDestroy and destroyed. Vue 3 renamed these to beforeUnmount and unmounted to better align with the mounting phase (beforeMount/mounted) and improve clarity.

### Template Syntax & Directives

#### Definition

Vue uses an HTML-based template syntax that allows you to declaratively bind the rendered DOM to the underlying component instance's data. The core of this system is built with directives, which are special attributes prefixed with v- that apply reactive behavior to the DOM. These directives are the primary mechanism for creating dynamic and interactive user interfaces in Vue.
#### Common Interview Questions

- **What is the primary purpose of Vue's template syntax?** To provide a declarative way to bind data to the DOM, enabling a clear relationship between the component's state and the rendered output. Vue compiles these templates into highly-optimized JavaScript render functions.
- **Explain the difference between the v-bind and v-on directives.** v-bind is used for one-way data binding from the component's data to an HTML attribute (e.g., id, class, href), often shortened to :. v-on is used to listen to DOM events and execute some JavaScript, often shortened to @.
- **What are the different ways to use the v-bind directive for an element's class?** You can use a string, an object (to toggle classes), or an array (to apply a list of classes). The object syntax is common for conditionally applying classes based on data properties.
- **How does the v-model directive work under the hood?** v-model is syntactic sugar that combines v-bind for value binding and v-on for input event listening. For a text input, `<input v-model="searchText">` is equivalent to `<input :value="searchText" @input="searchText = $event.target.value">`.
- **What is the purpose of the v-if vs v-show directives and when would you choose one over the other?** v-if conditionally renders the element, fully destroying and recreating it and its children/components. v-show always renders the element and toggles its CSS display property. Use v-if if the condition is unlikely to change at runtime (lazy, toggles infrequently). Use v-show for expensive elements that need to be toggled very frequently.
- **Describe the use case for the v-for key attribute. Why is it important?** The key attribute is a unique identifier for each iterated node. It helps Vue's virtual DOM algorithm identify nodes and efficiently patch and reorder elements when the list changes. Using a stable, unique key (like an id) is crucial for performance and correct component state management within lists.
- **What are the available argument modifiers for the v-on directive?** Vue provides event modifiers to handle common DOM event details. Common modifiers include .stop (equivalent to event.stopPropagation()), .prevent (equivalent to event.preventDefault()), .capture, and .self.
- **What is the purpose of the v-once and v-memo directives?** v-once renders the element and component once, skipping future updates. v-memo is a performance optimization that skips updates of a subtree unless a specific set of dependencies has changed, useful for large v-for lists.
### Reactivity Fundamentals: ref(), reactive(), and Proxies

#### Definition

Reactivity is the core feature of Vue that automatically updates the DOM when the application state changes. Vue 3's reactivity system is built on JavaScript Proxies, a modern language feature that enables fine-grained dependency tracking and efficient updates. The primary APIs for creating reactive data are ref() and reactive(), which are part of the Composition API. Vue 3.4 improved reactivity performance, especially for large reactive graphs.
#### Common Interview Questions

- **What is the fundamental difference between ref() and reactive()?** ref() is used for creating a reactive reference to a single value (primitive or object) and requires accessing the value via the .value property in JavaScript. reactive() is used for creating a reactive object and provides direct property access without the need for .value.
- **Why does ref() exist when we have reactive()?** ref() is necessary because Vue's reactivity system relies on object properties. Primitive values (like strings, numbers, booleans) cannot be tracked as objects. ref() wraps the primitive in an object, allowing it to be reactive, whereas reactive() only works with objects.
- **How does Vue 3's Proxy-based reactivity differ from Vue 2's Object.defineProperty approach?** Vue 2's Object.defineProperty could only track property access and mutation, requiring workarounds for adding new properties (Vue.set). Vue 3's Proxies can track property addition, deletion, and array index mutation natively, providing more comprehensive reactivity with better performance.
- **When should you use ref() over reactive(), and vice versa?** Use ref() for: primitive values, when you need to replace the entire object reference, or when working with template refs. Use reactive() for: groups of data that logically belong together and you don't intend to replace the entire object reference.
- **What is the limitation of destructuring a reactive() object?** Destructuring a reactive object breaks the reactivity connection because the extracted variables are primitive values or simple references. To maintain reactivity when destructuring, you must use toRefs() which converts each property into a ref.
- **Explain the purpose of the toRef() and toRefs() utility functions.** toRef() creates a ref for a specific property on a reactive source object. toRefs() converts a reactive object into a plain object where each property is a ref pointing to the corresponding property of the original object. This is essential for maintaining reactivity when destructuring or returning reactive state from composables.
- **Why might you see .value in your JavaScript code but not in your templates?** Vue automatically unwraps refs in templates, so you can use {{ count }} instead of {{ count.value }}. However, in the `<script setup>` section, you must use .value to access and mutate the underlying value of a ref.
- **What happens if you try to use reactive() with a primitive value?** Using reactive() with a primitive value (string, number, boolean) will return the original value unchanged and will issue a warning in development mode, as reactive() only works with objects and arrays.

### Computed Properties & Watchers

#### Definition

Computed properties and watchers are two reactive composition utilities that respond to changes in state. Computed properties declaratively compute derived values based on other reactive data, caching the result until dependencies change. Watchers imperatively perform side effects in response to changes in specific reactive data sources, providing more granular control over reactive operations.
#### Common Interview Questions

- **What is the primary purpose of a computed property versus a watcher?** Computed properties are for deriving values based on other reactive state (declarative). Watchers are for performing side effects when data changes (imperative), such as fetching data, manipulating the DOM directly, or executing asynchronous operations.
- **How does the caching mechanism in computed properties work?** A computed property caches its result and only re-evaluates when one of its reactive dependencies has changed. If dependencies remain unchanged, multiple accesses return the cached value without re-running the computation function.
- **When would you choose to use a watcher over a computed property?** Use a watcher when you need to perform asynchronous operations, execute side effects that don't return a value, or when you need fine-grained control over when and how to react to changes in specific data sources.
- **What is the difference between the watch and watchEffect functions?** watch requires explicitly specifying the data source(s) to watch and provides previous and current values. watchEffect automatically tracks any reactive dependencies accessed during its synchronous execution and runs immediately, but doesn't provide the previous value.
- **How do you define a computed property in the Composition API versus the Options API?** In the Options API: computed: { fullName() { return this.first + ' ' + this.last } }. In the Composition API: const fullName = computed(() => firstName.value + ' ' + lastName.value).
- **What are the performance implications of computed properties versus methods?** Computed properties are cached based on their dependencies, so they're more efficient than methods when the same value is accessed multiple times without dependency changes. Methods run fresh on every invocation.
- **How can you watch multiple sources simultaneously with the watch function?** You can pass an array of reactive sources as the first argument: watch([source1, source2], ([newVal1, newVal2], [oldVal1, oldVal2]) => { /* handler */ }).
- **What does the once option do in watch()?** It automatically stops the watcher after the first trigger, which is useful for one-time reactions like initial fetches based on a reactive value.
- **When should you use the immediate and deep options with watchers?** Use immediate: true to trigger the callback immediately after watcher creation. Use deep: true to watch nested properties of objects, forcing the watcher to traverse deep into the object structure for changes.
- **What is a common pitfall when watching reactive objects without the deep option?** Without deep: true, the watcher only triggers when the object reference itself changes, not when its nested properties are modified. This can lead to missing important state changes in complex objects.

### Conditional Rendering & List Rendering

#### Definition

Conditional rendering controls whether elements are rendered in the DOM based on truthy conditions, primarily using the v-if, v-else-if, v-else, and v-show directives. List rendering dynamically generates elements based on an array or object data source using the v-for directive, which is essential for displaying collections of data.
#### Common Interview Questions

- **What is the key difference in DOM behavior between v-if and v-show?** v-if is "true" conditional rendering - it fully destroys and recreates elements and their child components in the DOM when toggled. v-show always renders the element and only toggles its CSS display property between none and its original value.
- **When would you choose v-show over v-if for performance reasons?** Use v-show when you need frequent toggling (high toggle frequency) and the initial render cost is acceptable. Use v-if when the condition is unlikely to change at runtime or when you want to avoid the initial render cost of hidden content.
- **Why is the key attribute crucial when using v-for?** The key attribute gives Vue a hint to track node identity, enabling efficient DOM patching and reordering. It prevents state inconsistencies in child components and ensures proper animation behavior when list items are added, removed, or reordered.
- **What happens if you use the same key value for multiple items in a v-for list?** Using duplicate keys can cause rendering errors, incorrect component state preservation, and unpredictable behavior during list updates. Keys must be unique among siblings.
- **How can you access the index when iterating with v-for?** You can access the current iteration index as the second parameter: v-for="(item, index) in items". For objects, you can access the key as the second parameter and index as the third: v-for="(value, key, index) in object".
- **What is the recommended approach for using v-if and v-for on the same element?** Avoid using both directives on the same element. Instead, move the v-if to a wrapper `<template>` tag or use a computed property to pre-filter the list before rendering with v-for.
- **How does Vue handle the lifecycle of components inside a v-if conditional?** When v-if is false, the component and its children are fully destroyed and removed from the DOM. When v-if becomes true again, the components are recreated and go through their full lifecycle (mounted, etc.).
- **What is the purpose of the v-else and v-else-if directives?** v-else provides an "else" block for v-if, and v-else-if provides an "else if" block. They must immediately follow a v-if or v-else-if element and allow for building complex conditional rendering logic.
- **How can you iterate over a range of numbers with v-for?** You can use v-for with a number: v-for="n in 10" will iterate from 1 to 10, with n taking values from 1 through 10.

### Class & Style Bindings

#### Definition

Class and style bindings are specialized forms of data binding that allow you to dynamically manipulate an element's CSS classes and inline styles based on component state. Vue provides enhanced support for both class and style attributes through the v-bind directive (often shortened to :) with special handling for objects and arrays.
#### Common Interview Questions

- **What are the three main ways to bind CSS classes in Vue?** You can bind classes using: a string (for static classes), an object (for toggling multiple classes conditionally), or an array (for applying a list of classes including conditional ones).
- **How does the object syntax for class binding work?** The object syntax allows you to toggle classes based on truthiness: :class="{ 'active': isActive, 'text-danger': hasError }". Each class will be present if the corresponding data property evaluates to true.
- **When would you use the array syntax for class binding?** Use array syntax when you need to apply a list of classes, potentially mixed with conditional classes: `:class="[baseClass, isActive ? activeClass : '', errorClass]"`.
- **How can you bind inline styles using the object syntax?** You can bind styles with an object where keys are CSS property names (in camelCase or kebab-case) and values are the style values: :style="{ color: activeColor, fontSize: fontSize + 'px' }".
- **What is the advantage of binding a style object directly versus using inline object syntax?** Binding a style object directly (:style="styleObject") keeps your template cleaner and allows you to compute complex style logic in the component's JavaScript, improving maintainability.
- **How does Vue handle vendor-prefixed CSS properties in style bindings?** When you use a CSS property that requires a vendor prefix in :style, Vue will automatically add appropriate prefixes. For example, if you use transform, Vue will detect and add -webkit-transform if needed.
- **Can you combine static classes with dynamic class bindings?** Yes, the static class attribute and dynamic :class binding merge automatically: `<div class="static" :class="{ active: isActive }">` will render as `<div class="static active">` when isActive is truthy.
- **How do you bind multiple style objects to the same element?** You can use the array syntax for styles to apply multiple style objects: :style="[baseStyles, overridingStyles]". The objects are merged, with later objects taking precedence for conflicting properties.
- **What happens when you use both static and dynamic style bindings?** Similar to class bindings, static style attributes and dynamic :style bindings are merged. For conflicting CSS properties, the dynamic binding takes precedence over the static attribute.

### Event Handling & Form Input Bindings

#### Definition

Event handling in Vue allows components to respond to user interactions through the v-on directive (shortened to @). Form input bindings create two-way data flow between form inputs and component state, primarily using the v-model directive, which simplifies synchronizing input values with reactive data.
#### Common Interview Questions

- **What is the difference between v-on:click and @click?** There is no functional difference. @click is the shorthand syntax for v-on:click. Both directives listen for the click event and execute the specified handler.
- **How do you access the native DOM event object in an event handler?** You can access the native event using the special $event variable: @click="handleClick($event)", or simply omit it if you're only using the event object in an inline handler.
- **What are event modifiers and why are they useful?** Event modifiers are postfixes denoted by a dot that indicate the directive should be called in a special way. Common modifiers include .stop (stops propagation), .prevent (prevents default action), and .self (only trigger if event.target is the element itself). They keep event handling logic declarative in templates.
- **How does v-model work under the hood for different form input types?** v-model is syntactic sugar that combines v-bind:value with v-on:input (for text inputs) or v-on:change (for checkboxes/radio buttons). It automatically handles the appropriate property and event based on the input element type.
- **What are the available modifiers for the v-model directive?** Common v-model modifiers include: .lazy (syncs after change events instead of input), .number (casts input to number), and .trim (trims whitespace from input).
- **How do you handle form submissions in Vue?** Use @submit.prevent="handleSubmit" on the form element. The .prevent modifier stops the default form submission, and the handler method can process the form data.
- **What's the difference between using v-model and manually binding :value with @input?** There is no functional difference - v-model is just more concise. However, using separate :value and @input bindings can be useful when you need more control over the data flow or want to perform custom transformations.
- **How do you handle multiple checkboxes with the same v-model?** When multiple checkboxes share the same v-model bound to an array, Vue automatically manages the array to include the values of checked boxes. Each checkbox should have a unique value attribute.
- **What is the proper way to implement custom form components with v-model?** In Vue 3, custom components can implement v-model by expecting a modelValue prop and emitting an update:modelValue event. For multiple v-model bindings, use specific prop names like v-model:title and emit update:title.
### Component Fundamentals: Props & Custom Events

#### Definition

Components are the building blocks of Vue applications, allowing you to split the UI into independent, reusable pieces. Props are custom attributes you can register on a component to pass data from a parent component down to a child component. Custom Events enable child components to communicate back to parent components by emitting events that can carry payload data.
#### Common Interview Questions

- **What are the two main ways to define props in Vue components?** Props can be defined as an array of strings (props: ['title', 'content']) for simple declaration, or as an object (props: { title: String, count: Number }) for type checking and validation.
- **What is the one-way data flow principle with props and why is it important?** Props follow a one-way data flow from parent to child. This prevents child components from accidentally mutating the parent's state, making the data flow easier to understand and debug. If a child needs to modify data, it should emit an event to request the change from the parent.
- **How do you validate props in Vue components?** When defining props as an object, you can specify validation requirements including type checking, required flag, default values, and custom validator functions to ensure props meet specific criteria.
- **What is the difference between kebab-case and camelCase for prop names?** In templates, you should use kebab-case (`<my-component user-name="John">`) because HTML is case-insensitive. In the component's JavaScript definition, use camelCase (props: ['userName']). Vue automatically handles the mapping between the two.
- **How do custom events enable child-to-parent communication?** Child components use the $emit method to trigger custom events (this.$emit('update', newValue)). Parent components listen to these events using v-on or @ (`<child-component @update="handleUpdate">`).
- **What are the best practices for naming custom events?** Use kebab-case for event names in templates, as HTML is case-insensitive. For multi-word event names, use descriptive names that clearly indicate the action being performed.
- **How can you pass data from a child to a parent component through custom events?** You can pass data as the second parameter of $emit: this.$emit('update', newValue). The parent component can access this data in the event handler method or via the $event variable in inline handlers.
- **What is the purpose of defining emitted events in a component using the emits option?** Declaring emitted events in the emits option makes the component's interface more self-documenting, enables Vue to validate event names, and allows you to define validation for event payloads in development mode.
- **How do you handle the scenario where a prop needs to be modified by a child component?** The child should not directly mutate the prop. Instead, it should emit an event to request the parent to update the original data, or use the prop as the initial value for local data and emit changes when the local data is modified.

### Vue.js Component Communication Patterns

#### Definition

Vue.js provides multiple patterns for component communication, each suited for different scenarios in the component hierarchy. Understanding when to use each pattern is crucial for building maintainable and scalable Vue applications. These patterns range from simple parent-child data flow to global state management for application-wide data sharing.
#### Common Interview Questions

- **What are the main component communication patterns in Vue.js and when should you use each?** Use Props for direct parent-to-child data flow, Custom Events for child-to-parent communication, v-model for two-way binding on form inputs, Slots for content distribution and composition, Provide/Inject for avoiding prop drilling in deep hierarchies, and Pinia for global state management across the entire application.
- **How do you choose between provide/inject and props for data passing?** Use props for direct parent-child relationships where the data flow is clear and traceable. Use provide/inject when you have deeply nested components that need access to data without intermediate components acting as pass-throughs, or when building component libraries where the component hierarchy isn't known in advance.
- **What is the difference between using slots and props for component content?** Props pass data values to child components, while slots pass template content. Use props for simple data values and configuration. Use slots when the parent needs control over the rendering of specific sections of the child component, or when building layout components that need to be highly flexible.
- **How does v-model work under the hood in Vue 3?** v-model is syntactic sugar for :modelValue prop plus @update:modelValue event. For custom components, you define a modelValue prop and emit update:modelValue events. Vue 3 also supports multiple v-model bindings using v-model:propName syntax.
- **When should you upgrade from local state to Pinia for state management?** Move to Pinia when: multiple unrelated components need the same data, you need to share state across routes, you require time-travel debugging, you have complex state logic that needs to be tested independently, or when prop drilling becomes unwieldy and hard to maintain.
#### Communication Patterns Reference
1. Props (Parent -> Child)
   - Step 1: Child defines props using defineProps in `<script setup>` or props option.
   - Step 2: Parent passes data via attributes using v-bind (e.g., :prop="value").
   - Step 3: Child uses props as reactive, read-only variables in template or script.
   - Note: Props are reactive and should not be mutated in the child to maintain unidirectional data flow.
2. Custom Events (Child -> Parent)
   - Step 1: Child defines events using defineEmits in `<script setup>` or emits option.
   - Step 2: Child emits event with optional payload using emit('event', data).
   - Step 3: Parent listens with v-on (e.g., @event="handler") and processes the event.
   - Note: Use update:propName event names for consistency with v-model.
3. v-model (Two-Way Binding)
   - Step 1: Child accepts modelValue prop (or custom prop like name).
   - Step 2: Child emits update:modelValue (or update:name) with new value.
   - Step 3: Parent uses v-model="data" (or v-model:name="data") to sync state.
   - Note: Supports custom prop/event names for multiple bindings (e.g., v-model:name).
4. Slots (Content Distribution)
   - Step 1: Child defines `<slot>` placeholders in its template (default or named).
   - Step 2: Parent provides content between component tags, optionally using v-slot for named/scoped slots.
   - Step 3: Vue renders parent's content in child's slots, with scoped slots passing child data to parent.
   - Note: Scoped slots (e.g., v-slot="{ item }") allow child-to-parent data sharing.
5. Provide/Inject (Deep Nesting)
   - Step 1: Ancestor provides data using provide(key, value) in `<script setup>`.
   - Step 2: Descendant (any level) injects data with inject(key).
   - Step 3: Descendant uses injected data, which is reactive if provided as ref or reactive.
   - Note: Ideal for avoiding prop drilling in deeply nested components.
6. State Management with Pinia (Global)
   - Step 1: Create a Pinia store using defineStore with state (e.g., ref), getters (e.g., computed), and actions.
   - Step 2: Components import the store (e.g., useCounterStore) and access state/getters/actions.
   - Step 3: Multiple components share and update the same reactive state.
   - Note: Pinia is Vue 3's recommended state management, similar to MobX; getters provide derived state.

### Slots & Content Distribution

#### Definition

Slots are a mechanism for content distribution in Vue components that allow parent components to inject content into a child component's template. They provide a way to create flexible and reusable components by leaving certain parts of the component's output open for parent-provided content, enabling complex composition patterns.
#### Common Interview Questions

- **What is the primary purpose of slots in Vue components?** Slots enable component composition by allowing parent components to pass template content to child components, creating flexible and reusable components that can adapt to different content needs while maintaining their core structure and behavior.
- **What is the difference between the default slot and named slots?** The default slot (anonymous slot) serves as the catch-all outlet for content that doesn't have a specific slot name. Named slots allow you to target specific areas in the child component's template using the v-slot directive with a slot name.
- **How do you use the v-slot directive with named slots?** In the parent template, use `<template v-slot:name>` to specify content for a named slot. The shorthand is #name. The child component uses `<slot name="name">` to define where the content should be rendered.
- **What are scoped slots and what problem do they solve?** Scoped slots allow child components to pass data back to the parent's slot content, enabling the parent to control the rendering while having access to the child's internal state. This solves the problem of making slot content dynamic based on the child component's data.
- **How do you provide fallback content in slots?** You can provide default content between the `<slot>` tags in the child component. This content will be rendered if the parent doesn't provide any content for that slot.
- **What is the difference between v-slot syntax on `<template>` versus directly on components?** v-slot can only be used on `<template>` elements unless the component has only a default slot. For default slots only, you can use v-slot directly on the component tag, but this is less common and can be confusing.
- **How can you access slot data in the parent component?** Using the scoped slot syntax: `<template #name="slotProps">` or `<template v-slot:name="slotProps">`. The slotProps object contains the data passed from the child component via v-bind on the slot.
- **What is the purpose of dynamic slot names?** Dynamic slot names allow you to determine the slot name at runtime using a variable: v-slot:[dynamicSlotName] or #[dynamicSlotName]. This enables more flexible component patterns where the slot target can change based on application state.
- **How do slots differ from props in terms of content distribution?** Props pass data values to child components, while slots pass template content. Props are better for simple data values, while slots are better for complex markup, layout composition, and when the parent needs control over the rendering of specific sections.

### Lifecycle Hooks (Options API vs Composition API)

#### Definition

Lifecycle hooks are functions that allow you to execute code at specific stages of a component's creation, update, and destruction process. Vue provides a series of hooks that correspond to different phases of the component lifecycle. The way these hooks are accessed differs between the Options API and Composition API, but they serve the same fundamental purpose.
#### Common Interview Questions

- **What are the main lifecycle phases in a Vue component?** The main phases are: Initialization (beforeCreate, created), DOM Mounting (beforeMount, mounted), Updating (beforeUpdate, updated), and Destruction (beforeUnmount, unmounted).
- **How do you access lifecycle hooks in the Options API versus the Composition API?** In the Options API, hooks are defined as methods on the component options object (created() { ... }). In the Composition API, hooks are imported from Vue and called within setup() (onMounted(() => { ... })).
- **What is the execution order of the main lifecycle hooks?** The core hooks execute in this order: beforeCreate → created → beforeMount → mounted → beforeUpdate → updated → beforeUnmount → unmounted.
  It's important to note that in the Composition API, the setup() function executes before the beforeCreate hook and serves a similar purpose to the created hook.
- **In which lifecycle hook is the component's reactive data fully available?** Reactive data becomes available in the created hook. In beforeCreate, the component's data, computed properties, and methods are not yet initialized.
- **Why is the mounted hook crucial for DOM-dependent operations?** The mounted hook is called after the component's DOM has been rendered for the first time, making it the only safe place to perform operations that require access to or manipulation of the rendered DOM elements.
- **What is the key difference between beforeCreate and created hooks?** In beforeCreate, the component instance is created but data observation, computed properties, and methods are not set up. In created, the instance is fully configured with reactive data, computed properties, methods, and watchers.
- **How do you handle cleanup logic in the Composition API versus Options API?** In the Options API, cleanup is done in beforeUnmount. In the Composition API, you can return a cleanup function from effect functions like onMounted and onUnmounted, or use onUnmounted directly for cleanup tasks.
- **What lifecycle hooks were renamed from Vue 2 to Vue 3 and why?** beforeDestroy was renamed to beforeUnmount and destroyed was renamed to unmounted to better align with the mounting phase (beforeMount/mounted) and improve naming consistency.
- **When would you use onUpdated versus a watcher for reacting to data changes?** Use onUpdated when you need to perform DOM operations after any component update. Use watchers when you need to react to changes in specific reactive properties with more granular control and access to previous values.
### Composition API Deep Dive: setup(), ref() vs reactive()

#### Definition

The Composition API is a set of function-based APIs introduced in Vue 3 that allows for better logic composition and reuse in components. The setup() function serves as the entry point for using the Composition API, where component logic is defined. ref() and reactive() are the two primary functions for creating reactive state, each with distinct use cases and behaviors.
#### Common Interview Questions

- **What is the purpose of the setup() function in the Composition API?** The setup() function is the entry point for Composition API usage. It executes before the component is created and is where you define reactive state, computed properties, functions, and lifecycle hooks, then return them for the template to access.
- **What are the main differences between using ref() and reactive()?** ref() works with any value type and requires accessing the value via .value in JavaScript. reactive() only works with objects and provides direct property access without .value. ref() is more versatile while reactive() provides cleaner syntax for objects.
- **When should you prefer ref() over reactive()?** Use ref() for primitive values, when you need to replace the entire object reference, when working with template refs, or when you need consistent syntax for both primitives and objects.
- **When is reactive() more appropriate than ref()?** Use reactive() when you have a group of properties that logically belong together and you don't plan to replace the entire object reference, providing cleaner property access without .value.
- **What are the arguments of the setup() function and what do they provide?** The setup() function receives two arguments: props (containing the component's props) and context (which provides access to attrs, slots, and emit for custom events).
- **Why is destructuring a reactive() object problematic and how do you solve it?** Destructuring breaks reactivity because it extracts primitive values. Use toRefs() to convert a reactive object into plain object with refs for each property, maintaining reactivity when destructuring.
- **What is the role of the context parameter in the setup() function?** The context parameter provides access to component features not available as props: attrs (fallthrough attributes), slots (slot content), and emit (function to emit custom events).
- **How do you access component instance properties like $emit in the Composition API?** In the Composition API, you don't use this. Instead, you access emit through the context parameter in setup(): setup(props, { emit }) { emit('event') }.
- **What is the mental model shift from Options API to Composition API?** The shift moves from organizing code by options type (data, methods, computed) to organizing code by feature/concern, colocating all logic related to a specific feature within the same scope in the setup() function.
### Compiler Macros (defineProps, defineEmits, defineModel in `<script setup>`)

#### Definition

Compiler macros are special functions that are processed at compile time rather than runtime when using Vue's `<script setup>` syntax. They provide a declarative way to define component props, emits, and model bindings directly within the script setup block. These macros are stripped away during compilation and replaced with appropriate runtime code, offering better TypeScript integration and more concise component definition compared to the Options API.
#### Common Interview Questions

- **What are compiler macros and how do they differ from regular JavaScript functions?** Compiler macros are special functions that are processed and removed during Vue's compilation process, unlike regular functions that execute at runtime. They provide hints to the Vue compiler about component structure and are replaced with actual JavaScript code in the final output.
- **What is the purpose of defineProps and how does it compare to the Options API props definition?** defineProps is used to declare component props within `<script setup>`. It's more concise than the Options API and provides better TypeScript support. Unlike the Options API where props are defined in a separate object, defineProps is called directly in the setup context and returns a reactive props object.
- **How do you define prop types and defaults using defineProps?** You can pass a type-based definition with `defineProps<{ title: string; count?: number }>()` for TypeScript, or a runtime declaration with `defineProps({ title: { type: String, required: true }, count: { type: Number, default: 0 } })`. For TypeScript with runtime defaults, use the `withDefaults` helper.
- **What is the role of defineEmits and how do you type emitted events?** defineEmits declares the events a component can emit. With TypeScript, you can use a type-based declaration: `defineEmits<{ (e: 'update', value: string): void; (e: 'submit'): void }>()`. This provides full type checking for event names and payloads.
- **What does defineModel do and when would you use it?** defineModel creates a typed `v-model` binding in `<script setup>` with optional defaults and modifiers. Use it when you want a concise, type-safe two-way binding API for a component.
- **How do you access the props and emits defined with macros in your component logic?** defineProps() returns a reactive object containing the props, which you can use directly in your template and logic. defineEmits() returns an emit function that you can call to trigger events: const emit = defineEmits(); emit('update', value).
- **What are the advantages of using compiler macros over the Options API?** Advantages include: better TypeScript integration with full type inference, reduced boilerplate, colocation of related logic, improved IDE support with autocompletion, and better tree-shaking since unused features can be eliminated at compile time.
- **Can you use defineProps and defineEmits with both TypeScript and JavaScript?** Yes, both work with JavaScript and TypeScript. However, TypeScript users get additional benefits like type checking, autocompletion, and the ability to use pure type-based definitions without runtime overhead.
- **What is the withDefaults helper and when would you use it?** withDefaults is a compiler macro that works with defineProps to provide default values for TypeScript-type-based prop definitions: `withDefaults(defineProps<{ size?: number }>(), { size: 10 })`. It's only needed when using type-only props definitions.
- **How do compiler macros enable better performance and smaller bundle sizes?** Since compiler macros are processed at build time, they can be optimized away and replaced with more efficient runtime code. This eliminates runtime overhead for prop validation in production and enables better dead code elimination through static analysis.
- **What are the limitations of using `<script setup>` with compiler macros?** Limitations include: no access to the component instance via this, more complex setup for certain advanced patterns like render functions, and potential confusion for developers coming from Options API. Some IDE setups may also require additional configuration for full support.
### Composables: The Vue Equivalent of Custom Hooks

#### Definition

Composables are functions that leverage Vue's Composition API to encapsulate and reuse stateful logic. They are the Vue equivalent of React's custom hooks, allowing you to extract reactive logic into reusable units that can be composed together in components. Composables typically use refs, reactive objects, and other Composition API functions to create self-contained logic pieces.
#### Common Interview Questions

- **What is a composable and how does it differ from a regular utility function?** A composable is a function that uses Vue's reactive system (ref, reactive, computed, etc.) to encapsulate stateful logic, while a regular utility function is typically pure and doesn't involve reactivity. Composables can maintain internal state and react to changes.
- **What are the naming conventions for composable functions?** Composables are typically prefixed with "use" (e.g., useCounter, useFetch) to distinguish them from regular functions and follow the same convention as React hooks.
- **How do composables enable better code organization compared to mixins?** Composables solve mixin problems by making the source of properties explicit through function calls, avoiding naming conflicts, and allowing types to be automatically inferred without manual merging.
- **What is the typical structure and return pattern of a composable?** Composables usually return an object or tuple containing reactive references and functions that operate on them. This allows destructuring in components while maintaining reactivity through refs.
- **How do composables handle reactivity and lifecycle?** Composables can use reactive state (ref, reactive), computed properties, and lifecycle hooks (onMounted, onUnmounted) internally. The lifecycle hooks are bound to the component that uses the composable.
- **What are common use cases for creating custom composables?** Common use cases include: data fetching (useFetch), form handling (useForm), mouse tracking (useMouse), local storage (useLocalStorage), and authentication (useAuth).
- **How do you handle asynchronous operations in composables?** Async operations in composables typically use refs to track loading state, error state, and data, often with lifecycle hooks to handle cleanup and cancellation.
- **Can composables accept parameters and how should they be made reactive?** Yes, composables can accept parameters. If parameters need to be reactive, they should be passed as refs, or the composable can convert them to refs using toRef or toRefs.
- **What is the composable equivalent of Options API patterns like data, methods, and computed?** Data becomes ref() or reactive(), methods become regular functions, and computed properties become computed() - all colocated within the composable function and returned to the component.
### Dependency Injection: provide() & inject()

#### Definition

Dependency injection in Vue is a pattern that allows ancestor components to serve as dependencies for all their descendants, regardless of how deep the component hierarchy is, without having to pass props down through every level. This is achieved through the provide() and inject() functions, which create a form of component-level dependency injection that works alongside rather than replacing props.
#### Common Interview Questions

- **What problem do provide() and inject() solve that props cannot handle efficiently?** They solve "prop drilling" - the need to pass props through multiple intermediate components that don't use them, just to get them to deeply nested child components that need them.
- **What is the key difference between using provide()/inject() and global state management like Pinia?** provide()/inject() are designed for component-scoped dependency injection within a specific subtree, while global state management is application-wide. Provide/inject is more suitable for component library authors and specific feature isolation.
- **How do you make provided values reactive?** You must provide reactive values (refs or reactive objects) explicitly. If you provide a regular JavaScript value, it won't be reactive to changes in the provider component.
- **What is the difference between providing in the Options API versus Composition API?** In Options API, use the provide option (can be object or function). In Composition API, use the provide() function within setup(). The Composition API approach is more flexible and type-friendly.
- **How can you ensure that a component only works when used within a provider component?** Use the inject() function with a default value or throw an error if the injection is not found, making the dependency requirement explicit.
- **What are the best practices for naming injection keys?** Use Symbol-based keys to avoid naming collisions, especially in large applications or when building component libraries. For smaller apps, string keys are acceptable but should be namespaced.
- **Can you update provided reactive values from the injecting component?** Yes, if you provide a reactive object or ref, the injecting component can mutate it, but this breaks the unidirectional data flow principle and should be done cautiously, typically by providing methods alongside data.
- **How does dependency injection work with TypeScript in Vue?** You can type both the provided values and injected values using TypeScript generics, but it requires careful setup with injection keys and type declarations to maintain type safety.
- **When should you avoid using provide()/inject() in favor of props?** Avoid provide/inject for simple parent-child communication or when the component relationships are shallow. Use props when the data flow is clear and the hierarchy isn't deep, as props are more explicit and easier to trace.

### Template Refs & Component Refs

#### Definition

Template refs are a feature that allows direct access to DOM elements or child component instances in your templates. By using the ref attribute, you create a reference to a specific element or component that can be accessed in your component's JavaScript logic. This provides an escape hatch from Vue's declarative paradigm when direct DOM manipulation or component instance access is necessary.
#### Common Interview Questions

- **What is the primary use case for template refs?** Template refs are used when you need direct access to DOM elements for operations like focus management, text selection, media playback control, or integrating with third-party libraries that require direct DOM manipulation.
- **How do you create a template ref in Vue 3's Composition API?** You declare a ref with `const myRef = ref(null)` and attach it to an element using the ref attribute: `<input ref="myRef">`. The DOM element will be assigned to myRef.value after component mount.
- **What is the timing consideration when accessing template refs?** Template refs are only populated after the component has mounted. Accessing them in setup() or created() lifecycle hooks will return null because the DOM hasn't been rendered yet. Use onMounted() to ensure refs are available.
- **How do template refs work with v-for directives?** When using ref inside v-for, the ref value becomes an array containing all the DOM elements or component instances. However, this array order is not guaranteed to match the data array order.
- **What is the difference between using refs on DOM elements versus component instances?** When used on DOM elements, the ref contains the actual DOM node. When used on Vue components, the ref contains the component instance, giving you access to its properties, methods, and emitted events.
- **How can you create multiple refs for elements in a list without using v-for?** You can use a function ref that gets called for each element: :ref="(el) => { if (el) itemsRefs.push(el) }". This gives you more control over how refs are collected.
- **What are the best practices for using template refs?** Use template refs sparingly as they break declarative patterns. Prefer declarative solutions when possible. Always check if the ref exists before using it, and clean up any event listeners or side effects in onUnmounted().
- **How do you access a child component's methods or data using refs?** By attaching a ref to a child component, you can access its exposed properties and methods via childRef.value.someMethod(). However, this creates tight coupling and should be used judiciously.
- **What is the TypeScript consideration when working with template refs?** You should properly type your refs: `const inputRef = ref<HTMLInputElement | null>(null)` for DOM elements, or `const childRef = ref<InstanceType<typeof ChildComponent> | null>(null)` for component instances.
### Built-in Components: `<KeepAlive>`, `<Teleport>`, `<Suspense>`

#### Definition

Vue provides several built-in components that solve common architectural patterns in web applications. `<KeepAlive>` enables state preservation for dynamic component switching, `<Teleport>` allows rendering content in different parts of the DOM tree, and `<Suspense>` provides a way to handle async component dependencies with fallback states.
#### Common Interview Questions

- **What problem does the `<KeepAlive>` component solve?** `<KeepAlive>` preserves component state and avoids re-rendering when dynamically switching between components, improving performance and maintaining user interaction state like form inputs, scroll positions, or component lifecycle.
- **How do you control which components get cached by `<KeepAlive>`?** Use the include and exclude props to specify which component names should or shouldn't be cached. You can use strings, regex patterns, or arrays to define the caching strategy.
- **What lifecycle hooks are triggered specifically for `<KeepAlive>` components?** Cached components trigger activated when inserted into the DOM and deactivated when removed from the DOM but kept in cache. These hooks help manage component state during cache transitions.
- **What is the primary use case for the `<Teleport>` component?** `<Teleport>` allows rendering a component's content in a different part of the DOM tree while maintaining its logical position in the Vue component hierarchy. This is essential for modals, tooltips, notifications, and other overlay elements.
- **How does `<Teleport>` maintain component context while rendering elsewhere?** Although the content renders in a different DOM location, the component remains part of the original parent's component tree, preserving props, event listeners, and injection context.
- **Can you conditionally disable `<Teleport>` and how?** Yes, use the disabled prop to conditionally disable teleportation. When disabled is true, the content renders in its original position instead of the target location.
- **What problem does `<Suspense>` solve in Vue applications?** `<Suspense>` provides a declarative way to handle async component dependencies and display fallback content while waiting for async operations to complete, simplifying loading state management.
- **What are the two types of async dependencies that `<Suspense>` can wait for?** `<Suspense>` can wait for: 1) Components with async setup() functions, and 2) Components with Async Components (dynamic imports with loading states).
- **What are the limitations of `<Suspense>` in its current implementation?** `<Suspense>` is still considered experimental in Vue 3, has limited nesting support, and doesn't work with Server-Side Rendering (SSR). It's primarily designed for component-level async dependencies rather than application-level data fetching.

### Error Handling with errorCaptured Hook

#### Definition

The errorCaptured hook is a lifecycle hook that allows a component to catch errors from its child components (both direct and nested descendants). Unlike React's Error Boundaries, which are component-based, errorCaptured is a hook that can be added to any component, providing a more flexible but manual approach to error handling. When an error is captured, the component can handle it, log it, and optionally prevent it from propagating further up the component tree.
#### Common Interview Questions

- **What is the errorCaptured hook and how does it differ from React's Error Boundaries?** errorCaptured is a lifecycle hook that any component can implement to catch errors from its child components. Unlike React's class-based Error Boundaries, Vue's approach is hook-based and doesn't require a special component type, offering more flexibility but requiring manual implementation.
- **What parameters does the errorCaptured hook receive?** The hook receives three parameters: (error, instance, info). error is the Error object, instance is the component instance that triggered the error, and info is a string containing information about where the error was captured (e.g., 'in render function').
- **What is the return value significance in errorCaptured?** If errorCaptured returns false, it prevents the error from propagating further up the parent chain. This allows a component to handle errors locally without affecting higher-level components.
- **What types of errors does errorCaptured catch versus what it misses?** Caught: Errors during render, in lifecycle hooks, and in custom event handlers of child components. Not Caught: Errors in async callbacks (setTimeout, Promise), event handlers in the same component where the hook is defined, or errors during SSR.
- **How would you implement a global error handler in a Vue application?** You can implement errorCaptured in the root component (App.vue) to catch all unhandled errors from descendant components. Combine this with window.onerror and window.onunhandledrejection for comprehensive error monitoring.
- **What's the execution order when multiple components in the hierarchy have errorCaptured hooks?** The hooks are triggered in the propagation order - from the component closest to the error source up to the root. Each component's errorCaptured hook is called in sequence until one returns false to stop propagation.
- **How do you handle async errors that errorCaptured cannot catch?** For async operations, use try/catch blocks within async functions and manually call error reporting services. For unhandled promise rejections, use window.addEventListener('unhandledrejection').
- **What are the best practices for using errorCaptured in production applications?** Use it strategically for critical component trees, always log errors to a monitoring service (like Sentry), provide graceful fallback UIs, and avoid overusing it as it can make error tracking more complex.
- **Can errorCaptured be used in both Options API and Composition API?** Yes, in Options API it's defined as errorCaptured() {}, and in Composition API it's used via onErrorCaptured() from the vue package.

### Render Functions & JSX in Vue

#### Definition

Render functions provide an alternative to templates by allowing you to programmatically create your component's VNode tree using JavaScript. Instead of using HTML-based templates, you use JavaScript's full programming power to describe the component's render output. JSX is a syntax extension that can be used with Vue's render functions, offering a more familiar, XML-like syntax similar to React's JSX. This approach is useful for highly dynamic components where the template structure needs to be determined programmatically. As of Vue 3.4, the global JSX namespace is no longer registered; set `jsxImportSource: "vue"` or import `vue/jsx` for TSX support.
#### Common Interview Questions

- **When would you choose render functions over templates in Vue?** Render functions are preferable when you need full programmatic control over the component's output, such as when creating highly dynamic components where the template structure is determined at runtime, when building component libraries that require maximum flexibility, or when working with complex functional components that need optimization beyond what templates can provide.
- **How does Vue's h() function compare to React's createElement?** Vue's h() (hyperscript) function is very similar to React's createElement. Both take the component/element type, props, and children as arguments. However, Vue's h() has more Vue-specific features built-in, such as direct support for Vue's directive system, scoped slots, and other Vue-specific template features that React's createElement doesn't handle.
- **What are the advantages and disadvantages of using JSX with Vue?** Advantages: Familiar syntax for React developers, better TypeScript support, full JavaScript programming power in markup, easier refactoring and code navigation. Disadvantages: Loses Vue-specific template optimizations, requires build configuration, steeper learning curve for Vue developers, and loses some Vue template features like scoped CSS in SFCs.
- **How do you handle conditional rendering and loops in render functions?** Use standard JavaScript constructs: ternary operators or && for conditionals (`condition ? h('div') : null`), and array.map() for loops (`items.map(item => h('li', item))`). This provides more flexibility than template directives but requires more manual code.
- **What is the equivalent of template directives (v-if, v-for) in render functions?** There are no direct equivalents - you use JavaScript instead. v-if becomes ternary operators or logical AND, v-for becomes array.map(), v-model becomes manual value binding and event handling, and v-bind becomes object property assignment.
- **How do you handle slots in render functions?** Access slots via the slots object in the render function's context parameter. Use slots.default() for default slot content and slots.name() for named slots. You can also pass data to scoped slots by providing arguments to the slot function calls.
- **What are the performance implications of using render functions vs templates?** Templates are generally faster for most use cases because they can be statically analyzed and optimized by Vue's compiler. Render functions have runtime overhead but can be more efficient for highly dynamic scenarios where the template compiler's optimizations don't apply.
- **How do you use JSX with Vue's Composition API?** You can return JSX directly from the setup() function, or use the `<script setup>` syntax with a JSX/TSX file. The Composition API's reactive variables and functions work seamlessly with JSX, making it a natural pairing for developers coming from React hooks.
- **What is the difference between functional components and regular components in render functions?** Functional components are stateless and instanceless - they don't have this context and receive props as the first argument. They're more performant for simple presentational components but cannot use lifecycle hooks or maintain local state.

### Custom Directives

#### Definition

Custom directives are a mechanism for creating reusable low-level DOM behavior abstractions in Vue. Similar to Angular's attribute directives, they allow you to attach custom behavior to DOM elements and can be used across components. Directives are primarily intended for direct DOM manipulation and are useful for creating reusable DOM interaction patterns that don't fit the component model, such as focus management, click-outside detection, tooltips, or integration with third-party DOM libraries.
#### Common Interview Questions

- **What are the different hook functions available in a custom directive?** Custom directives have several lifecycle hooks: created (called before the element's attributes or event listeners are applied), beforeMount (called once when the directive is first bound to the element), mounted (called when the bound element's parent component is mounted), beforeUpdate (called before the containing component's VNode is updated), updated (called after the containing component's VNode and the VNodes of its children have updated), beforeUnmount (called before the bound element's parent component is unmounted), and unmounted (called only once when the directive is unbound from the element).
- **How do you pass values and arguments to custom directives?** Values are passed through the directive's value (v-my-directive="value"), arguments are specified after a colon (v-my-directive:argument), and modifiers are specified as dots (v-my-directive.modifier). These are accessible in the directive hooks via the binding parameter as binding.value, binding.arg, and binding.modifiers.
- **What is the difference between custom directives and components?** Components are for creating reusable UI pieces with their own template and logic, while directives are for low-level DOM manipulation on existing elements. Use components when you need to create new UI elements; use directives when you need to add behavior to existing DOM elements.
- **When should you use a custom directive versus a composable?** Use custom directives when you need to directly manipulate the DOM element itself (focus, scroll, animations) or integrate with DOM-based libraries. Use composables for reactive logic and state management that doesn't require direct DOM access. Directives are element-centric, while composables are logic-centric.
- **How do you create global vs local custom directives?** Global directives are registered using app.directive('name', directiveDefinition) and are available throughout the application. Local directives are registered in a component's directives option and are only available within that component and its children.
- **What are some common real-world use cases for custom directives?** Common use cases include: click-outside detection (v-click-outside), focus management (v-focus), infinite scroll (v-infinite-scroll), permission checks (v-permission), tooltip/popover triggers (v-tooltip), and lazy loading images (v-lazy).
- **How do you handle cleanup in custom directives?** Cleanup should be performed in the beforeUnmount or unmounted hooks. This includes removing event listeners, clearing timeouts/intervals, and disconnecting observers. Any resources allocated in mounted should be released in the unmount hooks.
- **Can custom directives be used with Vue's reactivity system?** Yes, directives can react to changes in their bound value. When the value changes, the beforeUpdate and updated hooks are triggered, allowing the directive to respond to reactive changes and update the DOM behavior accordingly.
- **What is the difference between a directive's value and oldValue parameters?** In the beforeUpdate and updated hooks, the binding parameter contains both value (the current value) and oldValue (the previous value). This allows you to compare changes and only update the DOM when necessary for performance optimization.

### Plugins

#### Definition

Plugins in Vue are a way to package and distribute reusable functionality that can be easily added to Vue applications. They are the primary mechanism for adding global-level features to Vue, such as components, directives, mixins, configuration options, or instance methods. Plugins allow you to extend Vue's core capabilities and provide a clean, standardized way to integrate third-party libraries and share functionality across multiple Vue applications.
#### Common Interview Questions

- **What is the purpose of Vue plugins and when would you create one?** Plugins are used to add global-level functionality to Vue applications. You would create a plugin when you need to: share reusable functionality across multiple applications, integrate third-party libraries with Vue, add global components/directives, or extend Vue's prototype with custom methods that should be available throughout the app.
- **What is the basic structure of a Vue plugin?** A Vue plugin is either a function or an object with an install method. The function receives the Vue app instance and optional options as parameters: const myPlugin = { install(app, options) { // plugin logic } } or const myPlugin = (app, options) => { // plugin logic }.
- **How do you register a plugin in a Vue application?** Plugins are registered using the app's use() method: app.use(myPlugin, options). The options parameter is optional and gets passed to the plugin's install function. Plugins must be registered before the application is mounted.
- **What are common use cases for creating custom plugins?** Common use cases include: adding global components/directives, integrating HTTP clients (Axios), adding state management, integrating UI libraries, adding authentication utilities, providing internationalization (i18n), and creating utility libraries that need app-wide access.
- **How do plugins differ from composables in terms of scope and usage?** Plugins are app-level and modify the Vue application instance globally, while composables are component-level and provide reusable logic that components opt into. Plugins are setup once per application, while composables can be used multiple times with isolated state.
- **What is the difference between a plugin and a mixin?** Plugins are registered at the application level and can add global features, while mixins are component-level and merge logic into individual components. Plugins are more suitable for library authors and app-wide extensions, while mixins are for sharing component logic within an application.
- **How do you handle configuration options in a plugin?** Configuration options are passed as the second parameter to app.use(plugin, options) and are received in the plugin's install function. These options should be validated and used to customize the plugin's behavior. Default options can be merged with user-provided options.
- **Can plugins be asynchronous and how do you handle async initialization?** Yes, plugins can be asynchronous. The use() method returns a promise if the plugin's install function returns a promise. This allows for async plugin initialization: await app.use(asyncPlugin).
- **What are the best practices for writing maintainable plugins?** Best practices include: providing clear documentation, using TypeScript for better type safety, validating configuration options, handling errors gracefully, avoiding side effects in the plugin's top-level scope, making plugins tree-shakable when possible, and following semantic versioning for releases.
- **How do you test Vue plugins?** Plugins can be tested by creating a test Vue application, registering the plugin, and verifying that the expected functionality is added. Use testing utilities like Vue Test Utils to mount components that use the plugin's features and assert the expected behavior.
### State Management: Pinia (Modern) vs Vuex (Legacy)

#### Definition

State management in Vue provides a centralized store for managing application-level state that needs to be shared across multiple components. Vuex was the official state management library for Vue 2, following a strict Flux-based pattern. Pinia is the modern, recommended state management solution for Vue 3, offering a simpler API, excellent TypeScript support with full type inference, and Composition API integration while maintaining similar concepts.
#### Common Interview Questions

- **What are the key differences between Pinia and Vuex?** Pinia has a simpler API with less boilerplate, no mutations (only actions and state), excellent TypeScript support with full type inference, and better Composition API integration. Vuex requires more verbose code with mutations, getters, and actions.
- **Why was Pinia created as a new solution instead of evolving Vuex?** Pinia was designed to address Vuex's complexity, boilerplate, and TypeScript limitations. It provides a more intuitive API that aligns with Vue 3's Composition API while solving the same state management problems more elegantly.
- **How does Pinia handle state changes compared to Vuex?** Pinia allows direct state mutation (store.count++) in addition to actions, while Vuex requires committing mutations for state changes. Pinia's approach is more flexible but still maintains devtools support.
- **What is the equivalent of Vuex modules in Pinia?** Pinia uses multiple stores instead of modules. Each store is defined separately and can be imported where needed, providing better code splitting and TypeScript support compared to Vuex's single store with modules.
- **How does TypeScript support differ between Pinia and Vuex?** Pinia offers excellent TypeScript support with full type inference out of the box. Vuex requires more manual type definitions and has limited type inference, especially with modules and dynamic registration.
- **What are the benefits of Pinia's Composition API integration?** Pinia stores can be used directly in setup() functions with full reactivity, and you can compose store logic using composable patterns. This provides a more natural developer experience in Vue 3 applications.
- **How do you handle asynchronous operations in Pinia versus Vuex?** Both use actions for async operations, but Pinia actions are simpler and can be regular async functions. Vuex actions require context parameter and committing mutations for state changes.
- **Is Pinia compatible with Vue 2 and what are the migration considerations?** Yes, Pinia works with both Vue 2 and Vue 3. Migration from Vuex involves converting modules to separate stores, removing mutations, and updating the API calls to use Pinia's simpler syntax.
- **What are the performance implications of using multiple stores in Pinia?** Pinia's multiple stores approach has minimal performance overhead and can actually improve performance through better code splitting and lazy loading compared to Vuex's single store with all modules bundled together.

### Routing with Vue Router

#### Definition

Vue Router is the official router for Vue.js applications, enabling the creation of Single Page Applications (SPAs) with client-side navigation between views. It maps application routes to Vue components, provides navigation controls, and handles URL synchronization, allowing for bookmarkable URLs and navigation history without full page reloads.
#### Common Interview Questions

- **What are the key differences between Vue Router 3 (for Vue 2) and Vue Router 4 (for Vue 3)?** Vue Router 4 has better TypeScript support, a Composition API, dynamic routing improvements, and changes to the API to align with Vue 3's creation pattern (e.g., createRouter instead of new Router).
- **How do you implement programmatic navigation in Vue Router?** You can use router.push() to navigate to a new route, router.replace() to replace the current entry, or router.go() to navigate through history. In components, you can access the router instance via useRouter().
- **What are navigation guards and what are the different types?** Navigation guards are hooks that allow you to control navigation. Types include: global guards (beforeEach, beforeResolve, afterEach), per-route guards (beforeEnter), and in-component guards (beforeRouteEnter, beforeRouteUpdate, beforeRouteLeave).
- **How do you handle dynamic route parameters and access them in components?** Define dynamic segments in routes using colons (path: '/user/:id') and access them in components via the params object: const route = useRoute(); const userId = route.params.id.
- **What is the difference between router.push and router.replace?** router.push adds a new entry to the browser history stack, while router.replace replaces the current history entry. Use replace when you don't want the user to navigate back to the previous URL.
- **How do you implement nested routes (child routes) in Vue Router?** Define nested routes using the children property in route configuration, and use `<router-view>` components in parent components to render the child routes.
- **What are route meta fields and how are they commonly used?** Route meta fields are custom properties attached to routes, commonly used for authentication guards, breadcrumbs, page titles, or any route-specific metadata that needs to be accessed in navigation guards or components.
- **How does Vue Router handle lazy loading of route components?** Use dynamic imports with component: () => import('./views/User.vue') in route configuration. This enables code splitting where each route's component is loaded only when needed.
- **What is the purpose of the useRoute and useRouter composables?** useRoute returns the current route object (read-only, for accessing params, query, hash), while useRouter returns the router instance (for programmatic navigation and router methods). Both are used in the Composition API.

### Data Fetching & Composables

#### Definition

Data fetching in Vue involves retrieving data from external sources like APIs and managing the associated reactive state within components. Composables provide a powerful pattern for encapsulating data fetching logic, handling loading states, errors, and caching, making this functionality reusable across different components while maintaining clean separation of concerns.
#### Common Interview Questions

- **What are the advantages of using composables for data fetching over placing logic directly in components?** Composables enable logic reuse, better separation of concerns, easier testing, and consistent handling of loading states, errors, and caching across multiple components.
- **What reactive properties should a typical data fetching composable expose?** A data fetching composable typically exposes: data (the fetched result), loading (boolean indicating fetch status), error (any error that occurred), and methods like refetch or execute to trigger fetches.
- **How do you handle automatic versus manual data fetching in composables?** Use immediate execution in the composable's setup for automatic fetching on component mount, or provide an execute function that components can call manually for user-triggered fetches.
- **What is the proper way to handle cancellation of ongoing requests in data fetching composables?** Use AbortController to cancel previous requests when a new request is made or when the component unmounts, preventing memory leaks and updating stale data.
- **How can you implement caching in data fetching composables?** Implement caching by storing fetched data in a reactive reference or Map, using request parameters as keys, and returning cached data when the same request is made again within a validity period.
- **What are the considerations for using data fetching composables with Server-Side Rendering (SSR)?** For SSR, ensure composables can run on the server, handle cross-request state pollution properly, and support hydration of client-side state without refetching already available data.
- **How do you handle dependent data fetching where one request relies on another's result?** Use computed properties to create dependencies between data sources, and watch effects to trigger subsequent fetches when dependent data changes, or use async/await chaining within the composable.
- **What is the role of the onServerPrefetch lifecycle hook in SSR data fetching?** onServerPrefetch allows you to perform async operations during SSR and wait for them to complete before rendering, ensuring the server sends fully populated HTML to the client.
- **How can you share data fetching state across multiple components using composables?** Return reactive state from composables and import the same composable instance in multiple components, or combine composables with global state management (Pinia) for application-wide data sharing.
### SSR & Hydration: Nuxt.js Framework

#### Definition

Server-Side Rendering (SSR) generates HTML on the server and sends fully-rendered pages to the client, improving initial load performance, SEO, and social media sharing. Hydration is the process where Vue takes over the static HTML sent from the server and makes it interactive on the client side. Nuxt.js is a full-stack framework built on Vue that provides built-in SSR capabilities and simplifies universal application development.
#### Common Interview Questions

- **What are the primary benefits of SSR over Client-Side Rendering (CSR)?** SSR improves SEO by providing crawlable content to search engines, enables faster First Contentful Paint (FCP) and Largest Contentful Paint (LCP), and provides better social media previews with proper meta tags.
- **How does Nuxt.js simplify SSR compared to manual Vue SSR setup?** Nuxt.js provides zero-configuration SSR out of the box, file-based routing, automatic code splitting, built-in meta tag management, and simplified data fetching patterns, eliminating the need for complex webpack and Vue Router configuration.
- **What is the difference between Universal SSR and Static Site Generation (SSG) in Nuxt?** Universal SSR generates HTML on-demand for each request, while SSG pre-renders HTML at build time. Universal SSR is dynamic and personalized, SSG is faster for static content and can be served via CDN.
- **What is hydration and what common issues can occur during this process?** Hydration is when Vue attaches to server-rendered HTML and makes it interactive. Issues include hydration mismatches when server and client render different HTML, causing elements to flicker or break interactivity.
- **How does Nuxt.js handle data fetching for SSR?** Nuxt provides special hooks like useAsyncData and useFetch composables, and asyncData method (in Options API) that execute on both server and client, ensuring data is fetched before rendering.
- **What are the key rendering modes available in Nuxt 3?** Nuxt 3 supports Universal (SSR + SPA hydration), Client-Side only (SPA), and Static Site Generation (pre-rendered HTML). The rendering mode can be configured per-route or globally.
- **How do you handle authentication in Nuxt SSR applications?** Use server-side middleware for authentication checks, store tokens in httpOnly cookies (accessible on server), and use composables like useAuth to manage authentication state consistently across server and client.
- **What is the purpose of the `<NuxtLink>` component compared to regular `<a>` tags?** `<NuxtLink>` provides intelligent client-side navigation with prefetching, automatic active state management, and prevents full page reloads while maintaining SSR benefits.
- **How does Nuxt.js optimize performance through automatic code splitting?** Nuxt automatically splits code by pages and components, loading only the necessary JavaScript for each route, significantly reducing initial bundle size and improving load times.

### Server-Side Rendering (SSR) Deep Dive

#### Definition

Server-Side Rendering (SSR) in Vue is a technique where Vue components are rendered on the server into HTML strings, which are then sent directly to the client. This differs from Client-Side Rendering (CSR) where empty HTML shells are sent and Vue takes over in the browser. The hydration process then "makes the app interactive" by attaching event listeners and reactive behavior to the server-rendered HTML. SSR improves initial load performance, enables better SEO, and provides faster First Contentful Paint.
#### Common Interview Questions

- **What is the difference between Client-Side Rendering (CSR) and Server-Side Rendering (SSR) in Vue?** CSR renders the entire application in the browser using JavaScript, resulting in slower initial loads but faster subsequent navigation. SSR renders the initial page on the server as HTML, providing faster initial loads and better SEO, but requires more server resources and complex setup.
- **What is hydration and what common issues can occur during this process?** Hydration is the process where Vue attaches to server-rendered HTML and makes it interactive by adding event listeners and reactive behavior. Common issues include hydration mismatches (when server and client render different HTML), element flickering, and interactive elements not working properly due to timing issues.
- **How do you handle component lifecycle differences between server and client in SSR?** Server-side only hooks include serverPrefetch and beforeCreate/created (but without DOM access). Client-side hooks like mounted and onMounted don't run on the server. Use conditional checks with process.client/process.server or onServerPrefetch for data fetching that needs to run on both environments.
- **What are the key considerations for writing universal code (code that runs on both server and client)?** Avoid browser-specific APIs like window, document, or DOM manipulation in server context. Use conditional checks, implement safe access patterns, and ensure third-party libraries support SSR. Handle async operations carefully since server rendering is synchronous.
- **How do you manage application state and data fetching in SSR applications?** Use onServerPrefetch in Composition API or serverPrefetch in Options API to fetch data on the server. The fetched data should be serialized and sent to the client for hydration to avoid refetching. Vuex/Pinia stores need special handling for state serialization.
- **What is the difference between SSR and Static Site Generation (SSG) in Vue?** SSR generates HTML on-demand for each request, making it suitable for dynamic, personalized content. SSG pre-renders HTML at build time, making it faster for static content that doesn't change frequently. SSR requires a running server, while SSG can be served from CDN.
- **How do you handle authentication and user sessions in SSR applications?** Use cookies for authentication tokens since they're automatically sent with both server and client requests. Implement authentication middleware on the server, and ensure sensitive data isn't exposed during SSR. Handle redirects and protected routes on both server and client.
- **What are common performance optimizations for Vue SSR applications?** Implement component-level caching for frequently rendered components, use micro-caching for dynamic content, optimize bundle splitting, leverage HTTP/2 push for critical assets, and implement proper cache headers for static resources.
- **How do you debug SSR-specific issues in Vue applications?** Use Vue DevTools in SSR mode, check server logs for rendering errors, compare server-rendered HTML with client-rendered HTML for mismatches, use debugging tools like vue-server-renderer's debug mode, and implement comprehensive error logging on both server and client.
- **What are the limitations and trade-offs of using SSR in Vue applications?** Trade-offs include: increased server costs and complexity, potential for slower Time to Interactive (TTI), more complex development setup, limitations with certain browser-only libraries, and challenges with complex animations and interactions that rely on client-side timing.
### Optimization & Performance: v-once, v-memo, and Virtual Scrolling

#### Definition

Vue provides several built-in optimization techniques to improve rendering performance in different scenarios. v-once and v-memo are directives that optimize re-rendering by limiting updates, while virtual scrolling is a pattern for efficiently rendering large lists by only displaying visible items in the viewport.
#### Common Interview Questions

- **What is the purpose of the v-once directive and when should it be used?** v-once renders an element or component once and skips future updates. It's useful for static content that never changes, such as terms of service text, legal disclaimers, or any content that doesn't depend on reactive data.
- **How does v-memo differ from v-once in terms of optimization strategy?** While v-once completely skips all future updates, v-memo selectively skips updates based on dependency changes. It only re-renders when specific values in the dependency array change, providing more granular control.
- **What are the optimal use cases for the v-memo directive?** v-memo is ideal for large v-for lists where item rendering is expensive, or for complex components with stable props where you want to prevent unnecessary re-renders when unrelated parent state changes.
- **How does virtual scrolling solve performance issues with large lists?** Virtual scrolling renders only the items visible in the viewport plus a small buffer, instead of rendering thousands of DOM elements. This dramatically reduces DOM node count and improves rendering performance for large datasets.
- **What are the trade-offs of using v-once versus v-memo?** v-once is simpler but completely static. v-memo requires maintaining a dependency array but allows controlled updates. v-memo provides better flexibility for content that might need rare updates.
- **When would you choose virtual scrolling over pagination?** Choose virtual scrolling when you need continuous scrolling experience, when users need to scan through large datasets quickly, or when maintaining scroll position and context is important for the user experience.
- **How do you implement basic virtual scrolling in Vue?** Virtual scrolling requires calculating visible items based on scroll position, container height, and item height, then rendering only those items while using a spacer element to maintain correct scroll height.
- **What performance metrics improve when using these optimization techniques?** These optimizations improve Interaction to Next Paint (INP), reduce Cumulative Layout Shift (CLS), decrease JavaScript execution time, and reduce main thread blocking by minimizing unnecessary DOM operations.
- **How does v-memo interact with Vue's reactivity system?** v-memo creates a conditional reactivity boundary. The component only re-renders when the memoized dependencies change, bypassing the normal reactive update triggers for that subtree.
- **What are the common pitfalls when implementing virtual scrolling?** Common pitfalls include incorrect item height calculations, poor handling of dynamic content sizes, inadequate buffer zones causing flickering, and accessibility issues with screen readers and keyboard navigation.

## Angular Deep Dive
Angular is a TypeScript‑based framework for building single‑page applications. It uses declarative templates, dependency injection and reactive programming (RxJS) to manage complex UIs.

### Component vs Directive

#### Definition

A Component is a special type of directive that has a template and is the primary UI building block in Angular. Components manage a view (HTML template), contain business logic in their class, and can receive data via @Input() and emit events via @Output(). A Directive is a class that adds custom behavior to existing DOM elements or components. There are three types of directives: Components (directives with templates), Structural Directives (alter DOM layout by adding/removing elements, e.g., `*ngIf`, `*ngFor`), and Attribute Directives (change appearance/behavior of elements, e.g., `NgClass`, `NgStyle`).
#### Evolution Timeline

- Angular 2+ (2016): Introduced components as the primary building blocks alongside directives
- Angular 14 (2022): Standalone components introduced as developer preview
- Angular 15 (2022): Standalone components became stable
- Angular 17 (2023): Standalone components became default for new projects
#### Common Interview Questions

- **What is the key difference between a component and a directive?** A component always has a template and represents a UI section, while a directive enhances existing DOM elements or components without introducing new templates.
- **How do structural directives differ from attribute directives?** Structural directives manipulate the DOM layout by adding/removing elements (using TemplateRef and ViewContainerRef), while attribute directives change the appearance or behavior of existing elements.
- **What are standalone components and when were they introduced?** Standalone components were introduced in Angular 14 (2022) as a developer preview, became stable in Angular 15, and became the default in Angular 17. They don't require NgModules and can import dependencies directly.
- **Can you mix NgModule-based and standalone components in the same application?** Yes, Angular supports a gradual migration path. You can bootstrap standalone components while still using NgModule-based components elsewhere in the application.

### Basic Bindings & Services

#### Definition

Bindings connect template markup to component data. Main flavors: interpolation ({{value}}), property ([src]="url"), event ((click)="save()"), two-way ([(ngModel)]="name"). Services are classes annotated with @Injectable() that encapsulate shared logic and are injected via Angular’s DI system.
#### Common Interview Questions

- **Why use a service instead of sharing logic directly between components?** Services promote single responsibility, reusability, and make it easy to test logic in isolation.
- **What does [(ngModel)] desugar to?** [ngModel]="value" + (ngModelChange)="value=$event" (property + event binding pair).
- **How do you scope a service to a lazy module?** Provide it in that module’s providers array (or via providedIn: MyModule), ensuring each lazy-loaded module gets its own instance.

### Pipes

#### Definition

Pipes transform data in templates. Angular provides built‑in pipes like date, currency, async and json. Custom pipes implement the PipeTransform interface and define a transform(value, …args) method.
#### Common Interview Questions

- **What is the difference between pure and impure pipes?** Pure pipes run only when their inputs change; impure pipes run on every change detection cycle. Impure pipes can be expensive and should be used sparingly.
- **How do you create a parameterized custom pipe?** Add parameters after the value in the template (value | myPipe:param1:param2) and declare corresponding arguments in the transform method.
- **When should you use the async pipe?** Use it to render Observables or Promises in templates and let Angular manage subscriptions and cleanup.

### Structural Directives

#### Definition

Structural directives shape the DOM by adding or removing elements. Built‑in directives include `*ngIf` for conditional rendering, `*ngFor` for iteration and `*ngSwitch` for multiple conditions. Custom directives can be created using the @Directive decorator and manipulating the TemplateRef and ViewContainerRef.

#### Common Interview Questions

- **What happens to component state when `*ngIf` toggles false?** The view is destroyed; component instances are removed and recreated on next true.
- **What is the difference between ngIf and [hidden]?** ngIf removes the element from the DOM entirely when the condition is false, whereas [hidden] toggles the CSS display property, keeping the element in the DOM.
- **How do you create a custom structural directive?** Inject TemplateRef (the template to render) and ViewContainerRef (the insertion point), then use createEmbeddedView and clear to control when the template is rendered.

### Attribute Directives

#### Definition

Attribute directives change the appearance or behaviour of elements. Examples include NgStyle, NgClass and custom directives that respond to host events via @HostListener and inject the host element with ElementRef.
#### Common Interview Questions

- **How do attribute directives differ from structural directives?** Attribute directives modify existing elements (e.g., add styles) without changing the DOM structure, while structural directives add or remove elements.
- **How can you respond to DOM events in a directive?** Use the @HostListener decorator to listen to events on the host element and execute logic inside the directive.
- **How do you bind host classes or styles in a directive?** Use @HostBinding or Renderer2 to update host properties in a safe, Angular-friendly way.

### Dependency Injection (DI)

#### Definition

Dependency Injection is a design pattern where classes receive their dependencies from external sources rather than creating them internally. Angular's DI system provides instances of classes (services) where they are needed, managing their lifecycle and ensuring singletons are shared appropriately. The @Injectable() decorator marks a class as available for injection.
#### Common Interview Questions

- **What is the provider hierarchy in Angular?** Providers registered with providedIn: 'root' are application-wide singletons. Providers in NgModules are available to that module and its children. Providers on components create new instances for each component instance and its children.
- **How do you inject tokens that are not classes (e.g., values)?** Use the InjectionToken and useValue provider:
  ```ts
  export const APP_CONFIG = new InjectionToken<any>('APP_CONFIG');
  providers: [
    { provide: APP_CONFIG, useValue: { apiUrl: '...', timeout: 5000 } }
  ];
  constructor(@Inject(APP_CONFIG) private config: any) {}
  // Optional dependency
  constructor(@Optional() private loggingService?: LoggingService) {}
  // Or with inject function
  private loggingService = inject(LoggingService, { optional: true });
  ```
- **What's the difference between useClass, useValue, useFactory, and useExisting?** useClass: Provides an instance of the given class
  useValue: Provides a specific value
  useFactory: Provides a value created by a factory function
  useExisting: Provides an existing token (alias)
- **How does providedIn: 'root' differ from providing in a module?** providedIn: 'root' makes the service an application-wide singleton and enables tree-shaking if the service isn't used. Module providers create instances scoped to the module's injector.
- **What is the inject function and when was it introduced?** The inject function was introduced in Angular 14 as part of the standalone components initiative. It allows dependency injection without constructor parameters, useful in functions, factories, or when order of injection matters.
- **How do you prevent a service from being provided multiple times in lazy-loaded modules?** Use providedIn: 'root' or provide the service only in the root module. Providing services in lazy modules creates separate instances for each module.
- **What is hierarchical injector and how does it work?** Angular has a hierarchical injector system: root injector → module injectors → component injectors. When resolving a dependency, Angular starts at the component level and moves up the hierarchy until it finds a provider.
- **How do you handle optional dependencies?** Use the @Optional() decorator or provide a default value:
  // Using @Optional() decorator
  constructor(@Optional() private optionalService: OptionalService) {
  if (this.optionalService) {
  // Use the service
  }
  }
  // Or with a default value
  constructor(@Optional() @Inject(SOME_TOKEN) private value: string = 'default') {}

### Lifecycle Hooks

#### Definition

Angular calls lifecycle hook methods at key points in a component's existence: from creation to destruction. These hooks allow you to run code at specific moments during a component's lifecycle, such as initialization, change detection, content projection, view rendering, and cleanup.
#### Evolution Timeline

- Angular 2 (2016): Original lifecycle hooks introduced (ngOnInit, ngOnChanges, etc.)
- Angular 16 (2023): New hooks added for signals and better SSR support (afterNextRender, afterRender)
- Angular 17 (2023): Enhanced hooks for improved performance and developer experience
#### Common Interview Questions

- **Which hook is safest for DOM query logic (ViewChild) and why?** ngAfterViewInit - the view and its child components are fully initialized at this point. Earlier hooks like ngOnInit may try to access DOM elements that don't exist yet.
- **Why can ngOnChanges fire before ngOnInit?** Input bindings are set first, so Angular detects changes even before initialization completes. ngOnChanges runs whenever inputs change, including the initial setting of values.
- **How would you perform cleanup for RxJS subscriptions in modern Angular?** In modern Angular (16+), use DestroyRef with takeUntilDestroyed operator:
  // Modern approach (Angular 16+)
  private data$ = this.dataService.getData().pipe(
  takeUntilDestroyed(this.destroyRef)
  );
  // Traditional approach
  private subscription: Subscription;
  ngOnInit() {
  this.subscription = this.dataService.getData().subscribe();
  }
  ngOnDestroy() {
  this.subscription.unsubscribe();
  }
- **What are the new lifecycle hooks introduced in Angular 16+ and when are they useful?** afterNextRender: Runs once after the next change detection cycle. Useful for one-time DOM operations.
  afterRender: Runs after every render cycle. Useful for analytics or DOM measurements.
  Use case: Accessing browser APIs safely without causing ExpressionChangedAfterItHasBeenChecked errors.
- **What is the execution order of the main lifecycle hooks?** constructor() (Component instantiation)
  ngOnChanges() (If inputs exist)
  ngOnInit() (One-time initialization)
  ngDoCheck() (Custom change detection)
  ngAfterContentInit() (Content projection initialized)
  ngAfterContentChecked()
  ngAfterViewInit() (View and children initialized)
  ngAfterViewChecked()
  ngOnDestroy() (Cleanup)
- **When would you use ngAfterContentInit vs ngAfterViewInit?** ngAfterContentInit: When you need to work with projected content (`<ng-content>`)
  ngAfterViewInit: When you need to access the component's own view and children (@ViewChild)
- **How do the new afterRender hooks help with SSR (Server-Side Rendering)?** The new hooksare skipped during server rendering, preventing errors from browser-specific APIs.
  This makes it safer to write code that works in both server and client environments.

### Change Detection

#### Definition

Angular's change detection mechanism determines when and how to update the UI when your application's data changes. The default strategy checks every component on async events when Zone-based change detection is enabled (via provideZoneChangeDetection). The OnPush strategy limits checks to input reference changes, component events, or manual triggers. Signals (Angular 16+) enable fine-grained reactivity for optimal performance.
#### Common Interview Questions

- **What triggers change detection in Angular?** When Zone-based change detection is enabled, DOM events, HTTP responses, timers, Promise resolutions, or Observable emissions trigger change detection cycles via Zone.js.
- **How do you manually trigger change detection?** Inject ChangeDetectorRef and call detectChanges() for immediate check or markForCheck() to schedule check in next cycle.
- **Why can mutating an array fail to update an OnPush component?** OnPush checks reference equality. Mutation keeps the same reference; create new reference: this.items = [...this.items, newItem].
- **How do Signals change Angular change detection?** Signals provide fine-grained reactivity - Angular knows exactly which components depend on which signals, enabling more efficient updates without Zone.js in future versions.
- **What is the performance difference between change detection strategies?** Default checks all components (slower for large apps). OnPush checks only when inputs change (better performance). Signals enable most efficient fine-grained updates.

### Signals (Reactivity Primitives)

#### Definition

A signal is a reactivity primitive that holds a value and notifies interested consumers (called "effects") when that value changes. Signals provide granular tracking of how state is used and updated, enabling frameworks to optimize rendering and ensure data flow is always consistent and efficient. While most associated with Angular, the pattern is also used in other frameworks like Preact, Solid.js, and Vue (via libraries).
#### Evolution Timeline

- Angular 16 (2023): Signals introduced as developer preview
- Angular 17 (2023): Signals became stable, integrated with templates
- Future: Planned to become the default reactivity model, potentially reducing Zone.js dependency
#### Common Interview Questions

- **What problem do Signals solve in Angular?** Signals provide a simpler, more granular, and more performant reactivity model compared to the traditional Zone.js-based change detection. They allow Angular to know exactly which parts of the template depend on which pieces of state, eliminating the need for unnecessary checks and enabling future optimizations like fine-grained re-rendering.
- **Explain the difference between signal(), computed(), and effect().**
  - `signal()`: A writable signal that holds a value. You can update it with .set() or .update().
  - `computed()`: A read-only signal that derives its value from other signals. Its value is cached and recalculated only when its dependencies change.
  - `effect()`: A side effect that runs whenever any signal value it depends on changes. It's used for operations like logging, DOM manipulation outside the template, or syncing with external libraries.
- **How do Signals differ from RxJS Observables in Angular?**
  | Feature | Signals | RxJS Observables |
  | --- | --- | --- |
  | Purpose | Primarily for reactive state in templates | General-purpose async/data streams |
  | Granularity | Fine-grained, know their dependencies | Coarse-grained, push-based |
  | Usage | Synchronous value access via () | Asynchronous subscription via subscribe() |
  | Strength | Simplicity, performance, deep integration with Angular templates | Powerful operators, handling complex async events, websockets |
- **What is "fine-grained reactivity" and how do Signals enable it?** Fine-grained reactivity means the framework can update the specific part of the DOM that depends on a changed value, instead of checking the entire component tree. Signals track their dependencies automatically, so when a signal changes, Angular can precisely know which components or even which HTML expressions need to be updated.
- **Can Signals replace RxJS and Zone.js in Angular?** Not entirely. Signals are intended to work alongside RxJS, not replace it. RxJS is still superior for complex async operations and event streams. Signals may eventually reduce or eliminate the need for Zone.js in many applications, as they provide a more explicit way to trigger change detection.
- **How would you use a Signal in an Angular template?** You use a signal the same way you use a component property, by calling it as a function (due to its getter nature).
  ```html
  <p>Count: {{ count() }}</p>
  <p>Doubled: {{ doubleCount() }}</p>
  <button (click)="count.set(count() + 1)">Increment</button>
  ```
- **Are there signals in other frameworks?** Yes. Preact and Solid.js have signals as their core reactivity model. Vue's ref() and computed() are very similar in concept to signals. This pattern is becoming a modern standard for state management due to its simplicity and performance.

### RxJS (Reactive Extensions for JavaScript)

#### Definition

RxJS is a library for reactive programming using Observables, making it easier to compose asynchronous or callback-based code. It provides powerful operators to transform, combine, manipulate, and work with streams of data. RxJS is fundamental to Angular's HTTP client, router, forms, and many other asynchronous operations.
#### Common Interview Questions

- **How do Observables differ from Promises?** Observables are lazy (execute only when subscribed), can emit multiple values over time, are cancellable, and support powerful operators. Promises are eager (execute immediately), emit a single value, and lack built-in cancellation or transformation operators.
  | Feature | Promise | Observable |
  | --- | --- | --- |
  | Eagerness | Eager (executes immediately) | Lazy (executes on subscription) |
  | Values | Single value | Multiple values over time (stream) |
  | Cancellation | No native cancellation | Native via unsubscribe() |
  | Operators | Limited static methods (all, race) | Rich operators (map, filter, switchMap) |
- **What is the purpose of the pipe method in RxJS?** pipe composes operators in sequence, returning a new Observable that applies each operator to the source stream. It enables functional, reusable operator composition without modifying the original Observable.
- **Explain the difference between switchMap, mergeMap, concatMap, and exhaustMap.**
  - switchMap: Cancels previous inner Observable when new value arrives (ideal for search).
  - mergeMap: Runs all inner Observables concurrently (order not guaranteed).
  - concatMap: Runs inner Observables sequentially (maintains order).
  - exhaustMap: Ignores new values until current inner Observable completes (ideal for save operations).
- **How do you prevent memory leaks with Observables?** Use proper subscription management:
  ```ts
  // Manual subscription (unsubscribe needed)
  private subscription = this.data$.subscribe();
  ngOnDestroy() { this.subscription.unsubscribe(); }
  // Using takeUntil pattern
  private destroy$ = new Subject<void>();
  this.data$.pipe(takeUntil(this.destroy$)).subscribe();
  ngOnDestroy() { this.destroy$.next(); this.destroy$.complete(); }
  // Using async pipe (automatic cleanup)
  data$ = this.service.getData();
  template: `{{ data$ | async }}`
  ```
- **What are Cold vs Hot Observables?** Cold Observables: Start execution on each subscription (e.g., http.get())
  Hot Observables: Emit values regardless of subscriptions (e.g., fromEvent, Subjects)
- **When would you use a Subject vs a regular Observable?** Use Subjects when you need to multicast (share execution among multiple subscribers) or imperatively push values. Use regular Observables for unicast, declarative data streams.
- **What are the different types of Subjects?** Subject: No initial value, only emits values after subscription
  BehaviorSubject: Requires initial value, emits current value to new subscribers
  ReplaySubject: Replays specified number of previous values to new subscribers
  AsyncSubject: Emits only the last value when completed

### Angular Component Communication Patterns

#### Definition

Angular provides a comprehensive set of patterns for component communication, leveraging its powerful dependency injection system and TypeScript integration. Understanding these patterns is essential for building maintainable Angular applications with proper separation of concerns and predictable data flow.
#### Common Interview Questions

- **What are the main component communication patterns in Angular and when should you use each?** Use @Input() for parent-to-child data flow, @Output() with EventEmitter for child-to-parent communication, [(ngModel)] for two-way binding in forms, Services with DI for sharing data between unrelated components, @ViewChild for parent accessing child component instances, and RxJS Subjects for reactive global state management.
- **How do you choose between services and input/output for component communication?** Use input/output for direct parent-child relationships where the communication is explicit and hierarchical. Use services when components are not directly related, when you need to share state across multiple components, or when the data needs to be persisted beyond component lifecycle.
- **What is the difference between @ViewChild and @ContentChild?** @ViewChild accesses elements/components that are part of the component's view (defined in its template), while @ContentChild accesses content that is projected into the component via `<ng-content>` (provided by the parent component).
- **How does Angular's change detection affect component communication?** Angular's change detection automatically updates views when data changes. With OnPush strategy, components only update when input references change or async pipes receive new values, making input-based communication more efficient but requiring careful immutability practices.
- **When should you use RxJS Subjects versus simple services for state management?** Use RxJS Subjects when you need reactive programming patterns, multiple subscribers, or complex state transformations. Use simple services for simple data sharing without the need for reactive updates or when the data flow is straightforward.
#### Communication Patterns Reference
1. @Input() (Parent -> Child)
   - Step 1: Child component defines input properties with @Input() decorator.
   - Step 2: Parent binds data using property binding syntax: [inputName]="value".
   - Step 3: Child uses input properties as regular class properties in template and logic.
   - Note: Inputs are detected by Angular's change detection. Use OnPush strategy with immutable updates for performance.
2. @Output() + EventEmitter (Child -> Parent)
   - Step 1: Child component defines output properties with @Output() and EventEmitter.
   - Step 2: Child emits events using eventEmitter.emit(payload).
   - Step 3: Parent listens with event binding: (outputName)="handler($event)".
   - Note: Always initialize EventEmitter and type it properly: `@Output() eventName = new EventEmitter<Type>()`.
3. [(ngModel)] (Two-Way Binding)
   - Step 1: Import FormsModule in your application module.
   - Step 2: Use [(ngModel)] on form elements: `<input [(ngModel)]="property">`.
   - Step 3: Angular automatically syncs the view and model values.
   - Note: For custom components, implement both @Input() and @Output() with update prefix: @Input() value and @Output() valueChange.
4. @ViewChild / @ViewChildren (Parent accessing Child)
   - Step 1: Parent uses @ViewChild decorator to query child component/element.
   - Step 2: Access child after view initialization in ngAfterViewInit lifecycle hook.
   - Step 3: Parent can call child methods or access child properties directly.
   - Note: Use static: true for elements that don't change dynamically; default is static: false.
5. Services + Dependency Injection (Any components)
   - Step 1: Create a service with @Injectable() and provide it at appropriate level (root, module, component).
   - Step 2: Inject service into components via constructor: constructor(private service: DataService).
   - Step 3: Components share data through service properties and methods.
   - Note: Service instances are singleton at their provided level. Use component providers for isolated instances.
6. RxJS Subjects (Global/Cross-component)
   - Step 1: Create BehaviorSubject/Subject in service: `private data$ = new BehaviorSubject<T>(initial)`.
   - Step 2: Expose as observable: public data$ = this.data$.asObservable().
   - Step 3: Components subscribe to observable and update via service methods.
   - Note: Use async pipe in templates for automatic subscription management and OnPush compatibility.
7. Route Parameters (URL-based)
   - Step 1: Define route with parameters: { path: 'user/:id', component: UserComponent }.
   - Step 2: Inject ActivatedRoute and subscribe to params: this.route.params.subscribe().
   - Step 3: Use parameter values to load component data.
   - Note: Use paramMap for better TypeScript support and snapshot for one-time access.
   - Note: Router state APIs increasingly use signals (e.g., lastSuccessfulNavigation), so invoke them when required.

### Forms & Validation

#### Definition

Angular provides two approaches for handling forms: template-driven forms (declarative, using directives like ngModel) and reactive forms (imperative, using FormGroup, FormControl, FormArray). Reactive forms offer more control, better testing, and complex validation scenarios. Both approaches support synchronous and asynchronous validation.
#### Evolution Timeline

- Angular 2 (2016): Both template-driven and reactive forms introduced
- Angular 14 (2022): Typed forms introduced for better type safety
- Angular 15+: Improved standalone component support for forms
#### Common Interview Questions

- **What are the key differences between template-driven and reactive forms?** Template-driven forms are declarative and simpler for basic scenarios. Reactive forms are imperative, provide better type safety, easier testing, and more control for complex scenarios like dynamic forms or cross-field validation.
- **How do you trigger validation manually in reactive forms?** Call form.markAllAsTouched() to mark all controls as touched, or control.updateValueAndValidity() to re-run validation on a specific control.
- **Why prefer async validators for uniqueness checks?** Async validators return Observable/Promise, allowing Angular to manage pending state and automatically handle HTTP debouncing and cancellation. They're ideal for server-side validation like username/email uniqueness.
- **What are typed forms (Angular 14+) and why use them?** Typed forms provide TypeScript type safety for form values. Instead of `FormControl<any>`, use `FormControl<string>` to get compile-time type checking and better IDE support.
- **How do you handle dynamic form arrays?** Use FormArray to manage dynamic lists of controls. Add/remove controls dynamically with formArray.push() and formArray.removeAt(), and iterate in template with `*ngFor`.
- **What's the difference between form.value and form.getRawValue()?** form.value excludes disabled controls, while form.getRawValue() includes all controls regardless of disabled state, useful when you need complete form data.
- **How do you implement cross-field validation?** Add validation at the form group level rather than individual controls:
  this.form = new FormGroup({
  password: new FormControl(''),
  confirmPassword: new FormControl('')
  }, { validators: this.passwordMatchValidator });
  passwordMatchValidator(form: FormGroup): ValidationErrors | null {
- **return form.get('password')?** .value === form.get('confirmPassword')?.value
  null : { mismatch: true };
  }
- **How do forms work with standalone components?** Import ReactiveFormsModule or FormsModule directly into standalone components. Form building and validation work identically, but with better tree-shaking and no NgModule dependencies.

### HTTP Interceptors

#### Definition

HTTP Interceptors are classes that implement the HttpInterceptor interface. They intercept outgoing HTTP requests and incoming responses, allowing you to modify headers, handle errors, add authentication tokens, log requests, or implement retry logic. Interceptors form a chain where each interceptor can process requests and responses in sequence.
#### Common Interview Questions

- **What are typical uses of HTTP interceptors?** Adding authentication tokens, logging requests/responses, handling errors globally, showing loading indicators, caching responses, adding retry logic, or modifying request/response data.
- **How do you register an interceptor?** Provide it in the root module or applicationConfig providers array:
- **How do you handle retry logic in an interceptor?** Use RxJS operators like retry or retryWhen:
  ```ts
  return next.handle(req).pipe(
    retryWhen(errors => errors.pipe(
      delay(1000),
      take(3)
    ))
  );
  ```
- **What does the multi: true option do?** It allows multiple interceptors to be registered for the same token (HTTP_INTERCEPTORS), forming an interceptor chain instead of replacing previous interceptors.
- **How do you ensure interceptors execute in a specific order?** Interceptors execute in the order they're provided. The first interceptor provided is the first to process the request and last to process the response.
- **What's the difference between functional and class-based interceptors?** Functional interceptors (Angular 15+) are simpler functions that work better with standalone components. Class-based interceptors implement the HttpInterceptor interface and work with dependency injection.
- **How can you skip an interceptor for specific requests?** Add a custom header to the request and check for it in the interceptor:
  ```ts
  const request = httpRequest.clone({
    headers: httpRequest.headers.set('Skip-Interceptor', 'true')
  });
  // In your interceptor
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    if (req.headers.has('Skip-Interceptor')) {
      return next.handle(req);
    }
    // ... normal interceptor logic
  }
  ```

### Modules & Lazy Loading

#### Definition

NgModules are containers that group related Angular artifacts (components, directives, pipes, services) and define their compilation context. The @NgModule decorator specifies what belongs to the module and how it interacts with other modules. Lazy Loading is an optimization technique where features are loaded on-demand when the user navigates to their associated routes.
#### Evolution Timeline

- Angular 2 (2016): Introduced NgModules as the primary organizing structure
- Angular 8 (2019): Ivy renderer introduced, paving the way for standalone components
- Angular 14 (2022): Standalone components introduced, beginning the shift away from NgModules
- Angular 16 (2023): Standalone APIs for bootstrapping and routing became stable
- Angular 17 (2023): Standalone became default, NgModules now considered "legacy" for new projects
#### Common Interview Questions

- **What is the difference between NgModule-based and standalone approaches?** NgModules require declaring components in modules and importing dependencies at module level. Standalone components manage their own dependencies and don't require NgModule wrappers.
- **How does lazy loading work with standalone components?** Use loadComponent for single components or loadChildren with import() to lazy load standalone routes. The Angular compiler creates separate chunks automatically.
- **What are the benefits of standalone components over NgModules?** Reduced boilerplate and simpler project structure
  Better tree-shaking and smaller bundle sizes
  Easier component reuse and testing
  Gradual migration path from NgModules
- **Can you still use NgModules with standalone components?** Yes, through importProvidersFrom() function which allows importing providers from existing NgModules into standalone applications.
- **How do you provide services in a standalone application?** Services can be provided in the bootstrapApplication providers array, using provideIn: 'root', or with the @Injectable({ providedIn: 'root' }) decorator.
- **What happened to AOT compilation with standalone components?** AOT compilation is still used and actually works better with standalone components since each component is more self-contained, enabling better optimization and tree-shaking.
- **When would you choose NgModules over standalone components today?** Mainly when maintaining legacy applications or when using third-party libraries that haven't been updated to support standalone components. For new projects, standalone is recommended.

### Lazy Loading Modules

#### Definition

Lazy loading is an optimization technique where feature modules are loaded on-demand when the user navigates to their associated routes, rather than loading everything at application startup. This reduces the initial bundle size and improves application loading performance. Angular supports both traditional NgModule-based lazy loading and modern standalone component lazy loading.
#### Common Interview Questions

- **What is the difference between RouterModule.forRoot() and RouterModule.forChild()?** forRoot() is used in the root module to configure the router and provide router services. forChild() is used in feature modules to define additional routes without providing a new router instance.
- **How can you verify that lazy loading is working correctly?** Open browser DevTools → Network tab and look for separate JavaScript chunks downloaded when navigating to lazy-loaded routes (e.g., customers-customers-module.js or admin-admin-component.js).
- **What is the role of the import() function in Angular lazy loading?** The import() function is a modern JavaScript feature for dynamic imports. The Angular compiler sees this syntax and automatically creates a separate chunk for the referenced module or component.
- **Can you preload lazy-loaded modules?** Yes. Use PreloadAllModules to load all lazy modules in the background after the main application stabilizes, or create a custom preloading strategy to load specific modules based on user behavior.
- **What's the difference between lazy loading modules vs. standalone components?** Module-based loading requires NgModule wrappers and uses loadChildren. Standalone component loading is more direct, uses loadComponent or loadChildren with routes, and has better tree-shaking.
- **How do you handle authentication guards with lazy loading?** Apply guards to the lazy-loaded route:
  {
  path: 'dashboard',
  loadChildren: () => import('./dashboard/dashboard.module').then(m => m.DashboardModule),
  canActivate: [AuthGuard]
  }
- **What are common lazy loading pitfalls?** Circular dependencies between modules, over-splitting (too many small chunks), forgetting to use forChild() in feature modules, or not handling loading states in the UI.
- **How does lazy loading affect SEO?** Search engines may not execute JavaScript to load lazy content. Use SSR (Angular Universal) or ensure critical content is in the initial bundle for SEO-sensitive pages.
### New Control Flow (@if, @for, @switch) - Angular 17+

#### Definition

The new control flow is a declarative template syntax introduced in Angular 17 that replaces the structural directives `*ngIf`, `*ngFor`, and `*ngSwitch` with more ergonomic, performant, and intuitive built-in blocks. This new syntax is compile-time transformed for better performance and provides better type checking and developer experience.
#### Common Interview Questions

- **What are the performance benefits of the new control flow?** The new syntax is compile-time transformed into efficient JavaScript, eliminating the need for directive instances and reducing runtime overhead. It also enables better tree-shaking and smaller bundle sizes.
- **How does the track clause differ from trackBy?** The track clause is required and more intuitive. Instead of a function reference, you directly specify the property to track: @for (item of items; track item.id). This provides better type safety and performance.
- **What is the @empty block and when is it useful?** The @empty block displays content when the collection is empty, eliminating the need for separate `*ngIf` checks. It's executed only when the array is empty, making it more declarative.
- **Why was this change introduced in Angular 17?** To improve developer experience with more intuitive syntax, better performance through compile-time optimizations, enhanced type checking, and alignment with modern JavaScript frameworks.
- **Do you need to import anything to use the new control flow?** No, the new control flow is built into the Angular template compiler starting from version 17. It works out of the box without any imports.
- **Can you mix old and new control flow syntax?** Yes, Angular supports gradual migration. You can use both `*ngIf` and @if in the same template, but it's recommended to migrate consistently for better maintainability.
- **How does the new syntax handle template scoping?** Variables defined in control flow blocks are block-scoped, preventing accidental variable leakage and providing better isolation than the previous directive approach.
- **What happened to ng-template with the new control flow?** The new syntax eliminates the need for most ng-template usage. The blocks are self-contained and don't require external template references, making the code more readable.
### Deferrable Views (@defer) - Angular 17+

#### Definition

Deferrable views are a performance optimization feature introduced in Angular 17 that enables lazy loading of component templates and their dependencies. Using the @defer block, you can declaratively specify when certain parts of your template should be loaded, reducing the initial bundle size and improving Core Web Vitals like Largest Contentful Paint (LCP).
#### Common Interview Questions

- **How do deferrable views improve Core Web Vitals?** Deferrable views reduce the initial JavaScript bundle size by loading non-critical components only when needed, improving Largest Contentful Paint (LCP) and Time to Interactive (TTI) metrics.
- **What triggers can be used with @defer?** on viewport: When the placeholder enters the browser viewport
  on interaction: On click, focus, or hover of a trigger element
  on hover: When the user hovers over a trigger area
  on immediate: Immediately after the page loads
  on timer(x): After a specified time delay
  on idle: When the browser is idle
  when condition: Based on a custom condition
- **What's the difference between @placeholder and @loading blocks?** @placeholder shows content before the deferred block starts loading. @loading shows content during the loading process. The placeholder is typically static content, while loading shows progress indicators.
- **How does prefetching work with deferrable views?** Prefetching loads the deferred content in the background without rendering it. Use prefetch on trigger to start loading when a condition is met, then the main on trigger to actually display it.
- **Can you defer content that depends on async data?** Yes, combine @defer with async pipes or use the when condition to defer based on data availability:
  ```html
  @defer (when dataLoaded) {
    <app-data-view [data]="data" />
  }
  ```
- **What error handling options are available?** The @error block displays when loading fails. You can also use error handling in the component's constructor or ngOnInit to manage failures gracefully.
- **How do deferrable views differ from traditional lazy loading?** Traditional lazy loading works at the route/component level, while deferrable views work at the template block level within a single component, providing more granular control.
- **When should you avoid using @defer?** Avoid deferring content that's critical for initial rendering, above-the-fold content, or components needed for immediate user interaction. Also avoid over-deferring, which can cause too many sequential loads.
### SSR & Hydration (Server-Side Rendering) - Angular 16+

#### Definition

Server-Side Rendering (SSR) generates HTML on the server and sends it to the client, improving initial load performance, SEO, and social media previews. Hydration is the process where Angular reuses the server-rendered DOM on the client side, attaching event listeners and making the application interactive. Angular's modern SSR (introduced in Angular 16+) features non-destructive hydration that prevents content flickering.

As of Angular 21, server bootstrap accepts a `BootstrapContext`, and `getPlatform()`/`destroyPlatform()` are no-ops on the server.
#### Common Interview Questions

- **What are the benefits of SSR over client-side rendering?** SSR improves SEO (search engines can crawl content), performance (faster First Contentful Paint), social media sharing (proper meta tags), and accessibility (content available without JavaScript).
- **What is hydration and why is it important?** Hydration is the process where Angular "revives" server-rendered HTML on the client by attaching event listeners and making it interactive. Modern Angular uses non-destructive hydration that preserves server-rendered content without flickering.
- **What common issues occur with SSR and how do you solve them?** Window/Document references: Use isPlatformBrowser() check or PLATFORM_ID injection
  Third-party browser libraries: Use lazy loading or conditional imports
  Authentication tokens: Use cookies instead of localStorage for server compatibility
  API calls: Ensure URLs are absolute and work in server environment
- **How do you handle authentication in SSR applications?** Use HTTP-only cookies for authentication tokens since they're automatically sent with both server and client requests, unlike localStorage which is browser-only.
- **What is the difference between prerender: true and SSR?** Prerendering generates static HTML at build time for known routes. SSR generates HTML on-demand for each request. Prerendering is faster but only works for static content.
- **How does Angular Universal differ from the new Angular SSR?** Angular Universal was the original SSR solution. The new Angular SSR (v16+) is integrated into the framework, has better hydration, and uses the same application configuration as client-side rendering.
- **What are hydration errors and how do you avoid them?** Hydration errors occur when server-rendered HTML doesn't match client-rendered HTML. Avoid by:
  Not manipulating DOM directly in components
  Using isPlatformBrowser() for browser-specific code
  Ensuring async operations resolve consistently on server and client
- **How do you optimize SSR performance?** Implement caching strategies for rendered pages
  Use TransferState to avoid duplicate API calls
  Optimize bundle size with lazy loading
  Use CDN for static assets

## ES6 Deep Dive
  While the ES6/ES6+ section earlier listed major features, this deep dive explores specific language constructs.

### Default Parameters & Rest/Spread

#### Definition

Functions can specify default parameter values (function foo(x = 10) {…}); rest parameters (…args) capture remaining arguments into an array; spread syntax (…iterable) expands arrays or objects into individual elements or properties.
#### Common Interview Questions

- **What happens when a default parameter depends on a previous parameter?** You can define a parameter as a function of earlier parameters: function greet(name, greeting = ‘Hello’ + name) {…}; the default value is evaluated at call time.
- **How does the rest operator differ from the spread operator?** The rest operator collects multiple arguments into a single array parameter, while spread expands an array or object into individual elements when passed to functions or array/object literals.
- **Do default parameters apply when the argument is null or undefined?** Only undefined triggers the default; null is treated as an explicit value.

### Modules

#### Definition

ES modules (ESM) use import and export. Named exports (export const foo = …) can be imported selectively; default exports (export default) export a single value. Dynamic imports (import(‘module.js’)) load modules asynchronously.
#### Common Interview Questions

- **What is the difference between named and default exports?** Named exports must be imported with the exact exported name in braces; default exports can be imported with any name and do not require braces.
- **How do ES modules differ from CommonJS?** ES modules are statically analyzable and support tree shaking; imports are hoisted. CommonJS uses require/module.exports, is dynamically evaluated and cannot be tree‑shaken as easily.
- **What are dynamic imports used for?** They load modules on demand (e.g., lazy loading routes) and return a Promise that resolves to the module’s exports.

### Classes & Inheritance

#### Definition

ES6 classes provide syntactic sugar over prototypes. Use class declarations with constructors, instance methods and static methods. Inheritance is achieved with extends and super().
#### Common Interview Questions

- **How do you create private properties in classes?** Use the # syntax (#privateField) or closures; private fields are only accessible within the class body.
- **How can you respond to DOM events in a directive?** Fields define properties on each instance; methods are placed on the prototype and shared across instances.
- **How does method overriding work in subclasses?** A subclass can define a method with the same name as its parent; calling super.method() invokes the parent implementation.

### Loops & Iteration

#### Definition

ES6 introduced for…of for iterating over iterable objects (arrays, strings, maps, sets); for…in iterates over enumerable properties. Generators (function*) can create custom iterators and produce sequences via yield.
#### Common Interview Questions

- **When would you prefer for…of over forEach?** for…of supports break, continue and return, works with any iterable (not just arrays) and can be used in async functions; forEach cannot be broken out of and is synchronous.
- **What is the difference between for…of and for…in?** for…of iterates over values of an iterable; for…in iterates over enumerable property keys (including inherited ones) and is better suited for objects.
  Generators
- **What are generators and the yield keyword?** Generators are functions that can be exited and later re‑entered while preserving their context. Declared with function*, they use yield to pause execution and return intermediate values. Example:
  function* idGenerator() {
  let id = 1;
  while (true) {
  yield id++;
  }
  }
  const gen = idGenerator();
  console.log(gen.next().value); // 1
  console.log(gen.next().value); // 2

### Variables & Scoping

#### Definition

let and const declare block‑scoped variables; const prevents reassignment. var is function‑scoped and hoisted. The temporal dead zone occurs when accessing a let/const before declaration.
#### Common Interview Questions

- **Why is it generally better to use const or let instead of var?** They avoid accidental global variables, respect block scope and help catch errors during compilation.
- **Can a const array be mutated?** Yes - const prevents reassignment of the binding, but the contents of objects and arrays can still be modified.
- **What is the temporal dead zone (TDZ)?** It is the period before a let/const declaration is initialized; accessing the binding then throws a ReferenceError.

### Arrow Functions

#### Definition

Arrow functions provide concise syntax and lexical this binding. They cannot be used as constructors, have no arguments object and do not have their own this.
#### Common Interview Questions

- **How does this behave in arrow functions versus regular functions?** Arrow functions capture this from the enclosing scope; regular functions have their own this depending on how they are called.
- **Why can’t you use an arrow function as a constructor?** Arrow functions lack an internal [[Construct]] method; attempting to use new with them throws a TypeError.
- **Do arrow functions have their own arguments object?** No. They capture arguments from the outer scope; use rest parameters when needed.

### Maps, Sets & Weak Collections

#### Definition

Map stores key–value pairs with any data type as keys; Set stores unique values. WeakMap and WeakSet hold weak references to keys/values (which must be objects) and do not prevent garbage collection.
#### Common Interview Questions

- **When would you use a Map over an object?** When you need non‑string keys, maintain insertion order or frequently add/remove entries; objects are best for static structured data.
- **Why might a WeakMap be useful?** A WeakMap allows objects to be garbage‑collected when there are no other references, useful for caching associated data without risking memory leaks.
- **Why can't WeakMap keys be primitives?** WeakMap keys must be objects so they can be garbage collected when there are no other references.

### Promises & Async/Await

#### Definition

Promises represent asynchronous operations that can be pending, fulfilled or rejected. then and catch register callbacks; static methods like Promise.all, Promise.race, Promise.allSettled and Promise.any manage multiple promises. async/await syntax sugar wraps promises in synchronous‑looking code.
#### Common Interview Questions

- **Explain Promise.all, Promise.allSettled, Promise.race, and Promise.any.**
  - Promise.all: fulfills when all promises fulfill; rejects if any promise rejects.
  - Promise.allSettled: waits for all promises to settle (fulfill or reject) and returns an array of their results.
  - Promise.race: settles as soon as any promise in the iterable settles (fulfills or rejects).
  - Promise.any: fulfills as soon as any promise fulfills; rejects only if all promises reject.
- **Why use async/await instead of plain promises?** async/await reads like synchronous code, enabling easier error handling via try/catch and avoiding nested callbacks.
- **What are the different ways to handle Promises and when should you use each?**
  - Async/Await - recommended for sequential logic, linear readable code with try/catch. Best for complex sequential operations and data processing pipelines.
  - `.then().catch()` - recommended for parallel operations and promise chains. Best for multiple independent async operations and Promise.all scenarios.
  - Combined approach - use for mixed scenarios needing both patterns, like parallel fetching with sequential processing.
- **What is the purpose of the finally block?** The finally block executes regardless of success or failure  -  ideal for cleanup operations like hiding loading spinners, resetting states, or logging completion.

### Built-in Methods & Utilities

#### Definition

ES6+ introduced many built‑in methods such as Object.assign, Object.values, Object.entries, Array.from, Array.includes, String.startsWith, String.padStart and Number.isNaN.
#### Common Interview Questions

- **What does Object.assign do?** It shallowly copies enumerable own properties from source objects into a target object and returns the target.
- **How is Array.from different from Array.of?** Array.from converts array‑like or iterable objects into arrays (optionally mapping each element), while Array.of creates a new array from a list of arguments.
- **What is the difference between Object.keys, Object.values, and Object.entries?** keys returns property names, values returns property values, and entries returns [key, value] pairs for own enumerable properties.

### Template Literals & Destructuring

#### Definition

Template literals (${placeholders}) support string interpolation and multi‑line strings. Destructuring assignments extract values from arrays or properties from objects into variables; default values and rest elements are supported.
#### Common Interview Questions

- **What are tagged template literals?** A tagged template literal is a function that preprocesses template literals; it receives the string parts and substitution values as arguments and returns a new string. Example:
  ```js
  function highlight(strings, ...values) {
    return strings.reduce((result, str, i) =>
      `${result}${str}<span class="hl">${values[i] || ''}</span>`, '');
  }
  const name = 'John';
  highlight`Hello, ${name}!`;
  // → "Hello, <span class=\"hl\">John</span>!"
  ```
- **How do you provide default values in destructuring?** Specify = value in the destructuring pattern (e.g., const { title = ‘N/A’ } = obj) to fall back when the property is undefined.
- **How can you rename variables during destructuring?** Use property: newName syntax (e.g., const { id: userId } = user).
## Bundlers & Compilers

This section covers modern tools that transform and bundle JavaScript applications.

### Webpack

#### Definition

Webpack is a module bundler that analyzes your dependency graph and outputs static assets. A configuration file defines the entry point(s), output, loaders (transformers for non‑JS files like CSS or images) and plugins (to perform tasks like HTML generation, environment variables, and optimisation). Features include code splitting (dynamic import()), tree shaking (removing unused exports) and a dev server with hot module replacement.

#### Common Interview Questions

- **What is the difference between a loader and a plugin in Webpack?** Loaders transform individual modules (e.g., transpiling JSX or TypeScript), while plugins tap into the bundling lifecycle to perform broader tasks like bundle optimization, asset injection or environment variable definition.
- **How does code splitting improve performance?** By splitting your code into smaller bundles and loading them on demand, you reduce initial bundle size and improve load times; Webpack implements this via dynamic import() and the SplitChunksPlugin.
- **What is tree shaking?** Tree shaking removes unused exports from your code during the build process. It requires ES module syntax and helps produce smaller bundles.

### Rollup

#### Definition

Rollup is a module bundler optimised for library development. It uses ES modules internally, produces ES and CommonJS bundles and results in smaller builds due to better static analysis. Plugins enable loading non‑JS assets and transpiling TypeScript.
#### Common Interview Questions

- **How does Rollup differ from Webpack?** Rollup focuses on bundling libraries and yields smaller, tree‑shaken outputs; Webpack targets full applications with features like a dev server and code splitting. Rollup requires fewer configuration options.
- **What are Rollup plugins and how are they used?** Plugins extend Rollup’s capabilities (e.g., @rollup/plugin-node-resolve resolves node modules; @rollup/plugin-commonjs converts CommonJS modules to ES modules; rollup-plugin-terser minifies output). They are configured in rollup.config.js.
- **How do you mark dependencies as external in Rollup?** Use the external option in rollup.config.js so the dependency is not bundled.

### Snowpack & Vite

#### Definition

Snowpack and Vite are modern build tools that serve ES modules in development and bundle for production. Snowpack uses the browser’s native ESM support to deliver unbundled modules during development; Vite builds on Esbuild and Rollup, providing fast hot module replacement and pre‑bundling dependencies.
#### Common Interview Questions

- **Why are Vite and Snowpack faster in development than Webpack?** They avoid bundling during development by serving source files as ES modules and leverage native browser support; Vite prebundles dependencies with Esbuild, leading to faster startup and updates.
- **Can you still perform code splitting and production optimisation with Vite?** Yes - Vite uses Rollup under the hood for production builds, supporting code splitting, minification and other optimizations.
- **What is hot module replacement (HMR) and how does Vite handle it?** HMR swaps updated modules via the dev server and WebSocket without a full page reload.

### TypeScript

#### Definition

TypeScript is a typed superset of JavaScript that compiles to plain JS. It adds optional static typing, interfaces, enums, generics and access modifiers. The compiler checks types at build time, enabling earlier error detection and improved tooling.
#### Common Interview Questions

- **What is the difference between interface and type?** Both define shape aliases, but interface can be extended and merged, whereas type can alias primitives, unions and tuples. Use interface for objects and classes; use type for complex unions or mapped types.
- **What are generics and when would you use them?** Generics allow functions and classes to operate on variable types while maintaining type safety (e.g., function identity(value: T): T). They are used for reusable data structures and APIs.
- **How does the unknown type differ from any?** unknown is a type‑safe counterpart of any; values of type unknown cannot be used unless narrowed, while any disables type checking.

### Babel

#### Definition

Babel is a JavaScript compiler that transforms newer ECMAScript syntax into backwards‑compatible code. Presets (e.g., @babel/preset-env, @babel/preset-react) define sets of plugins to support target environments. Babel can also compile JSX and TypeScript when combined with appropriate plugins.
#### Common Interview Questions

- **How does Babel differ from TypeScript?** Babel transpiles syntax but does not perform type checking; TypeScript compiles code and enforces types. You can use Babel with TypeScript to compile TS without type checks or combine both for full type safety and modern syntax.
- **What is a Babel plugin vs. preset?** A plugin transforms specific syntax (e.g., class fields); a preset is a collection of plugins bundled for convenience (e.g., @babel/preset-env targets ES features based on browser support).
- **How does @babel/preset-env decide what to transform?** It uses target environments (browserslist) and can add polyfills with core-js via useBuiltIns.

### Node Package Managers & Publishing

#### Definition

npm, Yarn and pnpm manage packages and dependencies. package.json lists dependencies, scripts and metadata; semver versioning uses major.minor.patch. Publishing a package involves adding a name, version and main field and running npm publish.

#### Common Interview Questions

- **What is the difference between dependencies, devDependencies and peerDependencies?** dependencies are required at runtime; devDependencies are needed only during development and testing; peerDependencies specify packages that your library expects consumers to install at the same version (e.g., React).
- **How does pnpm differ from npm or Yarn?** pnpm uses a content‑addressable storage to store dependencies and creates hard links, significantly reducing disk space and speeding up installs compared to npm and Yarn.
- **What is semantic versioning (semver) and how do caret (^) and tilde (~) ranges work?** Semver defines major.minor.patch versions; ^1.2.0 allows any minor/patch updates up to (but not including) 2.0.0, while ~1.2.0 allows patch updates up to (but not including) 1.3.0.
## Threading & Data Streams

This section covers concurrency and streaming APIs available in the browser.

### Service Workers

#### Definition

A service worker is a script running in the background separate from web pages. It intercepts network requests, enabling offline caching, background sync and push notifications. It operates as a programmable network proxy and persists across page reloads.
#### Common Interview Questions

- **What are the lifecycle phases of a service worker?** Installation, activation and fetch. During install, you pre‑cache assets; activation cleans up old caches; fetch intercepts requests to serve cached responses or fall back to the network.
- **How does a service worker enable offline experiences?** By caching assets and API responses in the Cache storage; when offline, the service worker serves responses from the cache instead of the network.
- **What controls a service worker's scope?** The registration path and optional scope setting; it can only control pages under that scope.

### Web Workers

#### Definition

Web Workers run JavaScript in a background thread, allowing you to offload CPU‑intensive tasks without blocking the main UI thread. Communication with the main thread happens via postMessage and onmessage events.
#### Common Interview Questions

- **When would you use a web worker?** For heavy computations, data processing or parsing large files, ensuring the UI remains responsive.
- **How do you communicate between a web worker and the main thread?** Use worker.postMessage(data) to send data and listen for message events via worker.onmessage.
- **What limitations do web workers have compared to the main thread?** They cannot access the DOM directly and communicate via message passing with a limited API surface.

### Server-Sent Events (SSE)

#### Definition

Server-Sent Events provide a one-way push mechanism where a server streams text data over HTTP. In the browser, an EventSource object listens for events sent by the server.
#### Common Interview Questions

- **How do SSE differ from WebSockets?** SSE is unidirectional (server to client) and built on top of HTTP; WebSockets are bidirectional and work over a persistent TCP connection.
- **What are typical use‑cases for SSE?** Live news feeds, notifications and other scenarios where the server needs to push updates to many clients with minimal overhead.
- **How do SSE clients handle reconnects?** EventSource auto-reconnects; servers can set retry and use Last-Event-ID to resume streams.

## Networking & Processing

### Same-Origin Policy (SOP) & CORS

#### Common Interview Questions

- **Explain the Same-Origin Policy (SOP) and CORS.** SOP prevents a script from one origin from interacting with resources from another origin (protocol + domain + port). CORS is a mechanism that relaxes SOP via HTTP headers like Access-Control-Allow-Origin, letting servers whitelist other origins.
- **What is a CORS preflight request and when does it occur?** The browser sends an OPTIONS request before certain cross-origin requests (non-simple methods, custom headers, or JSON content type) to check if the server allows it.
- **What makes a request "simple" under CORS rules?** Simple requests use GET/HEAD/POST with only simple headers and Content-Type of text/plain, application/x-www-form-urlencoded, or multipart/form-data; these skip preflight.
- **What does the Access-Control-Allow-Credentials header do?** It allows cookies or auth headers to be sent on cross-origin requests, and it requires a specific origin (not "*") in Access-Control-Allow-Origin.
- **How do CORS errors show up in the browser?** The request may succeed on the network, but the browser blocks access to the response and logs a CORS error in the console.

### REST vs GraphQL vs WebSockets

#### Definition

REST is an architectural style that uses HTTP verbs to operate on resources identified by URLs. GraphQL is a query language that allows clients to request only the data they need; it uses a single endpoint and strongly typed schema. WebSockets provide full‑duplex communication over a single persistent connection.
#### Common Interview Questions

- **When might you choose GraphQL over REST?** When clients require flexible queries, need to avoid over/under‑fetching data and benefit from a strongly typed schema. GraphQL also helps reduce the number of network requests.
- **What are the advantages of WebSockets over HTTP polling?** WebSockets maintain a persistent connection and support real‑time, bidirectional communication, reducing latency and overhead compared to repeated HTTP requests.
- **How do you secure REST and GraphQL APIs?** Use HTTPS, authentication (e.g., OAuth 2.0, JWT), authorization checks, input validation and rate limiting.

### WebSockets & Real-Time Communication

#### Definition

The WebSocket protocol provides full-duplex, bidirectional communication over a single, long-lived TCP connection. Unlike HTTP's request-response model, a WebSocket connection allows the server to push data to the client at any time without the client first making a request. This is ideal for real-time applications like chat, live feeds, collaborative tools, and gaming. Libraries like SignalR (ASP.NET) and Socket.IO (Node.js) build on this protocol, offering additional features like automatic reconnection, fallback transports, and higher-level abstractions like hubs and rooms.
#### Example (Native WebSocket API)

```js
const socket = new WebSocket('wss://echo.websocket.org');
socket.onopen = (event) => {
  socket.send('Hello Server!');
};
socket.onmessage = (event) => {
  console.log('Message from server:', event.data);
};
socket.onerror = (error) => {
  console.error('WebSocket error:', error);
};
socket.onclose = (event) => {
  console.log('Connection closed:', event.code, event.reason);
};
```
#### Common Interview Questions

- **How do WebSockets differ from HTTP polling, long polling, or Server-Sent Events (SSE)?** HTTP Polling: Client repeatedly requests new data at regular intervals. Inefficient due to constant empty responses and header overhead.
  HTTP Long Polling: Client requests data; server holds the request open until new data is available or a timeout occurs. The client then immediately reconnects. Better than polling but still has significant overhead.
  SSE: Server can push data to the client over a single, long-lived HTTP connection, but it's unidirectional (server-to-client only). Built on HTTP, easier to implement but less flexible than WebSockets.
  WebSockets: True bidirectional communication over a dedicated protocol (ws:// or wss://). Once the handshake is complete, overhead is minimal (tiny frames), making it the most efficient option for high-frequency, low-latency communication in both directions.
- **Describe the WebSocket connection handshake.** The handshake is an HTTP upgrade request. The client sends a standard HTTP request with Connection: Upgrade and Upgrade: websocket headers. The server responds with HTTP/1.1 101 Switching Protocols if it supports WebSockets. This initial handshake allows WebSockets to work over standard HTTP ports (80, 443), easing deployment with firewalls and proxies.
- **What are some common challenges when working directly with the native WebSocket API, and how do libraries like SignalR or Socket.IO address them?** Challenge: Manual reconnection logic if the connection drops.
  Library Solution: Automatic reconnect with configurable strategies and backoff.
  Challenge: Lack of built-in heartbeat/ping-pong to detect dead connections.
  Library Solution: Automatic keep-alive/ping messages.
  Challenge: Fallback for environments that block WebSockets.
  Library Solution: Fallback transports (e.g., long polling) to ensure connectivity.
  Challenge: Structuring messages (e.g., routing a message to a specific handler).
  Library Solution: Abstraction like Hubs (SignalR) or Namespaces/Rooms (Socket.IO) for grouping connections and invoking RPC-like methods (hub.invoke('SendMessage', text)).
- **You mentioned SignalR. What is its primary advantage?** SignalR is an abstraction layer that automatically chooses the best transport (WebSockets, Server-Sent Events, long polling) based on client and server capabilities. It provides a simple RPC-like model using Hubs, allowing the server to call methods on clients and vice versa. This dramatically simplifies the code needed to build real-time applications compared to managing raw WebSocket connections and message parsing manually.
- **How would you implement basic reconnection logic for a native WebSocket connection?** You would use a pattern involving setTimeout and increasing delay (exponential backoff). On the onclose event, you would attempt to reconnect after a short delay. If that fails, you wait longer before trying again, up to a maximum limit.
  ```js
  let reconnectInterval = 1000; // Start with 1 second
  function connect() {
    const ws = new WebSocket('wss://my-server.com');
    // ... setup event handlers ...
    ws.onclose = function (e) {
      console.log('Socket is closed. Reconnect will be attempted in ' + reconnectInterval + 'ms.', e.reason);
      setTimeout(function () {
        reconnectInterval *= 2; // Double the delay each time
        connect();
      }, reconnectInterval);
    };
    ws.onopen = function () {
      reconnectInterval = 1000; // Reset the delay on a successful connection
    };
  }
  connect();
  ```
- **What security considerations are important for WebSocket connections?** Use wss:// (WebSocket Secure), the equivalent of HTTPS, to encrypt data in transit.
  Validate and sanitize all data received over the WebSocket connection on both the client and server, just as you would with HTTP requests, to prevent injection attacks.
  Authenticate the connection during the initial HTTP handshake using cookies, tokens, or query parameters. Do not try to implement a custom authentication protocol after the WebSocket is open.
  Be mindful of origin restrictions. The WebSocket protocol itself does not enforce Same-Origin Policy, but the server should validate the Origin header of the initial handshake to prevent cross-site WebSocket hijacking (CSWSH).

### HTTP Interceptors & OAuth 2.0

#### Definition

HTTP interceptors (client side) allow you to modify requests/responses, add authentication tokens or handle errors. OAuth 2.0 is an authorization framework for delegated access using flows like authorization code, client credentials and implicit flow.
#### Common Interview Questions

- **What is the difference between authentication and authorization?** Authentication verifies user identity; authorization determines what the authenticated user can access.
- **Which OAuth 2.0 flow would you use in a single‑page application?** The authorization code flow with PKCE (Proof Key for Code Exchange) is recommended for SPAs; it uses a code verifier and challenge to secure the exchange.
- **How can you implement request retry logic in an interceptor?** In Angular or Axios interceptors, catch errors and reissue the request a limited number of times with a delay or exponential backoff.

### MSAL (Microsoft Authentication Library)

#### Definition

MSAL (Microsoft Authentication Library) is a library that implements OAuth 2.0 and OpenID Connect protocols, specifically optimized for the Microsoft identity platform (Azure AD, Microsoft accounts, Azure AD B2C). It provides a simplified API for acquiring, caching, and refreshing tokens, handling complex security flows like PKCE (Proof Key for Code Exchange) automatically. MSAL supports multiple application types (SPA, web apps, mobile, desktop) and offers built-in token management, silent authentication, and enterprise features like conditional access and multi-tenant support.
#### Example

```
import { PublicClientApplication } from "@azure/msal-browser";

const msalConfig = {
    auth: {
        clientId: "your-azure-ad-app-id",
        authority: "https://login.microsoftonline.com/your-tenant-id",
        redirectUri: "http://localhost:3000",
    },
    cache: {
        // ✅ RECOMMENDED: sessionStorage for balance of UX and security
        cacheLocation: "sessionStorage",
        // ❌ AVOID: localStorage due to XSS persistence risk
        // cacheLocation: "localStorage",
        // ⚠️ MEMORY ONLY: Most secure but poor UX (loses auth on refresh)
        // cacheLocation: "memoryStorage",
    }
};

const msalInstance = new PublicClientApplication(msalConfig);

// ✅ Backend proxy pattern for maximum security
const getTokenViaBackend = async () => {
    // Frontend talks to own backend, which handles MSAL
    const response = await fetch('/api/auth/token', {
        credentials: 'include' // HttpOnly cookies automatically sent
    });

    if (response.ok) {
        return response.json();
    }
    // Backend handles refresh logic, returns new token
};

// ✅ Alternative: MSAL with sessionStorage (practical approach)
const getTokenSecurely = async () => {
    const accounts = msalInstance.getAllAccounts();

    if (accounts.length > 0) {
        try {
            return await msalInstance.acquireTokenSilent({
                scopes: ["User.Read"],
                account: accounts[0]
            });
        } catch (error) {
            // Token expired or invalid - require re-authentication
            return await msalInstance.loginPopup({
                scopes: ["User.Read"]
            });
        }
    }
    return await msalInstance.loginPopup({ scopes: ["User.Read"] });
};
```

#### Common Interview Questions

- **What's the security trade-off between different MSAL cache locations?** localStorage: Persists across sessions but vulnerable to XSS attacks
  sessionStorage: Cleared when tab closes, better security but worse UX
  memoryStorage: Most secure (cleared on page reload) but poor user experience
  HttpOnly cookies: Most secure option but requires backend involvement
- **Why isn't HttpOnly cookie storage the default for MSAL in SPAs?** MSAL is designed primarily for client-side applications where the backend may not be involved in authentication. HttpOnly cookies require server-side token handling, which adds complexity but provides superior XSS protection.
- **What's the recommended approach for production applications?** For maximum security, use a backend-for-frontend (BFF) pattern where your frontend talks to your own backend API, which handles MSAL authentication and returns HttpOnly cookies. For simpler apps, sessionStorage provides a reasonable balance.
- **How does MSAL's cache location affect single sign-on (SSO) experience?** localStorage enables seamless SSO across tabs and browser sessions. sessionStorage maintains SSO within a single tab session. memoryStorage requires fresh login on every page reload.
- **What enterprise security features does MSAL provide regardless of storage choice?** MSAL enforces PKCE, validates token signatures, supports conditional access policies, and provides automatic token refresh with secure replay protection.

## Libraries & State Management

### Redux & Flux

#### Definition

Flux is an architectural pattern enforcing a unidirectional data flow with actions, dispatcher, stores and views. Redux is a predictable state container based on Flux; it stores the entire app state in a single object and updates it via pure reducer functions in response to dispatched actions.
#### Common Interview Questions

- **What are the three principles of Redux?** The state of your whole application is stored in a single immutable object tree;
  State is read‑only and the only way to change it is by dispatching actions;
  Changes are made with pure reducer functions.
- **How does Redux differ from Flux?** Redux has a single store instead of multiple stores, eliminates the dispatcher and encourages use of middleware for async logic.
- **What is the purpose of middleware in Redux?** Middleware extends dispatch functionality (e.g., handling asynchronous actions with redux-thunk or side effects with redux-saga).

### MobX, Vuex & Other Stores

#### Definition

MobX is a reactive state management library that uses observables and automatic dependency tracking; Vuex is a state container for Vue.js with a single store, mutations and actions. Other libraries like Zustand and Recoil provide simplified or experimental state management for React.
#### Common Interview Questions

- **How do MobX and Redux differ?** MobX uses mutable state and automatically tracks dependencies; changes are propagated reactively. Redux enforces immutable state and manual updates via reducers. MobX is easier to set up; Redux scales better for large apps.
- **Why would you use a state management library at all?** To manage shared state across components, support undo/redo, time‑travel debugging, or maintain predictable data flow.
- **What is a selector in a state management library?** A function that derives or selects a piece of state; selectors can be memoized to avoid unnecessary recomputations.

### Lodash & Utility Libraries

#### Definition

Lodash is a modular utility library offering functions for array and object manipulation, deep cloning, debouncing/throttling, memoization and more. It can be imported per function to reduce bundle size.
#### Common Interview Questions

- **When would you choose Lodash over native JavaScript functions?** For convenience functions not available natively (e.g., _.cloneDeep) and cross‑browser consistency; however, modern JavaScript provides many alternatives (e.g., Array.map, Object.keys).
- **How can you reduce Lodash’s footprint in your bundle?** Import only specific modules (import debounce from ‘lodash/debounce’) or use Lodash ES modules with tree shaking.
- **What is the difference between .assign and .merge?** .assign performs a shallow copy of source properties; .merge recursively merges nested objects.
- **What are debouncing and throttling?** Write a simple debounce function.
  Debouncing: Group multiple sequential calls into one; wait until a period of inactivity before invoking the function (e.g., search suggestions).
  Throttling: Ensure a function is called at most once in a specified period (e.g., infinite scroll). Debounce implementation:
  function debounce(func, delay) {
  let timeoutId;
  return (…args) => {
  clearTimeout(timeoutId);
  timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
  }

### GraphQL & Apollo

#### Definition

GraphQL is a query language and runtime that lets clients specify exactly what data they need. Apollo Client manages GraphQL queries on the client, caching responses and providing hooks like useQuery and useMutation.
#### Common Interview Questions

- **How does Apollo Client cache work?** Apollo normalizes query responses by IDs and fields, allowing efficient updates and refetches. Cache policies determine how results are stored and merged.
- **What is the difference between queries and mutations in GraphQL?** Queries read data; mutations modify data and can have side effects. Both accept variables and return a defined data shape.
- **How do subscriptions work in GraphQL?** Subscriptions use WebSockets to push real‑time updates from the server to clients; Apollo Client exposes useSubscription to manage them.

## Tools & Testing

### Linters & Formatters

#### Definition

Linters such as ESLint analyze code for potential errors and style violations. Formatters like Prettier automatically format code according to a specified style. Git hooks (e.g., Husky, lint‑staged) run these tools before commits to maintain code quality.
#### Common Interview Questions

- **Why use both ESLint and Prettier?** ESLint focuses on finding problematic patterns and enforcing best practices; Prettier enforces consistent formatting. The two can be combined with eslint-plugin-prettier.
- **How do Git hooks improve code quality?** Pre‑commit hooks can run linters, formatters and tests, preventing bad code from entering the repository.
- **What is lint‑staged?** A tool that runs linters on only the staged files, reducing execution time and ensuring only changed files are checked.

### Testing Frameworks

#### Definition

Jest, Mocha and Jasmine are testing frameworks; React Testing Library and Enzyme facilitate component testing; Cypress and Playwright enable end‑to‑end testing in the browser. Tests are categorized as unit, integration and end‑to‑end.
#### Common Interview Questions

- **What is the difference between unit and integration tests?** Unit tests focus on individual functions or components in isolation; integration tests verify interactions between modules; end‑to‑end tests validate the entire application flow.
- **How do you mock HTTP requests in tests?** Use libraries like jest.mock, msw (Mock Service Worker) or Axios mock adapter to simulate network requests without hitting real endpoints.
- **Why prefer React Testing Library over Enzyme?** React Testing Library encourages testing components through user interactions and the DOM rather than implementation details, leading to more maintainable tests.

### Storage APIs

#### Definition

Web Storage (LocalStorage and SessionStorage) stores key–value pairs in the browser; LocalStorage persists across sessions until cleared, SessionStorage lasts for the page session. IndexedDB is a transactional, NoSQL database for storing larger amounts of structured data on the client.
#### Common Interview Questions

- **What are the security concerns with localStorage vs. cookies for storing tokens?** localStorage is vulnerable to XSS because malicious scripts can read the token. Cookies can be secured with flags: HttpOnly makes the cookie inaccessible to JavaScript; Secure ensures it is only sent over HTTPS; SameSite restricts when cookies are sent, preventing CSRF.
- **When should you use IndexedDB over LocalStorage?** When you need to store large or complex data (objects, files) or perform queries; IndexedDB supports indexes.
- **How do localStorage and sessionStorage differ?** LocalStorage persists until cleared; SessionStorage is cleared when the tab or browser is closed.
- **Compare localStorage, sessionStorage, and cookies.**
  | Feature | localStorage | sessionStorage | Cookies |
  | --- | --- | --- | --- |
  | Capacity | ~5-10 MB | ~5-10 MB | ~4 KB |
  | Lifetime | Persistent until manually cleared | Lasts only for the session (tab is open) | Set with max-age/expires |
  | Accessibility | Client-side only | Client-side only | Sent with every HTTP request (client & server-side) |
  | Use case | Storing user preferences | Storing temporary form data | Authentication tokens, user tracking |

### Graphics & Media

#### Definition

Canvas and SVG are two distinct web technologies for creating graphics in the browser. Canvas is a raster-based drawing API that provides a pixel-level interface for programmatic rendering, ideal for dynamic, performance-intensive graphics. SVG (Scalable Vector Graphics) is an XML-based vector format that describes shapes as mathematical objects, making it resolution-independent and DOM-accessible. The choice between them depends on factors like interactivity, scalability, performance requirements, and accessibility needs.
#### Common Interview Questions

- **Explain Canvas and SVG and when to use each.** Canvas: A raster API that lets you draw pixels programmatically. Best for complex graphics, games and pixel-based operations; it is resolution-dependent. SVG: A vector API using XML to describe shapes. Best for icons, logos, charts and maps that need to scale without quality loss; resolution-independent and accessible via the DOM.
- **What are the performance implications of Canvas vs SVG?** Canvas performs better for many moving objects or frequent redraws (games, animations) because it operates at the pixel level. SVG can become slow with thousands of elements since each is a DOM node, but excels for static or moderately complex vector graphics.
- **How does accessibility differ between Canvas and SVG?** SVG is inherently accessible - elements can have titles, descriptions, and respond to CSS. Canvas requires additional ARIA attributes and custom keyboard handling since it's essentially a bitmap image to screen readers.
- **Can you combine Canvas and SVG in the same application?** Yes, they can be used together - SVG for UI elements and static graphics, Canvas for dynamic visualizations or game elements. They can even be layered using CSS positioning.
- **How do you handle user interaction differently in Canvas vs SVG?** SVG elements can have direct event listeners (click, mouseover). Canvas requires manual hit detection by tracking mouse coordinates against drawn objects and managing event delegation programmatically.
- **What are the memory considerations for each technology?** Canvas memory usage depends on canvas size and pixel density. SVG memory scales with the number of elements in the DOM. Large canvases can consume significant GPU memory, while complex SVGs can impact DOM performance.
## Architecture & Patterns

### SOLID Principles

#### Definition

SOLID is an acronym for five key principles in object-oriented design aimed at creating software that's easier to maintain, extend, and understand. These principles help developers write code that's flexible, scalable, and less prone to bugs. They were introduced by Robert C. Martin (Uncle Bob) and are widely applied in JavaScript frameworks like React and Angular, especially in large-scale applications.
#### Principle Overview

| Principle | In Simple Terms | Frontend Benefit |
| --- | --- | --- |
| Single Responsibility (SRP) | One job per component | Easier testing & maintenance |
| Open/Closed (OCP) | Extend without modifying | Safe feature additions |
| Liskov Substitution (LSP) | Substitutes work as expected | Predictable components |
| Interface Segregation (ISP) | Don't force unused features | Cleaner dependencies |
| Dependency Inversion (DIP) | Depend on abstractions | Flexible & testable code |

#### Detailed Breakdown

Single Responsibility Principle (SRP): Every class, module, or function should have only one job. For example, a React component should either display data OR handle business logic, but not both. This isolation makes changes safer and testing simpler.
Open/Closed Principle (OCP): Code should be open for extension but closed for modification. Like adding new rooms to a house without tearing down walls - you can add PayPal support to a payment system without rewriting the core credit card logic.
Liskov Substitution Principle (LSP): Subclasses should seamlessly replace their parent classes. A "Square" should work anywhere a "Shape" is expected without breaking functionality - ensuring inheritance doesn't create surprises.
Interface Segregation Principle (ISP): Don't force components to depend on interfaces they don't use. Instead of one giant "Printer" interface handling print, scan, and fax, create separate focused interfaces so simple components only implement what they need.
Dependency Inversion Principle (DIP): High-level modules (business logic) should not depend on low-level details (API calls). Both should depend on abstractions. This allows swapping REST APIs for GraphQL without rewriting your entire application.
#### Why SOLID Matters in Frontend

Following SOLID principles creates code that's more testable, reusable, and adaptable to changes - crucial in fast-paced frontend development. They're particularly valuable in large React/Angular applications where maintainability determines long-term success.
#### Common Interview Questions

- **What's the core idea behind Single Responsibility?** Each component should have one clear purpose, making changes isolated and testing straightforward.
- **How does Open/Closed make teams more efficient?** Developers can add new features without touching existing, working code - reducing bugs and merge conflicts.
- **Why does Liskov Substitution prevent inheritance headaches?** It ensures subclasses behave predictably, preventing surprises when substituting components in different contexts.
- **How does Interface Segregation improve component design?** It prevents "interface pollution" where components are forced to implement methods they don't need.
- **What's the practical value of Dependency Inversion?** It makes code more flexible and testable by reducing tight coupling between components and their dependencies.

### Microservices, Microfrontends & Monorepos

#### Definition

Microservices architecture decomposes applications into independently deployable services. Microfrontends apply the same idea to the UI by composing features from separately built and deployed front‑end apps. A monorepo stores multiple related projects in a single repository, enabling shared tooling and atomic commits.
#### Common Interview Questions

- **What are the benefits and challenges of microfrontends?** Benefits include independent deployment and autonomy; challenges include consistency, shared state and increased complexity.
- **When might a monorepo be advantageous?** It simplifies dependency management, promotes code sharing and ensures synchronized versioning across packages.
- **How do you handle communication between microservices or microfrontends?** Use APIs (REST/GraphQL), message queues, custom events or shared state management, depending on coupling requirements.

### Clean Architecture & Other Patterns

#### Definition

Clean Architecture emphasizes separation of concerns: domain/business logic is independent of frameworks, UI and infrastructure. Layers are arranged around the domain with strict boundaries. Other patterns include MVC (Model‑View‑Controller), MVVM (Model‑View‑ViewModel) and the Observer pattern.
#### Common Interview Questions

- **What problem does Clean Architecture solve?** It decouples business rules from implementation details, making code more testable and adaptable to changes in frameworks or infrastructure.
- **How does MVVM differ from MVC?** MVVM introduces a ViewModel that exposes observables to the view, improving data binding and separation; MVC uses a controller to manage interactions.
- **Give an example of the Observer pattern in web development?** DOM events are an example: event listeners subscribe to changes and react when events occur.

### DevOps & CI/CD

#### Definition

DevOps integrates development and operations to automate software delivery. Continuous Integration (CI) runs tests and builds on every commit; Continuous Deployment/Delivery (CD) automatically releases or prepares releases. Tools include GitHub Actions, GitLab CI, Jenkins and Azure Pipelines.
#### Common Interview Questions

- **Why use Docker in front‑end development?** Docker packages the application and its dependencies into containers, ensuring consistent environments across development and production.
- **What are the benefits of CI/CD pipelines?** Automated builds and tests catch issues early, shorten feedback loops, ensure reproducible builds and enable fast, reliable deployments.
- **How do npm, Yarn and pnpm fit into CI workflows?** These package managers install dependencies and run scripts (e.g., build, test) within CI; lockfiles ensure deterministic builds across environments.

## Git
#### Common Interview Questions

- **Explain the difference between git merge and git rebase.** git merge integrates changes from one branch into another by creating a new merge commit, preserving the full history of both branches. git rebase moves a branch to begin on the tip of another branch, rewriting history by creating new commits. It results in a cleaner linear history. Golden rule: rebase local branches; merge when integrating into main.
- **What is the .gitignore file?** It specifies intentionally untracked files that Git should ignore - such as node_modules/, .env files with secrets, build directories and OS-specific files like .DS_Store.
- **Describe a typical Git workflow.** A common feature-branch workflow:
  - main contains production-ready code.
  - Create a feature branch: `git checkout -b feature/new-login-page`.
  - Commit changes: make changes, `git add`, and `git commit` on the feature branch.
  - Push and open a PR: `git push origin feature/new-login-page` and open a pull request.
  - Code review and merge: teammates review; after approval, the PR is merged.
- **What is git stash?** `git stash` temporarily shelves (stashes) changes in your working copy so you can work on something else. Later, `git stash pop` applies the stashed changes and removes them from the stash list.
- **How do you revert a commit that has already been pushed?** `git revert` creates a new commit that undoes the changes of the specified commit; it is safe for shared history. `git reset --hard` moves the branch pointer backwards, effectively erasing commits; it rewrites history and should only be used on commits that have not been shared.
- **What is git cherry-pick and what problem does it solve?** `git cherry-pick` applies the changes introduced by a specific commit from one branch onto another. It takes a commit's patch and replays it on your current branch, creating a new commit with a different hash. It solves the problem of needing a specific change or bug fix in a branch (like main or a release branch) without merging the entire feature branch or history.
- **What is the typical workflow for using git cherry-pick?**
  - Identify the hash of the commit you want to apply.
  - Check out the destination branch (e.g., `git checkout main`).
  - Execute the command `git cherry-pick <commit-hash>`.
  - Resolve any merge conflicts if they occur, then `git add` and `git cherry-pick --continue`.
- **What are the main risks or drawbacks of using git cherry-pick?** The primary risk is duplicating commits in the repository history. If you later merge the original feature branch, Git may not recognize that the cherry-picked commit is essentially the same change, leading to confusing history and potential merge conflicts. It can also break the semantic link between related commits if you cherry-pick one commit but not others that depend on it.
- **How does git cherry-pick differ from git merge or git rebase?** git merge integrates all changes from one branch into another, preserving the entire history. git rebase replays a sequence of commits from one branch onto another, creating a linear history. git cherry-pick is more surgical; it selectively applies individual commits, not entire branches or commit sequences.
- **When is it appropriate to use git cherry-pick?** It is appropriate for specific scenarios like:
  - Hotfixes: applying a critical bug fix from a development branch to a stable release or main branch.
  - Saving work: rescuing a specific, well-defined change from a messy or abandoned feature branch.
  - Partial feature integration: moving a single, self-contained feature or commit from one long-running branch to another without merging everything.

## Agile, Kanban & Scrum

#### Definition

Agile is a project management philosophy based on the Agile Manifesto, emphasizing iterative development, customer collaboration, and adaptability to change. Scrum and Kanban are specific frameworks that implement Agile principles:

| Aspect | Scrum | Kanban |
| --- | --- | --- |
| Approach | Iterative, time-boxed sprints (2-4 weeks) | Continuous flow |
| Roles | Product Owner, Scrum Master, Developers | No prescribed roles (can overlay existing teams) |
| Cadence | Regular ceremonies (sprint planning, review, retrospective) | Continuous delivery |
| Change Policy | Changes discouraged during sprint | Changes can be made at any time |
| Metrics | Velocity, Sprint Burndown, Burnup Chart | Cycle Time, Throughput, Cumulative Flow Diagram |

#### Example

```
// Scrum: Sprint artifacts and roles (Scrum Guide 2020+)
const scrumFramework = {
  roles: ["Product Owner", "Scrum Master", "Developers"], // Official Scrum roles
  externalParticipants: ["Stakeholders"], // Engage with Scrum Team but not formal roles
  artifacts: ["Product Backlog", "Sprint Backlog", "Increment"],
  events: [ // Official Scrum events
    "Sprint Planning",
    "Daily Scrum",
    "Sprint Review",
    "Sprint Retrospective"
  ],
  activities: [
    "Backlog Refinement" // Ongoing activity (not a formal event)
  ]
};

// Kanban: Visual workflow with WIP limits
const kanbanBoard = {
  columns: [
    { name: "Backlog", wipLimit: null },
    { name: "Ready", wipLimit: null },
    { name: "Analysis", wipLimit: 2 },
    { name: "Development", wipLimit: 3 },
    { name: "Testing", wipLimit: 2 },
    { name: "Done", wipLimit: null }
  ],
  metrics: ["Cycle Time", "Throughput", "Cumulative Flow Diagram"],
  advantage: "Can be overlaid onto existing roles and teams without disruption"
 };
```

#### Common Interview Questions

- **What are the key differences between Scrum and Kanban?** Scrum uses fixed-length sprints with three defined roles (Product Owner, Scrum Master, Developers) and formal events; Kanban focuses on continuous flow with visual workflow management, per-column WIP limits, and can overlay existing team structures without role changes.
- **What are the three primary Scrum roles and their responsibilities?**
  - Product Owner: Maximizes product value, manages backlog, defines priorities.
  - Scrum Master: Ensures Scrum adherence, removes impediments, facilitates events.
  - Developers: Cross-functional group that delivers potentially releasable increments each sprint.
- **Is Backlog Refinement a formal Scrum event?** No, according to the Scrum Guide 2020+, Backlog Refinement is an ongoing activity rather than a formal event. It involves continuously grooming and preparing backlog items for future sprints.
- **What is the advantage of Kanban having "no prescribed roles"?** Kanban can be implemented on top of existing team structures and roles without organizational disruption, making it easier to adopt incrementally and suitable for teams with established responsibilities.
- **Name the four formal Scrum events and their purposes.**
  - Sprint Planning: Determine what can be delivered in the upcoming sprint and how.
  - Daily Scrum: 15-minute sync for Developers to plan next 24 hours.
  - Sprint Review: Inspect the increment and adapt the Product Backlog.
  - Sprint Retrospective: Plan improvements for next sprint.
- **What metrics would you track to improve team performance in each framework?**
  - Scrum: Velocity (forecasting), Sprint Burndown (daily progress), Burnup Chart (scope changes).
  - Kanban: Cycle Time (lead time optimization), Throughput (work completion rate), Cumulative Flow Diagram (bottleneck identification).

## SEO & Accessibility

#### Definition

Search Engine Optimization (SEO) is the practice of improving website visibility in organic search results through technical optimization, quality content, and user experience enhancements. Accessibility (a11y) ensures web content is usable by people with disabilities, following guidelines like WCAG (Web Content Accessibility Guidelines) to support assistive technologies, keyboard navigation, screen readers, and various interaction modes.
#### Example

```
<!-- SEO: Semantic HTML and meta tags -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Frontend Interview Cookbook - JavaScript Fundamentals</title>
  <meta name="description" content="Master JavaScript type system, closures, promises, and more for frontend interviews.">
  <meta name="keywords" content="javascript, frontend, interview, closure, promise">
  <link rel="canonical" href="https://cookbook.com/javascript-fundamentals">
  <!-- Structured data (JSON-LD) -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Article",
    "headline": "JavaScript Fundamentals",
    "author": "Igor Živković"
  }
  </script>
</head>
<body>
  <!-- Accessibility: Semantic landmarks and ARIA -->
  <header role="banner">
    <nav aria-label="Main navigation">
      <ul>
        <li><a href="#home" aria-current="page">Home</a></li>
      </ul>
    </nav>
  </header>
  <main role="main">
    <h1>JavaScript Fundamentals</h1>
    <button aria-expanded="false" aria-controls="content">Show Content</button>
    <div id="content" aria-hidden="true">Content here...</div>
  </main>
</body>
</html>
```

#### Common Interview Questions

- **What are the three pillars of technical SEO?** Crawling (can search engines discover your content?), Indexing (can they understand and store it?), and Ranking (how relevant is your content to search queries?).
- **What is the difference between ARIA labels and native HTML semantics?** Native HTML elements (`<button>`, `<nav>`) provide built-in accessibility. ARIA attributes (aria-label, role) should only be used when native semantics are insufficient, as they don't provide actual functionality.
- **How does site performance affect SEO?** Performance is a direct ranking factor (Core Web Vitals: LCP, FID/INP, CLS). Slow sites have higher bounce rates, which indirectly affects rankings through user behavior signals.
- **What are the four WCAG principles (POUR)?** Perceivable (available to senses), Operable (usable via various input methods), Understandable (clear and predictable), and Robust (compatible with current and future tools).
- **How would you make a complex data table accessible?** Use proper `<table>` structure with `<caption>`, `<th>` headers with scope attributes, and associate data cells with headers using headers attribute or aria-describedby.

## Internationalization (i18n) & Theming

#### Definition

Internationalization (i18n) is the design and development of applications to adapt to various languages, regions, and cultures without engineering changes. It involves translation files, locale-specific formatting (dates, numbers, currencies), and pluralization rules. Theming allows visual customization of an application's appearance (colors, typography, spacing) through CSS variables, design tokens, or component library configurations, supporting light/dark modes and brand variations.
#### Example

```
// i18n: Translation files and formatting
// locales/en.json
{
  "welcome": "Welcome, {name}!",
  "items": "{count, plural, =0 {No items} one {# item} other {# items}}",
  "date": "Today is {date, date, long}"
}

// locales/fr.json
{
  "welcome": "Bienvenue, {name} !",
  "items": "{count, plural, =0 {Aucun élément} one {# élément} other {# éléments}}",
  "date": "Aujourd'hui c'est le {date, date, long}"
}

// Theming: CSS Custom Properties and React context
// themes.css
:root {
  --primary-color: #007bff;
  --background: #ffffff;
  --text-color: #333333;
}

[data-theme="dark"] {
  --primary-color: #4dabf7;
  --background: #1a1a1a;
  --text-color: #f8f9fa;
}

// ThemeContext.jsx
import React, { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      <div data-theme={theme}>
        {children}
      </div>
    </ThemeContext.Provider>
  );
};
```

#### Common Interview Questions

- **What's the difference between i18n and l10n (localization)?** i18n is the technical foundation that makes adaptation possible (framework, structure). l10n is the actual process of adapting content for a specific locale (translations, cultural adaptations).
- **How do you handle right-to-left (RTL) language support?** Use CSS logical properties (margin-inline-start instead of margin-left), the dir="rtl" attribute, and test with RTL languages like Arabic or Hebrew. CSS frameworks often provide RTL variants.
- **What are common pitfalls when implementing dark mode?** Forgetting to set the initial theme based on user preference (prefers-color-scheme media query), not testing contrast ratios in both modes, and hardcoding colors instead of using CSS variables.
- **How would you structure translation files for a large application?** Use namespaced JSON files grouped by feature/page, implement lazy loading of locale data, and consider using a professional i18n library like react-i18next, i18next, or FormatJS.
- **What are design tokens and how do they relate to theming?** Design tokens are platform-agnostic variables storing visual attributes (colors, spacing, typography). They serve as the single source of truth that can be transformed for different platforms (CSS, iOS, Android) and themes.

## Performance Optimization & Proxying

#### Definition

Performance optimization encompasses techniques to improve website speed, responsiveness, and user experience. Key strategies include lazy loading (deferring non-critical resources), code splitting (breaking JavaScript into smaller chunks), compression (reducing file sizes), caching (storing frequently used data), minimizing browser reflows/repaints (reducing layout recalculations), and using efficient algorithms.
Proxying involves using a proxy server during development to route API requests, avoiding CORS (Cross-Origin Resource Sharing) issues by making all requests appear to come from the same origin.
#### Critical Rendering Path

The Critical Rendering Path is the sequence of steps browsers follow to convert HTML, CSS, and JavaScript into pixels on screen:
1. DOM Construction: Parse HTML and build the Document Object Model tree
2. CSSOM Construction: Parse CSS and build the CSS Object Model tree
3. JavaScript Execution: Run scripts (can block parsing if synchronous)
4. Render Tree: Combine DOM and CSSOM into a render tree
5. Layout/Reflow: Calculate positions and sizes of elements
6. Paint: Fill in pixels for each element
7. Compositing: Layer and composite the final page
#### Common Interview Questions

- **What's the difference between reflow and repaint?** Reflow (layout) involves calculating element positions and geometry, which is computationally expensive. Repaint involves drawing pixels to screen after reflow. Reflow always triggers repaint, but repaint can occur without reflow (e.g., color changes).
- **How does code splitting improve performance, and what are the different types?** Code splitting reduces initial bundle size by loading code only when needed. Types include: route-based (split by pages), component-based (split heavy components), and dynamic imports (load on interaction).
- **What are effective caching strategies for static assets?** Use Cache-Control headers with long max-age (e.g., 1 year) and unique filenames for immutable assets. For API responses, use ETag or Last-Modified headers for conditional requests.
- **Why does JavaScript block parsing by default, and how can you prevent this?** JavaScript can modify DOM/CSSOM, so browsers pause parsing to execute scripts. Use async (executes after download, doesn't block parsing) or defer (executes after parsing complete) attributes on script tags.
- **What's the difference between async and defer attributes?** async: Downloads in parallel, executes immediately after download (order not guaranteed)
  defer: Downloads in parallel, executes in order after HTML parsing completes
  Neither: Blocks parsing until script downloads and executes
- **How do you identify performance bottlenecks?** Use Chrome DevTools Performance tab to record and analyze runtime performance, Lighthouse for audits, Core Web Vitals (LCP, FID, CLS) for user-centric metrics, and Bundle Analyzer for bundle size insights.
- **What are common techniques to reduce Cumulative Layout Shift (CLS)?** Specify width and height attributes on images/video, reserve space for dynamic content, avoid inserting content above existing content, and use transform animations instead of properties that trigger layout.
- **When would you use a development proxy vs. configuring CORS on the server?** Use a proxy during development for convenience. In production, configure proper CORS headers on the server for security and performance (avoiding unnecessary hops).

## Angular vs React vs Vue Cheat Sheet

The following comparisons summarize key differences between Angular, React, and Vue across common categories:

| Category | Angular | React | Vue |
| --- | --- | --- | --- |
| Architecture & Philosophy | Full-featured, "batteries-included" framework with strong opinions. | Lean UI library focused on flexibility, requiring choices for routing, state, etc. | Progressive framework; can be used for simple enhancement or full SPAs with official libraries. |
| Building Blocks | Components, templates, and directives compose the UI; services provide logic. | Function components and hooks compose the UI; logic is encapsulated in custom hooks. | Single-File Components (SFCs) with templates, script, and style; logic via Options API or Composition API. |
| Template Syntax | HTML templates with directives (`*ngIf`, `*ngFor`, `[(ngModel)]`) and pipes. | JSX (JavaScript + XML) with JavaScript expressions, ternaries, and curly braces. | HTML-based templates with directives (v-if, v-for, v-model); logic via template expressions. |
| Loops & Conditionals | `*ngFor`, `*ngIf`, `*ngSwitch` modify the DOM. | Use array methods (map) and short-circuit or ternary expressions to conditionally render elements. | v-for, v-if, v-else-if, v-else, v-show directives. |
| Styling & Classes | Binding syntax: [ngClass], [ngStyle]. | Use className, conditional expressions, or libraries like classnames. | Direct class/style binding, :class (object/array), :style (object/array) bindings. |
| Local State & Reactivity | Signals, Change Detection (Zone.js or OnPush), RxJS in components. | useState, useReducer; updates trigger re-render. | ref(), reactive() (Composition API); data() (Options API). Automatic dependency tracking. |
| Global State Management | Services + RxJS, NgRx (Redux), NgRx/ComponentStore, Akita. | Context API, Redux Toolkit, Zustand, Jotai, Recoil, MobX. | Pinia (official, modern), Vuex (legacy). Composables for shared stateful logic. |
| DI & Services | Built-in hierarchical Dependency Injection system injects services. | Context API or external libraries for sharing data. | provide()/inject() for component-scoped dependency injection. |
| Routing | @angular/router with declarative routes and guards. | React Router library with `<Routes>`, `<Route>` and components. | Vue Router (official library) with `<RouterLink>`, `<router-view>`. |
| Forms | Template-driven and reactive forms with built-in validators. | Controlled/uncontrolled components; third-party libraries like React Hook Form and Formik. | v-model for two-way binding; built-in modifiers (.lazy, .number, .trim); Vuelidate/VeeValidate for complex validation. |
| Lazy Loading | Built-in with loadChildren in routes (NgModule) or loadComponent (standalone). | Code splitting with React.lazy and Suspense. | Dynamic imports in Vue Router (component: () => import(...)). |
| SSR & Hydration | Angular Universal renders on the server. | Next.js or frameworks like Remix handle server-side rendering. | Nuxt.js (full-stack framework) or Vite/SSR plugin. |
| Testing | Angular CLI with Jasmine/Karma; TestBed for unit tests. | Jest, React Testing Library and Enzyme for component tests. | Jest/Vitest with Vue Test Utils for unit testing; Cypress/Playwright for E2E. |
| CLI & Build | Angular CLI handles scaffolding, building, and testing. | Create React App (CRA), Vite or custom Webpack handle builds. | Vue CLI (legacy), Vite (modern, recommended) with official plugin. |
| i18n | Built-in i18n and locale support. | Libraries like react-intl or next-i18next. | Vue I18n (official library). |
| Learning Curve | Steeper due to broad API surface and TypeScript-first nature. | Moderate; easy to start, mastery requires understanding of ecosystem. | Gentle; easy to learn from HTML/JS, with a gradual progression to advanced concepts. |
| Microfrontends | Angular Elements or Module Federation. | Module Federation, Single-SPA or Webpack 5 for integrating React apps. | Module Federation (via Vite or Webpack). |

## Bonus

See `BONUS.md` for the narrative walkthrough.

## Changelog

See `CHANGELOG.md`.

## Author

LinkedIn: https://www.linkedin.com/in/igor-%C5%BEivkovi%C4%87-124431124/

## License

This project is licensed under the MIT License. See `LICENSE`.
