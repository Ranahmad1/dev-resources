# Regex (Regular Expressions) Cheatsheet

## Syntax Basics
```
.        → any character except newline
^        → start of string (or line with /m flag)
$        → end of string (or line with /m flag)
\        → escape character
|        → alternation (OR)
```

## Character Classes
```
[abc]      → a, b, or c
[^abc]     → NOT a, b, or c
[a-z]      → lowercase a to z
[A-Z]      → uppercase A to Z
[0-9]      → digit
[a-zA-Z]   → letter
[a-zA-Z0-9_] → word character

\d         → [0-9]
\D         → [^0-9]
\w         → [a-zA-Z0-9_]
\W         → [^a-zA-Z0-9_]
\s         → whitespace: space, tab, newline
\S         → non-whitespace
\b         → word boundary
\B         → NOT word boundary
\n \t \r   → newline, tab, carriage return
```

## Quantifiers
```
*          → 0 or more (greedy)
+          → 1 or more (greedy)
?          → 0 or 1 (optional)
{n}        → exactly n times
{n,}       → n or more times
{n,m}      → between n and m times

# Lazy (as few as possible)
*?  +?  ??  {n,m}?

# Possessive (no backtracking — atomic, regex flavor dependent)
*+  ++  ?+  {n,m}+
```

## Groups & Lookarounds
```
(abc)       → Capture group
(?:abc)     → Non-capture group
(?P<name>abc) → Named capture group (Python)
(?<name>abc)  → Named capture group (JS/PHP)

(?=abc)     → Lookahead: followed by abc
(?!abc)     → Negative lookahead
(?<=abc)    → Lookbehind: preceded by abc
(?<!abc)    → Negative lookbehind

# Backreferences
\1          → refers to first capture group
(\w+)\s\1   → matches repeated word (e.g. "hello hello")
```

## Flags / Modifiers
```
/pattern/g   → global (find all, not just first)
/pattern/i   → case-insensitive
/pattern/m   → multiline (^ $ match line start/end)
/pattern/s   → dotall (. matches newline too)
/pattern/u   → unicode
/pattern/gi  → combine flags
```

## Common Patterns
```js
// Email
/^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$/

// Pakistan phone
/^(\+92|0)(3[0-9]{2})[0-9]{7}$/

// International phone
/^\+?[1-9]\d{6,14}$/

// URL
/^https?:\/\/(www\.)?[\w\-]+(\.[a-z]{2,})+([\/?#][\S]*)?$/i

// Pakistani CNIC
/^[0-9]{5}-[0-9]{7}-[0-9]{1}$/

// Password (8+ chars, uppercase, lowercase, digit)
/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d@$!%*?&]{8,}$/

// Strong password (+ special char)
/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/

// Date YYYY-MM-DD
/^(19|20)\d{2}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01])$/

// IPv4 address
/^((25[0-5]|2[0-4]\d|[01]?\d\d?)\.){3}(25[0-5]|2[0-4]\d|[01]?\d\d?)$/

// Hex color
/^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$/

// Slug (URL-safe)
/^[a-z0-9]+(?:-[a-z0-9]+)*$/

// Username (letters, numbers, underscore, 3-20 chars)
/^[a-zA-Z][a-zA-Z0-9_]{2,19}$/

// Credit card (basic)
/^[0-9]{4}[- ]?[0-9]{4}[- ]?[0-9]{4}[- ]?[0-9]{4}$/

// HTML tag (basic — avoid parsing HTML with regex!)
/<([a-z][a-z0-9]*)\b[^>]*>(.*?)<\/\1>/is
```

## JavaScript Usage
```js
// Create
const re1 = /^\d+$/;              // literal (preferred)
const re2 = new RegExp('^\\d+$'); // constructor (for dynamic)

// Test — returns boolean
/^\d+$/.test('123');              // true
/^\d+$/.test('abc');              // false

// Match — returns array or null
'hello world'.match(/\w+/g);      // ['hello', 'world']
'phone: 03001234567'.match(/(0\d{10})/)?.[1]; // '03001234567'

// Named groups (ES2018+)
const m = '2026-09-04'.match(/(?<y>\d{4})-(?<m>\d{2})-(?<d>\d{2})/);
m?.groups;  // { y: '2026', m: '09', d: '04' }

// Replace
'foo bar'.replace(/foo/, 'baz');          // 'baz bar'
'a1b2c3'.replace(/\d/g, '#');            // 'a#b#c#'
'hello world'.replace(/(\w+)/g, (_, w) => w.toUpperCase()); // 'HELLO WORLD'

// Replace with named group
'2026-09-04'.replace(
  /(?<y>\d{4})-(?<m>\d{2})-(?<d>\d{2})/,
  '$<d>/$<m>/$<y>'
); // '04/09/2026'

// Split
'one,  two;  three'.split(/[,;]\s*/);     // ['one','two','three']

// Exec — iterate all matches with groups
const re = /(?<word>\w+)/g;
let match;
while ((match = re.exec('hello world')) !== null) {
  console.log(match.groups.word, 'at', match.index);
}

// String.matchAll (ES2020)
for (const m of 'a1 b2 c3'.matchAll(/(\w)(\d)/g)) {
  console.log(m[1], m[2]); // a 1 / b 2 / c 3
}

// Validation helper
const validators = {
  email:   /^[\w.+-]+@[\w-]+\.[a-z]{2,}$/i,
  phone:   /^(\+92|0)3\d{9}$/,
  url:     /^https?:\/\/.+/i,
  digits:  /^\d+$/,
};

const validate = (type, value) => validators[type]?.test(value.trim()) ?? false;
validate('email', 'a@b.com');  // true
```
