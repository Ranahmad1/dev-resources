# Regex (Regular Expressions) Cheatsheet

## Basic Syntax
```
.        → Any character except newline
^        → Start of string
$        → End of string
\        → Escape character
|        → OR (alternation)
```

## Character Classes
```
[abc]    → a, b, or c
[^abc]   → NOT a, b, or c
[a-z]    → lowercase a to z
[A-Z]    → uppercase A to Z
[0-9]    → any digit
[a-zA-Z0-9] → alphanumeric
\d       → digit [0-9]
\D       → non-digit
\w       → word char [a-zA-Z0-9_]
\W       → non-word char
\s       → whitespace (space, tab, newline)
\S       → non-whitespace
```

## Quantifiers
```
*        → 0 or more
+        → 1 or more
?        → 0 or 1 (optional)
{n}      → exactly n times
{n,}     → n or more times
{n,m}    → between n and m times
*?       → lazy (as few as possible)
+?       → lazy
```

## Groups
```
(abc)    → Capture group
(?:abc)  → Non-capture group
(?=abc)  → Lookahead (followed by abc)
(?!abc)  → Negative lookahead
(?<=abc) → Lookbehind (preceded by abc)
(?<!abc) → Negative lookbehind
```

## Common Patterns
```js
// Email
/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/

// Pakistan phone number
/^(\+92|0)3[0-9]{9}$/

// URL
/^https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#?&//=]*)$/

// Password (min 8 chars, 1 uppercase, 1 lowercase, 1 number)
/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$/

// IP Address
/^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}$/

// Date (YYYY-MM-DD)
/^\d{4}-\d{2}-\d{2}$/

// Digits only
/^\d+$/

// Alphanumeric + underscore
/^[a-zA-Z0-9_]+$/

// Hex color
/^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$/
```

## JavaScript Regex Usage
```js
// Test
/^\d+$/.test("123");              // true

// Match
"hello world".match(/\w+/g);     // ["hello", "world"]

// Replace
"foo bar".replace(/foo/, "baz"); // "baz bar"
"a1b2".replace(/\d/g, "#");      // "a#b#"

// Extract groups
const match = "2024-09-04".match(/(\d{4})-(\d{2})-(\d{2})/);
// match[1] = "2024", match[2] = "09", match[3] = "04"

// Split
"one, two,  three".split(/,\s*/)  // ["one", "two", "three"]
```
