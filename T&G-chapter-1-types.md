# Types

## TL;DR
JavaScript has seven built-in types: `null`, `undefined`, `boolean`, `number`, `string`, `object`, `symbol`. They can be identified by the typeof operator. All of these except for `object` are called primitives. 

`null` is the only primitive value that is "falsy" (aka false-like; see Chapter 4) but that also returns "object" from the typeof check.

Variables don't have types, but the values in them do. These types define intrinsic behavior of the values.

Many developers will assume "undefined" and "undeclared" are roughly the same thing, but in JavaScript, they're quite different. undefined is a value that a declared variable can hold. "Undeclared" means a variable has never been declared.

JavaScript unfortunately kind of conflates these two terms, not only in its error messages ("ReferenceError: a is not defined") but also in the return values of typeof, which is "undefined" for both cases.

However, the safety guard (preventing an error) on typeof when used against an undeclared variable can be helpful in certain cases.

## `undefined` vs `undeclared`
Variables that have no value currently, actually have the `undefined` value. Calling `typeof` against such variables will return `"undefined"`:
```javascript
var a;

typeof a; // "undefined"

var b = 42;
var c;

// later
b = c;

typeof b; // "undefined"
typeof c; // "undefined"
```

An "undefined" variable is one that has been declared in the accessible scope, but at the moment has no other value in it. By contrast, an "undeclared" variable is one that has not been formally declared in the accessible scope.
```javascript
var a;

a; // undefined
b; // ReferenceError: b is not defined
```
There's also a special behavior associated with typeof as it relates to undeclared variables that even further reinforces the confusion. Consider:
```javascript
var a;

typeof a; // "undefined"

typeof b; // "undefined"
```

## `typeof` Undeclared
???
