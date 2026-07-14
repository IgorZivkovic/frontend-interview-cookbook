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

> **Last content verification:** July 12, 2026. Frontend patch releases and support windows change quickly, so confirm exact versions in the official documentation before upgrading a production project.

## Table of Contents

- [Frontend Interview Questions (Q&A)](#frontend-interview-questions-qa)
- [JavaScript Fundamentals](#javascript-fundamentals)
- [Markup (Modern HTML)](#markup-modern-html)
- [Styling](#styling)
- [Node.js Fundamentals](#nodejs-fundamentals)
- [React Deep Dive](#react-deep-dive)
- [Vue Deep Dive](#vue-deep-dive)
- [Angular Deep Dive](#angular-deep-dive)
- [Modern JavaScript Deep Dive](#modern-javascript-deep-dive)
- [TypeScript Deep Dive](#typescript-deep-dive)
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

JavaScript is dynamically typed and permits implicit type coercion; the informal “weakly typed” label is common but not a standardized classification. Variables are not bound to a type - instead, values carry one of the seven primitive types (undefined, null, boolean, number, bigint, string, symbol) or an object type (which includes arrays, dates, etc.). Functions are callable objects. Type checks happen at runtime and you may freely assign different kinds of values to the same variable without a compile-time error.
#### Example

```js
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

```js
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
- **What happens when the `+` operator is used with mixed types?** JavaScript first converts objects to primitive values. If either resulting primitive is a string, it concatenates their string forms. Otherwise it performs numeric addition after Number/BigInt conversion; mixing a BigInt and Number throws rather than silently coercing them. Symbols also require explicit string conversion.
  `"3" + 2` → `"32"`; `2 + 3` → `5`; `2n + 3n` → `5n`; `2n + 3` → `TypeError`.

### Prototypical inheritance

#### Definition

JavaScript uses prototype-based inheritance: every object has an internal `[[Prototype]]` link, inspected with `Object.getPrototypeOf(obj)`, to another object or `null`. When a property is not found on the object itself, lookup follows that chain. Functions are callable objects, but only constructable functions (such as ordinary function declarations/expressions and classes) have `[[Construct]]` and can be used with `new`; arrow and method functions are not constructors.
#### Example

```js
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

```js
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
- **How can you protect an object argument from unintended mutation inside a function?** Choose the copy depth deliberately. `{...obj}` protects only top-level properties because nested objects remain shared. `structuredClone(obj)` creates a deep clone for supported structured-clone values and handles cycles. A library may be appropriate for custom instances or domain-specific rules; the JSON stringify/parse trick is lossy, rejects cycles and BigInt, and is not a general deep-clone API.

### This keyword

#### Definition

The this keyword in JavaScript refers to the execution context of a function and is determined by how the function is called, not where it's defined. JavaScript has four main binding rules that determine what this references, plus special behavior for arrow functions. Understanding these rules is crucial for predicting function behavior in different calling contexts.
#### Example

```js
// 1. Default binding (standalone function call)
function standAlone() {
    console.log(this); // globalThis in sloppy mode; undefined in strict mode
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

// 5. Arrow functions capture `this` from the enclosing scope
const arrowObj = {
    name: "Carol",
    greet() {
        const sayHello = () => {
            console.log(`Hello, ${this.name}`);
        };
        sayHello();
    }
};
arrowObj.greet(); // "Hello, Carol"
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

```js
const user = {};
Object.defineProperty(user, 'id', {
  value: 123,
  writable: false,
  enumerable: false,
  configurable: false
});
console.log(user.id);           // 123
user.id = 456;                  // ignored in sloppy mode; TypeError in strict mode
console.log(Object.keys(user)); // []
Object.freeze(user);            // shallowly freezes this object's own properties
```

#### Common Interview Questions

- **What does the configurable attribute of a property descriptor control?** If configurable: false, the property cannot be deleted and its descriptor (other than writable from true to false) cannot be changed.
- **Compare Object.seal and Object.freeze.** Object.seal(obj) stops adding or deleting properties but leaves existing properties’ writability intact. Object.freeze(obj) also makes existing data properties non-writable. Both operations are shallow: nested objects remain mutable unless they are frozen separately.
- **How would you create a property that is read-only and hidden from enumeration?** Use Object.defineProperty with { writable: false, enumerable: false }. Example: Object.defineProperty(obj, 'secret', { value: 42, writable: false, enumerable: false, configurable: true });

### Property getters/setters

#### Definition

JavaScript supports accessor properties - functions that run when a property is read (getter) or assigned (setter). They are declared with the get and set keywords (shorthand) or via Object.defineProperty. Accessor properties store no value themselves; they compute or validate on access while appearing like “regular” fields to callers.
#### Example

```js
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

```js
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

```js
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

```js
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

```js
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

```js
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

- **What is the difference between a synchronous and an asynchronous callback?** A synchronous callback (for example, `Array.prototype.map`) runs before the current call returns. An asynchronous API such as a timer or file read arranges for its callback to run later. Event listeners are invoked synchronously when an event is dispatched, although the browser or runtime may dispatch that event during a later event-loop task.
- **Why can heavy use of nested callbacks lead to “callback hell,” and how can it be mitigated?** Deeply nested callbacks are hard to read, test, and handle errors in (“pyramid of doom”). Solutions include breaking functions into smaller named callbacks, using Promises/async-await, or employing utilities like Promise.all for parallel work.
- **What is the “error-first” (Node-style) callback convention and why is it useful?** A callback receives the signature (error, result). On success error is null; on failure it holds an Error object. This unifies success and failure paths, enables early returns on errors, and standardizes APIs across the Node.js ecosystem.

### Chainable methods

#### Definition

Method chaining is a design pattern in which each method returns an object that exposes more methods, allowing multiple operations to be performed in a single, fluent statement (e.g., obj.doA().doB().doC()). Most chains return this (the same instance) or a new wrapper object so the chain can continue.
#### Example

```js
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

```js
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

```js
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

```js
const memoizeOne = (fn) => {
  const cache = new Map();
  return (arg) => {
    if (cache.has(arg)) return cache.get(arg);
    const result = fn(arg);
    cache.set(arg, result);
    return result;
  };
};

let fib;
fib = memoizeOne((n) => (
  n < 2 ? n : fib(n - 1) + fib(n - 2)
));
console.log(fib(40));
```

#### Common Interview Questions

- **When does memoization not help?** If the function is impure (relies on mutable state or side-effects) or rarely called with the same inputs, caching adds overhead without benefit.
- **How does memoization differ from general caching?** Memoization is usually function-scoped and automatic - tied to specific argument sets - whereas caching is broader (API responses, files) and may be managed manually with eviction, TTLs, etc.
- **What pitfalls arise when memoizing functions that accept objects or arrays?** Map keys compare objects and arrays by reference, not by deep content, so two identical-looking literals produce separate entries. Use stable references or a carefully designed key resolver when structural equality is required. A naive `JSON.stringify(args)` key can collide, depends on serialization details, and fails on circular structures and unsupported values.

### Regular expressions (RegExp)

#### Definition

A regular expression (RegExp) is a concise pattern-language for matching, searching, and replacing text. In JavaScript you create them with literals (/pattern/flags) or the RegExp constructor, and use them with string methods (match, replace, search, split) or RegExp methods (test, exec). Common flags include i (ignore case), g (global – find all matches), m (multiline anchors), and u (Unicode).
#### Example

```js
const yearRE = /\b(\d{4})\b/g;
const str = "2019 was wet, 2025 is sunny";
console.log(str.match(yearRE)); // ["2019", "2025"]
```

#### Common Interview Questions

- **Explain the difference between test and exec.** Both belong to a RegExp instance. test(str) returns a boolean (was there a match?). exec(str) returns the match object (array of matched text plus captured groups) or null. When the regex has the g or y flag, successive calls to exec continue from the previous match thanks to the regex’s internal lastIndex pointer.
- **Why might /foo/gi.test("FOO") return false on the second call?** With the g flag, test also advances lastIndex. After the first successful match lastIndex sits at the end of the string; the next test starts there and fails. Reset lastIndex = 0 or omit g when you need a simple boolean check.
- **How do lookaheads and lookbehinds help build zero-width assertions?** Give an example. Lookaheads `(?=...)` and lookbehinds `(?<=...)` assert surrounding text without consuming it. `/\d+(?=%)/` matches `15` before the percent sign in `"up 15%"`. To match an integer token that does not begin after a dot, `/(?<![\d.])\d+/` checks before the first digit and also prevents the engine from taking a partial match later in the same digit sequence; it matches `5` but not `3` in `"version 5.3"`.
### Modern JavaScript (ECMAScript 2015–2026)

#### Definition

ES6 is the historical name for ECMAScript 2015. JavaScript has shipped in annual ECMAScript editions since then; as of July 2026, the current published snapshot is ECMAScript 2026 (ECMA-262, 17th edition). Important additions across those editions include:

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
- Recent standardized APIs: iterator helpers and new Set methods (ES2025); `Array.fromAsync`, `Iterator.concat`, `Math.sumPrecise`, `Error.isError`, Uint8Array Base64/hex conversion, JSON parse source access, and Map/WeakMap get-or-insert methods (ES2026).
#### Example

```js
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

- **Why are `let` and `const` preferable to `var`, and when would you choose `let` over `const`?** `let` and `const` are block-scoped and remain in the temporal dead zone until initialization; their declarations are still hoisted, but they avoid `var`'s function scope and pre-initialization `undefined` behavior. `const` prevents rebinding, not mutation of an object value. Use `let` when the binding genuinely needs reassignment.
- **How does spread differ in array and object literals?** Array spread iterates its operand and inserts those values into the surrounding array; it does not recursively flatten nested arrays. Object spread copies own enumerable properties into a new object, shallowly. Typical uses include `const combined = [...a, ...b]` and `const options = {...defaults, ...overrides}`.
- **What problem did async/await solve over plain Promises, and how does it work under the hood?** async/await lets asynchronous code look synchronous, improving readability and error handling (try/catch). Under the hood the async function returns a Promise; each await pauses execution until the awaited value resolves, then resumes by chaining .then behind the scenes.

### Event loop

#### Definition

The browser event loop coordinates JavaScript tasks, microtasks and rendering opportunities. It selects a runnable task from one of several task queues/sources (timers, user interaction, networking and others), runs it to completion, and then drains the microtask queue (`Promise` reactions, `queueMicrotask`, `MutationObserver`). The browser may then update rendering when appropriate; a repaint is not guaranteed after every task. Node.js has a different phase-based event loop.
#### Example

```js
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
- **What is the difference between `window.load` and `DOMContentLoaded`?** `DOMContentLoaded` fires after parsing and after deferred/module scripts have executed. It does not directly wait for images or every stylesheet, although a stylesheet can delay a deferred script and therefore indirectly delay the event. `load` waits for the page's dependent resources such as images and stylesheets.

### Iterators

#### Definition

An iterator is an object that implements the iterator protocol: it has a next() method that returns an object with { value, done }. If done is false the value is the next item; when done becomes true, the iteration ends. Objects that expose an iterator via a Symbol.iterator method are iterables and work with language constructs like for…of, spread (...), array destructuring, and Array.from().
#### Example

```js
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
- **Why does `for...of` skip properties added to `Array.prototype` but iterate array elements?** It uses the array iterator, which reads element values in index order. Arbitrary enumerable properties, including inherited ones, are not part of that iterator sequence.
- **What advantages do generator functions offer when creating custom iterators?** Generators (function*) automatically keep state and produce iterators for you. yield replaces manual bookkeeping of value/done, making complex sequences (e.g., infinite streams, async flows with for await…of) easier to implement and read.

### Immutable vs mutable data

#### Definition

Immutable data cannot be changed after it’s created; any “update” produces a new value. Mutable data can be altered in-place. In JavaScript, all primitive values (undefined, null, boolean, number, bigint, string, symbol) are inherently immutable, while objects (including arrays, functions, dates, maps, sets) are mutable unless you deliberately freeze or clone them.
#### Example

```js
// Immutable primitive
let a = 'Hi';
a[0] = 'h';  // ignored in sloppy mode; TypeError in strict mode
a = 'Hello'; // creates a new string
// Mutable object
const user = { name: 'Ann' };
user.name = 'Ana';
// Forcing immutability
const frozen = Object.freeze({ year: 2025 });
frozen.year = 2030; // ignored in sloppy mode; TypeError in strict mode
```

#### Common Interview Questions

- **Which built-in JavaScript types are immutable and why does it matter?** All primitives are immutable: you can’t change a number, boolean, or the characters inside a string - only reassign the variable. This guarantees the value won’t be altered elsewhere, making primitives naturally safe to share across logic.
- **How can you create an immutable (read-only) object or array?** `Object.freeze(obj)` prevents changes to that object's own properties, but it is shallow. Deep object graphs require recursively freezing supported nested values or, more commonly, applying immutable updates with structural sharing through explicit copies or a library such as Immer. `Object.seal()` alone does not make existing data properties read-only.
- **Why is immutability beneficial in UI frameworks such as React or state libraries like Redux/MobX?** Immutable updates make change detection simple (shallow reference checks), enable time-travel debugging, and prevent accidental side-effects.
  Note: Redux enforces immutability as a core principle. MobX, on the other hand, often uses observable mutable state with transparent tracking. While immutability is not required in MobX, it can still be applied if a project prefers immutable patterns.

### Pure functions

#### Definition

A pure function is a function that, for the same input arguments, always returns the same output and has no observable side-effects (doesn’t modify external state, perform I/O, mutate its parameters, log, etc.). Its behavior is entirely determined by its inputs, which makes it predictable, testable, and cache-friendly (memoizable).
#### Example

```js
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

```js
console.log(foo); // undefined

try {
  console.log(bar);
} catch (error) {
  console.log(error.name); // "ReferenceError" (TDZ)
}

speak(); // "Hi!"
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

```js
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

A deep copy recursively duplicates the supported mutable data graph so cloned nodes do not share those mutable references with the source. Exact semantics depend on the cloning API: prototypes, functions, host objects, private state, transferables, and other special values may be rejected, transformed, transferred, or require domain-specific handling.
#### Example

```js
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
- **When is a shallow copy sufficient, and when must you choose a deep copy?** A shallow copy is sufficient when only top-level values change or when unchanged nested objects can be shared. Immutable Redux updates normally copy each object or array along the changed path and preserve references to unchanged branches (structural sharing); they do not deep-clone the whole state. Use a deep copy only when you genuinely need an independent clone of every supported nested value, such as before handing mutable configuration to untrusted code.

### DOM manipulation

#### Definition

DOM manipulation is the act of reading or changing the browser’s Document Object Model - the live tree of nodes representing HTML and XML documents - via JavaScript APIs. Core operations include selecting nodes (querySelector), creating/inserting/removing nodes (createElement, append, remove), modifying attributes and classes (setAttribute, classList), changing inline styles (style), and reacting to events (addEventListener).
#### Example

```html
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
- **Why is DocumentFragment useful for performance?** A DocumentFragment is a lightweight container for assembling a group of nodes before inserting them into the document. A single insertion can reduce repeated DOM mutation and layout work, although it does not guarantee exactly one render or zero reflows; browsers may batch direct DOM updates too, so measure performance-sensitive code.
- **What are data attributes?** Data attributes (`data-*`) store custom data on standard HTML elements. They are available in JavaScript through `element.dataset` (for example, `data-user-id` becomes `element.dataset.userId`).
- **What is the difference between the nodeValue and textContent properties?** nodeValue: For text nodes, it returns the text content. For element nodes, it returns null.
  textContent: Returns the concatenated text of all descendants, including `<script>` and `<style>` content. It is more performant than innerText as it doesn't require layout calculations.

## Markup (Modern HTML)

### Definition

Modern HTML is defined by the WHATWG HTML Living Standard; “HTML5” is still common shorthand, but the old numbered W3C snapshots are no longer the current standard. HTML includes semantic elements (`<header>`, `<article>`, `<nav>`), media and canvas, forms and validation, responsive images, interactive controls such as `<dialog>` and `<details>`, and integration points for Web Components. WebGL is a separate Khronos API exposed through `<canvas>`, not part of the HTML specification itself.
#### Example

```html
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
<button id="open-modal" type="button">Open dialog</button>
<dialog id="modal">
  <p>Hello, world!</p>
  <form method="dialog">
    <button>Close</button>
  </form>
</dialog>
<script>
  const modal = document.getElementById('modal');
  document.getElementById('open-modal').addEventListener('click', () => {
    modal.showModal();
  });
</script>
```

#### Common Interview Questions

- **What is the purpose of the `<!DOCTYPE html>` declaration?** It is the required short doctype that makes browsers render the document in no-quirks (standards) mode. It is a legacy-compatible preamble, not a declaration of an HTML version.
- **What are the benefits of using semantic HTML5 elements instead of generic tags?** They improve accessibility, SEO and maintainability by giving meaning to page regions (e.g., `<header>`, `<nav>`, `<main>`, `<footer>`). Screen readers and search engines can better understand the document structure.
- **How do you implement responsive images in HTML5?** For art direction (different crops or image compositions at different breakpoints): Use the `<picture>` element with multiple `<source>` elements and media queries.
  For resolution switching (serving the same image at different resolutions/sizes): Use the srcset and sizes attributes directly on an `<img>` element, which is often simpler and sufficient for many use cases.
  The browser chooses the appropriate image based on device resolution, viewport size, and supported formats. Fallback to an `<img>` tag ensures support in non-compliant browsers.
- **What is a Web Component and why are `<template>` and `<slot>` used?** Web Components combine APIs such as custom elements and Shadow DOM. A custom element does not encapsulate markup or styles by itself; attaching a shadow root creates that boundary. `<template>` holds inert markup that can be cloned, while `<slot>` defines insertion points inside a shadow tree for the host element's light-DOM children.
- **Describe the difference between the `defer` and `async` attributes in the `<script>` tag.** For external classic scripts, both download in parallel with HTML parsing. `async` executes as soon as its download finishes, may pause parsing, and does not preserve order. `defer` executes after parsing, before `DOMContentLoaded`, and preserves document order. Module scripts defer by default; `defer` has no effect on them, while `async` makes a module execute as soon as its dependency graph is ready.

### EJS

#### Definition

EJS (Embedded JavaScript) is a server-side templating engine for Node.js. EJS uses `<%= value %>` for HTML-escaped output, `<%- value %>` for unescaped output, `<% code %>` for control flow, and `<%- include('partial') %>` to render a partial. It normally renders templates on the server and sends the resulting HTML to the client.
#### Example

```ejs
<!-- profile.ejs -->
<h2><%= user.name %></h2>
<p>Age: <%= user.age %></p>
<p>Bio: <%- user.bio %></p>
<%- include('footer') %>
```

```js
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

- **How does EJS differ from JSX or Handlebars?** EJS is string-based templating: templates compile to functions that return HTML strings. JSX is syntax that compiles to element/component calls and can be rendered on the client, server, or at build time. Handlebars intentionally limits inline logic and delegates it to helpers, whereas EJS can run arbitrary JavaScript inside `<% %>`.
- **Explain the security implications of `<%- %>` vs. `<%= %>` in EJS.** `<%= %>` applies EJS's HTML/XML escaping and is the right default for data placed in ordinary HTML text. It is not a universal context-aware encoder: do not rely on it alone when inserting untrusted values into JavaScript, CSS, event-handler, or URL contexts. `<%- %>` outputs raw, unescaped HTML, so use it only for trusted content or HTML sanitized with a suitable allowlist sanitizer.
- **What performance optimizations are available when using EJS in production?**
  - Template caching: app.set('view cache', true) or ejs.renderFile(path, data, {cache:true}) stores the compiled function in memory.
  - Compilation: cache functions created with `ejs.compile()` or compile templates during application startup/build with a project-specific build step. The official EJS CLI renders templates; `ejs-cli` is a separate third-party package.
  - Minify HTML after render to reduce payload size, and avoid heavy logic in templates - prepare data beforehand in controllers or middleware.

### Underscore

#### Definition

Underscore.js is a compact utility library that provides more than 100 functional helpers for working with arrays, objects, functions, and collections - such as _.map, _.filter, _.reduce, _.clone, _.debounce, and _.template. It emphasizes functional style, immutability (most helpers return new values), and works in both browser and Node environments without extending native prototypes.
#### Example

```js
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

This section summarizes modern CSS modules and features, preprocessors, Bootstrap, and Material Design.

### Modern CSS

#### Definition

CSS is a collection of modules that evolve independently; there is no single “CSS4” version of the whole language. Module levels such as Selectors Level 4 describe individual specifications, while browsers ship features continuously. Major modern capabilities include:
- Layout - Flexbox, Grid, multi-column, position: sticky.
- Visual effects - 2-D/3-D transforms, transitions, animations, filters, blend modes.
- Responsive design - media queries level 4, container queries, @supports feature queries.
- Typography & UI - web fonts (@font-face), variable fonts, :focus-visible, logical properties.
- Variables & math - custom properties (--var), calc(), clamp(), color functions (color-mix(), hsl()), @property for typed variables.
- Selectors level 4+ - :is(), :where(), :not(), relational selectors (:has()), case-insensitive attribute flags.
Production support still depends on the specific feature and target browsers, so check compatibility rather than relying on a single CSS version label.
#### Common Interview Questions

- **Compare Flexbox and CSS Grid. When would you pick one over the other?** Flexbox is one-dimensional (row or column) and excels at aligning items along a single axis (nav bars, toolbars, centering). Grid is two-dimensional (rows and columns simultaneously) and is suited for full page/section layouts or complex card galleries. You can nest them - use Grid for macro layout, Flexbox for micro alignment inside each cell.
- **Explain the CSS Box Model. What is box‑sizing: border‑box?** Every element is a box consisting of content → padding → border → margin. In the default content-box model, the width and height properties include only the content; in border‑box the width/height include content, padding and border, making sizing more intuitive.
- **What advantages do CSS custom properties (`--vars`) provide over preprocessor variables?** They are live in the cascade: values can change at runtime, inherit, and work with `calc()` and color functions. JavaScript can read them with `getComputedStyle(element).getPropertyValue('--name')` and update them with `element.style.setProperty()`. Sass/LESS variables are resolved at compile time.
- **Explain the cascade and two ways to override a highly specific rule without `!important`.** The cascade considers origin and importance, cascade layers, specificity, scoping proximity, and finally source order. Specificity is compared only after the declarations are in the winning origin/importance/layer. Put third-party CSS in a lower-priority cascade layer, or use an equal/higher-specificity selector later in the same layer. When authoring reusable CSS, wrap contextual selectors in `:where()` so the original rule stays deliberately easy to override; `:where()` cannot retroactively reduce another rule's specificity.
- **What is CSS specificity and how is it calculated?** Selector specificity is compared lexicographically in three columns: IDs, classes/attributes/pseudo-classes, and types/pseudo-elements. The universal selector and combinators add nothing, and `:where()` plus its arguments always contribute `0-0-0`. Inline styles are handled separately by the cascade and outrank normal author stylesheet rules. For example, `#nav .list li a:hover` has one ID, two class/pseudo-class selectors, and two type selectors, so its specificity is `1-2-2` (or `(0,1,2,2)` in the older four-part teaching notation).
- **What is the difference between display: none, visibility: hidden and opacity: 0?** display: none: the element is removed from the document flow, takes no space and causes reflow.
  visibility: hidden: the element is hidden but still occupies space; not interactive.
  opacity: 0: the element is fully transparent but still takes up space and is interactive.
- **How does z-index work and what is a stacking context?** `z-index` orders boxes within a stacking context. It applies to positioned elements and also to flex and grid items without requiring `position`. Stacking contexts can be created by the root, positioned elements with a non-auto `z-index`, opacity below 1, transforms, containment, isolation, and other properties. A descendant cannot escape its nearest ancestor stacking context, which is not necessarily its direct parent.
- **How would you create a responsive website?** Include `<meta name="viewport" content="width=device-width, initial-scale=1">`, use fluid layouts, media or container queries, flexible images (`max-width: 100%; height: auto`), and modern Grid/Flexbox layout. Prefer relative units where they serve the design rather than relying on viewport units for everything.
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
- **What is BEM?** Block-Element-Modifier is a naming convention that makes CSS more maintainable. A block is a standalone entity (for example, `.menu`), an element is part of a block (`.menu__item`), and a modifier changes appearance (`.menu--hidden` or `.menu__item--active`).

### Bootstrap

#### Definition

Bootstrap is a popular open-source CSS and JavaScript framework originally created at Twitter. Bootstrap 5.3.8 is the current stable release as of July 12, 2026. It ships a responsive grid, utility classes, and interactive components implemented with vanilla JavaScript. Components that need dynamic positioning, such as dropdowns, popovers, and tooltips, use Popper; components such as modals and toasts do not all depend on it.
#### Common Interview Questions

- **Explain Bootstrap’s grid breakpoints and how the class naming works (e.g., `col-12`, `col-md-6`).** Bootstrap has six default tiers: xs <576 px, sm ≥576, md ≥768, lg ≥992, xl ≥1200, and xxl ≥1400. Classes follow `col-{breakpoint?}-{span}`: omit the breakpoint for the xs base tier; specify one to apply from that width upward. `col-md-6` is half-width from 768 px upward and, when paired with `col-12`, full-width below.
- **What advantages do Bootstrap utility classes (.d-flex, .text-center, .mt-3, etc.) provide over writing custom CSS?** They enable rapid, consistent styling directly in markup, reduce context-switching between HTML/CSS, and leverage a well-tested design system. Utilities are generated via Sass maps, so you can customize scales and colors while keeping bundle size predictable.
- **How would you customize Bootstrap’s default theme colors without editing the core files?** Install Bootstrap’s Sass source, then override Sass variables before importing:
  ```scss
  // custom.scss
  $primary: #6f42c1; // brand purple
  $font-family-base: 'Inter', sans-serif;
  @import "bootstrap"; // compiles with new variables
  ```
  Compile with Dart Sass or Bootstrap's build tools for a coherent global theme. Bootstrap 5 also exposes global and component-level CSS custom properties for targeted runtime changes, but changing only `--bs-primary` does not automatically regenerate related RGB values or every component's local variables such as `--bs-btn-bg`.

### Material (Material Design & MUI)

#### Definition

Material Design is Google’s opinionated design system for visual, motion, and interaction patterns. MUI's Material UI package is a React component library; as of July 12, 2026, its current major is v9 (9.2.x), and it still implements Material Design 2 rather than automatically turning an application into a Material 3 design. It supplies theming and accessibility-oriented primitives, but application-level accessibility still depends on labels, content, contrast, composition, and developer testing.
#### Common Interview Questions

- **How does MUI’s theming system differ from plain CSS variables or Sass themes?** `createTheme()` creates a JavaScript theme consumed through React context and accessible with `useTheme()`. CSS custom properties remain runtime values too: JavaScript can read and update them through DOM APIs. Sass variables, by contrast, disappear into generated CSS at build time. MUI can also emit CSS theme variables, so these approaches can be combined.
- **Explain the pros and cons of MUI’s `styled()` API compared with the `sx` prop.** `styled()` is useful for reusable, named design-system components; `sx` is concise for local, theme-aware overrides. Both can involve MUI's runtime styling engine. `styled()` is not inherently a larger bundle in every case, and `sx` is a prop rather than something that is itself tree-shaken; performance and maintainability depend on usage patterns.
- **What accessibility features are built into MUI components, and how would you audit them?** Many components provide native semantics, focus management, and keyboard behavior. For example, `IconButton` renders a native button by default, but an icon-only button still needs a developer-provided accessible name such as `aria-label`. Test keyboard and screen-reader behavior, contrast, names, roles, states, and focus order with automated tools such as axe/Lighthouse plus manual testing; a component library alone cannot guarantee WCAG conformance.

## Node.js Fundamentals

As of July 12, 2026, Node.js 26 is the Current line, Node.js 24 is Active LTS, and Node.js 22 is Maintenance LTS. Node.js 18 and 20 are end-of-life. The examples below target supported modern Node versions and call out module-system differences where they matter.

### Node.js Single-Threaded Nature & Concurrency Model

#### Definition

Node.js runs JavaScript in a single main thread, meaning only one piece of JavaScript code is executed at any given moment. However, this does not make Node.js “single-tasking.” Concurrency is achieved through an event-driven, non-blocking I/O model powered by the event loop and the libuv library. Long-running or blocking operations (such as file system access, DNS lookups, compression, or cryptography) are delegated to the operating system or to a background thread pool, while the main thread continues executing other callbacks. This design allows Node.js to efficiently handle a large number of concurrent I/O-bound operations without the overhead of managing one thread per request.
#### Example

```js
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
Since libuv 1.45 / Node 20, timers run only after the Poll phase rather than both before and after it in an iteration. Node also drains the `process.nextTick` queue and V8 microtasks at defined boundaries. In ordinary CommonJS callback execution, `nextTick` runs before Promise/`queueMicrotask` work, but top-level ESM evaluation has different ordering because the module itself runs as a microtask. Recursive `nextTick` use can starve the event loop; prefer `queueMicrotask()` for portable microtask deferral unless `nextTick` semantics are specifically required.
#### Example

```js
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

Guaranteed prefix for this CommonJS example:
```text
Start
End
nextTick
Promise
```

The remaining `setTimeout`, `setImmediate`, and I/O messages do not have one portable total order in this top-level example. Timer readiness, file-system completion, platform behavior, and the event-loop iteration in which each callback becomes eligible all matter. When a timer and an immediate are scheduled from inside the same I/O callback, the immediate normally runs first because the Check phase follows Poll.

Explanation:
Synchronous code runs first.
process.nextTick runs before Promise microtasks.
Promise callbacks run before the next event loop phase.
Eligible I/O callbacks are handled in Poll, `setImmediate` callbacks in Check, and timers when their threshold has elapsed. Those phase names do not make the completion time of an external I/O operation deterministic.

#### Common Interview Questions

- **What is the role of the event loop in Node.js?** It coordinates the execution of asynchronous callbacks by cycling through its phases and executing queued tasks when the call stack is empty. This allows Node.js to handle many concurrent I/O operations without blocking the main thread.
- **What is the difference between setTimeout(fn, 0) and setImmediate(fn)?** setTimeout schedules the callback in the Timers phase after at least the given delay.
  setImmediate schedules the callback in the Check phase, which runs immediately after the Poll phase.
  When scheduled from an I/O callback, setImmediate usually fires before setTimeout(…, 0).
- **How do microtasks differ from event-loop phase callbacks in Node.js?** Promise reactions and `queueMicrotask` use V8's microtask queue; `process.nextTick` uses a separate Node queue. Both are drained at boundaries before the loop proceeds to timers/I/O/immediates, but exact top-level ordering differs between CommonJS and ESM. Within an ordinary CommonJS callback, `nextTick` is drained first and can starve I/O if recursively refilled.
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

```js
const crypto = require('crypto');

console.time('hash');

crypto.pbkdf2('password', 'salt', 100000, 64, 'sha512', () => {
  console.timeEnd('hash');
});
```

If you run this several times in parallel:

```js
for (let i = 0; i < 8; i++) {
  crypto.pbkdf2('password', 'salt', 100000, 64, 'sha512', () => {
    console.log(`Hash ${i} done`);
  });
}
```

Only four operations will run simultaneously by default, because the libuv thread pool has four worker threads. The remaining tasks wait in a queue until a thread becomes free.
You can change the pool size:

```sh
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

Event Emitters are a core Node.js pattern for event-driven communication. The EventEmitter class (from the events module) provides a simple publish-subscribe mechanism where objects emit named events and listeners subscribe to them. Calling `emit()` itself is synchronous unless a listener explicitly schedules asynchronous work.
An emitter maintains an internal mapping of event names to listener arrays. When an event is emitted via emit(eventName, ...args), all registered listeners for that event are synchronously invoked in the order they were added. This pattern underlies many built-in Node.js APIs, including streams, HTTP servers, sockets, and process-level events.
#### Example

```js
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
```text
Order 42 created
First order only
Order 43 created
```

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

```js
const fs = require('fs');

const readable = fs.createReadStream('bigfile.txt');
const writable = fs.createWriteStream('copy.txt');

readable.pipe(writable);
```

This copies a large file without loading it fully into memory. Data flows chunk by chunk, and the writable stream signals when it is ready to receive more.
Transform stream example:

```js
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
- **What does pipe() do internally?** pipe() connects a readable stream to a writable stream, forwards chunks, manages backpressure, and ends the destination by default when the source ends. It does not provide complete error propagation and cleanup across every stream in a chain; for production pipelines, prefer stream.pipeline() or its promise-based API so failures close the whole pipeline consistently.
- **What is the difference between a Duplex and a Transform stream?** Both are readable and writable. A Duplex stream treats input and output as independent (e.g., a TCP socket). A Transform stream modifies the data, so its output is a transformed version of its input (e.g., gzip, encryption, JSON parsing).
- **Are streams synchronous or asynchronous?** Stream data often arrives from asynchronous I/O, but streams inherit EventEmitter semantics, so a particular `emit()` invokes listeners synchronously. Callback timing depends on the stream API and implementation. A `_transform` method may complete synchronously or call its callback later, and CPU-heavy synchronous chunk processing still blocks the event loop.
- **How are streams related to Event Emitters?** All streams inherit from EventEmitter and use events such as data, end, error, finish, and close to signal state changes and data availability.

### Buffers vs Strings

#### Definition

In JavaScript, a String is an immutable sequence of UTF-16 code units; it is not stored as “usually UTF-8”. An encoding matters when text is converted to or from bytes. A Buffer is Node.js's mutable byte-array type for binary data such as files, network packets, images, audio, and encrypted content.
Strings are suitable for text manipulation. Buffers are designed for byte-oriented I/O and interoperate with Node APIs and `Uint8Array`.

#### Example

```js
const text = "💡";
const buf = Buffer.from(text, 'utf8');

console.log(text.length); // 2 UTF-16 code units
console.log(buf.length);  // 4 UTF-8 bytes

// Binary modification
const greeting = Buffer.from('hello', 'utf8');
greeting[0] = 0x48; // 'H'
console.log(greeting.toString('utf8')); // "Hello"
```

Reading a file as a Buffer vs as a String:

```js
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
CommonJS (CJS) – the original Node.js module system, using require() to import and module.exports / exports to export. Traditional CommonJS loading is synchronous and resolution happens at runtime.
ECMAScript Modules (ESM) – the standardized JavaScript module system, using import / export syntax. Modules are statically analyzable, support top-level await, and are resolved using URLs and file extensions.
`.cjs` is always CommonJS and `.mjs` is always ESM. For `.js`, the nearest package.json `"type"` field normally supplies the default. Modern Node can also use syntax detection for ambiguous `.js` input without an explicit package type, but packages should set `"type"` to avoid ambiguity.
#### Example

`math.cjs`:

```js
module.exports.add = (a, b) => a + b;
```

`app.cjs`:

```js
const { add } = require('./math.cjs');
console.log(add(2, 3));
```

ES Modules:

`math.mjs`:

```js
export const add = (a, b) => a + b;
```

`app.mjs`:

```js
import { add } from './math.mjs';
console.log(add(2, 3));
```

Package configured for ESM:

```json
{
  "type": "module"
}
```

```js
// app.js (now treated as ESM)
import fs from 'node:fs';
const mod = await import('./math.mjs');
```

Dynamic `import()` also works in CommonJS, but top-level `await` does not:

```js
// app.cjs
async function loadMath() {
  const mod = await import('./math.mjs');
  return mod;
}
```

#### Common Interview Questions

- **What are the main differences between CommonJS and ES Modules?** CommonJS uses synchronous require and module.exports, is resolved at runtime, and is the traditional Node.js format. ES Modules use static import/export, are resolved before execution, support tree-shaking, top-level await, and are the standard across browsers and Node.
- **How does Node.js decide whether a file is treated as CommonJS or ESM?** `.cjs` means CommonJS and `.mjs` means ESM. The nearest package scope's `"type"` controls `.js`. With no explicit marker, current Node may detect ESM-only syntax in ambiguous input; declare `"type"` explicitly rather than relying on detection.
- **Can you import a CommonJS module from an ES Module and vice versa?** Yes, but with interop rules. ESM can import CJS via default import (import pkg from 'pkg') or named exports when Node can statically detect them. Modern Node can require() synchronous ES modules that do not use top-level await, while dynamic import() works from CommonJS for all ESM cases and remains the safest universal option.
- **Why are ES Modules considered better for tooling and optimization?** Because their static structure allows bundlers and runtimes to perform tree-shaking, dead-code elimination, and dependency analysis at build time.
- **What is the difference between module.exports and exports?** exports is initially a reference to module.exports. Reassigning exports breaks that link; only module.exports is actually returned by require().
- **Why do ES Modules require explicit file extensions in Node.js imports?** Because ESM resolution follows URL semantics, not CommonJS’s extension-guessing algorithm, making resolution deterministic and compatible with browser module loading.

### Process & Global Objects

#### Definition

Node.js provides APIs that are available without imports, including `process`, `Buffer`, timers, `URL`, `fetch` and `console`. The standard cross-runtime reference to the global object is `globalThis`; Node's `global` alias is legacy. Many apparently top-level values are module-scoped rather than global—for example, `__dirname` and `__filename` in CommonJS.
The process object is an instance of EventEmitter and provides access to:
- Environment variables (process.env)
- Command-line arguments (process.argv)
- Process ID and parent process (process.pid, process.ppid)
- Exit codes and termination (process.exit)
- Current working directory (process.cwd)
- Runtime events (exit, uncaughtException, unhandledRejection, SIGINT, ...)
#### Example

```js
console.log(process.env.NODE_ENV);
console.log(process.argv);

process.on('SIGINT', () => {
  console.log('Gracefully shutting down...');
  process.exit(0);
});
```

Using the standard global reference versus module scope:

```js
globalThis.appName = 'Cookbook';

console.log(globalThis.appName); // 'Cookbook'
```

#### Common Interview Questions

- **What is the difference between the global object in Node.js and `window` in browsers?** Use `globalThis` to refer to the global object portably. In a browser window, `window` is both the global object and a DOM browsing-context API; Node has no DOM and retains `global` only as a legacy alias. Top-level declarations in Node modules are module-scoped rather than automatically becoming global properties.
- **What information can you get from process.env and how is it typically used?** process.env exposes environment variables. It is commonly used for configuration (NODE_ENV, database URLs, API keys, feature flags) and to separate development, staging, and production behavior without changing code.
- **How do you pass arguments to a Node.js script and read them?** Arguments are passed after the script name: node app.js arg1 arg2. They are available in process.argv as an array, where index 0 is the Node binary and index 1 is the script path.
- **What is the difference between process.exit() and letting the event loop finish naturally?** process.exit() terminates the process immediately, even if there are pending I/O operations, which can lead to data loss or incomplete work. Letting the event loop drain allows all pending callbacks and handles to complete gracefully.
- **Why is process an EventEmitter and what events does it emit?** Because the process lifecycle is event-driven. It emits events such as exit, beforeExit, uncaughtException, unhandledRejection, and OS signals like SIGTERM and SIGINT, allowing applications to perform cleanup and graceful shutdown.
- **What are some other commonly used global objects in Node.js?** `Buffer`, timers, `queueMicrotask`, `URL`, `fetch`, `console`, and `globalThis` are common globals. `__dirname` and `__filename` are CommonJS module-scoped variables, not globals, and are unavailable in ESM; use `import.meta.dirname` / `import.meta.filename` in supported modern Node versions or derive paths from `import.meta.url` when wider compatibility is required.

### Async Patterns in Node

#### Definition

Asynchronous programming in Node.js is built around non-blocking operations and callback-based APIs, which evolved into Promises and later into async / await. These patterns allow Node.js to perform I/O and long-running tasks without blocking the event loop, enabling high concurrency on a single JavaScript thread.
The three main async patterns in Node.js are:
Callbacks – the original pattern, often following the error-first convention (err, result).
Promises – represent a value that will be available in the future and allow chaining via .then() / .catch().
async / await – syntactic sugar over Promises that enables writing asynchronous code in a synchronous style using try / catch for error handling.
#### Example

```js
const fs = require('node:fs');

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
const fsSync = require('node:fs');
const syncData = fsSync.readFileSync('file.txt', 'utf8');
console.log(syncData);

// Asynchronous with callback:
fsSync.readFile('file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});

// Promise-based:
const fs = require('node:fs/promises');
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
const fs = require('node:fs');

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

// Last-resort process-level safety net. Do not continue normal operation.
function fatal(label, reason) {
  const error = reason instanceof Error ? reason : new Error(String(reason));
  fs.writeSync(process.stderr.fd, `${label}: ${error.stack ?? error.message}\n`);
  // Only synchronous cleanup is safe here; let a supervisor restart the process.
  process.exit(1);
}

process.on('unhandledRejection', (reason) => fatal('Unhandled rejection', reason));
process.on('uncaughtException', (error) => fatal('Uncaught exception', error));
```

#### Common Interview Questions

- **What is the difference between `uncaughtException` and `unhandledRejection`?** `unhandledRejection` is emitted when a Promise remains rejected without a handler for a turn of the event loop. `uncaughtException` is emitted when an exception reaches the event loop; under Node's default unhandled-rejection mode, an unhandled rejection can ultimately be raised through this path too. Installing either listener changes default process behavior, so a listener must implement an explicit fatal/shutdown policy rather than merely logging and continuing.
- **Why is it dangerous to continue running after an uncaughtException?** The application state may be corrupted (open handles, partial writes, inconsistent memory). Node.js documentation recommends logging the error and performing a controlled shutdown rather than attempting to continue.
- **How should errors be handled in async/await code?** Wrap await calls in try/catch blocks or let the error propagate to a higher-level handler. Always ensure rejected Promises are either awaited or returned so they can be caught.
- **What is the recommended pattern for error handling in Express / HTTP servers?** Centralized error-handling middleware that receives (err, req, res, next) and converts internal errors into sanitized HTTP responses while logging full stack traces server-side.
- **What are “operational errors” vs “programmer errors”?** Operational errors are expected runtime issues (network timeouts, invalid input, missing files) and should be handled gracefully.
  Programmer errors are bugs (null dereference, logic flaws, uncaught exceptions) and usually require process restart after logging.
- **How do you implement graceful shutdown on fatal errors or signals?** For `SIGTERM`/`SIGINT`, stop accepting requests, close resources, enforce a timeout, and then exit. An `uncaughtException` means process state may be unsafe: perform only synchronous last-resort logging/cleanup and terminate so a supervisor can restart it. Define and test an explicit fatal policy for unhandled rejections rather than installing a handler that merely logs and continues.

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
- **How do child processes help with CPU-bound tasks in Node.js?** They move CPU-intensive work into separate processes that the OS can schedule in parallel, potentially on different CPUs, so the parent's event loop remains responsive. The scheduler—not Node—decides the actual core placement.
- **What is the security implication of using exec with user input?** Since exec runs a shell, unsanitized input can lead to command injection. spawn or execFile should be preferred for safer argument handling.

### Clustering & the Cluster Module

#### Definition

Clustering runs multiple Node.js worker processes that can share a server port. The primary process spawns workers, each with its own event loop, JavaScript isolate and memory. Connections are distributed by the operating system or Node's scheduling strategy. Multiple processes let the OS use available CPUs, but one worker is not permanently bound to one physical core unless deployment tooling explicitly configures CPU affinity.
#### Example

```js
const cluster = require('cluster');
const http = require('http');
const { availableParallelism } = require('node:os');

if (cluster.isPrimary) {
  const workerCount = availableParallelism();

  for (let i = 0; i < workerCount; i++) {
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

Workers are independently schedulable processes and may run in parallel; there is no guaranteed one-worker-to-one-core binding.

#### Common Interview Questions

- **Why is clustering needed if Node.js already supports asynchronous I/O?** Asynchronous I/O enables high concurrency but does not provide CPU parallelism. A single Node.js process can use only one core for JavaScript execution. Clustering allows multiple processes to run in parallel across cores, increasing throughput for CPU-bound and high-load servers.
- **How does the cluster module distribute incoming connections?** By default, the primary process uses a round-robin strategy on most platforms to distribute sockets to workers. On some systems/configurations, workers accept connections through the shared handle and the operating system schedules them.
- **What happens if a worker process crashes?** The primary process can observe the `exit` event and start a replacement, improving availability. In-flight work owned by the failed process may still be lost, so restart logic alone does not guarantee zero downtime or correct recovery.
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

```json
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
    "node": ">=22"
  }
}
```

#### Common Interview Questions

- **What is the purpose of the `main` field?** It is the legacy primary package entry used when `exports` is absent; the file's module format is determined by its extension and package `type`. `exports` takes precedence in supported Node versions. The non-standard `module` field is a bundler convention, not a Node-defined package entry field.
- **What does the type field control?** It sets the explicit default module system for `.js` files in that package scope: `"module"` means ESM and `"commonjs"` means CommonJS. If it is absent, current Node may apply syntax detection to ambiguous input, so published packages should always declare the field.
- **How does the exports field differ from main?** exports provides fine-grained, explicit control over which files are publicly accessible and supports conditional exports (import vs require, browser vs node). It is the modern, recommended alternative to exposing the entire package structure.
- **What is the difference between dependencies and devDependencies?** dependencies are required at runtime in production.
  devDependencies are only needed for development and build tooling (TypeScript, bundlers, linters, test frameworks).
- **What are peerDependencies used for?** They express a compatible version range for a host package, such as a plugin declaring support for React 18 or 19, without bundling a private copy of that host. npm 7+ attempts to install compatible peer dependencies automatically and reports or fails on conflicts; library consumers still need to ensure the complete peer graph is compatible.
- **What is the bin field used for?** It exposes executables when the package is installed globally or via npx, enabling CLI tools.
- **Why is the engines field important?** It declares the Node.js versions a package has tested and supports, allowing package managers and CI to warn or fail on incompatible runtimes. As of July 2026, Node 18 and 20 are end-of-life; new production projects should target supported releases (Node 22 or newer, preferably an appropriate LTS line) and express the tested range precisely.
### Package Managers: npm vs pnpm vs yarn (2026 perspective)

#### Definition

Package managers are tools that install, resolve, and manage JavaScript dependencies based on package.json and a lockfile. In the Node.js ecosystem, the three dominant managers are npm, yarn, and pnpm. By 2026, all three are mature, stable, and production-ready, but they differ in architecture, performance, disk usage, and ecosystem conventions.
#### Example

Basic install commands are conceptually equivalent:

```sh
npm install
yarn install
pnpm install
```

Each writes its own lockfile (`package-lock.json`, `yarn.lock`, or `pnpm-lock.yaml`). npm normally creates `node_modules`; modern Yarn can use either Plug'n'Play (no `node_modules`) or a node-modules linker; pnpm normally creates `node_modules` backed by a project virtual store and a content-addressable package store.

#### Common Interview Questions

- **What are the main architectural differences?** npm uses a hoisted `node_modules` installation and deduplicates compatible versions where possible. Modern Yarn supports multiple linkers: Plug'n'Play records dependency resolution in a `.pnp.cjs` map and can avoid `node_modules`, while its node-modules linker supports conventional layouts. pnpm uses a content-addressable store plus a project virtual store, linking or cloning package files into the project and exposing a compatibility-oriented `node_modules` tree.
- **Are pnpm installs inherently more deterministic?** All three can produce reproducible installs when the lockfile is committed and CI uses the frozen/immutable install mode. pnpm's shared store can reduce repeated downloads and disk usage; actual speed depends on cache state, filesystem, lifecycle scripts, network, and project shape.
- **What are phantom dependencies?** They are packages imported without being declared by the importing project/package. Hoisting can accidentally make them resolvable. pnpm's default layout and Yarn PnP make undeclared access harder, but configuration and compatibility hoisting can relax those boundaries, so no package manager removes the need for correct manifests and CI checks.
- **How do you choose for a monorepo?** Evaluate workspace features, constraints/policies, tool compatibility, cache behavior, CI support, and the team's existing lockfile. pnpm is attractive for store efficiency and workspace filtering; Yarn offers PnP and a constraint-rich modern toolchain; npm provides the built-in, widely compatible baseline and workspaces.
- **In one sentence, how would you summarize the landscape?** npm, Yarn, and pnpm are all production-capable; choose based on installation model, tooling compatibility, workspace needs, and team operations rather than treating one as universally “serious” or another as merely legacy.

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
- **How does AsyncLocalStorage differ from `async_hooks.createHook()`?** `AsyncLocalStorage` is the supported high-level API for propagating request context and includes runtime optimizations and safer lifecycle handling. `createHook()` exposes low-level async-resource callbacks for instrumentation; implementing context propagation directly with it is harder, easier to leak, and generally less efficient. Prefer `AsyncLocalStorage` for application context and use low-level hooks only when building specialized diagnostics tooling.
- **When should you avoid AsyncLocalStorage?** In extremely high-throughput, low-latency paths where the overhead of async hooks may become measurable, or in simple services where explicit parameter passing is clearer.
- **What is the difference between performance.now() and Date.now()?** performance.now() provides monotonic, high-resolution timestamps (sub-millisecond, unaffected by system clock changes). Date.now() is wall-clock time with lower precision.
- **Why are performance hooks important in production Node systems?** They enable:
  Precise latency measurement
  Event loop delay tracking
  Garbage collection profiling
  Custom APM instrumentation
  Bottleneck detection in async pipelines

## React Deep Dive
This section explores advanced React concepts beyond basic hooks and components. As of July 2026, React 19.2.x is the current stable line; npm's latest stable patch is 19.2.7. React Server Components and Server Functions should use the latest patched 19.2.x packages because earlier React 19 RSC packages were affected by security vulnerabilities; 19.2.4 was the minimum fixed version in the January 2026 advisory, but 19.2.7 is the current recommendation.

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
- **What are the performance implications of passing new object/function references as props?** A new reference matters when a consumer relies on reference equality, for example a child wrapped in `memo` or a Hook dependency. Use `useMemo` or `useCallback` only when that stabilization has a measured purpose; they are performance optimizations, not semantic guarantees, and React Compiler can reduce the need for manual memoization.
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
- **What changed about refs in React 19?** React 19 supports ref as a prop for function components, so new components usually do not need `forwardRef`. It also supports callback ref cleanup functions that React calls when the DOM node is removed. `forwardRef` remains supported and common in existing code.
#### Communication Patterns Reference
1. Props (Parent -> Child)
   - Step 1: Parent passes data to child via props: `<Child data={value} />`.
   - Step 2: Child receives props as function parameters: function Child({ data }).
   - Step 3: Child uses props in rendering and logic; props are read-only.
   - Note: Parent renders normally render children too. `React.memo()` can skip a child render when its props are unchanged, but it is a performance optimization and is most useful when the child is expensive and receives stable props.
2. Callback Functions (Child -> Parent)
   - Step 1: Parent passes callback function as prop: `<Child onUpdate={handleUpdate} />`.
   - Step 2: Child calls callback with data: props.onUpdate(newData).
   - Step 3: Parent handles the callback and updates its state accordingly.
   - Note: `useCallback()` is useful when a stable function identity is consumed by a memoized child or a Hook dependency; by itself it does not prevent child re-renders.
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
7. Refs as props / forwardRef (Parent accessing Child)
   - Step 1: In React 19+ code, let the child accept `ref` as a prop: `function Child({ ref }) { ... }`.
   - Step 2: Attach the ref to a DOM element or expose a limited imperative API with `useImperativeHandle`.
   - Step 3: Parent creates ref and passes to child: `<Child ref={childRef} />`.
   - Note: `forwardRef()` is still supported for compatibility and pre-React 19 patterns. Use refs sparingly as they break component encapsulation; prefer declarative props when possible.
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
- React 19 (2024): Action Hooks - use hook, useOptimistic, useActionState, and react-dom's useFormStatus
- React 19.2 (2025): Latest stable 19.2.x line - useEffectEvent, `<Activity>`, cacheSignal for React Server Components, Performance Tracks, and partial prerendering APIs
- React 19.x APIs listed above are stable in the React 19 line; `<ViewTransition>` and addTransitionType remain Canary APIs and should not be described as stable.
#### Common Interview Questions

- **How did hooks change React development patterns since their introduction in 16.8?** Hooks made function components the standard choice for state, effects, context, reusable stateful logic, and concurrent features. Classes remain supported, and React still has no direct function-component equivalents for Error Boundaries or `getSnapshotBeforeUpdate`.
- **What problem do concurrent hooks (useTransition, useDeferredValue) solve in React 18?** They enable non-blocking user interfaces by allowing React to interrupt rendering, prioritize urgent updates (like typing), and defer non-urgent updates (like rendering large lists), improving perceived performance.
- **How does the use hook in React 19 differ from traditional async patterns?** The `use()` API can read Promises and Context directly during render, and unlike normal hooks it can be called conditionally. Do not create an uncached promise directly in render; pass Promises from a framework, cache, or Suspense-compatible data source. Treat `use()` as a rendering primitive, not as a replacement for TanStack Query, React Router loaders, or framework data APIs in everyday client data fetching.
- **What does useEffectEvent solve?** It lets you move event-like logic out of effects while still reading the latest props/state, reducing stale-closure bugs without re-running the effect.
- **What changed in React 19.2 and what remains Canary?** React 19.2 stabilizes `<Activity>`, useEffectEvent, cacheSignal, Performance Tracks, and partial prerendering support. `<ViewTransition>` and addTransitionType remain Canary-only APIs.
- **When would you choose useLayoutEffect over useEffect?** `useLayoutEffect` runs after DOM mutations but before the browser repaints and blocks paint, so reserve it for measurements or visual DOM work that must happen synchronously. `useEffect` runs after React commits and usually after paint, although interaction-caused effects may run before paint.
- **How do you synchronize an external system while a component is mounted?** Use an Effect with the correct dependencies and return a cleanup function. Cleanup runs before the Effect re-runs with changed dependencies and when the component unmounts. In development Strict Mode, React deliberately performs an extra setup -> cleanup -> setup cycle to expose missing cleanup.
- **What common operations require cleanup on unmount?** Timers/Intervals  -  clearTimeout, clearInterval
  Subscriptions  -  unsubscribe, websocket.close
  Event Listeners  -  removeEventListener
  API Requests  -  AbortController.abort
  DOM References  -  document.removeEventListener
- **What's the difference between an empty dependency array and no dependency array in useEffect?** An empty array means the Effect has no reactive dependencies and runs after the initial commit, with the additional development-only setup/cleanup cycle under Strict Mode. Omitting the array runs the Effect after every commit. Either form can loop if the Effect updates state in a way that retriggers itself.
- **When would you need to use useRef with useEffect?** When you need to access the latest state/value in a cleanup function without causing re-renders.
- **What's the difference between useMemo and useCallback, and when should each be used?** `useMemo` caches a calculation result and `useCallback` caches a function definition between renders. Use them when profiling or a reference-equality consumer justifies the optimization, such as an expensive calculation, a memoized child, or a Hook dependency. React may discard these caches, and React Compiler can apply memoization automatically.
- **How have hooks evolved to handle common patterns like forms and optimistic updates?** React 19 introduced useActionState, react-dom's useFormStatus, and useOptimistic to provide built-in solutions for patterns that previously required external libraries or complex custom hook implementations. useFormStatus must be called from a component rendered inside the relevant form.
- **When would you use useOptimistic?** Use `useOptimistic` to show instant UI feedback while a client action, form action, or server action confirms a mutation. It pairs well with `useActionState` and form actions, but the optimistic value must be reconciled with the real action result and rolled back or corrected on errors.
- **What are the Rules of Hooks and why are they necessary?** Regular Hooks must be called at the top level and only from React function components or custom Hooks so their call order stays stable. React 19's `use()` is the documented exception: it may be called in loops or conditions, but not inside `try`/`catch`, and it must still be called while React is rendering a component or Hook.
  Common Effect Patterns:
  External synchronization without reactive dependencies  -  `useEffect(setup, [])`
  Re-synchronize when dependencies change  -  `useEffect(setup, [dep])`
  Dispose resources before re-synchronization and unmount  -  return cleanup from `setup`
  Synchronize after every commit  -  `useEffect(setup)` (use sparingly)

### React Component Lifecycle

#### Definition

The React component lifecycle refers to the series of phases a component goes through from creation to removal from the DOM. Function components use Hooks, but an Effect should be understood as synchronization with an external system rather than as a one-to-one replacement for class lifecycle methods: one synchronization process may start, clean up, and restart several times while a component remains mounted.
#### Lifecycle Phases

- **Mounting** - Component is created and inserted into DOM
- **Updating** - Component re-renders due to state or prop changes
- **Unmounting** - Component is removed from DOM
- **Error Handling** - Catching errors in child components

#### Common Interview Questions

- **What happens during component mount and unmount?** Mount Phase  -  React renders, commits DOM changes, and then runs Effects according to their dependencies. In development Strict Mode, Effect setup is immediately tested with an extra cleanup/setup cycle.
  Unmount Phase  -  React removes the component and runs Effect cleanup. Cleanup also runs before an Effect re-synchronizes after dependency changes and when an Activity subtree becomes hidden.
- **How do you temporarily hide UI without losing state?** Use `<Activity>` (React 19.2+) with `mode="visible"` or `mode="hidden"`. Hidden activities visually hide the subtree, clean up effects, process hidden updates at lower priority, and preserve component state for faster return navigation.
- **What caveat does Activity introduce for DOM side effects?** Hidden Activity subtrees keep DOM nodes around while cleaning up effects. Elements with their own browser side effects, such as `video`, `audio`, and `iframe`, may need explicit effect cleanup so playback, subscriptions, or embedded work stop while hidden.
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
- React 19 (2024): use hook can consume context; `<Context value={...}>` can be used as a provider shorthand
#### Common Interview Questions

- **How has the Context API evolved from its experimental beginnings to the current stable API?** The API moved from an unstable, type-unsafe pattern using contextTypes to a first-class, type-safe API with createContext, explicit Providers, and multiple consumption methods (Consumer render prop, useContext hook).
- **What changed for Context providers in React 19?** New code can render the context object directly as a provider, for example `<ThemeContext value={theme}>...</ThemeContext>`. `<ThemeContext.Provider>` remains supported and common in existing codebases.
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

Error Boundaries are React components that catch errors thrown while React renders or processes supported lifecycle work in their descendant tree, log those errors, and display fallback UI instead of the crashed subtree. They do not act as a general JavaScript exception handler for every callback or asynchronous task.
#### Version Evolution Timeline

- React 16 (2017): Error Boundaries introduced as class components with componentDidCatch
- React 16.2+: Community libraries emerge for functional component error boundaries
- React 18 (2022): Improved error recovery and development error overlay
- React 19 (2024): Continued class-component requirement for Error Boundary fallback UI, plus root error logging options
#### Common Interview Questions

- **Why are Error Boundaries still implemented as class components in modern React?** A boundary uses static `getDerivedStateFromError` to derive fallback state during rendering and `componentDidCatch` for commit-time reporting. React 19 has no direct Hook equivalent, although libraries can wrap the class implementation in a function-oriented API.
- **How has error handling evolved with community solutions despite the class component requirement?** Libraries like react-error-boundary provide functional-style APIs that wrap the class component implementation, offering better developer experience with features like error resetting, recovery callbacks, and more flexible fallback UIs.
- **What types of errors do Error Boundaries catch versus what they miss?** Caught: errors during rendering, constructors, supported lifecycle methods, and errors thrown from a Transition function. Not caught: event handlers, detached asynchronous callbacks such as `setTimeout` or unhandled Promise work, server-side rendering errors, and errors thrown by the boundary itself.
- **How do you handle errors that Error Boundaries can't catch?** Use window.onerror for global errors, window.addEventListener('unhandledrejection') for promise rejections, and try/catch blocks in event handlers and async operations.
- **What root-level error options did React 19 add?** `createRoot()` and `hydrateRoot()` support `onCaughtError` for errors caught by Error Boundaries, `onUncaughtError` for errors not caught by an Error Boundary, and `onRecoverableError` for errors React can automatically recover from. These are for production logging/reporting and do not replace Error Boundary fallback UI.
- **What improvements did React 18 and 19 bring to error handling?** React 18 improved component stacks and recovery in concurrent rendering. React 19 improved error logging and added root-level reporting hooks for caught, uncaught, and recoverable errors.
- **How would you implement error boundaries for a large application?** Use a hierarchical approach: a top-level boundary for critical errors, feature-level boundaries for sections of the app, and component-level boundaries for isolated features. Each level can have different fallback strategies.
- **What's the difference between getDerivedStateFromError and componentDidCatch?** Static `getDerivedStateFromError` is a pure render-phase state derivation used to choose fallback UI. `componentDidCatch` runs after React commits the fallback and is the place for side effects such as error reporting.

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
- **What performance considerations differ between these patterns?** An HOC defined once adds a wrapper layer; calling an HOC inside render is an anti-pattern because it creates a new component identity. Inline render props create new function references, and compound components can propagate context updates to many consumers. Measure the actual tree and update cost instead of assuming one pattern is always fastest.
- **How has TypeScript support evolved across these patterns?** HOCs were notoriously difficult to type correctly. Render props and hooks have much better TypeScript inference. Modern patterns prioritize type safety from the ground up.

### React Hook Form

#### Definition

React Hook Form emerged as a performance-focused alternative to existing form libraries, leveraging uncontrolled inputs, refs, and granular subscriptions to minimize rendering work. Browser-native constraint validation is optional through `shouldUseNativeValidation`; RHF otherwise uses registered rules or schema resolvers.
#### Version Evolution Timeline

- v2-v5 (2019-2020): Early releases establish the uncontrolled-input and subscription-based performance model
- v6 (July 2020): Major API and TypeScript migration step
- v7 (April 2021): Smaller API, stronger TypeScript inference, and migration changes from v6
- v7.20-v7.40 (November 2021-November 2022): Continued `useFieldArray`, validation, typing, and subscription improvements
- v7.81.0 (July 2026): Current stable release on the actively maintained v7 line; v8 remains prerelease
#### Common Interview Questions

- **How has React Hook Form's performance story evolved across versions?** Early versions focused heavily on uncontrolled inputs for minimal re-renders. Recent versions maintain performance while adding features like real-time validation, without sacrificing the core performance benefits.
- **What significant improvements did v7 bring compared to v6?** Complete TypeScript rewrite providing excellent type inference, better developer experience with improved error messages, enhanced useFieldArray for dynamic forms, and more flexible validation strategies.
- **How does React Hook Form integrate with modern validation libraries like Zod and Yup?** Through resolver adapters (yupResolver, zodResolver) that allow using schema-based validation while maintaining RHF's performance characteristics. The library converts schema errors to RHF's error format.
- **What's the evolution of the Controller component and when should it be used?** Initially for integrating controlled components, now also handles complex UI libraries. Use Controller for third-party components that don't expose refs, or when you need fine-grained control over re-renders.
- **How has React Hook Form adapted to React 18+ concurrent features?** Better handling of concurrent renders, proper cleanup of effects, and compatibility with concurrent mode patterns. The library respects React's rendering priorities.
- **What are the main differences between React Hook Form's approach and Formik's?** React Hook Form favors uncontrolled native inputs and granular subscriptions, while Formik keeps form values in React state and exposes controlled-form helpers. RHF can minimize per-keystroke rendering, but validation mode, `Controller`, `useWatch`, and `formState` subscriptions can still cause renders; Formik can also limit updates with field-level composition and `FastField`.
- **How does useFormContext enable better form composition in complex applications?** Allows deeply nested form components to access form methods without prop drilling, enabling better component separation and reuse in large form architectures.
- **What performance optimizations has React Hook Form maintained throughout its evolution?** Minimal re-renders through uncontrolled inputs, optimized validation execution, smart subscription management to formState, and efficient dirty field tracking.

### Formik

#### Definition

Formik was one of the first comprehensive form libraries for React, pioneering the controlled component approach to form management. Its evolution reflects the React ecosystem's journey from class-based forms to hooks, and eventually to more performance-focused alternatives.
#### Version Evolution Timeline

- v1 (2017): Initial release with render props and HOC patterns
- v2 (2019): Hook support added with useFormik, TypeScript improvements
- v2.1+ (2020): useField hook, better TypeScript support
- v2.4.x (2023-2025): Incremental fixes and compatibility releases; 2.4.9 is the current stable release
- Current State: Mature and still widely used; compare its state-driven model with React Hook Form rather than treating either library as a universal replacement
#### Common Interview Questions

- **How did Formik's architecture evolve from render props to hooks?** Formik started with a render props pattern (v1) which was familiar to React developers at the time. With the introduction of hooks in React 16.8, Formik added useFormik and useField hooks (v2) to provide a more modern API while maintaining backward compatibility.
- **What were Formik's main contributions to the React form ecosystem?** Formik popularized the concept of a comprehensive form library, introduced seamless Yup integration, demonstrated effective validation patterns, and provided a blueprint for form state management that influenced later libraries.
- **Why might a team choose React Hook Form over Formik?** RHF's uncontrolled-input and subscription model can reduce rendering work in large forms and offers strong schema-resolver and TypeScript support. Formik remains a reasonable choice when a team values its state-driven mental model, established Yup integration, or existing component ecosystem; profile complex forms rather than assuming either approach is always faster.
- **What is the purpose of FastField and how does it optimize performance?** `FastField` uses shallow comparisons to avoid updates caused by unrelated Formik state. It still re-renders when its own value, error, touched state, `name`, or props change, and when global state it observes such as `isSubmitting` changes.
- **How does Formik's validation approach differ from React Hook Form's?** Formik commonly stores controlled form state in React and supports field, form-level, or optional schema validation such as Yup. RHF commonly uses registered rules or resolver adapters with granular subscriptions; native browser validation is opt-in through `shouldUseNativeValidation`. Both libraries let you configure validation timing.
- **What are Formik's strengths compared to React Hook Form?** More intuitive for developers familiar with controlled components, excellent Yup integration, simpler mental model for basic forms, and better support for complex conditional logic in some cases.
- **How did Formik handle the transition to TypeScript?** Formik v2 included significant TypeScript improvements with better type inference for form values, field names, and error types, though it never achieved the same level of type safety as RHF v7+.
- **What is Formik's current status in the React ecosystem?** Formik is a mature library on the 2.4.x line and remains used in existing and new applications, although its release pace is slower than React Hook Form's. Choose based on rendering behavior, API fit, validation needs, bundle constraints, and team familiarity.

### React Router

#### Definition

React Router has been the standard routing solution for React applications since the early days of the ecosystem. Its evolution mirrors React's own journey from class components to hooks, and from simple SPA routing to full-stack application patterns.
#### Version Evolution Timeline

- v1-2 (2015-2016): Early versions with basic routing, mixin-based patterns
- v3 (2016): Stable API with configuration-based routing
- v4 (2017): Declarative routing revolution - components as routes
- v5 (2019): Stability release with hooks support
- v6 (2021): Modern rewrite with relative routing, nested routes, and Hooks
- v6.4+ (2022): Data routers, loaders, actions (Remix-inspired patterns)
- v7 (2024): Stable release with Declarative, Data, and Framework modes; Remix v2 path converges into React Router
- v7.5+ (2025): More granular Data Mode route.lazy support for loading route properties separately
- v7.9+ (2025): React Server Components support remains preview/unstable
- v8 (June 2026): ESM-only major requiring React 19.2.7+ and Node.js 22.22+, with `react-router-dom` removed; Framework Mode uses a Vite 7+ baseline
- v8.2.0 (July 2026): Current stable release; Declarative, Data, and Framework modes continue
#### Common Interview Questions

- **How did React Router's architecture change from v3 to v4?** v3 used a configuration-based approach with a central route config. v4 introduced a declarative, component-based approach where routes are components that render based on the current location, enabling more dynamic routing patterns.
- **What are the key differences between React Router v5 and v6?** v6 uses element prop instead of component, Routes instead of Switch, relative paths by default, required Outlet for nested routes, and new hooks like useNavigate instead of useHistory.
- **What are React Router v8's main modes?** Declarative Mode is the familiar SPA router with `<BrowserRouter>`, `<Routes>`, `<Link>`, and Hooks. Data Mode adds route objects, loaders, actions, pending UI, and `RouterProvider`. Framework Mode adds the Vite-powered full-stack layer with type generation, route modules, SSR/SSG options, and code splitting. These modes were introduced in v7 and continue in v8.
- **How do data routers (v6.4+ and v7 Data Mode) change the way we think about routing?** Data routers move routing configuration outside components, enable data loading before navigation, provide built-in form handling with actions, and support error boundaries at the route level - similar to Remix's approach.
- **What problem do loaders and actions solve in modern React Router?** Loaders coordinate route data with navigation and can start before route UI renders; actions integrate mutations and form submissions with revalidation. They reduce component-level fetching boilerplate, but applications still need pending UI through navigation state, Suspense, or hydrate fallbacks.
- **How has React Router's approach to code splitting evolved?** Early versions required manual code splitting. v6+ integrates with React.lazy and Suspense, Framework Mode handles route module splitting, and v7.5+ Data Mode supports granular `route.lazy` objects so loaders, actions, Components, and hydrate fallbacks can be loaded separately.
- **What's the difference between useNavigate and the old useHistory?** useNavigate returns a function that can be called with a path or numerical delta, while useHistory returned an object with methods like push, replace, and goBack. useNavigate is more concise and consistent.
- **How does React Router handle authentication and protected routes in modern versions?** Through loader functions that can redirect or throw errors for unauthorized access, combined with route-level error boundaries to handle authentication failures gracefully.
- **What is the status of React Router's RSC support?** React Router v8 retains preview/unstable React Server Components support. It is not the default stable routing model, and RSC-specific APIs may change outside normal semver guarantees.
- **What are the performance implications of React Router's evolution?** v6's relative paths and improved matching algorithm improved routing performance. v7 Data and Framework modes reduce waterfalls by loading route data before render, splitting route modules, and enabling stronger server integration patterns.

### React Query

#### Definition

TanStack Query (formerly React Query) revolutionized server state management in React applications by introducing sophisticated caching, background synchronization, and mutation handling. Its evolution reflects the React ecosystem's shift from client-state-centric solutions (like Redux) to specialized server-state management.
#### Version Evolution Timeline

- v1 (2019): Initial release with basic query caching and mutations
- v2 (2020): Improved TypeScript support, infinite queries, focus on DX
- v3 (2021): Major API overhaul - better defaults, infinite query improvements
- v4 (2022): Renamed to TanStack Query, framework-agnostic core
- v5 (2023): Modernized API - removed deprecated features, better error handling
- v5.40+ (2024): Pending-query dehydration improves framework streaming and Server Component integration; v5 remains the current major in July 2026
#### Common Interview Questions

- **How has TanStack Query's caching strategy evolved across versions?** Early releases accepted simple string or array keys; v4 made query keys arrays consistently. v5 refined cache timing, structural sharing, hydration, and TypeScript behavior while retaining hierarchical array keys and targeted invalidation.
- **What problem does TanStack Query solve that traditional useState/useEffect patterns struggle with?** It handles complex server-state concerns like caching, background updates, pagination, deduplication, error retries, and optimistic updates that are tedious to implement manually.
- **How has the mutation API evolved from v1 to v5?** Early releases had basic mutation callbacks, later versions added richer optimistic-update and rollback patterns, and v5 standardized object configuration and refined error handling. Suspense-specific Hooks are query APIs; mutations can opt into Error Boundary propagation with `throwOnError`.
- **What's the difference between useQuery and useSuspenseQuery in v5?** useQuery provides traditional loading states via isLoading/isError props. useSuspenseQuery throws promises for Suspense boundaries to catch, enabling more declarative loading states.
- **How does TanStack Query handle real-time data synchronization?** Through background refetching (window focus, network reconnect), staleTime configurations, and WebSocket integrations that update cache directly, ensuring UI consistency with server state.
- **What are the key differences between client state (Redux/Zustand) and server state (TanStack Query)?** Client state is owned by the application and may be synchronous, ephemeral, or optionally persisted. Server state is owned remotely and needs asynchronous fetching, caching, freshness, synchronization, and mutation handling.
- **How has TypeScript support evolved across versions?** v1 had basic TypeScript support. v3 significantly improved type inference. v5 provides excellent type safety with query key inference and mutation type propagation.
- **What performance optimizations does TanStack Query provide?** Request deduplication, smart background refetching, garbage collection, pagination/infinite query optimizations, and efficient cache updates through structural sharing.

### SSR & Hydration (Server-Side Rendering)

#### Definition

Server-Side Rendering has evolved from basic string rendering to sophisticated hybrid approaches that balance performance, SEO, and developer experience. React core now provides streaming SSR, selective hydration, Server Components integration points, and framework-facing prerender/resume APIs, while higher-level routing and rendering strategies are usually handled by frameworks.
#### SSR Evolution Timeline

- React 0.14 (2015): Basic ReactDOMServer.renderToString() introduced
- React 15-16 (2016-2017): ReactDOM.hydrate() replaces render() for SSR
- Next.js 1-8 (2016-2019): Framework-level SSR abstractions
- React 18 (2022): Streaming SSR with renderToPipeableStream()
- React 19.2 (2025): Resume/prerender APIs for partial prerendering and Node Web Streams support
- Next.js 13+ (2022): App Router, React Server Components, nested layouts, and streaming-first data boundaries
- Next.js 16 (2025-2026): Cache Components and Partial Prerendering as an opt-in framework model; 16.2.10 is the current stable patch in July 2026
- Modern Era (2023+): Streaming SSR, selective hydration, RSC, partial prerendering, and framework-managed client/server boundaries
#### Common Interview Questions

- **How has SSR evolved from basic string rendering to modern streaming approaches?** `renderToString` waits for a complete render and cannot progressively reveal Suspense content. React 16 also offered a basic Node stream, while React 18's Suspense-aware streaming can send a shell and progressively reveal boundaries as they resolve, often improving perceived performance.
- **What problem does React 18's streaming SSR solve?** It eliminates the "waterfall" problem where servers wait for all data before sending HTML. Now, the shell can be sent immediately while dynamic content streams in later.
- **What do the React 19.2 resume/prerender APIs enable?** They let frameworks pause a prerender and later resume it to a stream (`resume*`), which supports partial prerendering and more flexible streaming/hydration workflows on both Web Streams and Node streams. Most app components should treat these as framework/server integration APIs, not everyday client component APIs.
- **How do React Server Components change the SSR landscape?** Server Component module code does not ship to the browser, so moving suitable work to the server can reduce client JavaScript. The rendered tree may still contain Client Components and requires the framework's RSC payload/runtime; RSC also enables direct server data access when supported by the framework or bundler.
- **What security context matters for React Server Components?** RSC and Server Functions depend on framework/bundler integration. React 19.2.4 was the minimum fixed version in the January 2026 advisory, but applications should install the latest coordinated React/RSC packages—19.2.7 as of July 2026—because older React 19 RSC packages had critical security fixes.
- **What is cacheSignal in React Server Components?** It's an RSC-only signal that lets server-side cached work detect when a cache lifetime ends, helping frameworks coordinate cache invalidation and refresh behavior. It is not a client component state primitive.
- **What's the difference between hydration in React 16 vs React 18?** React 16 hydration was all-or-nothing. React 18 supports selective hydration, where interactive parts can hydrate independently from static content, improving perceived performance.
- **How does Next.js's App Router improve upon the Pages Router for SSR?** App Router uses React Server Components by default, enables nested layouts with individual loading states, supports parallel data fetching, and provides better error boundaries. Pages Router is still supported, but App Router is the modern path for React's latest server features.
- **What are hydration mismatches and how can they be prevented?** Mismatches occur when server-rendered HTML differs from client-rendered HTML. Prevention: Avoid browser-specific APIs during render, use useEffect for side effects, and ensure consistent data between server and client.
- **What React 19 DOM APIs matter for SSR performance?** React 19 improved document metadata handling, stylesheet/script coordination, and resource loading APIs such as `preload`, `preinit`, `preconnect`, and `prefetchDNS`. These help frameworks and apps coordinate critical resources without hand-writing head management in every route.
- **How do modern frameworks handle the transition from SSR to SPA-like navigation?** They use hybrid approaches: SSR or prerendering for initial load, then client-side navigation for subsequent page changes, providing both SEO benefits and smooth user experience.

### React Ecosystem Frameworks (Next.js, React Router/Remix)

#### Definition

The React ecosystem has evolved from library-only solutions to full-stack frameworks that provide opinionated, production-ready architectures. Next.js and React Router Framework Mode represent two common approaches to solving production concerns such as routing, data loading, rendering, deployment, performance, SEO, and developer experience.
#### Framework Evolution Timeline

- Pre-Framework Era (2015-2016): Manual React + Router + Webpack setups
- Next.js 1-3 (2016-2018): SSR-focused framework emerges
- Gatsby Era (2018-2020): SSG-focused alternative gains popularity
- Next.js 9-12 (2019-2021): API routes, incremental static regeneration
- Remix Launch (2021): Web standards-focused framework
- Next.js 13+ (2022): App Router, React Server Components
- React Router v7 (2024): Framework Mode absorbs Remix-style full-stack routing into React Router
- Next.js 16 (2025): Turbopack stable by default, React Compiler integration, Cache Components / PPR direction
- Modern Era (2023+): Full-stack React dominance, meta-frameworks, RSC-capable routing and rendering
#### Common Interview Questions

- **How has the React ecosystem evolved from library to framework approach?** Early React required assembling many pieces manually. Modern frameworks provide integrated solutions for routing, data fetching, rendering, and deployment, reducing configuration complexity.
- **What are the key philosophical differences between Next.js and Remix/React Router Framework Mode?** Next.js focuses on integrated full-stack React with App Router, RSC, caching, and deployment optimizations. Remix's ideas now live primarily in React Router v8 Framework Mode (introduced in v7), emphasizing web standards, progressive enhancement, nested routing, loaders, and actions.
- **How does the App Router in Next.js 13+ differ from the Pages Router?** App Router uses React Server Components by default, enables nested layouts, supports parallel data fetching, and provides more fine-grained loading states through the file system. Pages Router is still supported but is no longer the path for React's latest server features.
- **What problem do meta-frameworks solve that vanilla React doesn't?** They provide production-ready solutions for SSR/SSG, image optimization, font loading, performance monitoring, and deployment that would require significant manual configuration.
- **How has data fetching evolved across React frameworks?** From manual useEffect calls to getServerSideProps/getStaticProps in Next.js Pages Router, to Server Components and Server Actions in App Router, to loaders/actions in React Router/Remix.
- **What's the current framework direction with React Server Components?** RSC removes Server Component module code from the client bundle and enables server data access, but Client Components and the RSC transport/runtime still contribute client-side code. Adoption is framework-driven: Next.js App Router is a stable path, while React Router RSC support remains preview/unstable.
- **What changed in Next.js 16?** Next.js 16 made Turbopack the default stable bundler, added stable React Compiler integration, and introduced Cache Components / Partial Prerendering as an opt-in caching and rendering model enabled with `cacheComponents: true`.
- **When would you choose a meta-framework vs vanilla React + Vite?** Meta-frameworks for production applications needing SEO, performance, and complex features. Vanilla React + Vite for SPAs, internal tools, or when you need maximum control.
- **How do modern frameworks handle the trade-offs between developer experience and performance?** They use compiler optimizations, intelligent bundling, route-level data APIs, resource preloading, and runtime/server analysis to provide good DX without sacrificing performance.

### Axios & Data Fetching

#### Definition

The evolution of data fetching in React applications mirrors the broader JavaScript ecosystem's journey from XMLHttpRequest to modern fetch API and specialized HTTP clients. Axios emerged as a feature-rich alternative to native fetch, while recent trends show a move toward simpler, more integrated solutions.
#### Data Fetching Evolution Timeline

- jQuery Era (2010-2015): $.ajax() dominated frontend HTTP requests
- Native Fetch API (2015): Standards-based alternative to XMLHttpRequest
- Axios Rise (2016-2018): Feature-rich HTTP client gaining popularity
- React Query Era (2019+): Specialized server state management reduces Axios usage
- Modern Patterns (2022+): Framework-integrated fetching (Next.js, React Router/Remix), React Server Components
#### Common Interview Questions

- **How has data fetching evolved from jQuery AJAX to modern patterns?** jQuery provided a unified API but was heavy. Native fetch emerged as a standards-based solution. Axios added convenience features. Modern patterns integrate fetching with state management and framework routing.
- **What are the key differences between Axios and native fetch?** Axios includes interceptors, request/response transforms, configured timeouts, and rejects responses outside its `validateStatus` range. Native `fetch` is built into modern runtimes, supports cancellation through `AbortController` and timeouts through `AbortSignal.timeout()`, and resolves normally for HTTP 4xx/5xx responses, so callers must check `response.ok`.
- **How did AbortController change request cancellation in modern JavaScript?** AbortController provided a standards-based way to cancel requests, replacing library-specific solutions like Axios CancelToken. It works consistently across fetch and Axios.
- **What role does Axios play in modern React applications with tools like React Query?** Axios serves as the HTTP transport layer, while React Query handles caching, background updates, and state management. They complement each other rather than compete.
- **How do framework-integrated data fetching solutions (Next.js, React Router/Remix) change the game?** They move data fetching closer to routes or components, enable server-side rendering with data, provide built-in loading states, and reduce client-side JavaScript through Server Components where supported.
- **What are the performance implications of different data fetching strategies?** Client-side fetching can delay useful content and add JavaScript work that affects responsiveness, but it can make subsequent navigation flexible. Server-side fetching improves initial HTML and SEO but requires server resources. Hybrid approaches balance LCP, INP, cacheability, and freshness.
- **How has error handling evolved across data fetching approaches?** From jQuery's success/error callbacks to promise-based try/catch, to React Query's built-in error states, to framework-level error boundaries and Suspense fallbacks.
- **What's the current recommended approach for data fetching in new React projects?** Prefer the data APIs of the chosen framework or router: for example Next.js Server Components/Actions or React Router loaders/actions. In a client-heavy SPA, TanStack Query can manage remote cache state over `fetch`, Axios, or another transport. React recommends starting many new apps with a framework, but Vite remains appropriate for SPAs and custom setups; no single transport or framework is universal.

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
- **What's the evolution of custom hook testing practices?** Early testing required wrapper components, and older projects used a separate hooks-testing package. For React 18+ and modern Testing Library, import `renderHook` from `@testing-library/react`; the separate hooks package is no longer the default.
- **How do custom hooks work with React's rules of hooks?** Custom Hooks must call regular Hooks at the top level and only during React rendering. The linter recognizes custom Hooks by the `use` prefix. React 19's `use()` is a special exception that can be conditional or inside a loop, but it must still run in a component or Hook and cannot be wrapped in `try`/`catch`.
- **What's the difference between a custom hook and a regular utility function?** Custom hooks can use other hooks, have access to React's lifecycle, and can cause re-renders. Utility functions are pure and don't interact with React's internals.
- **How has TypeScript support for custom hooks evolved?** Early hook typing was basic. Modern TypeScript provides excellent inference for hook return types, generic constraints, and proper typing of complex hook patterns.
- **What are common pitfalls when creating custom hooks?** Forgetting dependency arrays, not handling cleanup, over-abstraction, creating hooks that are too specific, and not considering re-render optimization.
- **How do custom hooks integrate with React 18's concurrent features?** They can use useTransition, useDeferredValue, and other concurrent features to build hooks that are aware of React's rendering priorities and can avoid blocking the main thread.
- **What's the future of custom hooks with React Server Components?** Custom hooks that rely on browser APIs won't work in Server Components. The trend is toward separating client-side and server-side logic, with hooks specifically designed for each environment.

### Virtual DOM

#### Definition

The Virtual DOM is the in-memory element-tree representation React reconciles before committing changes to a host environment such as the browser DOM. React's Fiber reconciler and Scheduler—not the Virtual DOM representation by itself—enable interruptible rendering, priorities, and concurrent features.
#### Virtual DOM Evolution Timeline

- React 0.3 (2013): Initial VDOM implementation - basic tree diffing
- React 15 (2016): Stack Reconciler - recursive tree traversal
- React 16 (2017): Fiber Architecture - incremental rendering, time slicing
- React 18 (2022): Concurrent Features - interruptible rendering, priorities
- React Compiler v1.0 (2025): Stable production-ready build-time auto-memoization that reduces manual React.memo/useMemo/useCallback boilerplate
#### Common Interview Questions

- **How has the Virtual DOM's role evolved from React's early versions to today?** React has always used element trees to describe UI and reconciliation to compute updates. Fiber replaced the stack reconciler with units of work, while the Scheduler coordinates priorities; together they enable interruptible rendering and Suspense. The Virtual DOM itself is the representation being reconciled, not the scheduling system.
- **What problem did the Fiber architecture (React 16) solve that the Stack Reconciler couldn't?** Stack Reconciler used recursive traversal that couldn't be interrupted, causing jank on large updates. Fiber introduced incremental, interruptible rendering that can pause work for high-priority updates.
- **How do React 18's concurrent features change the VDOM reconciliation process?** Concurrent features introduce priority levels to updates. High-priority work (like user input) can interrupt low-priority work (like rendering offscreen content), making the UI more responsive.
- **How does React's Virtual DOM implementation differ from that of other frameworks, like Vue?** While both use a virtual DOM, the key difference is in the reconciliation algorithm. React's Fiber architecture, introduced in v16, enables features like concurrent rendering and interruptible updates. Vue's reactivity system is more fine-grained by default, allowing it to more precisely know which components need to re-render without a full tree comparison in many cases.
- **What's the significance of the key prop in VDOM diffing, and how has its importance evolved?** Keys help React identify which items have changed, been added, or removed. Their importance increased with dynamic lists and reordering capabilities. Modern React uses keys for more efficient reconciliation and state preservation.
- **What does React Compiler v1.0 change about performance optimization?** React Compiler is a stable build-time optimizer that automatically memoizes components and hooks when code follows the Rules of React. It supports incremental adoption and compiler-powered lint rules, but it does not remove the Virtual DOM or replace the need to understand rendering costs.
- **How do React 19.2 Performance Tracks help debugging?** They add React-specific Scheduler and Components tracks to Chrome DevTools performance profiles, making it easier to connect browser work with React rendering and scheduling behavior.
- **What's the difference between VDOM reconciliation and actual DOM manipulation performance?** VDOM reconciliation is JavaScript execution (generally fast). DOM manipulation is browser-native (potentially slow). The VDOM optimizes by minimizing expensive DOM operations through batching and diffing.
- **How does Suspense integrate with the VDOM and reconciliation process?** Suspense allows React to "pause" rendering of components that are waiting for data, showing fallback UI instead. The VDOM tracks which parts of the tree are suspended and resumes them when data is ready.

### Lazy Loading & Code Splitting

#### Definition

Code splitting has evolved from manual script management to sophisticated framework-integrated patterns that automatically optimize bundle delivery. This evolution reflects the JavaScript ecosystem's growing complexity and the need for performance optimization in modern web applications.
#### Code Splitting Evolution Timeline

- Early Web (2000s): Manual script tags with dependency management
- Require.js Era (2010-2014): AMD modules with runtime loading
- Webpack 1-2 (2014-2016): Static code splitting with require.ensure
- Webpack 3+ (2017): Dynamic import() syntax support
- React 16.6 (2018): React.lazy() and Suspense for components
- Modern Era (2020+): Framework-level splitting (Next.js, Vite), React Server Components
- React Router v7.5+ (2025): Granular `route.lazy` in Data Mode for lazy-loading route properties separately
#### Common Interview Questions

- **How has code splitting evolved from manual script tags to modern frameworks?** We've moved from manual dependency management to build-tool automation, then to framework-level optimizations, and now to hybrid server-client splitting strategies.
- **What's the difference between static imports and dynamic code splitting?** Static imports join the eager module graph and normally load with the entry or parent chunk. A dynamic `import()` creates an asynchronous split point that the bundler analyzes at build time and the runtime loads on demand, for example after navigation or interaction.
- **How does React.lazy() differ from direct dynamic imports?** React.lazy() wraps dynamic imports with React component semantics, integrates with Suspense for loading states, and works with React's reconciliation process.
- **What problems did Suspense solve for code splitting?** Suspense provides a declarative way to handle loading states, avoids callback hell, enables better error boundaries, and allows for more sophisticated loading patterns.
- **How do modern frameworks like Next.js change code splitting strategies?** Next.js automates route-based splitting, provides built-in loading states, enables hybrid rendering, and optimizes chunk delivery through framework intelligence.
- **What's the impact of React Server Components on code splitting?** Server Component module code is excluded from the client bundle, which shifts client code-splitting attention toward Client Components and third-party browser code. The application still sends an RSC payload/runtime and any Client Component bundles required by the rendered tree.
- **How does React Router v7.5+ improve route code splitting?** Data Mode can use a granular `route.lazy` object so route properties like `loader`, `action`, `Component`, and hydrate fallback code can load separately instead of waiting for one large lazy route module.
- **How do you decide what to code split in a modern React application?** Split routes, heavy components, below-the-fold content, features behind authentication, and third-party libraries. Use analytics to identify slow-loading components.
- **What are common pitfalls with code splitting and how have solutions evolved?** Early pitfalls: Flash of loading content, poor error handling. Modern solutions: Skeleton screens, error boundaries, preloading strategies, selective hydration, and framework-level route splitting.

### Functional Components

#### Definition

A function component is a JavaScript function that returns a React node, commonly written with JSX but also able to return strings, numbers, arrays, `null`, or values created with `createElement`. The evolution of function components mirrors React's shift from class-based to function-based architecture.
#### Version Evolution Timeline

- React 0.14 (2015): Functional components introduced as "stateless functional components" - could only receive props and return JSX, no state or lifecycle.
- React 16.8 (2019): Hooks introduced - function components gained state, Effects, context, refs, and reusable stateful logic
- React 18 (2022): Concurrent Features - functional components work seamlessly with concurrent rendering, transitions, and Suspense.
- React 19 (2024): Actions and Optimistic Updates - new hooks like useOptimistic and useActionState further enhance functional components.
#### Common Interview Questions

- **How has the role of function components evolved across React versions?** They progressed from simple presentational components to React's recommended model for most new code through Hooks and concurrent APIs. A few class-only capabilities remain, notably defining an Error Boundary and `getSnapshotBeforeUpdate`.
- **What capabilities were added to functional components in React 16.8 vs React 18 vs React 19?** 16.8: State (useState), effects (useEffect), context (useContext), refs (useRef)
  18: Concurrent features (useTransition, useDeferredValue), useId, useSyncExternalStore
  19: use hook, useOptimistic, useActionState, better Suspense integration
- **Why did React shift from class components to function components as the preferred approach?** Function components with Hooks avoid `this` binding and compose reusable stateful logic through custom Hooks. They are the recommended style for new code and align with modern React APIs, but they are not inherently faster or more tree-shakeable merely because they are functions.
- **Can function components handle all use cases that class components could?** They cover almost all everyday component work, but React still has no direct Hook equivalents for Error Boundaries or `getSnapshotBeforeUpdate`. Libraries that expose function-oriented Error Boundary APIs wrap a class boundary internally.
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
- **What performance concern can arise with controlled forms?** A controlled input schedules a React update for each edit. That is usually fine, but broad subscriptions or expensive parent rendering can make large forms slow. Uncontrolled inputs can keep the current DOM value outside React state and let a library notify only interested consumers.
- **How do modern form libraries like React Hook Form bridge the controlled/uncontrolled gap?** They favor uncontrolled native inputs and granular subscriptions while exposing declarative validation and form-state APIs. Components using `Controller`, `useWatch`, or subscribed `formState` can still rerender, so the benefit is reduced update scope rather than zero rendering.
- **What's the impact of React Server Components on form patterns?** Server Components enable forms that submit directly to server actions, reducing client-side JavaScript and blurring the line between controlled and uncontrolled approaches.
- **When are controlled components still preferable despite performance concerns?** For real-time validation, complex conditional logic, immediate UI feedback, and when integrating with state management systems that require full control over form state.
- **How do React 19's new hooks like useActionState change form handling?** They provide built-in support for server actions with loading states and error handling, making it easier to build forms that work well with both client and server rendering.
- **What are the accessibility considerations for each approach?** Accessibility is largely orthogonal to where the value is stored. Both approaches need native labels, instructions, error association, focus management and appropriate live announcements. Controlled state can make immediate derived feedback convenient, while uncontrolled fields retain native browser behavior by default.
- **How should developers choose between approaches?** Choose controlled state when the UI must derive behavior from every edit and the update cost is acceptable. Choose uncontrolled or subscription-based state when native form behavior and narrower updates fit better. Hybrid forms are common; profile large forms instead of deciding from ideology.

### Class Components

#### Definition

A class component is an ES6 class that extends `React.Component` (or `React.PureComponent`) and implements a `render()` method returning a React node. Before React 16.8, classes were the built-in component model for local state and lifecycle methods; refs also existed for other component patterns, while Error Boundaries remain class-based in React 19.
#### Evolution Context

- React 0.13 (2015): Class components introduced as primary component type
- React 16.8 (2019): Hooks made function components the recommended model for most stateful component work
- React 18+ (2022+): Functional components become recommended approach
- Current Status: Class components remain supported but are not recommended for new code; they are common in existing codebases and still provide Error Boundaries and `getSnapshotBeforeUpdate`
#### Modern Usage Perspective

- While class components are no longer the recommended approach for new development, understanding them remains valuable for:
- Maintaining legacy codebases (many enterprise applications still use class components)
#### Understanding React's evolution and architectural decisions

Interview contexts where knowledge of lifecycle methods and historical patterns is tested
#### Common Interview Questions

- **When would you choose class components over functional components today?** Primarily for maintaining existing codebases or in rare cases where error boundaries are needed without external libraries. For new development, functional components with hooks are recommended.
- **What are the key differences in lifecycle management between class and function components?** Classes split work across methods such as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`. Function components model independent synchronization processes with Effects; cleanup can run before dependency-driven re-synchronization as well as on unmount, so Effects are not one-to-one lifecycle translations.
- **Why did the React team shift towards function components with Hooks?** They provide direct composition through custom Hooks, avoid `this` binding, and are the model used by modern React APIs. Runtime performance still depends on component work, state placement, and rendering behavior rather than component syntax alone.
- **Can function components completely replace class components?** They replace classes for most application components. Native Error Boundaries and `getSnapshotBeforeUpdate` still require a class; libraries such as `react-error-boundary` provide a function-oriented interface over a class implementation.
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

Vue 3 represents a significant evolution from Vue 2, introducing a new, flexible paradigm for organizing component logic while preserving familiar concepts instead of promising compatibility without breaking changes. It includes breaking changes from Vue 2, with official migration paths and a migration build for incremental upgrades. The most transformative addition is the Composition API, a set of additive, function-based APIs that allow logic to be defined and reused more effectively than the Options API of Vue 2. Vue 2 reached end of life on 2023-12-31, so Vue 3 is the current baseline for new applications. As of July 2026, Vue 3.5.39 is the latest stable release; Vue 3.6 is still a beta line.
#### Common Interview Questions

- **What are the primary advantages of Vue 3 over Vue 2?** Key advantages include the Composition API for logic reuse, Proxy-based reactive objects plus ref primitives, a more optimized renderer, first-class TypeScript support, multiple isolated app instances, and a tree-shakable core. A complete application's size and performance still depend on its components, router, store, and other dependencies.
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

- **What is the key difference between creating a Vue 2 instance and a Vue 3 application?** Vue 2 creates root instances with `new Vue()`, but constructor-level configuration APIs can mutate shared global state. Vue 3 uses `const app = createApp(App)` and scopes plugins, components, directives, and configuration to each application instance, so multiple apps coexist more cleanly.
- **Explain the purpose of the mount method in both Vue 2 and Vue 3.** Mounting attaches the Vue root to a DOM element and starts rendering. Vue 2 uses `new Vue({...}).$mount('#app')` or the `el` option; Vue 3 uses `createApp(App).mount('#app')`.
- **What are the main lifecycle phases of a Vue component?** The main phases are: Creation (initial setup), Mounting (DOM insertion), Updating (re-rendering due to data changes), and Destruction (teardown and cleanup).
- **Name the most commonly used lifecycle hooks and their execution order.** The core hooks in execution order are: beforeCreate, created, beforeMount, mounted, beforeUpdate, updated, beforeUnmount (Vue 3) / beforeDestroy (Vue 2), and unmounted (Vue 3) / destroyed (Vue 2).
- **In which lifecycle hook are component data and events initialized?** Data observation and events are initialized at the end of the beforeCreate hook and are fully available within the created hook.
- **Why is the mounted hook useful for DOM-dependent operations?** `mounted`/`onMounted` runs after the component's initial DOM tree is created. Use `nextTick` or `onUpdated` when access must reflect later reactive updates, and remember that mounted only guarantees the DOM is in-document when the application's root container is in-document.
- **What is the difference between beforeUnmount and unmounted?** `beforeUnmount` runs while the instance is still functional; `unmounted`/`onUnmounted` runs after its reactive effects and child tree have stopped. Cleanup may be registered in either as appropriate, with `onUnmounted` commonly used to dispose timers, listeners, and subscriptions.
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

Reactivity is the core feature of Vue that automatically updates the DOM when application state changes. Vue 3 uses Proxies for objects returned by `reactive()` and getter/setter access for refs. The primary Composition API primitives are `ref()` and `reactive()`. Vue 3.4 improved reactivity performance, while stable Vue 3.5 added features such as reactive props destructuring, numeric watcher depth, `onWatcherCleanup`, `useTemplateRef`, lazy hydration, `data-allow-mismatch`, and deferred Teleport.
#### Common Interview Questions

- **What is the fundamental difference between ref() and reactive()?** ref() is used for creating a reactive reference to a single value (primitive or object) and requires accessing the value via the .value property in JavaScript. reactive() is used for creating a reactive object and provides direct property access without the need for .value.
- **Why does ref() exist when we have reactive()?** ref() is necessary because Vue's reactivity system relies on object properties. Primitive values (like strings, numbers, booleans) cannot be tracked as objects. ref() wraps the primitive in an object, allowing it to be reactive, whereas reactive() only works with objects.
- **How does Vue 3's Proxy-based reactivity differ from Vue 2's Object.defineProperty approach?** Vue 2's Object.defineProperty could only track property access and mutation, requiring workarounds for adding new properties (Vue.set). Vue 3's Proxies can track property addition, deletion, and array index mutation natively, providing more comprehensive reactivity with better performance.
- **When should you use ref() over reactive(), and vice versa?** Use ref() for: primitive values, when you need to replace the entire object reference, or when working with template refs. Use reactive() for: groups of data that logically belong together and you don't intend to replace the entire object reference.
- **What is the limitation of destructuring a reactive() object?** Destructuring disconnects a local variable from the source property's get/set tracking, so assigning that variable will not update the source and primitive values no longer trigger through the original property. A destructured nested object can still be a reactive proxy. Use `toRef()`/`toRefs()` when the local binding must stay linked to source properties.
- **Explain the purpose of the toRef() and toRefs() utility functions.** toRef() creates a ref for a specific property on a reactive source object. toRefs() converts a reactive object into a plain object where each property is a ref pointing to the corresponding property of the original object. This is essential for maintaining reactivity when destructuring or returning reactive state from composables.
- **Why might you see .value in your JavaScript code but not in your templates?** Vue unwraps top-level refs exposed to a template, so `{{ count }}` normally replaces `{{ count.value }}`. JavaScript still uses `.value`. Refs inside reactive arrays or native collections such as `Map` are not automatically unwrapped, and template unwrapping has additional nested-expression caveats.
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
- **When should you use the immediate and deep options with watchers?** Use `immediate: true` to run the callback during watcher creation. Watching a reactive object directly is implicitly deep. A getter that returns an object is shallow unless `{ deep: true }` is supplied; in Vue 3.5+, `deep` may also be a number that limits traversal depth.
- **What is a common pitfall when watching objects?** `watch(reactiveObject, callback)` reacts to nested mutations, but the old and new callback values are the same object reference for such mutations. A getter such as `watch(() => state.someObject, callback)` reacts to replacement by default, not nested mutation, unless `deep` is enabled.

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
- **How do you access the native DOM event object in an event handler?** A method handler such as `@click="handleClick"` automatically receives the native event as its first argument. In an inline expression, use the special `$event` value, for example `@click="handleClick('save', $event)"`.
- **What are event modifiers and why are they useful?** Event modifiers are postfixes denoted by a dot that indicate the directive should be called in a special way. Common modifiers include .stop (stops propagation), .prevent (prevents default action), and .self (only trigger if event.target is the element itself). They keep event handling logic declarative in templates.
- **How does v-model work under the hood for different form input types?** v-model is syntactic sugar that combines v-bind:value with v-on:input (for text inputs) or v-on:change (for checkboxes/radio buttons). It automatically handles the appropriate property and event based on the input element type.
- **What are the available modifiers for the v-model directive?** Common v-model modifiers include: .lazy (syncs after change events instead of input), .number (casts input to number), and .trim (trims whitespace from input).
- **How do you handle form submissions in Vue?** Use @submit.prevent="handleSubmit" on the form element. The .prevent modifier stops the default form submission, and the handler method can process the form data.
- **What's the difference between using v-model and manually binding :value with @input?** For a simple text input they can express the same data flow, but `v-model` selects the correct property/event for text, checkbox, radio, and select controls, handles IME composition details, supports modifiers, and maps to component model props/events. Manual bindings are useful when custom transformation or timing is required.
- **How do you handle multiple checkboxes with the same v-model?** When multiple checkboxes share the same v-model bound to an array, Vue automatically manages the array to include the values of checked boxes. Each checkbox should have a unique value attribute.
- **What is the proper way to implement custom form components with v-model?** In Vue 3, custom components can implement v-model by expecting a modelValue prop and emitting an update:modelValue event. For multiple v-model bindings, use specific prop names like v-model:title and emit update:title.
### Component Fundamentals: Props & Custom Events

#### Definition

Components are the building blocks of Vue applications, allowing you to split the UI into independent, reusable pieces. Props are custom attributes you can register on a component to pass data from a parent component down to a child component. Custom Events enable child components to communicate back to parent components by emitting events that can carry payload data.
#### Common Interview Questions

- **What are the two main ways to define props in Vue components?** Props can be defined as an array of strings (props: ['title', 'content']) for simple declaration, or as an object (props: { title: String, count: Number }) for type checking and validation.
- **What is the one-way data flow principle with props and why is it important?** Props follow a one-way data flow from parent to child. This prevents child components from accidentally mutating the parent's state, making the data flow easier to understand and debug. If a child needs to modify data, it should emit an event to request the change from the parent.
- **How do you validate props in Vue components?** When defining props as an object, you can specify validation requirements including type checking, required flag, default values, and custom validator functions to ensure props meet specific criteria.
- **What is the difference between kebab-case and camelCase for prop names?** JavaScript prop declarations use camelCase. Kebab-case attributes are required when writing directly in an in-DOM HTML template because HTML lowercases names; Vue Single-File Component templates also accept camelCase/PascalCase, although kebab-case props remain a common convention.
- **How do custom events enable child-to-parent communication?** Child components use the $emit method to trigger custom events (this.$emit('update', newValue)). Parent components listen to these events using v-on or @ (`<child-component @update="handleUpdate">`).
- **What are the best practices for naming custom events?** Define and emit multi-word events in camelCase in JavaScript and listen with kebab-case in templates, which Vue maps automatically. The HTML case-insensitivity caveat specifically matters for in-DOM templates.
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
   - Note: Pinia is Vue's recommended state-management library; getters provide derived state and actions encapsulate business logic and asynchronous work.

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
- **Why is the mounted hook useful for DOM-dependent operations?** It runs after the initial component DOM is created, so initial element refs are available. For DOM that must reflect later reactive changes, use `nextTick` or `onUpdated`; mounted is not the only valid DOM-access point.
- **What is the key difference between beforeCreate and created hooks?** In beforeCreate, the component instance is created but data observation, computed properties, and methods are not set up. In created, the instance is fully configured with reactive data, computed properties, methods, and watchers.
- **How do you handle cleanup logic in the Composition API versus Options API?** Options API components use `beforeUnmount` or `unmounted`. Composition API code registers cleanup with `onUnmounted`; Vue ignores a value returned from `onMounted`. Watchers instead support their callback's `onCleanup` argument and Vue 3.5's `onWatcherCleanup`.
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
- **Why can destructuring a reactive() object be problematic and how do you solve it?** A destructured local binding is no longer linked to the source property's get/set trap, although a nested object value can still itself be reactive. Use `toRef()` for one property or `toRefs()` for the properties of a reactive object when those bindings must remain linked.
- **What is the role of the context parameter in the setup() function?** The context parameter provides access to component features not available as props: attrs (fallthrough attributes), slots (slot content), and emit (function to emit custom events).
- **How do you access component instance properties like $emit in the Composition API?** In the Composition API, you don't use this. Instead, you access emit through the context parameter in setup(): setup(props, { emit }) { emit('event') }.
- **What is the mental model shift from Options API to Composition API?** The shift moves from organizing code by options type (data, methods, computed) to organizing code by feature/concern, colocating all logic related to a specific feature within the same scope in the setup() function.
### Compiler Macros (defineProps, defineEmits, defineModel in `<script setup>`)

#### Definition

Compiler macros are special functions that are processed at compile time rather than runtime when using Vue's `<script setup>` syntax. They provide a declarative way to define component props, emits, and model bindings directly within the script setup block. These macros are stripped away during compilation and replaced with appropriate runtime code, offering better TypeScript integration and more concise component definition compared to the Options API.
#### Common Interview Questions

- **What are compiler macros and how do they differ from regular JavaScript functions?** Compiler macros are special functions that are processed and removed during Vue's compilation process, unlike regular functions that execute at runtime. They provide hints to the Vue compiler about component structure and are replaced with actual JavaScript code in the final output.
- **What is the purpose of defineProps and how does it compare to the Options API props definition?** defineProps is used to declare component props within `<script setup>`. It's more concise than the Options API and provides better TypeScript support. Unlike the Options API where props are defined in a separate object, defineProps is called directly in the setup context and returns a reactive props object.
- **How do you define prop types and defaults using defineProps?** You can pass a type-based definition with `defineProps<{ title: string; count?: number }>()` or a runtime declaration with `defineProps({ title: { type: String, required: true }, count: { type: Number, default: 0 } })`. In Vue 3.5+, reactive props destructuring supports JavaScript defaults, for example `const { count = 0 } = defineProps<Props>()`; `withDefaults` remains useful when retaining a props object.
- **What is the role of defineEmits and how do you type emitted events?** defineEmits declares the events a component can emit. With TypeScript, you can use a type-based declaration: `defineEmits<{ (e: 'update', value: string): void; (e: 'submit'): void }>()`. This provides full type checking for event names and payloads.
- **What does defineModel do and when would you use it?** defineModel creates a typed `v-model` binding in `<script setup>` with optional defaults and modifiers. Use it when you want a concise, type-safe two-way binding API for a component.
- **How do you access the props and emits defined with macros in your component logic?** defineProps() returns a reactive object containing the props, which you can use directly in your template and logic. defineEmits() returns an emit function that you can call to trigger events: const emit = defineEmits(); emit('update', value).
- **What are the advantages of using compiler macros over the Options API?** They reduce boilerplate, colocate declarations with setup logic, and provide strong TypeScript and IDE inference. They are compile-time syntax rather than inherently faster or more tree-shakeable than an equivalent Options API component.
- **Can you use defineProps and defineEmits with both TypeScript and JavaScript?** Yes, both work with JavaScript and TypeScript. Type-based declarations improve checking and autocompletion, and their TypeScript syntax is erased, but the Vue compiler still generates runtime props/emits metadata where it can; they are not literally free of runtime declarations.
- **What is the withDefaults helper and when would you use it?** `withDefaults(defineProps<Props>(), defaults)` adds defaults to a type-based props object. Since Vue 3.5, destructured props can instead use native defaults; use `withDefaults` when code needs to keep and access the complete props object.
- **How are compiler macros emitted?** The macro calls themselves are compiled away, but Vue still generates the runtime component declarations needed for props, emits, and models. Type-based props become equivalent runtime declarations where possible and use a compact production form.
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
- **Can composables accept parameters and how should they be normalized?** Yes. A composable can accept a plain value, ref, or getter and normalize it with `toValue()`; `toRef()` can similarly normalize a value/ref/getter to a ref. `toRefs()` is specifically for turning the properties of a reactive object into linked refs.
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
- **How do you create a template ref in Vue 3's Composition API?** In Vue 3.5+, prefer `const input = useTemplateRef('input')` with `<input ref="input">`; the language tools can infer the element type. The pre-3.5 pattern `const input = ref(null)` with a matching template name remains supported.
- **What is the timing consideration when accessing template refs?** Template refs are only populated after the component has mounted. Accessing them in setup() or created() lifecycle hooks will return null because the DOM hasn't been rendered yet. Use onMounted() to ensure refs are available.
- **How do template refs work with v-for directives?** When using ref inside v-for, the ref value becomes an array containing all the DOM elements or component instances. However, this array order is not guaranteed to match the data array order.
- **What is the difference between using refs on DOM elements versus component instances?** A DOM ref contains the element. A component ref exposes its public instance, not emitted events as readable members. Options API components expose their public instance unless limited with `expose`; `<script setup>` components are private by default and expose only values declared with `defineExpose`.
- **How can you create multiple refs for elements in a list without using v-for?** You can use a function ref that gets called for each element: :ref="(el) => { if (el) itemsRefs.push(el) }". This gives you more control over how refs are collected.
- **What are the best practices for using template refs?** Use template refs sparingly as they break declarative patterns. Prefer declarative solutions when possible. Always check if the ref exists before using it, and clean up any event listeners or side effects in onUnmounted().
- **How do you access a child component's methods or data using refs?** Attach a ref to the child and access only its public surface, for example `childRef.value?.someMethod()`. A `<script setup>` child must explicitly publish that method with `defineExpose({ someMethod })`. Prefer props/events because component refs create tighter coupling.
- **What is the TypeScript consideration when working with template refs?** Vue 3.5 and `@vue/language-tools` can often infer `useTemplateRef()` types. Explicit types remain useful for ambiguous or dynamic refs, for example `useTemplateRef<HTMLInputElement>('input')` or a component's exposed public type.
### Built-in Components: `<KeepAlive>`, `<Teleport>`, `<Suspense>`

#### Definition

Vue provides several built-in components that solve common architectural patterns in web applications. `<KeepAlive>` enables state preservation for dynamic component switching, `<Teleport>` allows rendering content in different parts of the DOM tree, and `<Suspense>` provides a way to handle async component dependencies with fallback states.
#### Common Interview Questions

- **What problem does the `<KeepAlive>` component solve?** `<KeepAlive>` caches inactive dynamic component instances so switching does not unmount and recreate them. It preserves local state, but an active cached component can still update and render when its dependencies change.
- **How do you control which components get cached by `<KeepAlive>`?** Use the include and exclude props to specify which component names should or shouldn't be cached. You can use strings, regex patterns, or arrays to define the caching strategy.
- **What lifecycle hooks are triggered specifically for `<KeepAlive>` components?** Cached components trigger activated when inserted into the DOM and deactivated when removed from the DOM but kept in cache. These hooks help manage component state during cache transitions.
- **What is the primary use case for the `<Teleport>` component?** `<Teleport>` allows rendering a component's content in a different part of the DOM tree while maintaining its logical position in the Vue component hierarchy. This is essential for modals, tooltips, notifications, and other overlay elements.
- **How does `<Teleport>` maintain component context while rendering elsewhere?** Although the content renders in a different DOM location, the component remains part of the original parent's component tree, preserving props, event listeners, and injection context.
- **Can you conditionally disable `<Teleport>` and how?** Yes, use the disabled prop to conditionally disable teleportation. When disabled is true, the content renders in its original position instead of the target location.
- **What did Vue 3.5 add for deferred Teleport targets?** The `defer` prop postpones target resolution until the rest of the same mount/update tick, allowing a target rendered later in Vue's tree to be used. The target must still appear within that same tick.
- **What problem does `<Suspense>` solve in Vue applications?** `<Suspense>` provides a declarative way to handle async component dependencies and display fallback content while waiting for async operations to complete, simplifying loading state management.
- **What are the two types of async dependencies that `<Suspense>` can wait for?** `<Suspense>` can wait for: 1) Components with async setup() functions, and 2) Components with Async Components (dynamic imports with loading states).
- **What are the limitations of `<Suspense>` in its current implementation?** `<Suspense>` is still considered experimental in Vue 3 and has edge cases around nesting and SSR integration. Treat it as a component-level async dependency tool that should be tested in the rendering mode you use, not as a general app-level data fetching primitive.

### Error Handling with errorCaptured Hook

#### Definition

The errorCaptured hook is a lifecycle hook that allows a component to catch errors from its child components (both direct and nested descendants). Unlike React's Error Boundaries, which are component-based, errorCaptured is a hook that can be added to any component, providing a more flexible but manual approach to error handling. When an error is captured, the component can handle it, log it, and optionally prevent it from propagating further up the component tree.
#### Common Interview Questions

- **What is the errorCaptured hook and how does it differ from React's Error Boundaries?** errorCaptured is a lifecycle hook that any component can implement to catch errors from its child components. Unlike React's class-based Error Boundaries, Vue's approach is hook-based and doesn't require a special component type, offering more flexibility but requiring manual implementation.
- **What parameters does the errorCaptured hook receive?** The hook receives three parameters: (error, instance, info). error is the Error object, instance is the component instance that triggered the error, and info is a string containing information about where the error was captured (e.g., 'in render function').
- **What is the return value significance in errorCaptured?** If errorCaptured returns false, it prevents the error from propagating further up the parent chain. This allows a component to handle errors locally without affecting higher-level components.
- **What types of errors does errorCaptured catch versus what it misses?** It captures descendant errors from renders, lifecycle hooks, setup, watchers, directives, transitions, and Vue-managed event handlers. Vue also observes rejected Promises returned by supported handlers. Detached work such as an unreturned Promise or `setTimeout` callback is outside that chain, and a component does not capture an error thrown by itself.
- **How would you implement a global error handler in a Vue application?** Configure `app.config.errorHandler` for uncaught Vue application errors and production reporting. Use `onErrorCaptured` for subtree fallback behavior; browser-level `error` and `unhandledrejection` listeners can supplement it for failures outside Vue's managed call chain.
- **What's the execution order when multiple components in the hierarchy have errorCaptured hooks?** The hooks are triggered in the propagation order - from the component closest to the error source up to the root. Each component's errorCaptured hook is called in sequence until one returns false to stop propagation.
- **How do you handle async errors outside Vue's error pipeline?** Return or await Promises from Vue-managed handlers when possible. Use `try`/`catch` around detached asynchronous work and report intentionally handled failures; a browser `unhandledrejection` listener is a final safety net, not the primary component error strategy.
- **What are the best practices for using errorCaptured in production applications?** Use it strategically for critical component trees, always log errors to a monitoring service (like Sentry), provide graceful fallback UIs, and avoid overusing it as it can make error tracking more complex.
- **Can errorCaptured be used in both Options API and Composition API?** Yes, in Options API it's defined as errorCaptured() {}, and in Composition API it's used via onErrorCaptured() from the vue package.

### Render Functions & JSX in Vue

#### Definition

Render functions provide an alternative to templates by allowing you to programmatically create your component's VNode tree using JavaScript. Instead of using HTML-based templates, you use JavaScript's full programming power to describe the component's render output. JSX is a syntax extension that can be used with Vue's render functions, offering a more familiar, XML-like syntax similar to React's JSX. This approach is useful for highly dynamic components where the template structure needs to be determined programmatically. As of Vue 3.4, the global JSX namespace is no longer registered; set `jsxImportSource: "vue"` or import `vue/jsx` for TSX support.
#### Common Interview Questions

- **When would you choose render functions over templates in Vue?** Render functions are preferable when you need full programmatic control over the component's output, such as when creating highly dynamic components where the template structure is determined at runtime, when building component libraries that require maximum flexibility, or when working with complex functional components that need optimization beyond what templates can provide.
- **How does Vue's h() function compare to React's createElement?** Both create virtual nodes from a type, props, and children. Vue components receive slots as functions in the children position. Custom directives are not a direct `h()` argument: apply them with `withDirectives(h(...), [[directive, value]])`, optionally resolving a registered directive with `resolveDirective`.
- **What are the advantages and disadvantages of using JSX with Vue?** Advantages include full JavaScript expressiveness, strong TypeScript tooling, and familiar syntax for React developers. Tradeoffs include required JSX tooling, different handling for template conveniences such as directives and `v-model`, and fewer template-specific compile-time optimizations. `<style scoped>` still works when JSX/render functions are used inside a Vue SFC.
- **How do you handle conditional rendering and loops in render functions?** Use standard JavaScript constructs: ternary operators or && for conditionals (`condition ? h('div') : null`), and array.map() for loops (`items.map(item => h('li', item))`). This provides more flexibility than template directives but requires more manual code.
- **What is the equivalent of template directives (v-if, v-for) in render functions?** There are no direct equivalents - you use JavaScript instead. v-if becomes ternary operators or logical AND, v-for becomes array.map(), v-model becomes manual value binding and event handling, and v-bind becomes object property assignment.
- **How do you handle slots in render functions?** Access slots via the slots object in the render function's context parameter. Use slots.default() for default slot content and slots.name() for named slots. You can also pass data to scoped slots by providing arguments to the slot function calls.
- **What are the performance implications of using render functions vs templates?** Templates are generally faster for most use cases because they can be statically analyzed and optimized by Vue's compiler. Render functions have runtime overhead but can be more efficient for highly dynamic scenarios where the template compiler's optimizations don't apply.
- **How do you use JSX with Vue's Composition API?** You can return JSX directly from the setup() function, or use the `<script setup>` syntax with a JSX/TSX file. The Composition API's reactive variables and functions work seamlessly with JSX, making it a natural pairing for developers coming from React hooks.
- **What is the difference between functional components and regular components in render functions?** A functional component is a plain render function that receives props and context and has no component instance or lifecycle. In Vue 3 its performance advantage over a stateful component is negligible, so use it for semantic simplicity rather than assuming it is faster.

### Custom Directives

#### Definition

Custom directives are a mechanism for creating reusable low-level DOM behavior abstractions in Vue. Similar to Angular's attribute directives, they allow you to attach custom behavior to DOM elements and can be used across components. Directives are primarily intended for direct DOM manipulation and are useful for creating reusable DOM interaction patterns that don't fit the component model, such as focus management, click-outside detection, tooltips, or integration with third-party DOM libraries.
#### Common Interview Questions

- **What are the different hook functions available in a custom directive?** Custom directives have several lifecycle hooks: created (called before the element's attributes or event listeners are applied), beforeMount (called once when the directive is first bound to the element), mounted (called when the bound element's parent component is mounted), beforeUpdate (called before the containing component's VNode is updated), updated (called after the containing component's VNode and the VNodes of its children have updated), beforeUnmount (called before the bound element's parent component is unmounted), and unmounted (called only once when the directive is unbound from the element).
- **How do you pass values and arguments to custom directives?** Values are passed through the directive's value (v-my-directive="value"), arguments are specified after a colon (v-my-directive:argument), and modifiers are specified as dots (v-my-directive.modifier). These are accessible in the directive hooks via the binding parameter as binding.value, binding.arg, and binding.modifiers.
- **What is the difference between custom directives and components?** Components are for creating reusable UI pieces with their own template and logic, while directives are for low-level DOM manipulation on existing elements. Use components when you need to create new UI elements; use directives when you need to add behavior to existing DOM elements.
- **When should you use a custom directive versus a composable?** Use custom directives when you need to directly manipulate the DOM element itself (focus, scroll, animations) or integrate with DOM-based libraries. Use composables for reactive logic and state management that doesn't require direct DOM access. Directives are element-centric, while composables are logic-centric.
- **How do you create global vs local custom directives?** Global directives registered with `app.directive('name', definition)` are available throughout that app. A directive in a component's `directives` option—or a `vName` variable in `<script setup>`—is available only in that component's own template, not automatically in descendant templates.
- **What are some common real-world use cases for custom directives?** Common use cases include: click-outside detection (v-click-outside), focus management (v-focus), infinite scroll (v-infinite-scroll), permission checks (v-permission), tooltip/popover triggers (v-tooltip), and lazy loading images (v-lazy).
- **How do you handle cleanup in custom directives?** Cleanup should be performed in the beforeUnmount or unmounted hooks. This includes removing event listeners, clearing timeouts/intervals, and disconnecting observers. Any resources allocated in mounted should be released in the unmount hooks.
- **Can custom directives be used with Vue's reactivity system?** Yes, directives can react to changes in their bound value. When the value changes, the beforeUpdate and updated hooks are triggered, allowing the directive to respond to reactive changes and update the DOM behavior accordingly.
- **What is the difference between a directive's value and oldValue parameters?** In the beforeUpdate and updated hooks, the binding parameter contains both value (the current value) and oldValue (the previous value). This allows you to compare changes and only update the DOM when necessary for performance optimization.

### Plugins

#### Definition

Plugins in Vue are a way to package and distribute reusable functionality that can be easily added to Vue applications. They are the primary mechanism for adding global-level features to Vue, such as components, directives, mixins, configuration options, or instance methods. Plugins allow you to extend Vue's core capabilities and provide a clean, standardized way to integrate third-party libraries and share functionality across multiple Vue applications.
#### Common Interview Questions

- **What is the purpose of Vue plugins and when would you create one?** Plugins add application-level functionality such as shared providers, configuration, components, directives, or third-party integrations. In Vue 3, publish app-wide properties through `app.config.globalProperties` when necessary, or prefer `app.provide()` and composables over the Vue 2 `Vue.prototype` pattern.
- **What is the basic structure of a Vue plugin?** A Vue plugin is either a function or an object with an install method. The function receives the Vue app instance and optional options as parameters: const myPlugin = { install(app, options) { // plugin logic } } or const myPlugin = (app, options) => { // plugin logic }.
- **How do you register a plugin in a Vue application?** Plugins are registered using the app's use() method: app.use(myPlugin, options). The options parameter is optional and gets passed to the plugin's install function. Plugins must be registered before the application is mounted.
- **What are common use cases for creating custom plugins?** Common use cases include: adding global components/directives, integrating HTTP clients (Axios), adding state management, integrating UI libraries, adding authentication utilities, providing internationalization (i18n), and creating utility libraries that need app-wide access.
- **How do plugins differ from composables in terms of scope and usage?** Plugins are app-level and modify the Vue application instance globally, while composables are component-level and provide reusable logic that components opt into. Plugins are setup once per application, while composables can be used multiple times with isolated state.
- **What is the difference between a plugin and a mixin?** Plugins are registered at the application level and can add global features, while mixins are component-level and merge logic into individual components. Plugins are more suitable for library authors and app-wide extensions, while mixins are for sharing component logic within an application.
- **How do you handle configuration options in a plugin?** Configuration options are passed as the second parameter to app.use(plugin, options) and are received in the plugin's install function. These options should be validated and used to customize the plugin's behavior. Default options can be merged with user-provided options.
- **Can plugins be asynchronous and how do you handle async initialization?** `app.use()` always returns the application instance and does not await a Promise returned by `install`. Complete required asynchronous initialization explicitly before `app.mount()`, or let the plugin expose a separate readiness Promise that application bootstrap awaits.
- **What are the best practices for writing maintainable plugins?** Best practices include: providing clear documentation, using TypeScript for better type safety, validating configuration options, handling errors gracefully, avoiding side effects in the plugin's top-level scope, making plugins tree-shakable when possible, and following semantic versioning for releases.
- **How do you test Vue plugins?** Plugins can be tested by creating a test Vue application, registering the plugin, and verifying that the expected functionality is added. Use testing utilities like Vue Test Utils to mount components that use the plugin's features and assert the expected behavior.
### State Management: Pinia (Modern) vs Vuex (Legacy)

#### Definition

State management in Vue provides stores for application-level state shared across components. Vuex 3 supports Vue 2 and Vuex 4 supports Vue 3; both use a Flux-inspired mutation/action model. Pinia is Vue's current default recommendation for new Vue 3 applications, offering a smaller API, strong TypeScript inference, and Composition API integration. Existing Vuex applications do not need to migrate solely because Pinia is the default.
#### Common Interview Questions

- **What are the key differences between Pinia and Vuex?** Pinia has a simpler API with less boilerplate, no mutations (only actions and state), excellent TypeScript support with full type inference, and better Composition API integration. Vuex requires more verbose code with mutations, getters, and actions.
- **Why was Pinia created as a new solution instead of evolving Vuex?** Pinia was designed to address Vuex's complexity, boilerplate, and TypeScript limitations. It provides a more intuitive API that aligns with Vue 3's Composition API while solving the same state management problems more elegantly.
- **How does Pinia handle state changes compared to Vuex?** Pinia allows direct state mutation (store.count++) in addition to actions, while Vuex requires committing mutations for state changes. Pinia's approach is more flexible but still maintains devtools support.
- **What is the equivalent of Vuex modules in Pinia?** Pinia uses separate stores instead of nested modules. This improves ownership and TypeScript inference; code splitting occurs only when store imports are placed behind asynchronous route/feature boundaries, not merely because several stores exist.
- **How does TypeScript support differ between Pinia and Vuex?** Pinia offers excellent TypeScript support with full type inference out of the box. Vuex requires more manual type definitions and has limited type inference, especially with modules and dynamic registration.
- **What are the benefits of Pinia's Composition API integration?** Pinia stores can be used directly in setup() functions with full reactivity, and you can compose store logic using composable patterns. This provides a more natural developer experience in Vue 3 applications.
- **How do you handle asynchronous operations in Pinia versus Vuex?** Both use actions for async operations, but Pinia actions are simpler and can be regular async functions. Vuex actions require context parameter and committing mutations for state changes.
- **Is Pinia compatible with Vue 2 and what are the migration considerations?** Pinia v3 targets Vue 3 only. Vue 2 projects should stay on Pinia v2 or migrate to Vue 3 before adopting Pinia v3. Migration from Vuex involves converting modules to separate stores, removing mutations, and updating the API calls to use Pinia's simpler syntax.
- **What are the performance implications of using multiple stores in Pinia?** Multiple stores primarily improve organization and subscription granularity. They do not automatically improve runtime performance or create lazy chunks; bundler output depends on the import graph and dynamic imports. Choose store boundaries by domain and measure hot update paths.

### Routing with Vue Router

#### Definition

Vue Router is the official router for Vue applications, enabling client-side navigation, nested layouts, data-aware guards, and URL synchronization. As of July 2026, Vue Router 5.1.0 is the current stable release for Vue 3.
#### Common Interview Questions

- **How did Vue Router evolve from v3 to the current v5?** Vue Router 3 targets Vue 2. Vue Router 4 adopted Vue 3's `createRouter`, Composition API, and stronger TypeScript model. Vue Router 5 is a stable transition release that integrates file-based routing and the former `unplugin-vue-router` workflow; v4 applications that did not use that plugin can generally upgrade without code modifications. Planned removals and the ESM-only baseline are deferred to v6.
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
- **What is the proper way to handle cancellation of ongoing requests in data fetching composables?** Use `AbortController` to stop obsolete requests when inputs change or the consumer unmounts. This avoids wasted work and stale result races; it is resource cleanup, not a guarantee that every uncancelled request would leak memory.
- **How can you implement caching in data fetching composables?** Implement caching by storing fetched data in a reactive reference or Map, using request parameters as keys, and returning cached data when the same request is made again within a validity period.
- **What are the considerations for using data fetching composables with Server-Side Rendering (SSR)?** For SSR, ensure composables can run on the server, handle cross-request state pollution properly, and support hydration of client-side state without refetching already available data.
- **How do you handle dependent data fetching where one request relies on another's result?** Use computed properties to create dependencies between data sources, and watch effects to trigger subsequent fetches when dependent data changes, or use async/await chaining within the composable.
- **What is the role of the onServerPrefetch lifecycle hook in SSR data fetching?** onServerPrefetch allows you to perform async operations during SSR and wait for them to complete before rendering, ensuring the server sends fully populated HTML to the client.
- **How can you share data fetching state across multiple components using composables?** Calling a composable normally creates a fresh state instance for each consumer. Share deliberately through Pinia, provide/inject, or module-scope state with SSR request-isolation safeguards; frameworks such as Nuxt can also deduplicate same-key `useAsyncData` state.
### SSR & Hydration: Nuxt 4 Framework

#### Definition

Server-Side Rendering (SSR) generates HTML on the server and sends rendered pages to the client; it can improve initial content delivery, crawlability, and social previews when caching and server latency are handled well. Hydration is the process where Vue takes over that HTML and makes it interactive. Nuxt is a full-stack Vue framework with universal rendering, server routes, hybrid route rules, and prerendering. Nuxt 4, released in July 2025, is the current stable major, with 4.4.8 current in July 2026.
#### Common Interview Questions

- **What are the primary benefits of SSR over Client-Side Rendering (CSR)?** SSR can provide crawlable content and social metadata before JavaScript executes and can improve FCP/LCP when server response and hydration costs are controlled. It is not automatically faster: server latency, cacheability, payload size, and client hydration still matter.
- **How does Nuxt simplify SSR compared to manual Vue SSR setup?** Nuxt provides universal rendering by default, file-based routing, route-level code splitting, head management, server routes, hydration-aware data utilities, and deployment presets without requiring an application to wire Vue's low-level SSR and router infrastructure manually.
- **What is the difference between Universal SSR and Static Site Generation (SSG) in Nuxt?** Universal SSR generates HTML on-demand for each request, while SSG pre-renders HTML at build time. Universal SSR is dynamic and personalized, SSG is faster for static content and can be served via CDN.
- **What is hydration and what common issues can occur during this process?** Hydration is when Vue attaches to server-rendered HTML and makes it interactive. Issues include hydration mismatches when server and client render different HTML, causing elements to flicker or break interactivity.
- **How does Nuxt 4 handle data fetching for SSR?** Use `useFetch` for an SSR-aware request, `useAsyncData` to wrap arbitrary asynchronous work, and `$fetch` for direct requests where payload transfer/deduplication is not required. The Nuxt 2 Options API `asyncData` method is not a Nuxt 4 data API.
- **What rendering modes are available in Nuxt 4?** Nuxt supports universal rendering, client-only rendering with `ssr: false`, prerendered/static routes, and hybrid rendering/caching configured per route with `routeRules`.
- **How do you handle authentication in Nuxt SSR applications?** Validate sessions in server routes and route middleware, and prefer `HttpOnly`, `Secure`, appropriately `SameSite` cookies so secrets are available to the server but not client JavaScript. `useAuth` is not a built-in Nuxt composable; it comes from an auth module or application code. Core utilities such as `useCookie` can support a custom implementation.
- **What is the purpose of the `<NuxtLink>` component compared to regular `<a>` tags?** `<NuxtLink>` provides intelligent client-side navigation with prefetching, automatic active state management, and prevents full page reloads while maintaining SSR benefits.
- **How does Nuxt optimize performance through code splitting?** Nuxt creates route/page chunks automatically. Components split on demand when they use Nuxt's lazy-component convention or an explicit dynamic import; ordinary eagerly imported components remain in their parent chunk.

### Server-Side Rendering (SSR) Deep Dive

#### Definition

Server-Side Rendering (SSR) in Vue renders components to HTML on the server and sends that result to the client. CSR typically begins with a smaller application shell and renders content in the browser. Hydration then attaches Vue's event handling and reactive behavior to server-rendered HTML. SSR can improve initial content and crawlability, but results depend on server latency, caching, payload size, and hydration cost.
#### Common Interview Questions

- **What is the difference between Client-Side Rendering (CSR) and Server-Side Rendering (SSR) in Vue?** CSR performs initial application rendering in the browser; SSR sends rendered HTML and hydrates it on the client. SSR can improve initial content and crawlability but consumes server resources and adds serialization/hydration constraints. Subsequent navigation speed depends on the chosen router, caching, and data strategy rather than CSR or SSR alone.
- **What is hydration and what common issues can occur during this process?** Hydration is the process where Vue attaches to server-rendered HTML and makes it interactive by adding event listeners and reactive behavior. Common issues include hydration mismatches (when server and client render different HTML), element flickering, and interactive elements not working properly due to timing issues.
- **How do you handle component lifecycle differences between server and client in SSR?** `beforeCreate`/`created` and setup code run during both server rendering and client creation. DOM lifecycle Hooks such as `mounted`/`onMounted` do not run on the server; `serverPrefetch`/`onServerPrefetch` lets SSR await data. In Nuxt, use `import.meta.server` and `import.meta.client` for environment-specific branches rather than legacy `process.server`/`process.client` flags.
- **What are the key considerations for writing universal code?** Avoid unguarded browser APIs such as `window` and `document` on the server, isolate mutable state per request, serialize only safe data, and ensure dependencies support SSR. Vue's `renderToString()` is asynchronous and returns a Promise, and Vue also provides streaming renderers, so SSR is not inherently synchronous.
- **How do you manage application state and data fetching in SSR applications?** Use onServerPrefetch in Composition API or serverPrefetch in Options API to fetch data on the server. The fetched data should be serialized and sent to the client for hydration to avoid refetching. Vuex/Pinia stores need special handling for state serialization.
- **What is the difference between SSR and Static Site Generation (SSG) in Vue?** SSR generates HTML on-demand for each request, making it suitable for dynamic, personalized content. SSG pre-renders HTML at build time, making it faster for static content that doesn't change frequently. SSR requires a running server, while SSG can be served from CDN.
- **How do you handle authentication and user sessions in SSR applications?** Validate sessions on the server and prefer `HttpOnly`, `Secure`, appropriately `SameSite` cookies. An HttpOnly session secret is sent with requests but cannot be read by client JavaScript. Avoid serializing tokens or sensitive user data into hydration payloads, and enforce protected routes on the server as well as in client navigation.
- **What are common performance optimizations for Vue SSR applications?** Use correct cache headers and CDN/micro-caching where personalization permits, split client code, preload only genuinely critical resources, use preconnect selectively, and consider `103 Early Hints`. HTTP/2 Server Push is no longer supported by major browsers and should not be recommended.
- **How do you debug SSR-specific issues in Vue applications?** Inspect server logs and Vue hydration warnings, compare server HTML with the first client render, verify serialized state, and use Nuxt DevTools/payload inspection for Nuxt applications. `vue-server-renderer` is the Vue 2 package, not a Vue 3 debugging mode; Vue 3.5's `data-allow-mismatch` should only suppress intentional differences.
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
- **What performance metrics can improve with these techniques?** Skipping expensive updates or virtualizing a large list can reduce JavaScript and main-thread work and may improve INP when rendering is the bottleneck. They do not inherently improve CLS; a virtual list with inaccurate or changing item dimensions can cause layout shifts, so reserve stable space and measure real-user metrics.
- **How does v-memo interact with Vue's reactivity system?** v-memo creates a conditional reactivity boundary. The component only re-renders when the memoized dependencies change, bypassing the normal reactive update triggers for that subtree.
- **What are the common pitfalls when implementing virtual scrolling?** Common pitfalls include incorrect item height calculations, poor handling of dynamic content sizes, inadequate buffer zones causing flickering, and accessibility issues with screen readers and keyboard navigation.

## Angular Deep Dive
Angular is a TypeScript-based framework for building web applications. It uses declarative templates, dependency injection, signals, and RxJS to manage complex UIs. As of July 12, 2026, Angular 22 is the active line, while Angular 21 and Angular 20 are in LTS. Angular 22 requires Node.js `^22.22.3 || ^24.15.0 || ^26.0.0` and TypeScript `>=6.0.0 <6.1.0`; TypeScript 7 is generally available, but it is outside Angular 22's supported compiler range.

Angular 22 promotes Signal Forms, `resource()`/`httpResource()`, and the Angular Aria pattern set to production-ready status. It also adds `@Service()` and stable `injectAsync()` for auto-provided and lazily loaded services, expands template syntax, makes `OnPush` the default for new applications, and deprecates the legacy webpack-based builders. The current Angular Aria guide and API reflect its stable status, although roadmap wording can lag release documentation.

### Running Example: Task Board

The examples below are focused excerpts from one standalone Task Board application. Imports and decorator metadata unrelated to the concept are omitted. Representative files include `task.model.ts`, `task.store.ts`, `task-api.ts`, `task-card.ts`, `task-board.ts`, `task-editor.ts`, and `tasks.routes.ts`.

```ts
export type TaskStatus = 'todo' | 'in-progress' | 'blocked' | 'done';
export type TaskPriority = 'low' | 'medium' | 'high';

export interface Task {
  readonly id: string;
  readonly title: string;
  readonly status: TaskStatus;
  readonly priority: TaskPriority;
  readonly assigneeId: string | null;
}
```

### Component vs Directive

#### Definition

A Component is a special type of directive that has a template and is the primary UI building block in Angular. Components manage a view, receive data through `input()` or `@Input()`, and emit events through `output()` or `@Output()`. A Directive adds behavior to an existing DOM element or component without owning a view. Components, structural directives, and attribute directives therefore solve different UI composition problems.

#### Evolution Timeline

- Angular 2+ (2016): Introduced components as the primary building blocks alongside directives
- Angular 14 (2022): Standalone components introduced as developer preview
- Angular 15 (2022): Standalone components became stable
- Angular 17 (2023): Standalone components became default for new projects

#### Example — Task Board

```ts
@Component({
  selector: 'app-task-card',
  imports: [PriorityDirective],
  template: `
    <article [appPriority]="task().priority">
      <h3>{{ task().title }}</h3>
    </article>
  `,
})
export class TaskCardComponent {
  readonly task = input.required<Task>();
}
```

**What it demonstrates:** The component owns a view and its input contract, while `PriorityDirective` adds reusable behavior to the existing `<article>` without supplying a template.

#### Common Interview Questions

- **What is the key difference between a component and a directive?** A component declares and owns a view. A non-component directive does not declare a component view: an attribute directive changes an existing host, while a structural directive controls embedded template instances.
- **How do structural directives differ from attribute directives?** Structural directives manipulate the DOM layout by adding/removing elements (using TemplateRef and ViewContainerRef), while attribute directives change the appearance or behavior of existing elements.
- **What are standalone components and when were they introduced?** Standalone components were introduced in Angular 14 (2022) as a developer preview, became stable in Angular 15, and became the default in Angular 17. They don't require NgModules and can import dependencies directly.
- **Can you mix NgModule-based and standalone components in the same application?** Yes, Angular supports a gradual migration path. You can bootstrap standalone components while still using NgModule-based components elsewhere in the application.

### Basic Bindings & Services

#### Definition

Bindings connect template markup to component data. Main flavors are interpolation (`{{ value }}`), property (`[src]="url"`), event (`(click)="save()"`), and two-way (`[(ngModel)]="name"`). Services are injectable classes that encapsulate shared logic. `@Injectable()` remains the configurable, general-purpose decorator; Angular 22 also provides `@Service()` for a simple automatically provided service.

#### Example — Task Board

```ts
@Component({
  selector: 'app-task-details',
  template: `
    <h2>{{ task().title }}</h2>
    <button [disabled]="task().status === 'done'" (click)="complete()">
      Complete
    </button>
  `,
})
export class TaskDetailsComponent {
  private readonly store = inject(TaskStore);
  protected readonly task = this.store.selectedTask;

  protected complete(): void {
    this.store.complete(this.task().id);
  }
}
```

**What it demonstrates:** Interpolation renders service-backed state, property binding controls the button, and event binding delegates the business operation to an injected service.

#### Common Interview Questions

- **Why use a service instead of sharing logic directly between components?** Services promote single responsibility, reusability, and make it easy to test logic in isolation.
- **What did Angular 22's `@Service()` decorator add?** `@Service()` replaces common `@Injectable({ providedIn: 'root' })` boilerplate and is auto-provided by default. It can define a factory or disable automatic provision for explicit route/component providers. Use `@Injectable()` for constructor injection, established `providedIn` scopes such as `platform`, or existing provider patterns; both decorators can use field-level `inject()`.
- **What does [(ngModel)] desugar to?** [ngModel]="value" + (ngModelChange)="value=$event" (property + event binding pair).
- **How do you scope a service to a lazy feature?** In modern standalone routing, provide it in the route's `providers` array, or provide it on a component/directive for an isolated subtree. In NgModule-based legacy apps, use that lazy module's `providers` array. Prefer `providedIn: 'root'` for shared app-wide singletons.

### Pipes

#### Definition

Pipes transform data in templates. Angular provides built-in pipes such as `date`, `currency`, `async`, and `json`. Custom pipes implement `PipeTransform` and define a `transform(value, ...args)` method.

#### Example — Task Board

```ts
@Pipe({ name: 'openTaskCount', standalone: true })
export class OpenTaskCountPipe implements PipeTransform {
  transform(tasks: readonly Task[]): number {
    return tasks.filter(task => task.status !== 'done').length;
  }
}
```

```html
<p>{{ tasks() | openTaskCount }} open tasks</p>
```

**What it demonstrates:** Custom pipes are pure by default, so this transformation reruns when the task-array reference changes rather than when an item is mutated in place.

#### Common Interview Questions

- **What is the difference between pure and impure pipes?** Pure pipes run only when their inputs change; impure pipes run on every change detection cycle. Impure pipes can be expensive and should be used sparingly.
- **How do you create a parameterized custom pipe?** Add parameters after the value in the template (value | myPipe:param1:param2) and declare corresponding arguments in the transform method.
- **When should you use the async pipe?** Use it to render Observables or Promises in templates and let Angular manage subscriptions and cleanup.

### Structural Directives

#### Definition

Structural directives create, remove, or move embedded views. The legacy `*ngIf`, `*ngFor`, and `[ngSwitch]` plus `*ngSwitchCase`/`*ngSwitchDefault` APIs are deprecated since Angular 20 in favor of the compiler-supported `@if`, `@for`, and `@switch` blocks covered later. Custom structural directives still use `TemplateRef` and `ViewContainerRef` when an application needs reusable view-creation behavior.

#### Example — Task Board

```ts
@Directive({ selector: '[appCan]' })
export class CanDirective {
  private readonly template = inject(TemplateRef<unknown>);
  private readonly container = inject(ViewContainerRef);
  private readonly permissions = inject(PermissionService);
  readonly appCan = input.required<string>();

  constructor() {
    effect(() => {
      this.container.clear();
      if (this.permissions.can(this.appCan())) {
        this.container.createEmbeddedView(this.template);
      }
    });
  }
}
```

```html
<button *appCan="'task:edit'">Edit task</button>
```

**What it demonstrates:** The directive controls whether Angular creates an embedded view. It controls presentation only; the backend must still authorize the operation.

#### Common Interview Questions

- **What happens to component state when `*ngIf` toggles false?** The view is destroyed; component instances are removed and recreated on next true.
- **What is the difference between `ngIf` and `[hidden]`?** `ngIf` destroys its embedded view when the condition is false. `[hidden]` binds the native `hidden` property, so the node remains in the DOM and is normally not rendered, although author CSS can override the browser's default hidden style.
- **How do you create a custom structural directive?** Inject TemplateRef (the template to render) and ViewContainerRef (the insertion point), then use createEmbeddedView and clear to control when the template is rendered.

### Attribute Directives

#### Definition

Attribute directives change the appearance or behavior of an existing element without changing DOM structure. Modern code can express host properties, attributes, classes, styles, and listeners through directive `host` metadata; `@HostBinding` and `@HostListener` remain supported for existing code.

#### Example — Task Board

```ts
@Directive({
  selector: '[appPriority]',
  host: {
    '[class.task--high]': "appPriority() === 'high'",
    '[attr.data-priority]': 'appPriority()',
  },
})
export class PriorityDirective {
  readonly appPriority = input.required<Task['priority']>();
}
```

```html
<article [appPriority]="task.priority">{{ task.title }}</article>
```

**What it demonstrates:** The directive updates a class and data attribute on its host element without adding, removing, or owning any DOM structure.

#### Common Interview Questions

- **How do attribute directives differ from structural directives?** Attribute directives modify existing elements (e.g., add styles) without changing the DOM structure, while structural directives add or remove elements.
- **How can you respond to DOM events in a directive?** Prefer an event entry in directive `host` metadata for new code, for example `'(click)': 'onClick($event)'`. `@HostListener` remains supported.
- **How do you bind host classes, styles, properties, or events in a directive?** Prefer the `host` property in directive metadata for new code because the bindings remain visible together. `@HostBinding` and `@HostListener` are supported, while `Renderer2` is useful when imperative DOM work is genuinely required.

### Dependency Injection (DI)

#### Definition

Dependency Injection is a design pattern where classes receive their dependencies from external sources rather than creating them internally. Angular's DI system provides instances of classes where they are needed and manages their lifecycle according to provider scope. `@Injectable()` supplies creation metadata and configurable provider options, while Angular 22's `@Service()` automatically provides a simple service. Angular dependencies are commonly consumed through constructor injection, `inject()`, or—when an auto-provided service should be code-split—stable `injectAsync()`.

#### Example — Task Board

```ts
export const TASK_ROUTES: Routes = [{
  path: 'projects/:projectId/tasks/:taskId/edit',
  providers: [TaskDraftStore],
  loadComponent: () =>
    import('./task-edit-page').then(module => module.TaskEditPage),
}];

export class TaskEditorComponent {
  protected readonly draft = inject(TaskDraftStore);
}

export class TaskToolbarComponent {
  protected readonly draft = inject(TaskDraftStore);
}
```

**What it demonstrates:** The editor and toolbar under one activated routed subtree resolve the same route-scoped draft store. A separate route injector that declares the provider receives its own instance.

#### Common Interview Questions

- **What is the provider hierarchy in Angular?** Angular resolves dependencies through two main hierarchies: `EnvironmentInjector` for application, route, and other environment-level providers, and `ElementInjector` for providers declared on components and directives. NgModule-based applications additionally configure a `ModuleInjector`. Resolution starts at the requesting element and walks the element hierarchy before continuing through the relevant environment hierarchy.
- **Constructor injection vs inject(): when would you use each?** Both are stable and supported. Constructor injection makes required dependencies visible in a class signature and remains common in class-based components and services. `inject()` is convenient in field initializers, functional guards/interceptors, provider factories, and reusable DI helpers, and provides precise typing for options such as optional injection. It can only run inside an injection context.
- **How is inject() different from @Inject()?** `inject()` is a function from `@angular/core` that reads a dependency from the current injection context. `@Inject()` is a constructor-parameter decorator used when Angular needs an explicit token, especially for InjectionToken values, primitives/config values, or cases where the TypeScript type is not enough.
- **Where can inject() be called?** Only inside an injection context: during construction or field initialization of a class Angular creates, inside provider factories, inside InjectionToken factories, or inside code run with runInInjectionContext. Calling it later from an ordinary class method outside that context throws an injection-context error.
- **How do you inject tokens that are not classes (e.g., values)?** Use a typed `InjectionToken` and an appropriate provider:
  ```ts
  interface AppConfig { apiUrl: string; timeout: number }
  export const APP_CONFIG = new InjectionToken<AppConfig>('APP_CONFIG');

  @Component({
    providers: [{
      provide: APP_CONFIG,
      useValue: { apiUrl: '/api', timeout: 5000 },
    }],
    template: `...`,
  })
  export class TaskBoardComponent {
    private readonly config = inject(APP_CONFIG);
  }
  ```
- **What's the difference between useClass, useValue, useFactory, and useExisting?** useClass: Provides an instance of the given class
  useValue: Provides a specific value
  useFactory: Provides a value created by a factory function
  useExisting: Provides an existing token (alias)
- **How does providedIn: 'root' differ from route, module, or component providers?** `providedIn: 'root'` makes the service an application-wide singleton and enables tree-shaking if the service isn't used. Route providers scope services to a routed feature, component/directive providers scope services to an element subtree, and NgModule providers are mainly relevant in NgModule-based applications.
- **What is the `inject()` function and why would you use it?** `inject()` reads a dependency from the current Angular injection context. It avoids constructor parameters, works naturally with functional APIs and provider factories, and provides precise typing for injection options. It must be called synchronously while an injection context is active, not later in a lifecycle hook or arbitrary callback.
- **How does Angular 22's `injectAsync()` differ from `inject()`?** It returns an async accessor that lazily loads and resolves an auto-provided service, enabling service-level code splitting and optional prefetch triggers. Use `injectAsync(() => import('./report.service'))` when the service is the module's default export, or `injectAsync(() => import('./report.service').then(m => m.ReportService))` for a named export. The target must be auto-provided with `@Service()` or `@Injectable({ providedIn: 'root' })`; ordinary `inject()` remains synchronous.
- **How do you prevent a service from being provided multiple times in lazy-loaded features?** Use `providedIn: 'root'` for true singletons, or provide the service only at the intended route/component scope. Providing the same service in multiple lazy scopes intentionally creates separate instances.
- **How does hierarchical injection work?** Angular first looks through the requesting element's `ElementInjector` ancestry, then the corresponding `EnvironmentInjector` ancestry. This enables local component instances, route-scoped services, and application-wide singletons without reducing the model to a simple root → module → component chain.
- **How do you handle optional dependencies and defaults?** Optional injection returns `null`, so use a nullish fallback such as `inject(SOME_TOKEN, { optional: true }) ?? 'default'`. Alternatively, define the default in the token itself with an `InjectionToken` factory. A TypeScript default parameter only handles `undefined`, not the `null` returned by optional DI.

### Lifecycle Hooks

#### Definition

Angular calls lifecycle hook methods at key points in a component's existence: from creation to destruction. These hooks allow you to run code at specific moments during a component's lifecycle, such as initialization, change detection, content projection, view rendering, and cleanup.

#### Evolution Timeline

- Angular 2 (2016): Original lifecycle hooks introduced (ngOnInit, ngOnChanges, etc.)
- Angular 16 (2023): Application-level render callbacks added for browser-only DOM work (afterNextRender, afterRender)
- Angular 20 (2025): afterRender was renamed/reframed as afterEveryRender; use afterRender in Angular 16-19 codebases and afterEveryRender in Angular 20+ codebases.

#### Example — Task Board

```ts
@Component({
  selector: 'app-task-editor',
  template: `<input #title [value]="task().title" aria-label="Task title">`,
})
export class TaskEditorComponent {
  readonly task = input.required<Task>();
  private readonly title = viewChild.required<ElementRef<HTMLInputElement>>('title');
  private readonly events = inject(TaskEvents);
  protected readonly lastSaved = signal<Task | null>(null);

  constructor() {
    afterNextRender({
      write: () => this.title().nativeElement.focus(),
    });
    this.events.saved$.pipe(takeUntilDestroyed())
      .subscribe(task => this.lastSaved.set(task));
  }
}
```

**What it demonstrates:** `viewChild()` exposes a query signal, `afterNextRender()` runs after Angular's next application render in the browser, and `takeUntilDestroyed()` ties the subscription to the component lifecycle.

#### Common Interview Questions

- **When should you use `ngAfterViewInit()` versus `afterNextRender()`?** Use `ngAfterViewInit()` for component-instance setup that depends on the initialized view or decorator queries. Use `afterNextRender()` for browser DOM work that must run after Angular completes the next application render; render callbacks are skipped during SSR and prerendering.
- **Why can ngOnChanges fire before ngOnInit?** Input bindings are set first, so Angular detects changes even before initialization completes. ngOnChanges runs whenever inputs change, including the initial setting of values.
- **How would you perform cleanup for RxJS subscriptions in modern Angular?** Prefer template `AsyncPipe` when the value is only rendered. For an imperative subscription, use `takeUntilDestroyed()` as in the example; call it in an injection context or pass an injected `DestroyRef`. Manual subscriptions still require explicit teardown.
- **What are the new render callbacks introduced in Angular 16+ and when are they useful?** afterNextRender: Runs once after the next application render. Useful for one-time DOM operations.
  afterRender / afterEveryRender: Runs after every application render. The name is afterRender in Angular 16-19 and afterEveryRender in Angular 20+. Useful for analytics or DOM measurements.
  Use case: Accessing browser APIs safely without causing ExpressionChangedAfterItHasBeenChecked errors.
  These are application-level render callbacks, not component-scoped lifecycle methods, although they are usually registered from an injection context such as a component constructor.
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
- **How do the new afterRender / afterEveryRender callbacks help with SSR (Server-Side Rendering)?** The render callbacks are skipped during server rendering and build-time prerendering, preventing errors from browser-specific APIs.
  This makes it safer to write code that works in both server and client environments.

### Change Detection

#### Definition

Angular's change detection mechanism determines when and how to update the UI when application state changes. Zoneless change detection has been stable since Angular 20.2 and is the default for newly generated applications since Angular 21. New Angular 22 applications also default components to `OnPush` and rename the eager strategy to `ChangeDetectionStrategy.Eager` (`Default` remains a deprecated alias). Existing applications can retain their configured strategy, and zone-based change detection remains available through `provideZoneChangeDetection()`.

#### Example — Task Board

```ts
@Component({
  selector: 'app-task-list',
  imports: [TaskCardComponent],
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    @for (task of tasks(); track task.id) {
      <app-task-card [task]="task" />
    }
  `,
})
export class TaskListComponent {
  readonly tasks = signal<Task[]>([]);

  add(task: Task): void {
    this.tasks.update(tasks => [...tasks, task]);
  }
}
```

**What it demonstrates:** A signal write notifies zoneless Angular, while the immutable collection update and stable `track` key work cleanly with `OnPush`.

#### Common Interview Questions

- **What triggers change detection in Angular?** With zone-based change detection, Angular normally schedules checks after patched browser tasks and microtasks—such as handlers for DOM events, timers, network completion, and Promise reactions—run inside the Angular zone. An Observable emission is not inherently a Zone.js trigger: `AsyncPipe` marks its view, while an imperative subscription depends on where the producer/callback runs or must notify Angular explicitly.
- **What changes in Angular 21+ zoneless applications?** Updates must notify Angular through supported mechanisms such as changing a signal read by a template, `AsyncPipe`, `ChangeDetectorRef.markForCheck()`, `ComponentRef.setInput()`, bound host/template listeners, or attaching/removing a marked view. Angular no longer relies on Zone.js patching every asynchronous task.
- **How do you manually trigger change detection?** Inject ChangeDetectorRef and call detectChanges() for immediate check or markForCheck() to schedule check in next cycle.
- **Why can mutating an array fail to update an OnPush component?** OnPush checks reference equality. Mutation keeps the same reference; create new reference: this.items = [...this.items, newItem].
- **How do Signals change Angular change detection?** When a template reads a signal, Angular tracks that dependency and marks the owning view when the signal changes. Signals make invalidation more precise, but Angular still schedules and checks views; they do not guarantee that one DOM expression is patched in isolation.
- **What triggers an OnPush component check?** New input values from template bindings, events handled in its subtree, signals read by its template changing, `AsyncPipe` emissions, `markForCheck()`, `ComponentRef.setInput()`, and view attachment/removal notifications can all make an OnPush view eligible for checking.

### Signals (Reactivity Primitives)

#### Definition

A signal is a reactivity primitive that holds a value and notifies tracked consumers when that value changes. Consumers include templates, `computed()` values, and `effect()` callbacks; they are not all effects. Signals provide granular dependency tracking while remaining synchronously readable.

#### Evolution Timeline

- Angular 16 (2023): Signals introduced as developer preview
- Angular 17 (2023): Signals became stable, integrated with templates
- Angular 20 (2025): effect() and linkedSignal() became stable
- Angular 21 (2025): Zoneless becomes the default for new applications
- Angular 22 (2026): `resource()` and `httpResource()` become stable for signal-based asynchronous reads

#### Example — Task Board

```ts
export class TaskFilters {
  readonly tasks = signal<Task[]>([]);
  readonly status = signal<'all' | TaskStatus>('all');

  readonly visibleTasks = computed(() => {
    const status = this.status();
    return status === 'all'
      ? this.tasks()
      : this.tasks().filter(task => task.status === status);
  });
}
```

**What it demonstrates:** Writable signals hold current state, while `computed()` expresses cached derived state without using an effect.

#### Common Interview Questions

- **What problem do Signals solve in Angular?** Signals provide synchronously readable state with explicit, granular dependency tracking. Angular can record which views and computed values consume a signal and mark relevant consumers when it changes. This can reduce broad change-notification work and simplify derived state, but actual rendering performance still depends on component structure and update patterns.
- **Explain the difference between signal(), computed(), and effect().**
  - `signal()`: A writable signal that holds a value. You can update it with .set() or .update().
  - `computed()`: A read-only signal that derives its value from other signals. Its value is cached and recalculated only when its dependencies change.
  - `effect()`: A side effect that runs whenever any signal value it depends on changes. It's used for operations like logging, DOM manipulation outside the template, or syncing with external libraries.
- **What are linkedSignal(), resource(), and httpResource()?** `linkedSignal()` is a stable writable signal, introduced in Angular 19 and stabilized in Angular 20, whose value can reset when a reactive source changes. Stable since Angular 22, `resource()` coordinates an asynchronous loader with signal state, while `httpResource()` provides an HTTP-oriented resource API. Resources are intended primarily for reads; use `HttpClient` or another explicit mutation flow for writes.
- **How do Signals differ from RxJS Observables in Angular?**
  | Feature | Signals | RxJS Observables |
  | --- | --- | --- |
  | Purpose | Primarily for reactive state in templates | General-purpose async/data streams |
  | Dependency model | Tracks consumers that read the signal | Push stream; may be unicast or multicast |
  | Usage | Synchronous value access via () | Subscription; emissions may be synchronous or asynchronous |
  | Strength | Simplicity, performance, deep integration with Angular templates | Powerful operators, handling complex async events, websockets |
- **What is "fine-grained reactivity" and how do Signals enable it?** Angular records which views consume each signal, so a change can mark the relevant consumers instead of treating every asynchronous task as evidence that the whole application may have changed.
- **Can Signals replace RxJS and Zone.js in Angular?** Signals replace neither every stream nor every asynchronous abstraction. They work well for current state and derived state, while RxJS remains useful for cancellation, coordination, and event streams. Zoneless is the default for newly generated Angular 21+ applications, with signals as one of several notification mechanisms.
- **How would you use a Signal in an Angular template?** Call its getter in the template, for example `{{ visibleTasks().length }}`. Angular records the template read as a dependency and marks the view when that signal changes.
- **Are there signals in other frameworks?** Yes. Solid uses signal-based reactivity at its core, Preact offers the `@preact/signals` library, and Vue's `ref()` and `computed()` are closely related concepts. Similar primitives do not imply identical scheduling or rendering behavior across frameworks.

### RxJS (Reactive Extensions for JavaScript)

#### Definition

RxJS is a library for reactive programming using Observables, making it easier to compose asynchronous or callback-based code. It provides powerful operators to transform, combine, manipulate, and work with streams of data. RxJS is fundamental to Angular's HTTP client, router, forms, and many other asynchronous operations.

#### Example — Task Board

```ts
@Component({
  selector: 'app-task-search',
  imports: [ReactiveFormsModule],
  template: `<input [formControl]="query" aria-label="Search tasks">`,
})
export class TaskSearchComponent {
  private readonly api = inject(TaskApi);
  readonly query = new FormControl('', { nonNullable: true });

  readonly results = toSignal(
    this.query.valueChanges.pipe(
      debounceTime(250),
      distinctUntilChanged(),
      switchMap(query => this.api.search(query)),
    ),
    { initialValue: [] },
  );
}
```

**What it demonstrates:** RxJS handles a debounced event stream and cancels stale HTTP work through `switchMap`; `toSignal()` owns the subscription in the component's injection context.

#### Common Interview Questions

- **How do Observables differ from Promises?** A Promise represents one eventual result. Its constructor executor runs immediately, although the Promise itself does not make arbitrary work "eager." An Observable represents a producer/subscription contract and may emit zero, one, or many values. Many Observables are lazy, but hot Observables already have an active producer. Unsubscribing stops delivery and may run teardown; it cancels underlying work only when that producer implements cancellation.
  | Feature | Promise | Observable |
  | --- | --- | --- |
  | Start behavior | Constructor executor runs immediately | Depends on source; often lazy, but can be hot |
  | Values | Single value | Multiple values over time (stream) |
  | Cancellation | No built-in Promise cancellation; APIs may accept AbortSignal | Unsubscribe runs teardown; cancellation depends on the source |
  | Operators | Limited static methods (all, race) | Rich operators (map, filter, switchMap) |
- **What is the purpose of the pipe method in RxJS?** pipe composes operators in sequence, returning a new Observable that applies each operator to the source stream. It enables functional, reusable operator composition without modifying the original Observable.
- **Explain the difference between switchMap, mergeMap, concatMap, and exhaustMap.**
  - switchMap: Cancels previous inner Observable when new value arrives (ideal for search).
  - mergeMap: Runs all inner Observables concurrently (order not guaranteed).
  - concatMap: Runs inner Observables sequentially (maintains order).
  - exhaustMap: Ignores new values until current inner Observable completes (ideal for save operations).
- **How do you prevent memory leaks with Observables?** Prefer `AsyncPipe` for template consumption and `takeUntilDestroyed()` for imperative subscriptions in modern Angular. `toSignal()` also manages its subscription in the owning injection context by default. For framework-independent RxJS code, retain the returned `Subscription` and unsubscribe it, or compose a teardown notifier carefully. Unsubscribing releases resources only when the source implements teardown.
- **What are Cold vs Hot Observables?** A cold Observable creates its producer for each subscription (for example, `HttpClient.get()`). A hot source exists independently of a particular subscriber (for example, a `Subject` or a shared socket). `fromEvent()` registers a listener for each subscription by default, so it is not a good example of an always-running hot producer.
- **When would you use a Subject vs another Observable?** Use a Subject when an imperative multicast bridge is genuinely needed. An Observable is not inherently unicast: its behavior depends on the source and operators such as `share()` or `shareReplay()`.
- **What are the different types of Subjects?** Subject: No initial value, only emits values after subscription
  BehaviorSubject: Requires initial value, emits current value to new subscribers
  ReplaySubject: Replays specified number of previous values to new subscribers
  AsyncSubject: Emits only the last value when completed

### Angular Component Communication Patterns

#### Definition

Angular provides a comprehensive set of patterns for component communication, leveraging its powerful dependency injection system and TypeScript integration. Understanding these patterns is essential for building maintainable Angular applications with proper separation of concerns and predictable data flow.

#### Example — Task Board

```ts
@Component({
  selector: 'app-task-card',
  template: `
    <button (click)="completed.emit(task().id)">
      Complete {{ task().title }}
    </button>
  `,
})
export class TaskCardComponent {
  readonly task = input.required<Task>();
  readonly completed = output<string>();
}
```

```html
<app-task-card [task]="task" (completed)="complete($event)" />
```

**What it demonstrates:** The parent passes data through a required signal input, and the child reports a semantic event through `output()`.

#### Common Interview Questions

- **What are the main component communication patterns in Angular and when should you use each?** Use `input()` / `@Input()` for parent-to-child data flow, `output()` / `@Output()` for child-to-parent events, `model()` or `[(ngModel)]` for two-way binding, Services with DI for unrelated components, query APIs for parent-to-child instance access, and RxJS Subjects for reactive cross-component state.
- **How do you choose between services and input/output for component communication?** Use input/output APIs for direct parent-child relationships where the communication is explicit and hierarchical. Use services when components are not directly related, when you need to share state across multiple components, or when the data needs to be persisted beyond component lifecycle.
- **How do signal-based input/output APIs differ from decorator APIs?** Modern Angular recommends `input()`, `output()`, and `model()` for new projects. `input()` returns an `InputSignal`, so read it by calling it like `myInput()`. `@Input()`, `@Output()`, and `EventEmitter` remain fully supported and are common in existing codebases.
- **What is the difference between viewChild() and contentChild()?** `viewChild()` accesses elements/components that are part of the component's own template, while `contentChild()` accesses content projected into the component via `<ng-content>`. The plural forms, `viewChildren()` and `contentChildren()`, return signal-based arrays of query results. Decorator APIs like `@ViewChild` and `@ContentChild` remain supported.
- **How does Angular's change detection affect component communication?** Inputs, outputs, template events, signals, `AsyncPipe`, and explicit change-detection notifications all participate. With OnPush, immutable input updates remain important, but input changes and async-pipe emissions are not the only triggers.
- **When should you use RxJS Subjects versus simple services for state management?** Use RxJS Subjects when you need reactive programming patterns, multiple subscribers, or complex state transformations. Use simple services for simple data sharing without the need for reactive updates or when the data flow is straightforward.

#### Communication Patterns Reference

| Need | Modern default | Use when |
| --- | --- | --- |
| Parent to child | `input()` | A parent supplies data or configuration |
| Child to parent | `output()` | A child reports a semantic event |
| Two-way component value | `model()` | A reusable control owns an editable value |
| View or projected-content query | `viewChild()` / `contentChild()` | A component needs a specific child instance or element |
| Shared state or behavior | Scoped service with DI | Components are not connected by a direct data-flow edge |
| Multivalue event stream | Observable or private Subject exposed as an Observable | Cancellation, composition, or multicast events are required |
| URL-owned state | Router parameters, query parameters, or router state | State should survive navigation or be shareable as a URL |

### Forms & Validation

#### Definition

Angular supports template-driven forms, reactive forms, and, in Angular 22+, stable Signal Forms. Reactive forms use `FormGroup`, `FormControl`, and `FormArray`; Signal Forms use a writable signal model, a generated field tree, and schema-based validation. All three approaches have valid use cases.

#### Evolution Timeline

- Angular 2 (2016): Both template-driven and reactive forms introduced
- Angular 14 (2022): Typed forms introduced for better type safety
- Angular 15+: Improved standalone component support for forms
- Angular 21 (2025): Signal Forms introduced as an experimental API in `@angular/forms/signals`
- Angular 22 (2026): Signal Forms become stable, with interoperability APIs for incremental migration

#### Example — Task Board

```ts
@Component({
  imports: [FormField],
  template: `
    <label>Title <input [formField]="taskForm.title"></label>
    <button [disabled]="taskForm().invalid()">Save</button>
  `,
})
export class TaskEditorComponent {
  readonly draft = signal({ title: '', priority: 'medium' as TaskPriority });
  readonly taskForm = form(this.draft, path => {
    required(path.title, { message: 'Title is required' });
    minLength(path.title, 3, { message: 'Use at least 3 characters' });
  });
}
```

**What it demonstrates:** An Angular 22 Signal Form derives a typed field tree from a writable model and applies schema-based validation.

#### Common Interview Questions

- **What are the key differences between template-driven and reactive forms?** Template-driven forms are declarative and simpler for basic scenarios. Reactive forms are imperative, provide better type safety, easier testing, and more control for complex scenarios like dynamic forms or cross-field validation.
- **What are Signal Forms?** Signal Forms are stable in Angular 22+. They model form data with a writable signal, expose type-safe field state, and define validation through schemas. Reactive forms remain stable and appropriate for existing applications and use cases that fit their APIs.
- **How do you trigger validation manually in reactive forms?** Call form.markAllAsTouched() to mark all controls as touched, or control.updateValueAndValidity() to re-run validation on a specific control.
- **Why use async validators for uniqueness checks?** Async validators return an Observable or Promise and let Angular represent a `PENDING` state while server validation completes. Angular does not automatically debounce HTTP calls. Use `updateOn: 'blur'`/`'submit'` or an explicit debounce strategy, and make cancellation/teardown behavior intentional.
- **What are typed forms (Angular 14+) and why use them?** Typed forms provide compile-time checking for controls and form values. For a non-nullable string control, use `new FormControl('', { nonNullable: true })`; without that option, reset semantics normally make the type `FormControl<string | null>`.
- **How do you handle dynamic form arrays?** Use `FormArray` to manage dynamic lists of controls. Add or remove controls with `push()` and `removeAt()`, and iterate in a modern template with `@for` (`*ngFor` in older templates).
- **What's the difference between `form.value` and `form.getRawValue()`?** For an enabled group, `.value` omits disabled descendants; a disabled group includes all descendants in its value. `getRawValue()` consistently includes disabled controls and is the clearer choice when the complete form model is required.
- **How do you implement cross-field validation?** Add the validator at the group level so it can compare sibling controls:
  ```ts
  const passwordsMatch = (group: AbstractControl): ValidationErrors | null =>
    group.get('password')?.value === group.get('confirmation')?.value
      ? null
      : { passwordsMismatch: true };

  const accountForm = new FormGroup({
    password: new FormControl('', { nonNullable: true }),
    confirmation: new FormControl('', { nonNullable: true }),
  }, { validators: passwordsMatch });
  ```
- **How do forms work with standalone components?** Import `ReactiveFormsModule` or `FormsModule` directly in the standalone component's `imports`. These are still NgModules, but the component can consume them without being declared in an application NgModule. Form behavior is otherwise the same; tree-shaking depends on the complete dependency graph and build.

### HTTP Interceptors

#### Definition

HTTP interceptors are middleware for Angular's HttpClient. Modern Angular recommends functional interceptors because they have more predictable behavior, especially in standalone and multi-injector setups. Class-based DI interceptors that implement HttpInterceptor remain supported for existing codebases.

#### Example — Task Board

```ts
export const SKIP_AUTH = new HttpContextToken(() => false);

export const apiInterceptor: HttpInterceptorFn = (req, next) => {
  if (!req.url.startsWith('/api/')) return next(req);
  let headers = req.headers.set('X-Correlation-ID', crypto.randomUUID());

  if (!req.context.get(SKIP_AUTH)) {
    const token = inject(AuthService).getAccessToken();
    if (token) headers = headers.set('Authorization', `Bearer ${token}`);
  }

  return next(req.clone({ headers }));
};
```

**What it demonstrates:** A functional interceptor scopes credentials to the application API, clones immutable request data, adds tracing and authentication metadata, and uses `HttpContextToken` for request-local policy.

#### Common Interview Questions

- **What are typical uses of HTTP interceptors?** Adding authentication tokens, logging requests/responses, handling errors globally, showing loading indicators, caching responses, adding retry logic, or modifying request/response data.
- **How do you register a modern functional interceptor?** Provide `HttpClient` in application configuration with `provideHttpClient(withInterceptors([apiInterceptor]))`. Interceptors run in the listed order for requests and unwind in reverse order for responses.
- **How do you keep class-based interceptors working?** Register existing DI-based interceptors with `withInterceptorsFromDi()` and the `HTTP_INTERCEPTORS` multi provider. Prefer functional interceptors for new code.
- **How do you handle retry logic in a functional interceptor?** Retry only transient failures and normally only idempotent methods. Cap retries and propagate permanent or final failures; production code should also honor `Retry-After` where applicable.

  ```ts
  const IDEMPOTENT_METHODS = new Set(['GET', 'HEAD', 'OPTIONS']);

  export const retryInterceptor: HttpInterceptorFn = (req, next) => {
    if (!IDEMPOTENT_METHODS.has(req.method)) return next(req);
    return next(req).pipe(
      retry({
        count: 3,
        delay: (error, retryCount) => {
          const transient = error instanceof HttpErrorResponse &&
            ([0, 408, 429].includes(error.status) || error.status >= 500);
          return transient ? timer(500 * 2 ** (retryCount - 1))
            : throwError(() => error);
        },
      }),
    );
  };
  ```
- **What does the multi: true option do?** In class-based interceptor registration, it allows multiple interceptors to be registered for the same `HTTP_INTERCEPTORS` token, forming a chain instead of replacing previous interceptors.
- **How do you ensure interceptors execute in a specific order?** Functional interceptors execute in the order listed in `withInterceptors([...])`. The first interceptor processes the request first and the response last.
- **What's the difference between functional and class-based interceptors?** Functional interceptors are plain functions and are the recommended default in modern Angular. Class-based interceptors implement the `HttpInterceptor` interface and rely on DI multi providers, which is still valid for existing apps.
- **How can you skip an interceptor for specific requests?** Prefer a custom `HttpContextToken`, as in the example, over a fake header that would be sent to the server. A public request can pass `context: new HttpContext().set(SKIP_AUTH, true)`.

### Standalone Components, NgModules & Route Lazy Loading

#### Definition

Standalone components, directives, and pipes declare their own template dependencies without requiring an NgModule declaration. NgModules still define compilation and provider scopes in existing applications and libraries. Route lazy loading is a separate concern: `loadComponent` and `loadChildren` use dynamic imports so a routed feature can be split from the initial JavaScript path.

#### Evolution Timeline

- Angular 2 (2016): Introduced NgModules as the primary organizing structure
- Angular 8 (2019): Ivy became available as an opt-in preview
- Angular 9 (2020): Ivy became the default compiler and rendering pipeline
- Angular 14 (2022): Standalone components introduced as a developer preview
- Angular 15 (2022): Standalone components, directives, and pipes became stable
- Angular 17 (2023): Standalone authoring became the default for new projects; NgModules remain supported

#### Example — Task Board

```ts
export const routes: Routes = [
  {
    path: 'projects/:projectId/tasks',
    providers: [TaskDraftStore],
    loadChildren: () =>
      import('./tasks/tasks.routes').then(module => module.TASK_ROUTES),
  },
  {
    path: 'task/:taskId',
    loadComponent: () =>
      import('./task-details').then(module => module.TaskDetailsComponent),
  },
  {
    path: 'legacy-reports',
    loadChildren: () =>
      import('./reports/reports.module').then(module => module.ReportsModule),
  },
];
```

**What it demonstrates:** The router can lazy-load a standalone component, a route array with route-scoped providers, or an existing NgModule.

#### Common Interview Questions

- **What is the difference between NgModule-based and standalone approaches?** NgModules declare components and collect template dependencies at module level. Standalone artifacts import their own template dependencies and do not require NgModule wrappers.
- **How does modern Angular lazy-load a routed feature, and how does the NgModule pattern differ?** Use `loadComponent` for one standalone component and `loadChildren` for a lazy route array or an NgModule. Both commonly use dynamic `import()`, allowing the build to create a separate chunk.
- **What are the benefits of standalone components over NgModules?** They make template dependencies explicit, reduce module boilerplate, simplify route configuration, and support incremental migration. Bundle size still depends on the dependency graph and build output; standalone authoring does not guarantee a smaller bundle by itself.
- **Can standalone components and NgModules interoperate?** Yes. A standalone component can import an NgModule directly, and an NgModule can import and export standalone components, directives, and pipes. `importProvidersFrom()` only extracts providers from NgModules for an environment injector; it is not required for template-level interoperability.
- **How do you provide services in a standalone application?** Use `providedIn: 'root'`, application-level providers, route providers, or component/directive providers according to the intended lifetime and sharing boundary.
- **What happened to AOT compilation with standalone components?** AOT compilation still applies. Standalone authoring changes how compilation scopes and dependencies are expressed; it does not replace AOT.
- **When would you choose NgModules today?** Existing NgModule applications and libraries can keep using them, and some teams retain them as organizational boundaries. Standalone is the recommended default for new Angular code, but NgModules remain supported.
- **What is the difference between `RouterModule.forRoot()` and `RouterModule.forChild()`?** In NgModule-based applications, `forRoot()` configures routes and root router providers once, while `forChild()` contributes feature routes without creating another router service. Standalone applications normally use `provideRouter()` instead.
- **How can you verify that lazy loading works?** Use the browser Network panel or a bundle analyzer and confirm that a separate JavaScript chunk loads when the route is first visited. Do not depend on a particular generated filename because production builds commonly hash and rename chunks.
- **Can you preload lazy routes?** Yes. `PreloadAllModules` can request all eligible lazy routes after initial navigation, or a custom preloading strategy can select routes. Preloading changes download timing; it does not make the code part of the initial bundle.
- **How do route-level providers work?** A route's `providers` array creates an environment injector for that routed subtree, which is useful for feature-scoped state such as `TaskDraftStore`.
- **How do you combine guards with lazy loading?** Use functional `canMatch` when route matching or loading should depend on a condition, and `canActivate` when guarding activation. Client guards improve navigation behavior but never replace backend authentication and authorization.
- **What are common lazy-loading pitfalls?** Over-splitting into many tiny sequential chunks, accidental eager imports, circular dependencies, duplicated providers, and missing loading or error states can erase the expected benefit.
- **How does lazy loading affect SEO?** A lazy chunk does not inherently harm SEO. The risk appears when meaningful route content exists only after client-side JavaScript runs; use SSR or prerendering for crawlability-sensitive routes when appropriate.

### New Control Flow (@if, @for, @switch) - Angular 17+

#### Definition

Built-in control flow is a declarative template syntax introduced in Angular 17 as the modern replacement for `NgIf`, `NgFor`, and `NgSwitch`, which are deprecated since Angular 20. The compiler understands `@if`, `@for`, and `@switch` directly, improving type checking and removing the need to import the corresponding directives.

#### Example — Task Board

```html
@if (loading()) {
  <p>Loading tasks...</p>
} @else {
  <ul>
    @for (task of filteredTasks(); track task.id) {
      <li>
        {{ task.title }}
        @switch (task.status) {
          @case ('blocked') { <span>Blocked</span> }
          @case ('done') { <span>Done</span> }
          @default { <span>Open</span> }
        }
      </li>
    } @empty {
      <li>No tasks match this filter.</li>
    }
  </ul>
}
```

**What it demonstrates:** `@if` selects a UI state, `@for` preserves item identity with `track`, `@empty` handles an empty result, and `@switch` renders status-specific content.

#### Common Interview Questions

- **What are the performance benefits of the new control flow?** Because the compiler handles these blocks directly, Angular does not create directive instances for them and can use an optimized list-diffing implementation for `@for`. This can reduce overhead, but bundle and runtime gains depend on the template and application; measure rather than promising a fixed improvement.
- **How does the `track` clause differ from `trackBy`?** `@for` requires a tracking expression, commonly a stable unique property such as `track item.id`. It can also invoke a method, such as `track trackItem(item)`; the important part is returning stable identity rather than an array index for changing collections.
- **What is the @empty block and when is it useful?** The @empty block displays content when the collection is empty, eliminating the need for separate `*ngIf` checks. It's executed only when the array is empty, making it more declarative.
- **Why was this change introduced in Angular 17?** To improve developer experience with more intuitive syntax, better performance through compile-time optimizations, enhanced type checking, and alignment with modern JavaScript frameworks.
- **Do you need to import anything to use the new control flow?** No, the new control flow is built into the Angular template compiler starting from version 17. It works out of the box without any imports.
- **Can you mix old and new control flow syntax?** Existing applications can migrate incrementally, so both forms may appear during transition. Because the legacy control-flow directives are deprecated since Angular 20, use built-in blocks for new code and migrate consistently.
- **How does the new syntax handle template scoping?** Variables defined in control flow blocks are block-scoped, preventing accidental variable leakage and providing better isolation than the previous directive approach.
- **What happened to ng-template with the new control flow?** The new syntax eliminates the need for most ng-template usage. The blocks are self-contained and don't require external template references, making the code more readable.

### Deferrable Views (@defer) - Angular 17+

#### Definition

Deferrable views are a performance feature introduced in Angular 17 that can split a template block and eligible dependencies into a lazily loaded chunk. With `@defer`, an application declares when that block should load. Deferring genuinely non-critical work can reduce the initial JavaScript path and improve metrics such as LCP or INP; deferring critical content can instead make the experience worse.

#### Example — Task Board

```html
<div aria-live="polite" aria-atomic="true">
  @defer (on viewport; prefetch on idle) {
    <app-task-analytics />
  } @placeholder {
    <div class="chart-skeleton">Analytics preview</div>
  } @loading (after 200ms; minimum 300ms) {
    <p>Loading analytics...</p>
  } @error {
    <p>Analytics could not be loaded.</p>
  }
</div>
```

**What it demonstrates:** The analytics chunk is prefetched while the browser is idle, rendered near the viewport, and given explicit placeholder, loading, error, and announcement states. The `@error` block handles deferred dependency-loading failures, not every API failure inside the component.

#### Common Interview Questions

- **How do deferrable views improve Core Web Vitals?** Deferrable views reduce the initial JavaScript bundle by loading non-critical components only when needed. This can improve Largest Contentful Paint (LCP) and help Interaction to Next Paint (INP) by keeping non-critical JavaScript work out of the initial load path.
- **What triggers can be used with `@defer`?** Common triggers include:
  - `on viewport`: the observed placeholder or trigger enters the viewport.
  - `on interaction`: the user clicks or presses a key on the trigger.
  - `on hover`: a mouseover or focus-in event occurs.
  - `on immediate`: Angular finishes the initial non-deferred rendering work.
  - `on timer(duration)`: the configured delay expires.
  - `on idle`: the browser becomes idle.
  - `when condition`: an application expression becomes truthy.
- **What's the difference between @placeholder and @loading blocks?** @placeholder shows content before the deferred block starts loading. @loading shows content during the loading process. The placeholder is typically static content, while loading shows progress indicators.
- **How does prefetching work with deferrable views?** Prefetching loads the deferred content in the background without rendering it. Use prefetch on trigger to start loading when a condition is met, then the main on trigger to actually display it.
- **Can you defer content that depends on async data?** Yes. Use a `when` condition derived from application state, or let the deferred component render its own async loading, success, empty, and error states. Keep code loading and data loading as separate concerns.
- **What error handling options are available?** The `@error` block handles failure to load a deferred dependency. If the component loads successfully but its data request or runtime work fails, the component must render and recover from that separate failure itself.
- **How do deferrable views differ from traditional lazy loading?** Traditional lazy loading works at the route/component level, while deferrable views work at the template block level within a single component, providing more granular control.
- **When should you avoid using @defer?** Avoid deferring content that's critical for initial rendering, above-the-fold content, or components needed for immediate user interaction. Also avoid over-deferring, which can cause too many sequential loads.

### Enter/Leave Animations

#### Definition

Modern Angular documents `animate.enter` and `animate.leave` as compiler-supported APIs for applying CSS classes or callbacks when elements enter or leave the DOM. The legacy `@angular/animations` package is deprecated since Angular 20.2; more complex animation systems can use CSS or dedicated JavaScript animation libraries.

#### Example — Task Board

```html
@for (task of tasks(); track task.id) {
  <li animate.enter="task-enter" animate.leave="task-leave">
    {{ task.title }}
  </li>
}
```

```css
.task-enter { animation: fade-in 160ms ease-out; }
.task-leave { animation: fade-out 120ms ease-in; }

@keyframes fade-in {
  from { opacity: 0; transform: translateY(4px); }
}

@keyframes fade-out {
  to { opacity: 0; transform: translateY(-4px); }
}

@media (prefers-reduced-motion: reduce) {
  .task-enter, .task-leave { animation: none; }
}
```

**What it demonstrates:** Angular applies CSS animation classes while an item enters and delays DOM removal until its leave animation finishes; the media query respects reduced-motion preferences.

#### Common Interview Questions

- **When would you use animate.enter and animate.leave?** Use them for focused enter/leave DOM animations where Angular should coordinate class application or element removal timing. Treat them as a useful modern Angular API, but a lower-priority interview topic than DI, Signals, forms, or change detection.

### SSR, Hybrid Rendering & Hydration

#### Definition

Server-Side Rendering (SSR) generates HTML on the server and sends it to the client. It can improve initial content delivery, crawlability, and social previews when server latency, caching and hydration are handled well. Hydration lets Angular reuse the server-rendered DOM on the client, attach behavior and make the application interactive. Modern Angular supports non-destructive hydration instead of discarding and recreating the server DOM.

Since Angular 21, server bootstrap accepts a `BootstrapContext`. On the server, `getPlatform()` returns `null`, while `destroyPlatform()` is a no-op.

#### Example — Task Board

```ts
import { ApplicationConfig, mergeApplicationConfig } from '@angular/core';
import { provideClientHydration } from '@angular/platform-browser';
import {
  provideServerRendering,
  RenderMode,
  ServerRoute,
  withRoutes,
} from '@angular/ssr';

export const serverRoutes: ServerRoute[] = [
  { path: '', renderMode: RenderMode.Prerender },
  { path: 'projects/:projectId', renderMode: RenderMode.Server },
  { path: 'projects/:projectId/tasks', renderMode: RenderMode.Client },
  { path: '**', renderMode: RenderMode.Server },
];

export const appConfig: ApplicationConfig = {
  providers: [provideClientHydration()],
};

export const serverConfig = mergeApplicationConfig(appConfig, {
  providers: [provideServerRendering(withRoutes(serverRoutes))],
});
```

**What it demonstrates:** The registered server routes prerender a static landing page, render a public project per request, and leave an authenticated workspace to CSR. Hydration reuses SSR/prerendered DOM, while backend authorization still protects data.

#### Common Interview Questions

- **What are the benefits of SSR over client-side rendering?** SSR can deliver meaningful HTML earlier and help crawlability and social previews. It may improve initial rendering on some routes, but server latency, caching, client JavaScript, and hydration still determine real performance. Semantic HTML can be read before JavaScript loads, while interactive behavior still requires successful client startup.
- **What is hydration and why is it important?** Hydration is the process where Angular "revives" server-rendered HTML on the client by attaching event listeners and making it interactive. Modern Angular uses non-destructive hydration that preserves server-rendered content without flickering.
- **What common issues occur with SSR and how do you solve them?** Common examples include:
  - Browser-only side effects: keep the initial rendered DOM consistent and use `afterNextRender()` or platform-specific providers.
  - Third-party browser libraries: defer initialization or load them only in the browser without changing the initial hydrated structure.
  - Authentication state: use a server-readable session architecture when SSR must render user-specific content; never serialize secrets into hydration data.
  - API calls: use URLs and credentials appropriate to both server and browser execution.
- **How do you handle authentication in SSR applications?** Prefer a server-managed session with a `Secure`, `HttpOnly`, appropriately `SameSite` cookie when the architecture allows it. The server can read the cookie while client JavaScript cannot. Because cookies are sent automatically, also apply CSRF defenses, origin validation, narrow cookie scope, session rotation and authorization checks; do not serialize secrets into hydration state.
- **What is the difference between prerendering and SSR?** Prerendering generates HTML at build time for routes known to the build and can fetch data while generating them. It is not limited to hard-coded content, but it cannot use request-specific state and requires a strategy for dynamic route parameters. SSR renders on demand for each request and can use request data.
- **How does Angular Universal differ from modern Angular SSR?** Angular Universal was the original SSR solution/name. Modern Angular SSR is centered on @angular/ssr, is integrated into the framework, has better hydration, and uses the same application configuration as client-side rendering.
- **What are hydration errors and how do you avoid them?** They occur when the server and client DOM structures differ. Keep initial template output deterministic, avoid direct DOM manipulation before hydration, use valid HTML, and move browser-only side effects to `afterNextRender()`. An `@if (isPlatformBrowser(...))` branch that changes initial markup can itself create a mismatch.
- **How do you optimize SSR performance?** Cache safe rendered responses, use transfer caching to avoid duplicate reads, keep server work bounded, optimize client bundles, and serve static assets through an appropriate cache or CDN. Measure server latency and hydration cost rather than assuming SSR is automatically faster.
- **What is incremental hydration in Angular?** It builds on SSR, hydration, and `@defer`. In Angular 22 it is enabled by default through `provideClientHydration()` and can be disabled with `withNoIncrementalHydration()`. Hydrate triggers such as `hydrate on interaction`, `hydrate on viewport`, `hydrate when condition`, and `hydrate never` control selected subtrees.
- **How does zoneless SSR know when async work is done?** Use PendingTasks for async work that must finish before server serialization. In zoneless apps, Angular cannot rely on Zone.js stability signals, so PendingTasks lets SSR wait for relevant data loading or rendering work.

### Angular Aria

#### Definition

Introduced as a developer preview in Angular 21, `@angular/aria` is stable in Angular 22 according to the [release announcement](https://blog.angular.dev/announcing-angular-v22-c52bb83a4664), [current guide](https://angular.dev/guide/aria/overview), and [API reference](https://angular.dev/api/aria/listbox/Listbox). Some [roadmap](https://angular.dev/roadmap) wording still describes stabilization as future work, so use the release and API documentation for the installed package version. Its twelve patterns cover autocomplete, listbox, select, multiselect, combobox, menu, menubar, toolbar, accordion, tabs, tree, and grid interactions. They supply accessible interaction primitives, not styling or automatic application-wide WCAG conformance.

#### Example — Task Board

```ts
import { Listbox, Option } from '@angular/aria/listbox';

@Component({
  imports: [Listbox, Option],
  template: `
    <ul ngListbox aria-label="Filter tasks by status"
        selectionMode="explicit" multi [(value)]="selectedStatuses">
      @for (status of statuses; track status) {
        <li ngOption [value]="status">{{ status }}</li>
      }
    </ul>
  `,
})
export class StatusFilterComponent {
  readonly statuses: TaskStatus[] = ['todo', 'in-progress', 'blocked', 'done'];
  selectedStatuses: TaskStatus[] = ['todo'];
}
```

**What it demonstrates:** Angular Aria supplies keyboard navigation, selection state, and screen-reader semantics, while the application still owns styling, filtering behavior, labels, and end-to-end accessibility testing.

#### Common Interview Questions

- **Can Angular Aria be treated as stable in Angular 22?** Yes. The v22 release announcement and current guide/API treat all twelve patterns as stable and production-ready. Check the installed package's API when targeting another version; roadmap prose may lag the released status.
- **When would you use `@angular/aria` instead of Angular Material?** Use `@angular/aria` when you need low-level accessible behavior while owning all rendering, styling, and application semantics yourself. Use Angular Material when you want complete, styled UI components.

## Modern JavaScript Deep Dive

This deep dive expands on the ECMAScript 2015-2026 features introduced in the earlier Modern JavaScript section.

### Default Parameters & Rest/Spread

#### Definition

Functions can specify default parameter values (`function foo(x = 10) {...}`); rest parameters (`...args`) capture remaining arguments into an array; spread syntax (`...iterable`) expands an iterable in calls/array literals, while object spread copies own enumerable properties into an object literal.
#### Common Interview Questions

- **What happens when a default parameter depends on a previous parameter?** Later parameters can reference earlier ones: `function greet(name, greeting = 'Hello ' + name) { return greeting; }`. The default expression is evaluated at call time only when the argument is `undefined`.
- **How does the rest operator differ from the spread operator?** The rest operator collects multiple arguments into a single array parameter, while spread expands an array or object into individual elements when passed to functions or array/object literals.
- **Do default parameters apply when the argument is null or undefined?** Only undefined triggers the default; null is treated as an explicit value.

### Modules

#### Definition

ES modules (ESM) use `import` and `export`. Named exports (`export const foo = ...`) can be imported selectively; a default export (`export default ...`) represents a module's single default binding. Dynamic `import('./module.js')` loads a module asynchronously and returns a Promise for its module namespace object.
#### Common Interview Questions

- **What is the difference between named and default exports?** Named exports use braces and expose declared export names, but an importer may alias them (`import { original as local }`). A default export is imported without braces and the importer chooses its local name.
- **How do ES modules differ from CommonJS?** ES modules are statically analyzable and support tree shaking; imports are hoisted. CommonJS uses require/module.exports, is dynamically evaluated and cannot be tree‑shaken as easily.
- **What are dynamic imports used for?** They load modules on demand (e.g., lazy loading routes) and return a Promise that resolves to the module’s exports.

### Classes & Inheritance

#### Definition

ES6 classes provide syntactic sugar over prototypes. Use class declarations with constructors, instance methods and static methods. Inheritance is achieved with extends and super().
#### Common Interview Questions

- **How do you create private properties in classes?** Use the # syntax (#privateField) or closures; private fields are only accessible within the class body.
- **How do class fields differ from prototype methods?** Fields define properties on each instance; methods are placed on the prototype and shared across instances.
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

Map stores key-value pairs with any data type as keys; Set stores unique values. WeakMap keys and WeakSet values can be objects or non-registered symbols and do not prevent those garbage-collectable values from being collected.
#### Common Interview Questions

- **When would you use a Map over an object?** When you need non‑string keys, maintain insertion order or frequently add/remove entries; objects are best for static structured data.
- **Why might a WeakMap be useful?** A WeakMap allows objects to be garbage‑collected when there are no other references, useful for caching associated data without risking memory leaks.
- **Why are most primitive values invalid WeakMap keys?** Weak keys must be garbage-collectable identities. Objects and non-registered symbols qualify; strings, numbers, booleans, bigints, `null`, `undefined`, and globally registered symbols do not.

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
  - `.then().catch()` - useful for transformation pipelines and APIs that naturally compose callbacks. It does not make work parallel; concurrency comes from starting independent operations before awaiting them and coordinating them with APIs such as `Promise.all()`.
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

- **What are tagged template literals?** A tag function receives the template's string parts and substitution values and can return any JavaScript value, not only a string. Example:
  ```js
  function highlight(strings, ...values) {
    return strings.reduce((result, str, i) => {
      const value = i < values.length
        ? `<span class="hl">${String(values[i])}</span>`
        : '';
      return result + str + value;
    }, '');
  }
  const name = 'John';
  highlight`Hello, ${name}!`;
  // → "Hello, <span class=\"hl\">John</span>!"
  ```
- **How do you provide default values in destructuring?** Specify `= value` in the pattern, for example `const { title = 'N/A' } = obj`. The default is used when the property value is `undefined`, not when it is `null`.
- **How can you rename variables during destructuring?** Use property: newName syntax (e.g., const { id: userId } = user).

## TypeScript Deep Dive

### Definition

TypeScript is a statically typed superset of JavaScript. It checks types at build time, improves editor tooling, and then erases type syntax so the emitted JavaScript runs normally in browsers and Node.js. Its type system is structural: compatibility is based on object shape, not class or interface names.

As of July 12, 2026, TypeScript 7 is generally available; it was released on July 8 as the native Go-based compiler and language server. TypeScript 7 adopts the modern defaults introduced by the TypeScript 6 bridge release and turns several TS 6 deprecations into errors, including legacy targets, module formats, and resolution modes.

TypeScript 7.0 does not expose a stable programmatic compiler API. Tools that embed TypeScript, including many Angular, Vue, Svelte, Astro, MDX, and linting workflows, may need to keep TypeScript 6 side-by-side until a compatible API is available. Angular 22 officially supports `>=6.0.0 <6.1.0`, not TypeScript 7 for Angular compilation and template tooling.

### Example

```ts
type Role = "admin" | "user";

interface ApiUser {
  id: string;
  role: Role;
  email?: string;
}

function isApiUser(value: unknown): value is ApiUser {
  if (typeof value !== "object" || value === null) return false;

  const candidate = value as Record<string, unknown>;
  return typeof candidate.id === "string" &&
    (candidate.role === "admin" || candidate.role === "user") &&
    (candidate.email === undefined || typeof candidate.email === "string");
}

const routes = {
  home: "/",
  user: "/users/:id",
} as const satisfies Record<string, string>;

type RouteName = keyof typeof routes; // "home" | "user"
```

### Common Interview Questions

- **What problem does TypeScript solve in frontend applications?** It catches many shape and API contract mistakes before runtime, improves refactoring safety, and gives better editor feedback for props, events, forms, API responses, and state. It does not make JavaScript runtime-safe by itself.
- **What does it mean that TypeScript types are erased?** Types are used by the compiler and tooling, then removed from emitted JavaScript. Runtime checks still need JavaScript logic or schema validators such as Zod or Valibot.
- **What is structural typing?** TypeScript compares object shapes. If a value has the required properties with compatible types, it can be assigned even if it does not explicitly implement an interface.
- **When should you rely on inference versus explicit annotations?** Let TypeScript infer local variables and simple return values. Add annotations at public boundaries: exported functions, component props, API DTOs, event payloads, and places where widening would hide intent.
- **What is the difference between type and interface?** Both can describe object shapes. Interfaces support declaration merging and are natural for public object contracts. Type aliases can express unions, tuples, primitives, mapped types, conditional types, and more complex compositions.
- **How do unions and intersections differ?** A union (`A | B`) means a value can be one of several alternatives. An intersection (`A & B`) combines requirements, so a value must satisfy all intersected types.
- **What is a discriminated union and why is it useful?** It is a union where every variant has a shared literal field such as `kind` or `status`. TypeScript can narrow by that field, making state machines, async states, and reducer actions safer.
- **How does narrowing work?** TypeScript narrows broad types using runtime checks such as `typeof`, `instanceof`, `in`, equality checks, discriminants, custom type guards (`value is T`), and assertion functions (`asserts value is T`).
- **How do any, unknown, never, and void differ?** `any` disables type checking. `unknown` accepts any value but requires narrowing before use. `never` represents impossible values and is useful for exhaustiveness checks. `void` means a function's return value should not be used.
- **How should null and undefined be handled?** With `strictNullChecks`, nullable values must be represented explicitly (`string | null`) and narrowed before use. Modern TS 6/7 defaults enable strict checking, while older configurations may still need to enable it explicitly.
- **What are generics and when should you use constraints?** Generics let functions and types preserve relationships between inputs and outputs. Constraints (`T extends { id: string }`) restrict what callers can pass while keeping useful inference.
- **How do keyof and indexed access types work?** `keyof T` produces the allowed property names of `T`. `T[K]` retrieves the type of a property. Together they let you write safe helpers for picking, mapping, and updating object fields.
- **What are mapped types and conditional types?** Mapped types transform keys (`{ [K in keyof T]: ... }`). Conditional types choose a type based on another type (`T extends Promise<infer U> ? U : T`). They power many utility types.
- **What are template literal types used for?** They build string literal unions from other types, useful for event names, route names, CSS variable names, or typed API keys.
- **Which utility types should a frontend engineer know?** `Partial`, `Required`, `Readonly`, `Pick`, `Omit`, `Record`, `ReturnType`, `Parameters`, `Awaited`, `NonNullable`, and `Extract`/`Exclude` are common in app and library code.
- **What do as const, satisfies, and const type parameters solve?** `as const` preserves literal types and readonly structure. `satisfies` checks that a value matches a type without widening away its precise inferred type. Const type parameters preserve literal inference inside generic APIs.
- **When should you avoid enums?** Regular enums emit runtime JavaScript and can add bundle/runtime complexity. For many frontend cases, literal unions or `as const` objects are simpler. Enums can still be useful for interop with existing APIs or generated code.
- **What TypeScript 7 configuration changes matter in interviews?** TS 7 inherits TS 6 defaults such as `strict: true`, `module: "esnext"`, and a modern default target. It removes or errors on deprecated choices such as `target: "es5"`, AMD/UMD/SystemJS output, `moduleResolution: "classic"`/`"node10"`, and `baseUrl`. Migrate a project on TS 6 before switching compilers.
- **Which tsconfig options are still worth discussing explicitly?** Even when strict mode is the default, explicitly committed project configuration can document intent. Interview-relevant options include `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, `verbatimModuleSyntax`, `target`, `module`, and `moduleResolution`.
- **What is the main TypeScript 7 ecosystem caveat?** TypeScript 7.0 has no stable programmatic API. The official compatibility package `@typescript/typescript6` allows tools that require the TS 6 API to run alongside the TS 7 `tsc` binary while the ecosystem migrates.
- **How should moduleResolution be chosen?** Use `nodenext` when code runs directly in modern Node.js and follows Node's ESM/CJS rules. Use `bundler` for Vite, Webpack, Rollup, Rsbuild, and similar tools that resolve imports during bundling.
- **How does TypeScript relate to runtime validation?** TypeScript only checks code at compile time. Data from APIs, localStorage, URL params, forms, or third-party scripts is still unknown at runtime; validate it with schemas or guards before trusting it.
- **How is TypeScript used differently in React, Angular, and Vue?** React commonly types props, hooks, events, refs, and server/client boundaries. Angular is TypeScript-first and uses types heavily in DI, typed forms, signals, and services. Vue uses TypeScript through SFC macros like defineProps/defineEmits, template refs, and typed composables.

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
- **What are Rollup plugins and how are they used?** Plugins extend Rollup's capabilities (for example, `@rollup/plugin-node-resolve` resolves packages, `@rollup/plugin-commonjs` converts CommonJS modules, and `@rollup/plugin-terser` minifies output). The old `rollup-plugin-terser` package is deprecated.
- **How do you mark dependencies as external in Rollup?** Use the external option in rollup.config.js so the dependency is not bundled.

### Vite & Legacy Snowpack

#### Definition

Vite is a modern frontend build tool that serves ES modules in development and bundles for production. As of Vite 8.1 (June 2026), Rolldown is the unified Rust-based bundler for dependency optimization and production builds, replacing the earlier split between esbuild and Rollup. Snowpack pioneered native-ESM development workflows, but it is no longer actively maintained and is legacy for new projects.
#### Common Interview Questions

- **Why is Vite fast in development compared to traditional bundler-first workflows?** It serves source files as native ES modules, performs dependency optimization up front, and uses Rolldown's Rust-based pipeline in modern Vite versions for faster transforms and builds.
- **Can you still perform code splitting and production optimization with Vite?** Yes - Vite supports production bundling, code splitting, minification, and plugin-based optimization through Rolldown in current versions.
- **What is hot module replacement (HMR) and how does Vite handle it?** HMR swaps updated modules via the dev server and WebSocket without a full page reload.

### Babel

#### Definition

Babel is a JavaScript compiler that transforms newer ECMAScript syntax into backwards‑compatible code. Presets (e.g., @babel/preset-env, @babel/preset-react) define sets of plugins to support target environments. Babel can also compile JSX and TypeScript when combined with appropriate plugins.
#### Common Interview Questions

- **How does Babel differ from TypeScript?** Babel transpiles syntax but does not perform type checking; TypeScript compiles code and enforces types. You can use Babel with TypeScript to compile TS without type checks or combine both for full type safety and modern syntax.
- **What is a Babel plugin vs. preset?** A plugin transforms specific syntax (e.g., class fields); a preset is a collection of plugins bundled for convenience (e.g., @babel/preset-env targets ES features based on browser support).
- **How does @babel/preset-env decide what to transform?** It uses target environments (browserslist) and can add polyfills with core-js via useBuiltIns.

### Node Package Managers & Publishing

#### Definition

npm, Yarn and pnpm manage packages and dependencies. `package.json` lists dependency ranges, scripts and package metadata; semver versioning uses major.minor.patch. A published package needs at least a valid name and version, while entry points are normally described with `exports` and/or `main`. Publishing also requires reviewing the included files, package access and registry authentication before running `npm publish` or an equivalent command.

#### Common Interview Questions

- **What is the difference between dependencies, devDependencies and peerDependencies?** `dependencies` are installed for package runtime use; `devDependencies` support development, testing, and builds; `peerDependencies` declare a compatible version range of a host package that should be shared with the consumer (for example, React). Since npm 7, compatible peers are installed automatically when possible, while conflicts can still require consumer resolution.
- **How does pnpm differ from npm or Yarn?** pnpm uses a content-addressable store plus a project-specific virtual store, which can reduce duplicate disk usage. Depending on the filesystem and configuration, pnpm may clone/copy-on-write, hard-link or copy package files. Yarn can use Plug'n'Play, `node_modules`, or a pnpm-style linker, while npm normally installs a `node_modules` tree; compare the chosen linker and workflow rather than assuming one universal layout.
- **What is semantic versioning (semver) and how do caret (^) and tilde (~) ranges work?** Semver defines major.minor.patch versions; ^1.2.0 allows any minor/patch updates up to (but not including) 2.0.0, while ~1.2.0 allows patch updates up to (but not including) 1.3.0.
## Threading & Data Streams

This section covers concurrency and streaming APIs available in the browser.

### Service Workers

#### Definition

A service worker is an event-driven worker that can intercept requests for pages in its scope, enabling offline caching, background sync, and push notifications. Its registration can persist across navigations and browser restarts, but the worker process itself can be terminated when idle and restarted for a later event. Code must not rely on in-memory state surviving between events.
#### Common Interview Questions

- **What are the lifecycle phases of a service worker?** A new worker installs, may wait while an older worker controls clients, and then activates. `fetch`, `push`, and `sync` are functional events handled after activation, not lifecycle phases themselves.
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

- **Explain the Same-Origin Policy (SOP) and CORS.** An origin is the tuple of scheme, host, and port. SOP restricts how documents/scripts from one origin can interact with resources from another. CORS is an HTTP-header protocol through which a server lets a browser expose selected cross-origin responses to frontend code.
- **What is a CORS preflight request and when does it occur?** The browser sends an OPTIONS request before certain cross-origin requests (non-simple methods, custom headers, or JSON content type) to check if the server allows it.
- **What makes a request "simple" under CORS rules?** Simple requests use GET/HEAD/POST with only simple headers and Content-Type of text/plain, application/x-www-form-urlencoded, or multipart/form-data; these skip preflight.
- **What does the Access-Control-Allow-Credentials header do?** It permits the browser to expose a credentialed cross-origin response when the request also uses credentials mode (`credentials: "include"` or Angular's `withCredentials`). The header does not itself send credentials, and a credentialed response requires a specific allowed origin rather than `*`.
- **How do CORS errors show up in the browser?** The request may succeed on the network, but the browser blocks access to the response and logs a CORS error in the console.

### REST vs GraphQL vs WebSockets

#### Definition

REST is an architectural style that uses HTTP semantics to operate on resources identified by URLs. GraphQL is a query language and execution model with a strongly typed schema that lets clients select fields. A single HTTP endpoint is a common deployment convention, not a GraphQL requirement. WebSockets provide full-duplex communication over a persistent connection.
#### Common Interview Questions

- **When might you choose GraphQL over REST?** When clients require flexible queries, need to avoid over/under‑fetching data and benefit from a strongly typed schema. GraphQL also helps reduce the number of network requests.
- **What are the advantages of WebSockets over HTTP polling?** WebSockets maintain a persistent connection and support real‑time, bidirectional communication, reducing latency and overhead compared to repeated HTTP requests.
- **How do you secure REST and GraphQL APIs?** Use HTTPS, authentication (e.g., OAuth 2.0, JWT), authorization checks, input validation and rate limiting.

### WebSockets & Real-Time Communication

#### Definition

The WebSocket protocol provides full-duplex, bidirectional communication over a long-lived connection. Unlike HTTP's request-response model, it allows either side to send messages after the opening handshake. SignalR and Socket.IO are higher-level real-time frameworks that can use WebSockets but also negotiate or fall back to other transports; they are not merely thin wrappers over the WebSocket protocol.
#### Example (Native WebSocket API)

```js
const socket = new WebSocket('wss://example.com/socket'); // Replace with your endpoint.
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
  const initialReconnectDelay = 1000;
  const maxReconnectDelay = 30_000;
  let reconnectDelay = initialReconnectDelay;

  function connect() {
    const ws = new WebSocket('wss://example.com/socket');
    // ... setup event handlers ...
    ws.onclose = function (event) {
      const delay = reconnectDelay;
      console.log(`Socket closed. Reconnecting in ${delay}ms.`, event.reason);
      setTimeout(connect, delay);
      reconnectDelay = Math.min(reconnectDelay * 2, maxReconnectDelay);
    };
    ws.onopen = function () {
      reconnectDelay = initialReconnectDelay;
    };
  }
  connect();
  ```
- **What security considerations are important for WebSocket connections?** Use wss:// (WebSocket Secure), the equivalent of HTTPS, to encrypt data in transit.
  Validate and sanitize all data received over the WebSocket connection on both the client and server, just as you would with HTTP requests, to prevent injection attacks.
  Prefer authentication during the opening handshake when the stack supports it, using a session cookie, an authorization-capable client, or a short-lived connection ticket. Browser WebSocket constructors cannot set arbitrary `Authorization` headers. Query-string tokens can leak through logs and should be short-lived if unavoidable.
  Authentication in the first application message is also a valid protocol design, provided the server rejects all other messages until authentication succeeds and applies timeouts and rate limits.
  Validate the `Origin` header for browser clients to prevent cross-site WebSocket hijacking, authorize every message/action rather than only the connection, validate message schemas and sizes, and enforce rate limits, heartbeat/idle timeouts, and backpressure.

### HTTP Interceptors & OAuth 2.0

#### Definition

HTTP interceptors (client side) allow you to modify requests/responses, add authentication tokens, or handle errors. OAuth 2.0 is an authorization framework for delegated access. Authorization Code with PKCE is the current browser-app pattern; the implicit flow is a legacy flow and should not be recommended for new SPAs.
#### Common Interview Questions

- **What is the difference between authentication and authorization?** Authentication verifies user identity; authorization determines what the authenticated user can access.
- **Which OAuth 2.0 flow would you use in a single‑page application?** The authorization code flow with PKCE (Proof Key for Code Exchange) is recommended for SPAs; it uses a code verifier and challenge to secure the exchange.
- **How can you implement request retry logic in an interceptor?** Retry only transient failures and, by default, idempotent requests. Cap the attempt count, use exponential backoff with jitter, respect `Retry-After` when present, and avoid blindly retrying authentication, authorization, validation, or other permanent failures.

### MSAL (Microsoft Authentication Library)

#### Definition

MSAL (Microsoft Authentication Library) is a family of libraries for OAuth 2.0 and OpenID Connect with the Microsoft identity platform, including Microsoft Entra ID (formerly Azure AD), Microsoft accounts, and External ID scenarios. `@azure/msal-browser` supports browser public clients with Authorization Code + PKCE, account discovery, token acquisition, and browser cache management. Server-side applications use a confidential-client library and keep secrets and server tokens outside the browser.
#### Example

```js
import {
  InteractionRequiredAuthError,
  PublicClientApplication
} from "@azure/msal-browser";

const msalConfig = {
    auth: {
        clientId: "your-azure-ad-app-id",
        authority: "https://login.microsoftonline.com/your-tenant-id",
        redirectUri: "http://localhost:3000",
    },
    cache: {
        cacheLocation: "sessionStorage",
    }
};

const msalInstance = new PublicClientApplication(msalConfig);
await msalInstance.initialize();
await msalInstance.handleRedirectPromise();

// Browser-only SPA pattern. Coordinate interactive calls in real applications
// so that only one login/token popup is active at a time.
const getTokenSecurely = async () => {
    let account = msalInstance.getAllAccounts()[0];

    if (!account) {
        const login = await msalInstance.loginPopup({ scopes: ["User.Read"] });
        account = login.account;
    }

    const request = { scopes: ["User.Read"], account };

    try {
        return await msalInstance.acquireTokenSilent(request);
    } catch (error) {
        if (error instanceof InteractionRequiredAuthError) {
            return msalInstance.acquireTokenPopup(request);
        }
        throw error;
    }
};

// BFF alternative: the browser receives business data, not an access token.
// The backend owns the OAuth client, token cache, refresh flow, and API calls.
const getProfileThroughBff = async (csrfToken) => {
    const response = await fetch('/api/profile', {
        credentials: 'include',
        headers: { 'X-CSRF-Token': csrfToken }
    });
    if (!response.ok) throw new Error(`Request failed: ${response.status}`);
    return response.json();
};
```

#### Common Interview Questions

- **What must happen before using `PublicClientApplication` APIs?** In current `msal-browser`, call and await `initialize()` first. If the app uses redirects, process `handleRedirectPromise()` during startup before beginning another interactive operation.
- **When should silent acquisition fall back to interaction?** Only when MSAL reports an interaction-required condition, such as `InteractionRequiredAuthError`. Network, configuration, and programming errors should be surfaced or handled according to their actual cause rather than automatically opening a popup.
- **What's the security trade-off between MSAL cache locations?** `sessionStorage` is tab-scoped, `localStorage` supports broader persistence and cross-tab scenarios, and `memoryStorage` is lost on navigation/reload. MSAL v4 encrypts persisted cache artifacts for persistence semantics, but browser JavaScript must still be able to use them; encryption is not a security boundary against XSS. Strong CSP, dependency hygiene, output encoding, and avoiding unsafe DOM sinks remain essential.
- **Is an HttpOnly cookie another MSAL SPA cache location?** No. It belongs to a server-managed session/BFF architecture. The backend stores or acquires downstream access tokens, and the browser receives a `Secure`, `HttpOnly`, appropriately `SameSite` session cookie. Because cookies are sent automatically, the BFF must also implement CSRF defenses and validate request origins.
- **What is a real BFF token pattern?** The browser calls its own backend for application operations. The backend attaches access tokens when calling downstream APIs and returns business data, not raw access or refresh tokens. An endpoint named `/token` that returns an access token to JavaScript defeats the main token-isolation benefit.
- **How does cache location affect SSO?** Cache location affects how the SPA shares and restores local account/token state. SSO also depends on the Microsoft identity provider's session cookies and tenant policy, such as "Keep me signed in"; `localStorage` alone does not guarantee authentication across browser sessions.
- **What security responsibilities remain outside MSAL?** MSAL implements the client-side authorization protocol, PKCE, cache, and interaction handling. The resource API—not the SPA—must validate access-token signature, issuer, audience, lifetime, and authorization claims. Ordinary bearer access tokens do not provide replay protection by default.

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

MobX is a reactive state management library that uses observables and automatic dependency tracking; Vuex is a state container for Vue.js with a single store, mutations and actions. Other libraries like Zustand and Jotai provide lightweight state management for React. Recoil is archived/legacy and should mainly be recognized for older codebases.
#### Common Interview Questions

- **How do MobX and Redux differ?** MobX uses observable state and automatic dependency tracking, while Redux models updates as explicit actions and immutable reducer transitions. MobX often needs less ceremony; Redux offers highly explicit data flow and mature debugging/tooling. Either can scale when its patterns fit the team and application.
- **Why would you use a state management library at all?** To manage shared state across components, support undo/redo, time‑travel debugging, or maintain predictable data flow.
- **What is a selector in a state management library?** A function that derives or selects a piece of state; selectors can be memoized to avoid unnecessary recomputations.

### Lodash & Utility Libraries

#### Definition

Lodash is a modular utility library offering functions for array and object manipulation, deep cloning, debouncing/throttling, memoization and more. It can be imported per function to reduce bundle size.
#### Common Interview Questions

- **When would you choose Lodash over native JavaScript functions?** For convenience functions not available natively (e.g., _.cloneDeep) and cross‑browser consistency; however, modern JavaScript provides many alternatives (e.g., Array.map, Object.keys).
- **How can you reduce Lodash’s footprint in your bundle?** Import only the functions you use (for example, `import debounce from 'lodash/debounce.js'`, depending on the toolchain/package entry) and verify the emitted bundle. An ESM build can also enable tree shaking when the import path and bundler support it.
- **What is the difference between `_.assign` and `_.merge`?** `_.assign` copies source properties shallowly onto the destination. `_.merge` recursively combines supported nested objects and mutates its destination, so use a fresh destination when the inputs must remain unchanged.
- **What are debouncing and throttling?** Write a simple debounce function.
  Debouncing: Group multiple sequential calls into one; wait until a period of inactivity before invoking the function (e.g., search suggestions).
  Throttling: Ensure a function is called at most once in a specified period (e.g., infinite scroll). Debounce implementation:

  ```js
  function debounce(func, delay) {
    let timeoutId;

    return function (...args) {
      clearTimeout(timeoutId);
      timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
  }
  ```

### GraphQL & Apollo

#### Definition

GraphQL is a query language and runtime that lets clients specify exactly what data they need. Apollo Client manages GraphQL queries on the client, caching responses and providing hooks like useQuery and useMutation.
#### Common Interview Questions

- **How does Apollo Client cache work?** Apollo normalizes query responses by IDs and fields, allowing efficient updates and refetches. Cache policies determine how results are stored and merged.
- **What is the difference between queries and mutations in GraphQL?** Queries read data; mutations modify data and can have side effects. Both accept variables and return a defined data shape.
- **How do subscriptions work in GraphQL?** A subscription is a long-lived operation that delivers multiple results over time. WebSocket transports are common, but the GraphQL specification does not require one transport; a stack may also use multipart HTTP or another streaming transport. Apollo Client exposes `useSubscription` and must be configured with the transport supported by the server.

## Tools & Testing

### Linters & Formatters

#### Definition

Linters such as ESLint analyze code for potential errors and style violations. Formatters like Prettier automatically format code according to a specified style. Git hooks (e.g., Husky, lint‑staged) run these tools before commits to maintain code quality.
#### Common Interview Questions

- **Why use both ESLint and Prettier?** ESLint finds problematic code patterns and enforces code-quality rules; Prettier formats code. A common setup runs them separately and uses `eslint-config-prettier` to disable conflicting formatting rules. `eslint-plugin-prettier` can run Prettier as an ESLint rule, but Prettier does not generally recommend that slower, noisier workflow.
- **How do Git hooks improve code quality?** Pre‑commit hooks can run linters, formatters and tests, preventing bad code from entering the repository.
- **What is lint‑staged?** A tool that runs linters on only the staged files, reducing execution time and ensuring only changed files are checked.

### Testing Frameworks

#### Definition

Jest, Mocha, Jasmine and Vitest are testing frameworks; React Testing Library is the recommended default for React component tests; Cypress and Playwright enable end-to-end testing in the browser. Enzyme is legacy and has no official modern React 18/19 support. Angular CLI has used Vitest by default for new projects since Angular 21, including current Angular 22 projects; Karma remains supported for existing projects and migration. Tests are categorized as unit, integration and end-to-end.
#### Common Interview Questions

- **What is the difference between unit and integration tests?** Unit tests focus on individual functions or components in isolation; integration tests verify interactions between modules; end‑to‑end tests validate the entire application flow.
- **How do you mock HTTP requests in tests?** Use libraries like jest.mock, msw (Mock Service Worker) or Axios mock adapter to simulate network requests without hitting real endpoints.
- **Why prefer React Testing Library over Enzyme for modern React?** React Testing Library encourages testing components through user interactions and the DOM rather than implementation details, leading to more maintainable tests. Enzyme is tied to older React internals and lacks official React 18/19 support.

### Storage APIs

#### Definition

Web Storage (LocalStorage and SessionStorage) stores key–value pairs in the browser; LocalStorage persists across sessions until cleared, SessionStorage lasts for the page session. IndexedDB is a transactional, NoSQL database for storing larger amounts of structured data on the client.
#### Common Interview Questions

- **What are the security concerns with localStorage vs. cookies for storing tokens?** A token in `localStorage` can be read by JavaScript that executes through an XSS vulnerability. An `HttpOnly` cookie cannot be read by JavaScript, `Secure` restricts it to HTTPS, and `SameSite` limits cross-site sending. `SameSite` mitigates many CSRF attacks but does not replace appropriate CSRF defenses, origin checks, short sessions and sound XSS prevention.
- **When should you use IndexedDB over LocalStorage?** When you need to store large or complex data (objects, files) or perform queries; IndexedDB supports indexes.
- **How do localStorage and sessionStorage differ?** LocalStorage persists until cleared; SessionStorage is cleared when the tab or browser is closed.
- **Compare localStorage, sessionStorage, and cookies.**
  | Feature | localStorage | sessionStorage | Cookies |
  | --- | --- | --- | --- |
  | Typical capacity | Often several MB; browser/quota dependent | Often several MB; browser/quota dependent | Roughly 4 KB per cookie; browser limits apply |
  | Lifetime | Persistent until manually cleared | Lasts only for the session (tab is open) | Set with max-age/expires |
  | Exposure/transport | Readable by same-origin JavaScript; not sent automatically | Readable by that tab's same-origin JavaScript; not sent automatically | Sent automatically only on matching domain/path requests; `HttpOnly` can block JavaScript reads |
  | Typical use | Non-sensitive preferences or caches | Temporary tab-scoped state | Server session identifiers or small server-readable state, with CSRF/privacy controls |

### Graphics & Media

#### Definition

Canvas and SVG are two distinct web technologies for creating graphics in the browser. Canvas is a raster-based drawing API that provides a pixel-level interface for programmatic rendering, ideal for dynamic, performance-intensive graphics. SVG (Scalable Vector Graphics) is an XML-based vector format that describes shapes as mathematical objects, making it resolution-independent and DOM-accessible. The choice between them depends on factors like interactivity, scalability, performance requirements, and accessibility needs.
#### Common Interview Questions

- **Explain Canvas and SVG and when to use each.** Canvas: A raster API that lets you draw pixels programmatically. Best for complex graphics, games and pixel-based operations; it is resolution-dependent. SVG: A vector API using XML to describe shapes. Best for icons, logos, charts and maps that need to scale without quality loss; resolution-independent and accessible via the DOM.
- **What are the performance implications of Canvas vs SVG?** Canvas performs better for many moving objects or frequent redraws (games, animations) because it operates at the pixel level. SVG can become slow with thousands of elements since each is a DOM node, but excels for static or moderately complex vector graphics.
- **How does accessibility differ between Canvas and SVG?** SVG has DOM elements that can be given names, descriptions, semantics and keyboard interactions, but it is not automatically accessible. Canvas exposes pixels rather than individual drawn objects; interactive canvas content needs an accessible DOM alternative or fallback, keyboard support and synchronized semantics. Adding ARIA only to the `<canvas>` element is not sufficient for a complex interactive scene.
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

### Common Interview Questions

- **Explain the difference between git merge and git rebase.** `git merge` integrates histories. It may fast-forward when the destination has not diverged; otherwise it normally creates a merge commit (or can be forced to with `--no-ff`). `git rebase` replays commits onto a new base and therefore creates new commit identities. Rebase is useful for cleaning up private/local work, but avoid rebasing commits other people already depend on unless the team explicitly coordinates it.
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
- **How does git cherry-pick differ from git merge or git rebase?** `git merge` integrates branch histories, while `git rebase` replays a sequence of commits onto a new base. `git cherry-pick` is more selective: it can apply one commit, several named commits, or a commit range without integrating the source branch as a branch history.
- **When is it appropriate to use git cherry-pick?** It is appropriate for specific scenarios like:
  - Hotfixes: applying a critical bug fix from a development branch to a stable release or main branch.
  - Saving work: rescuing a specific, well-defined change from a messy or abandoned feature branch.
  - Partial feature integration: moving a single, self-contained feature or commit from one long-running branch to another without merging everything.

## Agile, Kanban & Scrum

### Definition

Agile is a project management philosophy based on the Agile Manifesto, emphasizing iterative development, customer collaboration, and adaptability to change. Scrum and Kanban are specific frameworks that implement Agile principles:

| Aspect | Scrum | Kanban |
| --- | --- | --- |
| Approach | Iterative work in fixed-length Sprints of one month or less | Optimizes flow through an explicitly defined workflow |
| Accountabilities | Product Owner, Scrum Master, Developers | No prescribed roles; it can overlay an existing way of working |
| Cadence | The Sprint contains Planning, Daily Scrum, Review and Retrospective | Continuous flow, with feedback cadences chosen by the organization |
| Change Policy | Scope may be clarified and renegotiated as long as the Sprint Goal is not endangered | Policies for selecting and moving work are made explicit and may evolve |
| Metrics | Scrum does not prescribe a metric set | Work in progress, throughput, work item age and cycle time are mandatory flow metrics |

#### Example

```js
// Scrum Guide 2020 terminology
const scrumFramework = {
  accountabilities: ["Product Owner", "Scrum Master", "Developers"],
  artifacts: ["Product Backlog", "Sprint Backlog", "Increment"],
  containerEvent: "Sprint (one month or less)",
  containedEvents: [
    "Sprint Planning",
    "Daily Scrum",
    "Sprint Review",
    "Sprint Retrospective"
  ],
  ongoingActivities: ["Product Backlog refinement"]
};

// Kanban Guide: manage and improve flow
const kanbanSystem = {
  practices: [
    "Define and visualize the workflow",
    "Actively manage items in the workflow",
    "Improve the workflow"
  ],
  flowMetrics: ["Work in Progress", "Throughput", "Work Item Age", "Cycle Time"]
};
```

#### Common Interview Questions

- **What are the key differences between Scrum and Kanban?** Scrum is a framework built around a Product Goal, Sprint Goals, fixed-length Sprints and defined accountabilities, events and artifacts. Kanban is a strategy for optimizing value flow through an explicitly defined workflow; it can augment Scrum or another existing way of working. Kanban requires controlling WIP, but that control does not have to be a limit on every board column.
- **What are the three Scrum accountabilities and their responsibilities?**
  - Product Owner: Maximizes product value, manages backlog, defines priorities.
  - Scrum Master: Establishes Scrum and is accountable for the Scrum Team's effectiveness, including causing impediments to be removed.
  - Developers: Create a usable, valuable Increment that meets the Definition of Done each Sprint.
- **Is Product Backlog refinement a formal Scrum event?** No. It is an ongoing activity in which Product Backlog items are broken down and further defined. The five formal Scrum events are the Sprint and its four contained events.
- **What is the advantage of Kanban having "no prescribed roles"?** Kanban can be implemented on top of existing team structures and roles without organizational disruption, making it easier to adopt incrementally and suitable for teams with established responsibilities.
- **Name the five formal Scrum events and their purposes.**
  - Sprint: The container for all other events; it creates a regular cadence for turning ideas into value.
  - Sprint Planning: Determine what can be delivered in the upcoming sprint and how.
  - Daily Scrum: A 15-minute event for Developers to inspect progress toward the Sprint Goal and adapt the Sprint Backlog.
  - Sprint Review: Inspect the increment and adapt the Product Backlog.
  - Sprint Retrospective: Plan improvements for next sprint.
- **What metrics would you track to improve team performance in each framework?**
  - Scrum: Choose evidence that helps the team inspect progress toward goals and outcomes. Velocity, burnup and burndown can support forecasting, but Scrum does not prescribe them and they should not be used as individual productivity targets.
  - Kanban: Track the four required flow metrics: WIP, throughput, work item age and cycle time. A cumulative-flow diagram is a useful visualization, not a replacement for those measures.

## SEO & Accessibility

### Definition

Search Engine Optimization (SEO) is the practice of improving website visibility in organic search results through technical optimization, quality content, and user experience enhancements. Accessibility (a11y) ensures web content is usable by people with disabilities. WCAG 2.2 is the current W3C Recommendation and organizes its success criteria under four principles: Perceivable, Operable, Understandable and Robust.
#### Example

```html
<!-- SEO: Semantic HTML and meta tags -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Frontend Interview Cookbook - JavaScript Fundamentals</title>
  <meta name="description" content="Master JavaScript type system, closures, promises, and more for frontend interviews.">
  <link rel="canonical" href="https://example.com/javascript-fundamentals">
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
  <!-- Prefer native semantics; add ARIA only where it adds information. -->
  <header>
    <nav aria-label="Main navigation">
      <ul>
        <li><a href="#home" aria-current="page">Home</a></li>
      </ul>
    </nav>
  </header>
  <main>
    <h1>JavaScript Fundamentals</h1>
    <details>
      <summary>Show content</summary>
      <div>Content here...</div>
    </details>
  </main>
</body>
</html>
```

#### Common Interview Questions

- **What are the three pillars of technical SEO?** Crawling (can search engines discover your content?), Indexing (can they understand and store it?), and Ranking (how relevant is your content to search queries?).
- **What is the difference between ARIA labels and native HTML semantics?** Native HTML elements (`<button>`, `<nav>`) provide built-in accessibility. ARIA attributes (aria-label, role) should only be used when native semantics are insufficient, as they don't provide actual functionality.
- **How does site performance affect SEO?** Google uses Core Web Vitals (LCP, INP and CLS) as part of its page-experience signals, but there is no single page-experience score and relevant, useful content can outweigh weaker performance. Improve performance primarily for users and conversions; do not present bounce rate as a confirmed direct ranking signal.
- **What are the four WCAG principles (POUR)?** Perceivable (available to senses), Operable (usable via various input methods), Understandable (clear and predictable), and Robust (compatible with current and future tools).
- **How would you make a complex data table accessible?** Use a real `<table>` with a descriptive `<caption>` and `<th>` cells. For simple tables, associate headers with `scope`. For complex multi-level headers, give each header an `id` and list the relevant IDs in each data cell's `headers` attribute. `aria-describedby` supplies an additional description; it is not the standard cell-to-header association mechanism.

## Internationalization (i18n) & Theming

### Definition

Internationalization (i18n) is the design and development of applications to adapt to various languages, regions, and cultures without engineering changes. It involves translation files, locale-specific formatting (dates, numbers, currencies), and pluralization rules. Theming allows visual customization of an application's appearance (colors, typography, spacing) through CSS variables, design tokens, or component library configurations, supporting light/dark modes and brand variations.
#### Example

```jsonc
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
```

```css
/* themes.css */
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
```

```jsx
// ThemeContext.jsx
import React, { createContext, useState } from 'react';

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

### Definition

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

- **What's the difference between reflow and repaint?** Reflow (layout) recalculates geometry for affected elements. Paint records/draws visual details, while compositing assembles layers. A layout change often requires paint and compositing for affected regions, but modern engines can limit the damaged area or skip work when cached layers remain valid, so "reflow always triggers repaint" is too absolute. A visual-only change such as color can require paint without layout, while a transform may be composited without either.
- **How does code splitting improve performance, and what are the different types?** Code splitting reduces initial bundle size by loading code only when needed. Types include: route-based (split by pages), component-based (split heavy components), and dynamic imports (load on interaction).
- **What are effective caching strategies for static assets?** Use Cache-Control headers with long max-age (e.g., 1 year) and unique filenames for immutable assets. For API responses, use ETag or Last-Modified headers for conditional requests.
- **Why does JavaScript block parsing by default, and how can you prevent this?** A parser-inserted classic script without `async` or `defer` can inspect or modify the document, so the parser waits for it to download and execute. For external classic scripts, use `defer` when execution should wait for parsing and preserve document order, or `async` for independent scripts that may execute as soon as they are ready. Module scripts are deferred by default.
- **What's the difference between `async` and `defer`?** For external classic scripts, both allow downloading in parallel with parsing. `async` executes as soon as the script is ready, can interrupt parsing and does not preserve order. `defer` preserves document order and executes after parsing, before `DOMContentLoaded`. A classic script with neither attribute is parser-blocking. For module scripts, `defer` has no effect because modules already defer by default; `async` makes the module execute as soon as its dependency graph is ready.
- **How do you identify performance bottlenecks?** Use Chrome DevTools Performance tab to record and analyze runtime performance, Lighthouse for audits, Core Web Vitals (LCP, INP, CLS) for user-centric metrics, and Bundle Analyzer for bundle size insights.
- **What are common techniques to reduce Cumulative Layout Shift (CLS)?** Specify width and height attributes on images/video, reserve space for dynamic content, avoid inserting content above existing content, and use transform animations instead of properties that trigger layout.
- **When would you use a development proxy vs. configuring CORS on the server?** A development proxy gives the browser a same-origin URL and is convenient for local work. In production, configure narrowly scoped CORS when browsers are intended to call a cross-origin API directly. A reverse proxy or backend-for-frontend can instead be the deliberate production architecture when it owns sessions, hides upstream services or centralizes policy; it is not inherently an unnecessary hop.

## Angular vs React vs Vue Cheat Sheet

The following comparisons summarize key differences between Angular, React, and Vue across common categories:

| Category | Angular | React | Vue |
| --- | --- | --- | --- |
| Architecture & Philosophy | Full-featured, "batteries-included" framework with strong opinions. | Lean UI library focused on flexibility, requiring choices for routing, state, etc. | Progressive framework; can be used for simple enhancement or full SPAs with official libraries. |
| Building Blocks | Components, templates, and directives compose the UI; services provide logic. | Function components and hooks compose the UI; logic is encapsulated in custom hooks. | Single-File Components (SFCs) with templates, script, and style; logic via Options API or Composition API. |
| Template Syntax | HTML templates with built-in control flow (`@if`, `@for`, `@switch`), bindings such as `[(ngModel)]`, and pipes; legacy structural directives remain supported. | JSX with JavaScript expressions, conditions and array operations. | HTML-based templates with directives (`v-if`, `v-for`, `v-model`) and template expressions. |
| Loops & Conditionals | Prefer `@for`, `@if` and `@switch` in modern Angular; `*ngFor`, `*ngIf` and `[ngSwitch]` remain relevant in older code. | Use array methods such as `map` plus JavaScript conditions, short-circuit expressions or ternaries. | Use `v-for`, `v-if`, `v-else-if`, `v-else` and `v-show`. |
| Styling & Classes | Binding syntax: [ngClass], [ngStyle]. | Use className, conditional expressions, or libraries like classnames. | Direct class/style binding, :class (object/array), :style (object/array) bindings. |
| Local State & Reactivity | Signals and RxJS; zoneless is the default for new projects since Angular 21, and new Angular 22 applications default components to OnPush. | `useState` and `useReducer`; state updates schedule rendering work. | `ref()` and `reactive()` in Composition API, or `data()` in Options API, with automatic dependency tracking. |
| Global State Management | Services + RxJS, NgRx (Redux), NgRx/ComponentStore, Akita. | Context API, Redux Toolkit, Zustand, Jotai, MobX; Recoil only in legacy codebases. | Pinia (official, modern), Vuex (legacy). Composables for shared stateful logic. |
| DI & Services | Built-in hierarchical Dependency Injection system injects services. | Context API or external libraries for sharing data. | provide()/inject() for component-scoped dependency injection. |
| Routing | `@angular/router` with declarative routes, guards and resolvers. | React Router v8 with Declarative, Data and Framework modes. | Vue Router 5 with `<RouterLink>` and `<RouterView>`. |
| Forms | Template-driven and reactive forms with built-in validators; Signal Forms are stable in Angular 22. | Controlled/uncontrolled inputs; libraries such as React Hook Form or Formik are optional. | `v-model` with modifiers such as `.lazy`, `.number` and `.trim`; Vuelidate/VeeValidate are options for complex validation. |
| Lazy Loading | Built-in with loadComponent for standalone routes or loadChildren for route arrays/NgModules. | Code splitting with React.lazy and Suspense. | Dynamic imports in Vue Router (component: () => import(...)). |
| SSR & Hydration | `@angular/ssr` provides server rendering and hydration; Angular Universal is the historical name. | Next.js App Router is a mainstream RSC path; React Router Framework Mode is another full-stack option. | Nuxt 4 is the mainstream full-stack option; custom Vue SSR is also possible. |
| Testing | Angular CLI uses Vitest by default for new projects (since v21); Karma is supported for existing projects, with TestBed for Angular tests. | Jest/Vitest with React Testing Library; Enzyme is for legacy code. | Vitest/Jest with Vue Test Utils; Cypress or Playwright for E2E. |
| CLI & Build | Angular CLI handles scaffolding, building, and testing. | Frameworks are recommended for new apps; for custom setups use Vite, Parcel, Rsbuild, or custom Webpack. Create React App is unmaintained/not recommended for new apps. | Vue CLI (legacy), Vite (modern, recommended) with official plugin. |
| i18n | Built-in i18n and locale support. | Libraries like react-intl or next-i18next. | Vue I18n (official library). |
| Learning Curve | Steeper due to broad API surface and TypeScript-first nature. | Moderate; easy to start, mastery requires understanding of ecosystem. | Gentle; easy to learn from HTML/JS, with a gradual progression to advanced concepts. |
| Microfrontends | Angular Elements or Module Federation. | Module Federation, Single-SPA or Webpack 5 for integrating React apps. | Module Federation (via Vite or Webpack). |

## Bonus

See [BONUS.md](BONUS.md) for the narrative walkthrough.

## Changelog

See [CHANGELOG.md](CHANGELOG.md).

## Author

LinkedIn: https://www.linkedin.com/in/igor-%C5%BEivkovi%C4%87-124431124/

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
