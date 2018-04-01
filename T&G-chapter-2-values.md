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

### Safe Integer Ranges
The maximum integer that can "safely" be represented (that is, there's a guarantee that the requested value is actually representable unambiguously) is 2^53 - 1, which is 9007199254740991. If you insert your commas, you'll see that this is just over 9 quadrillion. So that's pretty darn big for numbers to range up to.
