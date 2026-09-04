# JavaScript Cheatsheet (ES6+)

## Variables
```js
var x = 1;    // function-scoped, hoisted — avoid
let y = 2;    // block-scoped, reassignable
const z = 3;  // block-scoped, not reassignable (prefer this)
```

## Data Types
```js
// Primitives
let str  = "Hello";          // String
let num  = 42;               // Number
let big  = 9007199254740993n;// BigInt
let bool = true;             // Boolean
let sym  = Symbol("id");     // Symbol
let n    = null;             // Null
let u    = undefined;        // Undefined

// Reference
let arr = [1, 2, 3];         // Array
let obj = { name: "Ahmad" }; // Object
let fn  = () => {};          // Function

// Type check
typeof "hello";  // "string"
typeof null;     // "object" (JS quirk!)
Array.isArray([]);// true
```

## String Methods
```js
const s = "  Hello, World!  ";

s.trim()                   // "Hello, World!"
s.trimStart() / trimEnd()  // one side
s.length                   // 15 (after trim)
s.toUpperCase()            // "HELLO, WORLD!"
s.toLowerCase()            // "hello, world!"
s.includes("World")        // true
s.startsWith("Hello")      // true (after trim)
s.endsWith("!")
s.indexOf("o")             // 4
s.lastIndexOf("o")         // 8
s.slice(7, 12)             // "World"
s.substring(7, 12)         // "World"
s.replace("World", "JS")   // replaces first
s.replaceAll("l", "L")
s.split(", ")              // ["Hello", "World!"]
s.padStart(10, "0")        // "00000Hello..."
s.padEnd(10, "-")
s.repeat(3)
s.at(-1)                   // "!" (last char)

// Template literals
const name = "Rana";
`Hello, ${name.toUpperCase()}! Result: ${2 + 2}`

// Tagged template
console.log`Hello ${name}`;
```

## Array Methods
```js
const arr = [1, 2, 3, 4, 5];

// Mutating
arr.push(6)          // add to end → returns new length
arr.pop()            // remove from end → returns removed
arr.unshift(0)       // add to start
arr.shift()          // remove from start
arr.splice(1, 2)     // remove 2 at index 1 → returns removed
arr.splice(1, 0, 9)  // insert 9 at index 1
arr.sort((a,b) => a-b)  // ascending
arr.reverse()
arr.fill(0, 1, 3)    // fill with 0 from index 1 to 3

// Non-mutating (return new array)
arr.map(x => x * 2)
arr.filter(x => x > 2)
arr.slice(1, 3)      // [2, 3]
arr.concat([6,7])
arr.flat()           // flatten one level
arr.flat(Infinity)   // flatten all levels
arr.flatMap(x => [x, x*2])
arr.toSorted((a,b) => a-b)    // ES2023 non-mutating sort
arr.toReversed()               // ES2023

// Search / Check
arr.find(x => x > 3)           // 4
arr.findIndex(x => x > 3)      // 3
arr.findLast(x => x < 4)       // 3  (ES2023)
arr.indexOf(3)                 // 2
arr.includes(4)                // true
arr.every(x => x > 0)         // true
arr.some(x => x > 4)          // true

// Aggregate
arr.reduce((acc, x) => acc + x, 0)   // 15
arr.reduceRight((acc, x) => acc + x)  // right to left
arr.join(", ")                         // "1, 2, 3, 4, 5"

// Iteration
arr.forEach((x, i) => console.log(i, x));

// Spread & Destructuring
const [a, b, ...rest] = arr;          // a=1, b=2, rest=[3,4,5]
const newArr = [...arr, 6, 7];

// Create
Array.from({ length: 5 }, (_, i) => i);  // [0,1,2,3,4]
Array.from("hello");                      // ["h","e","l","l","o"]
Array.of(1, 2, 3);                        // [1,2,3]
```

## Objects
```js
const person = {
  name: "Ahmad",
  age: 20,
  address: { city: "Faisalabad" },
  greet() { return `Hi, I'm ${this.name}`; }
};

// Access
person.name;           // dot
person["age"];         // bracket (dynamic key)
person?.address?.city; // optional chaining — safe

// Destructuring with rename & default
const { name: fullName, age, role = "user" } = person;

// Spread
const updated = { ...person, age: 21, role: "admin" };

// Computed keys
const key = "score";
const obj = { [key]: 100 }; // { score: 100 }

// Static methods
Object.keys(person)       // ["name", "age", "address"]
Object.values(person)     // ["Ahmad", 20, {...}]
Object.entries(person)    // [["name","Ahmad"], ...]
Object.assign({}, person) // shallow clone
Object.freeze(obj)        // prevent modifications
Object.fromEntries([["a",1],["b",2]])  // {a:1, b:2}
```

## Functions
```js
// Function declaration (hoisted)
function add(a, b) { return a + b; }

// Arrow (no own this)
const add = (a, b) => a + b;
const double = x => x * 2;    // single param: no parens needed
const getObj = () => ({ x: 1 }); // return object: wrap in ()

// Default params
const greet = (name = "Guest") => `Hello, ${name}!`;

// Rest & Spread
const sum = (...nums) => nums.reduce((a, b) => a + b, 0);
sum(1, 2, 3, 4);  // 10

