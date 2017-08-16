# Effective Javascript

---

### Introduction

---

### Item 2: Understand JavaScript’s Floating-Point Numbers

+++

- All numbers in JavaScript are `double-precision floating-point numbers` (64-bit)

- `-9,007,199,254,740,992` (–2^53) `to 9,007,199,254,740,992` (2^53)

```
typeof 17; // "number" 
typeof 98.6; // "number" 
typeof -2.1; // "number"
```
+++

- bitwise arithmetic operators are special
- they implicitly convert them to 32-bit integers
```
8 | 1; // 9
```

+++
```
0.1 * 1.9 // 0.19 
-99 + 100; // 1
21 - 12.3; // 8.7
2.5 / 5; // 0.5 
21 % 8; // 5
```
---

### Item 3: Beware of Implicit Coercions

+++
- Javascript is dynamically typed languages
- JavaScript can be surprisingly forgiving when it comes to type errors.
```
3 + true; // 4
```
- JavaScript *coerces* a value to the expected type by following various automatic conversion protocols

```
2 + 3; // 5
"hello" + " world"; // "hello world"
```
_But what happens when_
```
"2" + 3; // "23" 
2 + "3"; // "23"
```
_Order has effect_
```
1 + 2 + "3"; // "33"
(1 + 2) + "3"; // "33"
1 + "2" + 3; // "123"
(1 + "2") + 3; // "123"
```
- coercions can also hide errors
```
1 + null // 1
1+ undefined // NaN
```
+++
#### Testing for `NaN`
- `isNaN` not reliable
```
isNaN("foo");
isNaN(undefined);
isNaN({});
isNaN({ valueOf: "foo" }); // true
```
- Reliable check (`NaN` Unique to itself)
```
var a = NaN;
a !== a; // true var b = "foo";
b !== b; // false
```
+++
#### Objects can also be coerced to primitives.
- To strings using `toString` method
```
"the Math object: " + Math; // "the Math object: [object Math]" 
"the JSON object: " + JSON; // "the JSON object: [object JSON]"
```
Objects can be converted to numbers via their valueOf method.
```
"J" + { toString: function() { return "S"; } }; // 
"JS" 2 * { valueOf: function() { return 3; } }; // 6
```
---
