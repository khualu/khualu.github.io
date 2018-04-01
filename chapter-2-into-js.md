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

### Truthy & Falsy
The specific list of "falsy" values in JavaScript is as follows:
* `""` (empty string
* `0`, `-0`, `NaN` (invalid number)
* `null`, `undefined`
* `false`

Any value that's not on this "falsy" list is "truthy." Here are some examples of those:
* `"hello"`
* `42`
* `true`
* `[ ]`, `[ 1, "2", 3 ]` (arrays)
* `{ }`, `{ a: 42 }` (objects)
* `function foo() { .. }` (functions)

### Equality
The difference between `==` and `===` is usually characterized that `==` checks for value equality and `===` checks for both value and type equality. However, this is inaccurate. The proper way to characterize them is that `==` checks for value equality with coercion allowed, and `===` checks for value equality without allowing coercion; `===` is often called "strict equality" for this reason.

#### How is the coercion allowed?
In the a == b comparison, JS notices that the types do not match, so it goes through an ordered series of steps to coerce one or both values to a different type until the types match, where then a simple value equality can be checked.

If you think about it, there's two possible ways a == b could give true via coercion. Either the comparison could end up as 42 == 42 or it could be "42" == "42". So which is it?

The answer: "42" becomes 42, to make the comparison 42 == 42. In such a simple example, it doesn't really seem to matter which way that process goes, as the end result is the same. There are more complex cases where it matters not just what the end result of the comparison is, but how you get there.

You should take special note of the `==` and `===` comparison rules if you're comparing two non-primitive values, like `objects` (including function and array). Because those values are actually held by reference, both `==` and `===` comparisons will simply check whether the references match, not anything about the underlying values.

For example, `arrays` are by default coerced to `strings` by simply joining all the values with commas (,) in between. You might think that two arrays with the same contents would be == equal, but they're not:

```javascript
var a = [1,2,3];
var b = [1,2,3];
var c = "1,2,3";

a == c;		// true
b == c;		// true
a == b;		// false
```

(This actually blew my mind. It doesn't make sense but it does at the same time)

### Inequality
The `<`, `>`, `<=`, and `>=` operators are used for inequality, referred to in the specification as "relational comparison." Typically they will be used with ordinally comparable values like numbers. It's easy to understand that `3 < 4`. But JavaScript `string` values can also be compared for inequality, using typical alphabetic rules (`"bar" < "foo"`). There are no strict inequiality operators. 

```javascript
var a = 41;
var b = "42";
var c = "43";

a < b;		// true
b < c;		// true
```
The biggest gotcha you may run into here with comparisons between potentially different value types -- remember, there are no "strict inequality" forms to use -- is when one of the values cannot be made into a valid number, such as:
```javascript
var a = 42;
var b = "foo";

a < b;		// false
a > b;		// false
a == b;		// false
```

## Variables
In JavaScript, variable names (including function names) must be valid identifiers. The strict and complete rules for valid characters in identifiers are a little complex when you consider nontraditional characters such as Unicode. If you only consider typical ASCII alphanumeric characters, though, the rules are simple.

An identifier must start with `a-z`, `A-Z`, `$`, or `_`. It can then contain any of those characters plus the numerals 0-9.

Generally, the same rules apply to a property name as to a variable identifier. However, certain words cannot be used as variables, but are OK as property names. These words are called "reserved words," and include the JS keywords (`for`, `in`, `if`, etc.) as well as `null`, `true`, and `false`.

## Function Scopes
You use the `var` keyword to declare a variable that will belong to the current function scope, or the global scope if at the top level outside of any function.

### Hoisting
Wherever a `var` appears inside a scope, that declaration is taken to belong to the entire scope and accessible everywhere throughout. Metaphorically, this behavior is called hoisting, when a var declaration is conceptually "moved" to the top of its enclosing scope. Technically, this process is more accurately explained by how code is compiled, but we can skip over those details for now. Consider:

```javascript
var a = 2;

foo();					// works because `foo()`
						// declaration is "hoisted"

function foo() {
	a = 3;

	console.log( a );	// 3

	var a;				// declaration is "hoisted"
						// to the top of `foo()`
}

console.log( a );	// 2
```

### Nested Scopes
If you try to access a variable's value in a scope where it's not available, you'll get a ReferenceError thrown. If you try to set a variable that hasn't been declared, you'll either end up creating a variable in the top-level global scope (bad!) or getting an error, depending on "strict mode" (see "Strict Mode"). Let's take a look:

```javascript
function foo() {
	a = 1;	// `a` not formally declared
}

foo();
a;			// 1 -- oops, auto global variable :(
```
This is a very bad practice. Don't do it! Always formally declare your variables. It should be: `var a = 1;`. 

In addition to creating declarations for variables at the function level, ES6 lets you declare variables to belong to individual blocks (pairs of { .. }), using the let keyword. Besides some nuanced details, the scoping rules will behave roughly the same as we just saw with functions:

```javascript
function foo() {
	var a = 1;

	if (a >= 1) {
		let b = 2;

		while (b < 5) {
			let c = b * 2;
			b++;

			console.log( a + c );
		}
	}
}

foo();
// 5 7 9
```
Because of `let`, `b` will only belong to the `if` statement and `c` belongs only to the `while` loop. 

## Conditionals
Another _conditional mechanism_ of JS is the `switch`. Instead of writing all different `if..else..if` statements you can do it with `switch` this way. 
```javascript
switch (a) {
	case 2:
		// do something
		break;
	case 10:
		// do another thing
		break;
	case 42:
		// do yet another thing
		break;
	default:
		// fallback to here
}
```

The break is important if you want only the statement(s) in one case to run. If you omit break from a case, and that case matches or runs, execution will continue with the next case's statements regardless of that case matching. This so called "fall through" is sometimes useful/desired:
```javascript
switch (a) {
	case 2:
	case 10:
		// some cool stuff
		break;
	case 42:
		// other stuff
		break;
	default:
		// fallback
}
```
The _ternary operator_ is another conditional operator. It goes like this: 
```javascript
var a = 42;

var b = (a > 41) ? "hello" : "world";

// similar to:

// if (a > 41) {
//    b = "hello";
// }
// else {
//    b = "world";
// }
```

## Strict Mode
ES5 added a "strict mode" to the language, which tightens the rules for certain behaviors. Generally, these restrictions are seen as keeping the code to a safer and more appropriate set of guidelines. Also, adhering to strict mode makes your code generally more optimizable by the engine. Strict mode is a big win for code, and you should use it for all your programs. You can opt in to strict mode for an individual function, or an entire file, depending on where you put the strict mode pragma:
```javascript
function foo() {
	"use strict";

	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is not strict mode
```
OR
```javascript
"use strict";

function foo() {
	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is strict mode
```
_One key difference_ (improvement!) with strict mode is _*disallowing the implicit auto-global variable declaration*_ from omitting the var:
```javascript
function foo() {
	"use strict";	// turn on strict mode
	a = 1;			// `var` missing, ReferenceError
}

foo();
```
## Functions as Values
So far, functions have been the primary mechanism of _scope_ in JS. (e.g. `function foo(){};`). `foo` is basically just a variable in the outer enclosing scope that's given a reference to the `function` being declared. A function itself can be a _value_ that's assigned to variables, or passed to or returned from other functions. Consider:
```javascript
var foo = function () {
	// stuff
}; 

var x = function bar () {
	// other stuff
};
```

The first function expression assigned to the foo variable is called anonymous because it has no name. The second function expression is named (bar), even as a reference to it is also assigned to the x variable. Named function expressions are generally more preferable, though anonymous function expressions are still extremely common.

### Immediately Invoked Function Expressions (IIFEs)
There's another way to execute a function expression, which is typically referred to as an immediately invoked function expression (IIFE):
```javascript
(function IIFE(){
	console.log( "Hello!" );
})();
// "Hello!"
```

The outer ( .. ) that surrounds the (function IIFE(){ .. }) function expression is just a nuance of JS grammar needed to prevent it from being treated as a normal function declaration. The final () on the end of the expression -- the })(); line -- is what actually executes the function expression referenced immediately before it. IIFEs can be used to create variable scope thus making variables that won't affect the surrounding code outside the IIFE:
```javascript
var a = 42;

(function IIFE(){
	var a = 10;
	console.log( a );	// 10
})();

console.log( a );		// 42

//they can also have return values

var x = (function IIFE(){
	return 42;
})();
x;	// 42
```

### Closure
You can think of closure as a way to _*"remember"*_ and continue to access a function's scope (its variables) even once the function has finished running. Consider:
```javascript
function makeAdder(x) {
	// parameter `x` is an inner variable

	// inner function `add()` uses `x`, so
	// it has a "closure" over it
	function add(y) {
		return y + x;
	};

	return add;
}
```


