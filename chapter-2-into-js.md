# Chapter 2: Into Javascript
We will introduce quite a few concepts in this chapter that will not be fully explored until subsequent _YDKJS_ books. You can think of this chapter as an overview of the topics covered in detail throughout the rest of this series.

## Values & Types
* `string`
* `number`
* `boolean`
* `null` & `undefined` (NOT EXACTLY THE SAME)
* `object`
* `symbol` (new to ES6)

JS provides a `typeof` operator which can examine a value and tell you the type it is. 

```javascript
var a;
typeof a;				// "undefined"

a = "hello world";
typeof a;				// "string"

a = 42;
typeof a;				// "number"

a = true;
typeof a;				// "boolean"

a = null;
typeof a;				// "object" -- weird, bug

a = undefined;
typeof a;				// "undefined"

a = { b: "c" };
typeof a;				// "object"
```

Notice how in this snippet the `a ` variable holds every different type of value, and that despite appearances, `typeof a` is not asking for the "type of a", but rather for the "type of the value currently in a." Only values have types in JavaScript; variables are just simple containers for those values.

`typeof null` is an interesting case, because it errantly returns "object", when you'd expect it to return "null".
*Warning:* This is a long-standing bug in JS, but one that is likely never going to be fixed. Too much code on the Web relies on the bug and thus fixing it would cause a lot more bugs!

## Objects
The `object` type refers to a compound value where you can set properties (named locations) that each hold their own values of any type. This is perhaps one of the most useful value types in all of JavaScript. Properties can either be accessed with dot notation (i.e., obj.a) or bracket notation (i.e., obj["a"]). Dot notation is shorter and generally easier to read, and is thus preferred when possible.

```javascript
var obj = {
	a: "hello world",
	b: 42,
	c: true
};

obj.a;		// "hello world"
obj.b;		// 42
obj.c;		// true

obj["a"];	// "hello world"
obj["b"];	// 42
obj["c"];	// true
``` 

There are a couple of other value types that you will commonly interact with in JavaScript programs: array and function. But rather than being proper built-in types, these should be thought of more like subtypes -- specialized versions of the object type.

## Arrays
An array is an `object` that holds values (of any type) not particularly in named properties/keys, but rather in numerically indexed positions. Because arrays are special objects (as typeof implies), they can also have properties, including the automatically updated length property. For example:

```javascript
var arr = [
	"hello world",
	42,
	true
];

arr[0];			// "hello world"
arr[1];			// 42
arr[2];			// true
arr.length;		// 3

typeof arr;		// "object"
```

## Functions
The other `object` subtype you'll use all over your JS programs is a function:
```javascript
function foo() {
	return 42;
}

foo.bar = "hello world";

typeof foo;			// "function"
typeof foo();		// "number"
typeof foo.bar;		// "string"
```

## Built-in Type Methods
The built-in types and subtypes we've just discussed have behaviors exposed as properties and methods that are quite powerful and useful.

```javascript
var a = "hello world";
var b = 3.14159;

a.length;				// 11
a.toUpperCase();		// "HELLO WORLD"
b.toFixed(4);			// "3.1416"
```


## Comparing Values
There are two main types of value comparison that you will need to make in your JS programs: equality and inequality. The result of any comparison is a strictly boolean value (true or false), regardless of what value types are compared.

### Coercion
Coercion comes in two forms in JavaScript: explicit and implicit. Explicit coercion is simply that you can see obviously from the code that a conversion from one type to another will occur, whereas implicit coercion is when the type conversion can happen as more of a non-obvious side effect of some other operation.

_*Explicit coercion*_
```javascript
var a = "42";

var b = Number( a );

a;				// "42"
b;				// 42 -- the number!
```
_*Implicit coercion*_
```javascript
var a = "42";

var b = a * 1;	// "42" implicitly coerced to 42 here

a;				// "42"
b;				// 42 -- the number!
```












