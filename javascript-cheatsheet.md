# JavaScript Cheatsheet (ES6+)

## Variables
```js
var x = 1;       // function scoped (avoid)
let y = 2;       // block scoped (use for mutable)
const z = 3;     // block scoped (use for constants)
```

## Data Types
```js
let str = "Hello";           // String
let num = 42;                // Number
let bool = true;             // Boolean
let arr = [1, 2, 3];        // Array
let obj = { name: "Ahmad" }; // Object
let nothing = null;          // Null
let undef = undefined;       // Undefined
```

## String Methods
```js
let s = "Hello World";
s.length;              // 11
s.toUpperCase();       // "HELLO WORLD"
s.toLowerCase();       // "hello world"
s.includes("World");   // true
s.startsWith("He");    // true
s.endsWith("ld");      // true
s.slice(0, 5);         // "Hello"
s.replace("World", "JS"); // "Hello JS"
s.split(" ");          // ["Hello", "World"]
s.trim();              // removes whitespace

// Template Literals
let name = "Rana";
console.log(`Hello, ${name}!`); // Hello, Rana!
```

## Array Methods
```js
let arr = [1, 2, 3, 4, 5];

arr.push(6);           // Add to end → [1,2,3,4,5,6]
arr.pop();             // Remove from end
arr.unshift(0);        // Add to start
arr.shift();           // Remove from start
arr.indexOf(3);        // 2
arr.includes(4);       // true
arr.join(", ");         // "1, 2, 3, 4, 5"
arr.reverse();         // [5, 4, 3, 2, 1]
arr.slice(1, 3);       // [2, 3] (non-destructive)
arr.splice(1, 2);      // Removes 2 items from index 1

// Iteration
arr.map(x => x * 2);             // [2, 4, 6, 8, 10]
arr.filter(x => x > 2);          // [3, 4, 5]
arr.reduce((acc, x) => acc + x, 0); // 15
arr.find(x => x > 3);            // 4
arr.findIndex(x => x > 3);       // 3
arr.every(x => x > 0);           // true
arr.some(x => x > 4);            // true
arr.forEach(x => console.log(x));
arr.sort((a, b) => a - b);       // ascending sort

// Spread & Destructuring
const [a, b, ...rest] = arr;     // destructuring
const newArr = [...arr, 6, 7];   // spread
```

## Objects
```js
const person = {
  name: "Ahmad",
  age: 20,
  greet() { return `Hi, I'm ${this.name}`; }
};

// Access
person.name;           // dot notation
person["age"];         // bracket notation

// Destructuring
const { name, age } = person;

// Spread
const updated = { ...person, age: 21 };

// Object methods
Object.keys(person);   // ["name", "age"]
Object.values(person); // ["Ahmad", 20]
Object.entries(person);// [["name","Ahmad"],["age",20]]
```

## Functions
```js
// Regular
function add(a, b) { return a + b; }

// Arrow
const add = (a, b) => a + b;

// Default parameters
const greet = (name = "Guest") => `Hello, ${name}!`;

// Rest parameters
const sum = (...nums) => nums.reduce((a, b) => a + b, 0);

// IIFE
(function() { console.log("Runs immediately"); })();
```

## Async / Await & Promises
```js
// Promise
const fetchData = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Data!"), 1000);
});
fetchData.then(data => console.log(data)).catch(err => console.error(err));

// Async/Await
async function getData() {
  try {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Error:", error);
  }
}
```

## DOM Manipulation
```js
// Select
document.getElementById("id");
document.querySelector(".class");
document.querySelectorAll("div");

// Modify
element.textContent = "New text";
element.innerHTML = "<strong>Bold</strong>";
element.style.color = "red";
element.classList.add("active");
element.classList.remove("active");
element.classList.toggle("active");
element.setAttribute("src", "img.jpg");
element.getAttribute("href");

// Create & Append
const div = document.createElement("div");
div.textContent = "Hello";
document.body.appendChild(div);
element.remove();

// Events
btn.addEventListener("click", (e) => {
  e.preventDefault();
  console.log("Clicked!", e.target);
});
```

## Local Storage
```js
localStorage.setItem("key", JSON.stringify(data));
const data = JSON.parse(localStorage.getItem("key"));
localStorage.removeItem("key");
localStorage.clear();
```

## Useful Snippets
```js
// Debounce
function debounce(fn, delay) {
  let timer;
  return function(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

// Deep clone
const clone = JSON.parse(JSON.stringify(obj));

// Random number
const rand = Math.floor(Math.random() * 100);

// Format date
const date = new Date().toLocaleDateString("en-PK", { year: "numeric", month: "long", day: "numeric" });
```
