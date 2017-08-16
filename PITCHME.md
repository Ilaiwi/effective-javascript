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
+++

*** _Order has effect_
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
Objects can be converted to numbers via their `valueOf` method.
```
"J" + { toString: function() { return "S"; } }; // 
"JS" 2 * { valueOf: function() { return 3; } }; // 6
```

+++
### What happens when we run this
```
var obj = {
    toString: function () {
        return "[object MyObject]";
    },
    valueOf: function () {
        return 17;
    }
};
"object: " + obj; // ???
```
+++

### Answer

`// "object: 17"`

+++

### Truthiness coercion

- `if`, `||`, and `&&` logically work with boolean values
- Truthiness does not involve implicitly invoking any coercion methods

- *falsy* values: `false`, `0`, `-0`, `""`, `NaN`, `null`, and `undefined`.
- It is not always safe to use truthiness to check whether a function argument or object property is defined.

```
function point(x, y) {
    if (typeof x === "undefined") {
        x = 320;
    }
    if (typeof y === "undefined") {
        y = 240;
    }
    return { x: x, y: y };
}
```
---

### Item 5: Avoid using == with Mixed Types

+++

What happens when we run this

```
"1.0e0" == { valueOf: function() { return true; } };
// true
```

+++
Be explicit and use `===`
```
var today = new Date();
if (form.month.value == (today.getMonth() + 1) && form.day.value == today.getDate()) {
    // happy birthday!
    // ...
}
```

```
var today = new Date();
if (+form.month.value === (today.getMonth() + 1) && // strict 
    +form.day.value === today.getDate()) { // strict 
        // happy birthday!
        // ...
}
```
+++
- When the two arguments are of the same type, there’s no difference in behavior between `==` and `===.`
- using strict equality is a good way to make it clear to readers that there is no conversion involved in the comparison

+++

#### Coercion rules

![Table](./assets/rules.png)

---

### Item 9: Always Declare Local Variables

+++

- JavaScript’s variable assignment rules make it all too easy to create global variables accidentally.

```
function swap(a, i, j) { temp = a[i]; // global a[i] = a[j];
a[j] = temp;
}
```
_Purposefully creating global variables is bad style, but accidentally creating global variables can be a downright disaster._

+++

