# Natives
The most commonly used natives
* `String()`
* `Number()`
* `Boolean()`
* `Array()`
* `Object()`
* `Function()`
* `RegExp()`
* `Date()`
* `Error()`
* `Symbol()` -- added in ES6!

As you can see, these natives are actually built-in functions. You can do stuff like this:
```javascript
var s = new String( "Hello World!" );

console.log( s.toString() ); // "Hello World!"
```

It is true that each of these natives can be used as a native constructor. But what's being constructed may be different than you think.
```javascript
var a = new String( "abc" );

typeof a; // "object" ... not "String"

a instanceof String; // true

Object.prototype.toString.call( a ); // "[object String]"
```
The result of the constructor form of value creation (`new String("abc")`) is an object wrapper around the primitive (`"abc"`) value.

Importantly, `typeof` shows that these objects are *not their own special types*, but more appropriately they are subtypes of the `object` type.

## Boxing Wrappers
These object wrappers serve a very important purpose. Primitive values don't have properties or methods, so to access `.length` or `.toString()` you need an object wrapper around the value. Thankfully, JS will automatically box (aka wrap) the primitive value to fulfill such accesses.

```javascript
var a = "abc";

a.length; // 3
a.toUpperCase(); // "ABC"
```

In general, there's basically no reason to use the object form directly. It's better to just let the boxing happen implicitly where necessary. In other words, never do things like `new String("abc")`, `new Number(42)`, etc -- always prefer using the literal primitive values `"abc"` and `42`.

### Object Wrappers Gotchas
There are some gotchas with using the object wrappers directly that you should be aware of if you do choose to ever use them. For example, consider Boolean wrapped values:
```javascript
var a = new Boolean( false );

if (!a) {
	console.log( "Oops" ); // never runs
}
```

If you want to manually box a primitive value, you can use the Object(..) function (no new keyword):
```javascript
var a = "abc";
var b = new String( a );
var c = Object( a );

typeof a; // "string"
typeof b; // "object"
typeof c; // "object"

b instanceof String; // true
c instanceof String; // true

Object.prototype.toString.call( b ); // "[object String]"
Object.prototype.toString.call( c ); // "[object String]"
```

## Unboxing
If you have an object wrapper and you want to get the underlying primitive value out, you can use the valueOf() method:
```javascript
var a = new String( "abc" );
var b = new Number( 42 );
var c = new Boolean( true );

a.valueOf(); // "abc"
b.valueOf(); // 42
c.valueOf(); // true
```

## Natives as constructors
For `array`, `object`, `function`, and regular-expression values, it's almost universally preferred that you use the literal form for creating the values, but the literal form creates the same sort of object as the constructor form does (that is, there is no nonwrapped value).

### `Array`
```javascript
var a = new Array( 1, 2, 3 );
a; // [1, 2, 3]

var b = [1, 2, 3];
b; // [1, 2, 3]
```
The Array constructor has a special form where if only one `number` argument is passed, instead of providing that value as contents of the array, it's taken as a length to "presize the array" (well, sorta).

So, if you wanted to actually create an array of actual undefined values (not just "empty slots"), how could you do it (besides manually)?
```javascript
var a = Array.apply( null, { length: 3 } );
a; // [ undefined, undefined, undefined ]
```

### `Object()`, `Function()` and `RegExp()`
The `Object(..)`/`Function(..)`/`RegExp(..)` constructors are also generally optional (and thus should usually be avoided unless specifically called for):
```javascript
var c = new Object();
c.foo = "bar";
c; // { foo: "bar" }

var d = { foo: "bar" };
d; // { foo: "bar" }

var e = new Function( "a", "return a * 2;" );
var f = function(a) { return a * 2; };
function g(a) { return a * 2; }

var h = new RegExp( "^a*b+", "g" );
var i = /^a*b+/g;
```

### `Date()` and `Error()`
The `Date(..)` and `Error(..)` native constructors are much more useful than the other natives, because there is no literal form for either. To create a date object value, you must use new `Date()`. `The Date(..`) constructor accepts optional arguments to specify the date/time to use, but if omitted, the current date/time is assumed. By far the most common reason you construct a date object is to get the current timestamp value (a signed integer number of milliseconds since Jan 1, 1970). You can do this by calling `getTime()` on a date object instance. 

But an even easier way is to just call the static helper function defined as of ES5: `Date.now()`. And to polyfill that for pre-ES5 is pretty easy:
```javascript
if (!Date.now) {
	Date.now = function(){
		return (new Date()).getTime();
	};
}
```

The `Error(..)` constructor (much like `Array()` above) behaves the same with the new keyword present or omitted.

The main reason you'd want to create an error object is that it captures the current execution stack context into the object (in most JS engines, revealed as a read-only `.stack` property once constructed). This stack context includes the function call-stack and the line-number where the error object was created, which makes debugging that error much easier. You would typically use such an error object with the throw operator:
```javascript
function foo(x) {
	if (!x) {
		throw new Error( "x wasn't provided" );
	}
	// ..
}
```

### `Symbol()`
New as of ES6, an additional primitive value type has been added, called "Symbol". Symbols are special "unique" (not strictly guaranteed!) values that can be used as properties on objects with little fear of any collision. They're primarily designed for special built-in behaviors of ES6 constructs, but you can also define your own symbols.

To define your own custom symbols, use the `Symbol(..)` native. The `Symbol(..)` native "constructor" is unique in that you're not allowed to use new with it, as doing so will throw an error.
```javascript
var mysym = Symbol( "my own symbol" );
mysym;				// Symbol(my own symbol)
mysym.toString();	// "Symbol(my own symbol)"
typeof mysym; 		// "symbol"

var a = { };
a[mysym] = "foobar";

Object.getOwnPropertySymbols( a );
// [ Symbol(my own symbol) ]
```

### Native Prototypes