// IIFE
(function() { console.log("Runs immediately"); })();
(() => console.log("Arrow IIFE"))();

// Closures
function counter() {
  let count = 0;
  return () => ++count;
}
const inc = counter();
inc(); // 1
inc(); // 2
```

## Promises & Async/Await
```js
// Promise
const fetchUser = (id) => new Promise((resolve, reject) => {
  if (id > 0) resolve({ id, name: "Ahmad" });
  else reject(new Error("Invalid ID"));
});

fetchUser(1)
  .then(user => console.log(user))
  .catch(err => console.error(err))
  .finally(() => console.log("Done"));

// Promise combinators
Promise.all([p1, p2, p3])         // all succeed or first fail
Promise.allSettled([p1, p2, p3])  // all settle (never rejects)
Promise.race([p1, p2])            // first to settle wins
Promise.any([p1, p2])             // first to succeed wins

// Async/Await
async function getUser(id) {
  try {
    const res  = await fetch(`/api/users/${id}`);
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    const data = await res.json();
    return data;
  } catch (err) {
    console.error("Error:", err.message);
    throw err;    // re-throw if caller needs to handle
  }
}

// Parallel async calls
const [users, posts] = await Promise.all([
  fetch("/api/users").then(r => r.json()),
  fetch("/api/posts").then(r => r.json()),
]);
```

## DOM Manipulation
```js
// Selecting
document.getElementById("id");
document.querySelector(".class");       // first match
document.querySelectorAll("div.card");  // NodeList
document.getElementsByClassName("btn");

// Content
el.textContent = "New text";   // safe: no HTML parsing
el.innerHTML   = "<b>Bold</b>"; // parses HTML — XSS risk
el.innerText;                   // visible text only

// Attributes
el.setAttribute("href", "https://...");
el.getAttribute("data-id");
el.removeAttribute("disabled");
el.hasAttribute("required");
el.dataset.userId;    // data-user-id

// Classes
el.classList.add("active", "visible");
el.classList.remove("hidden");
el.classList.toggle("dark");
el.classList.contains("active");   // true/false
el.classList.replace("old", "new");

// Styles
el.style.color = "red";
el.style.cssText = "color:red; font-size:16px";
getComputedStyle(el).color;     // actual computed style

// Create & Insert
const div = document.createElement("div");
div.textContent = "Hello";
div.className = "card";

parent.appendChild(div);
parent.prepend(div);
parent.insertBefore(div, referenceEl);
el.insertAdjacentHTML("afterend", "<p>New</p>");
div.remove();
parent.removeChild(div);

// Traversal
el.parentElement;
el.children;          // HTMLCollection
el.childNodes;        // includes text nodes
el.firstElementChild;
el.lastElementChild;
el.nextElementSibling;
el.previousElementSibling;
el.closest(".card");  // walk up DOM tree
```

## Events
```js
// Add listener
btn.addEventListener("click", handler);
btn.addEventListener("click", handler, { once: true });  // fire once
btn.addEventListener("click", handler, { passive: true }); // perf hint

// Remove listener
btn.removeEventListener("click", handler);

// Event object
btn.addEventListener("click", (e) => {
  e.preventDefault();    // stop default behavior
  e.stopPropagation();   // stop bubbling
  console.log(e.target, e.currentTarget);
  console.log(e.clientX, e.clientY);  // mouse position
});

// Event delegation (efficient)
document.querySelector("#list").addEventListener("click", (e) => {
  if (e.target.matches("li.item")) {
    console.log("Clicked:", e.target.textContent);
  }
});

// Common events
// click, dblclick, contextmenu
// mouseenter, mouseleave, mousemove
// keydown, keyup, keypress
// input, change, submit, reset, focus, blur
// scroll, resize
// DOMContentLoaded, load
// touchstart, touchend

document.addEventListener("DOMContentLoaded", () => {
  // DOM ready — safe to query elements
});
```

## ES Modules
```js
// named export
export const PI = 3.14;
export function add(a, b) { return a + b; }
export { PI, add };

// default export
export default function greet(name) { return `Hi ${name}`; }

// import
import greet from "./greet.js";          // default
import { PI, add } from "./math.js";     // named
import { add as sum } from "./math.js";  // alias
import * as Math from "./math.js";       // all
import("./module.js").then(m => m.fn()); // dynamic import
```

## Useful Snippets
```js
// Debounce
const debounce = (fn, delay) => {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
};

// Throttle
const throttle = (fn, limit) => {
  let inThrottle;
  return (...args) => {
    if (!inThrottle) {
      fn(...args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
};

// Deep clone
const clone = structuredClone(obj);  // modern (preferred)
const clone2 = JSON.parse(JSON.stringify(obj)); // old way

// Random between min and max
const random = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;

// Unique array
const unique = [...new Set(arr)];

// Flatten nested object keys
const flat = Object.entries(obj).reduce((acc, [k, v]) => {
  if (typeof v === "object" && v !== null) Object.assign(acc, flat(v));
  else acc[k] = v;
  return acc;
}, {});

// Format date
new Date().toLocaleDateString("en-PK", {
  year: "numeric", month: "long", day: "numeric"
});

// Copy to clipboard
await navigator.clipboard.writeText("text to copy");
```
