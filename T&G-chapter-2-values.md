# Values

## TL;DR
In JavaScript, `arrays` are simply numerically indexed collections of any value-type. `strings` are somewhat "array-like", but they have distinct behaviors and care must be taken if you want to treat them as `arrays`. Numbers in JavaScript include both "integers" and floating-point values.

Several special values are defined within the primitive types.

The `null` type has just one value: `null`, and likewise the `undefined` type has just the `undefined` value. `undefined` is basically the default value in any variable or property if no other value is present. The void operator lets you create the `undefined` value from any other value.

`number`s include several special values, like `NaN` (supposedly "Not a Number", but really more appropriately "invalid number"); `+Infinity` and `-Infinity`; and `-0`.

Simple scalar primitives (`string`s, `number`s, etc.) are assigned/passed by value-copy, but compound values (`object`s, etc.) are assigned/passed by reference-copy. References are not like references/pointers in other languages -- they're never pointed at other variables/references, only at the underlying values.

## Array's
*Warning:* Using delete on an array value will remove that slot from the array, but even if you remove the final element, it does not update the length property, so be careful! We'll cover the delete operator itself in more detail in Chapter 5.
You don't need to presize your arrays (see "Arrays" in Chapter 3), you can just declare them and add values as you see fit:
```javascript
var a = [ ];

a.length;	// 0

a[0] = 1;
a[1] = "2";
a[2] = [ 3 ];

a.length;	// 3
```

`arrays` are numerically indexed (as you'd expect), but the tricky thing is that they also are objects that can have `string` keys/properties added to them (but which don't count toward the length of the array). Generally, it's not a great idea to add string keys/properties to arrays. Use objects for holding values in keys/properties, and save arrays for strictly numerically indexed values.

```javascript
var a = [ ];

a[0] = 1;
a["foobar"] = 2;

a.length;		// 1
a["foobar"];	// 2
a.foobar;		// 2
``` 

## Array-Likes
There will be occasions where you need to convert an array-like value (a numerically indexed collection of values) into a true array, usually so you can call array utilities (like indexOf(..), concat(..), forEach(..), etc.) against the collection of values. One very common way to make such a conversion is to borrow the slice(..) utility against the value:
```javascript
function foo() {
	var arr = Array.prototype.slice.call( arguments );
	arr.push( "bam" );
	console.log( arr );
}

foo( "bar", "baz" ); // ["bar","baz","bam"]
```

As of ES6, there's also a built-in utility called Array.from(..) that can do the same task:
```javascript
var arr = Array.from( arguments );
```

## Strings
t's a very common belief that strings are essentially just arrays of characters. While the implementation under the covers may or may not use arrays, it's important to realize that JavaScript strings are really not the same as arrays of characters. The similarity is mostly just skin-deep.
```javascript
var a = "foo";
var b = ["f", "o", "o"];
```
Strings do have a shallow resemblance to arrays -- array-likes, as above -- for instance, both of them having a length property, an indexOf(..) method (array version only as of ES5), and a concat(..) method:
```javascript
a.length;							// 3
b.length;							// 3

a.indexOf( "o" );					// 1
b.indexOf( "o" );					// 1

var c = a.concat( "bar" );			// "foobar"
var d = b.concat( ["b","a","r"] );	// ["f","o","o","b","a","r"]

a === c;							// false
b === d;							// false

a;									// "foo"
b;									// ["f","o","o"]
```
JavaScript strings are immutable, while arrays are quite mutable. Moreover, the a[1] character position access form was not always widely valid JavaScript. Older versions of IE did not allow that syntax (but now they do). Instead, the correct approach has been a.charAt(1). A further consequence of immutable strings is that none of the string methods that alter its contents can modify in-place, but rather must create and return new strings. By contrast, many of the methods that change array contents actually do modify in-place.

## Numbers
JavaScript has just one numeric type: number. This type includes both "integer" values and fractional decimal numbers. I say "integer" in quotes because it's long been a criticism of JS that there are not true integers, as there are in other languages. That may change at some point in the future, but for now, we just have numbers for everything.

So, in JS, an "integer" is just a value that has no fractional decimal value. That is, 42.0 is as much an "integer" as 42.

### Small Decimals
The most (in)famous side effect of using binary floating-point numbers (which, remember, is true of all languages that use IEEE 754 -- not just JavaScript as many assume/pretend) is:
```javascript
0.1 + 0.2 === 0.3; // false
```
Simply put, the representations for 0.1 and 0.2 in binary floating-point are not exact, so when they are added, the result is not exactly 0.3. It's really close: 0.30000000000000004, but if your comparison fails, "close" is irrelevant.

As of ES6, Number.EPSILON is predefined with this tolerance value, so you'd want to use it, but you can safely polyfill the definition for pre-ES6:
```javascript
if (!Number.EPSILON) {
	Number.EPSILON = Math.pow(2,-52);
}
```
We can use this Number.EPSILON to compare two numbers for "equality" (within the rounding error tolerance):
```javascript
function numbersCloseEnoughToEqual(n1,n2) {
	return Math.abs( n1 - n2 ) < Number.EPSILON;
}

var a = 0.1 + 0.2;
var b = 0.3;

numbersCloseEnoughToEqual( a, b );					// true
numbersCloseEnoughToEqual( 0.0000001, 0.0000002 );	// false
```
### Safe Integer Ranges
The maximum integer that can "safely" be represented (that is, there's a guarantee that the requested value is actually representable unambiguously) is 2^53 - 1, which is 9007199254740991. If you insert your commas, you'll see that this is just over 9 quadrillion. So that's pretty darn big for numbers to range up to.

### Testing for Integers
```javascript
Number.isInteger( 42 );		// true
Number.isInteger( 42.000 );	// true
Number.isInteger( 42.3 );	// false

//polyfill pre-ES6
if (!Number.isInteger) {
	Number.isInteger = function(num) {
		return typeof num == "number" && num % 1 == 0;
	};
}
// test for safe integer
Number.isSafeInteger( Number.MAX_SAFE_INTEGER );	// true
Number.isSafeInteger( Math.pow( 2, 53 ) );			// false
Number.isSafeInteger( Math.pow( 2, 53 ) - 1 );		// true

// polyfill pre-ES6
if (!Number.isSafeInteger) {
	Number.isSafeInteger = function(num) {
		return Number.isInteger( num ) &&
			Math.abs( num ) <= Number.MAX_SAFE_INTEGER;
	};
};
```

## Special Values
### The Non-value Values
* `null` is an empty value
* `undefined` is a missing value
OR
* `null` had a value and doesn't anymore
* `undefined` hasn't had a value yet

### Undefined
In non-strict mode, it's actually possible (though incredibly ill-advised!) to assign a value to the globally provided undefined identifier:

```javascript
function foo() {
	"use strict";
	undefined = 2; // TypeError!
}

foo();

// You can do this, but just don't
function foo() {
	"use strict";
	var undefined = 2;
	console.log( undefined ); // 2
}

foo();
```

### `void` Operator
The expression `void ___` "voids" out any value, so that the result of the expression is always the `undefined` value. It doesn't modify the existing value; it just ensures that no value comes back from the operator expression.
```javascript
var a = 42;

console.log( void a, a ); // undefined 42
```
But the void operator can be useful in a few other circumstances, if you need to ensure that an expression has no result value (even if it has side effects).
```javascript
function doSomething() {
	// note: `APP.ready` is provided by our application
	if (!APP.ready) {
		// try again later
		return void setTimeout( doSomething, 100 );
	}

	var result;

	// do some other stuff
	return result;
}

// were we able to do it right away?
if (doSomething()) {
	// handle next tasks right away
}
```

### Special Numbers
`number` includes several special values. 

#### The Not Number, Number
`NaN` literally stands for "not a number", though this label/description is very poor and misleading, as we'll see shortly. It would be much more accurate to think of NaN as being "invalid number," "failed number," or even "bad number," than to think of it as "not a number."
```javascript
var a = 2 / "foo";		// NaN

typeof a === "number";	// true
```
In other words: "the type of not-a-number is 'number'!" Hooray for confusing names and semantics.  The error condition is, in essence: "_I tried to perform a mathematic operation but failed, so here's the failed number result instead_."

So, if you want to see if in some variable there is a special-failed number, `NaN` is not the answer. `NaN` is a very special value in that it's never equal to another `NaN` value (i.e., it's never equal to itself). It's the only value, in fact, that is not reflexive (without the Identity characteristic `x === x`). So, `NaN !== NaN`. A bit strange, huh? Check this: 
```javascript
var a = 2 / "foo";

a == NaN;	// false
a === NaN;	// false

// This is how we test for it
// Built-in utility from JS (ES6?)
var a = 2 / "foo";

isNaN( a ); // true
```

Check this next example: Clearly, "foo" is literally not a number, but it's definitely not the NaN value either! This bug has been in JS since the very beginning (over 19 years of ouch).
```javascript
var a = 2 / "foo";
var b = "foo";

a; // NaN
b; // "foo"

window.isNaN( a ); // true
window.isNaN( b ); // true -- ouch!
```

As of ES6, finally a replacement utility has been provided: Number.isNaN(..). A simple polyfill for it so that you can safely check NaN values now even in pre-ES6 browsers is:
```javascript
if (!Number.isNaN) {
	Number.isNaN = function(n) {
		return (
			typeof n === "number" &&
			window.isNaN( n )
		);
	};
}

var a = 2 / "foo";
var b = "foo";

Number.isNaN( a ); // true
Number.isNaN( b ); // false -- phew!

// Easier polyfill
if (!Number.isNaN) {
	Number.isNaN = function(n) {
		return n !== n;
	};
}
```
#### Infinites
Developers from traditional compiled languages like C are probably used to seeing either a compiler error or runtime exception, like "Divide by zero,". However, in JS, this operation is well-defined and results in the value Infinity (aka Number.POSITIVE_INFINITY). Unsurprisingly:
```javascript
var a = 1 / 0;	// Infinity
var b = -1 / 0;	// -Infinity
```
JS uses finite numeric representations (IEEE 754 floating-point, which we covered earlier), so contrary to pure mathematics, it seems it is possible to overflow even with an operation like addition or subtraction, in which case you'd get Infinity or -Infinity.

```javascript
var a = Number.MAX_VALUE;	// 1.7976931348623157e+308
a + a;						// Infinity
a + Math.pow( 2, 970 );		// Infinity
a + Math.pow( 2, 969 );		// 1.7976931348623157e+308
```

#### Zeros
While it may confuse the mathematics-minded reader, JavaScript has both a normal zero 0 (otherwise known as a positive zero +0) and a negative zero -0. Before we explain why the -0 exists, we should examine how JS handles it, because it can be quite confusing.

Besides being specified literally as -0, negative zero also results from certain mathematic operations. _Addition and subtraction cannot result in a negative zero_. For example:
```javascript
var a = 0 / -3; // -0
var b = 0 * -3; // -0
```

*If you try to stringify a negative zero value, it will always be reported as "0", according to the spec.*
```javascript
var a = 0 / -3;

// (some browser) consoles at least get it right
a;							// -0

// but the spec insists on lying to you!
a.toString();				// "0"
a + "";						// "0"
String( a );				// "0"

// strangely, even JSON gets in on the deception
JSON.stringify( a );		// "0"

// The other way around seems to work just fine

Interestingly, the reverse operations (going from string to number) don't lie:

+"-0";				// -0
Number( "-0" );		// -0
JSON.parse( "-0" );	// -0
``` 

Just check this shit. No wonder backend does not use nodeJS a lot. 
```javascript
var a = 0;
var b = 0 / -3;

a == b;		// true
-0 == 0;	// true

a === b;	// true
-0 === 0;	// true

0 > -0;		// false
a > b;		// false
```
Nice ass function to distinguish `0` from `-0`. The Reason this is important is because apparently there is no difference for JS between `0` & `-0`. 
```javascript
function isNegZero(n) {
	n = Number( n );
	return (n === 0) && (1 / n === -Infinity);
}

isNegZero( -0 );		// true
isNegZero( 0 / -3 );	// true
isNegZero( 0 );			// false
```
### Special Euqlity
As of ES6, there's a new utility that can be used to test two values for absolute equality, without any of these exceptions. It's called `Object.is(..)`:
```javascript
var a = 2 / "foo";
var b = -3 * 0;

Object.is( a, NaN );	// true
Object.is( b, -0 );		// true

Object.is( b, 0 );		// false

// The polyfill
if (!Object.is) {
	Object.is = function(v1, v2) {
		// test for `-0`
		if (v1 === 0 && v2 === 0) {
			return 1 / v1 === 1 / v2;
		}
		// test for `NaN`
		if (v1 !== v1) {
			return v2 !== v2;
		}
		// everything else
		return v1 === v2;
	};
}
```

## Value vs. Reference
A reference in JS points at a (shared) value, so if you have 10 different references, they are all always distinct references to a single shared value; *none of them are references/pointers to each other*. Moreover, in JavaScript, there are no syntactic hints that control value vs. reference assignment/passing. Instead, the type of the value solely controls whether that value will be assigned by value-copy or by reference-copy. Check this shit:
```javascript
var a = 2;
var b = a; // `b` is always a copy of the value in `a`
b++;
a; // 2
b; // 3

var c = [1,2,3];
var d = c; // `d` is a reference to the shared `[1,2,3]` value
d.push( 4 );
c; // [1,2,3,4]
d; // [1,2,3,4]
```
_Explanation:_
In the above snippet, because `2` is a scalar primitive, a holds one initial copy of that value, and `b` is assigned another copy of the value. When changing `b`, you are in no way changing the value in `a`.

But both `c` and `d` are separate references to the same shared value [1,2,3], which is a compound value. It's important to note that neither `c` nor `d` more *"owns"* the [1,2,3] value -- both are just equal peer references to the value. So, when using either reference to modify `(.push(4))` the actual shared `array` value itself, it's affecting just the one shared value, and both references will reference the newly modified value [1,2,3,4].

* Simple values (aka scalar primitives) are always assigned/passed by value-copy: `null`, `undefined`, `string`, `number`, `boolean`, and ES6's `symbol`.
* Compound values -- `objects` (including `array`s, and all boxed object wrappers -- see Chapter 3) and `function`s -- always create a copy of the reference on assignment or passing.

Since references point to the values themselves and not to the variables, you cannot use one reference to change where another reference is pointed. In other words, you can put information into both, and change only one (& loose the reference)
```javascript
var a = [1,2,3];
var b = a;
a; // [1,2,3]
b; // [1,2,3]

// later
b = [4,5,6];
a; // [1,2,3]
b; // [4,5,6]
```

