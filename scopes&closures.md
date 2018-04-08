# Chapter 2: Lexical scope
Two models: Lexical scope vs. Dynamic scope

## Lexical Scope
Lexical scope means that scope is defined by author-time decisions of where functions are declared. The lexing phase of compilation is essentially able to know where and how all identifiers are declared, and thus predict how they will be looked-up during execution.

![Lexical scope visualized](https://raw.githubusercontent.com/getify/You-Dont-Know-JS/master/scope%20%26%20closures/fig2.png)
No matter where a function is invoked from, or even how it is invoked, its lexical scope is only defined by where the function was declared.

## Cheating Lexical
### `eval(..)`
On subsequent lines of code after an eval(..) has executed, the Engine will not "know" or "care" that the previous code in question was dynamically interpreted and thus modified the lexical scope environment. The Engine will simply perform its lexical scope look-ups as it always does.

Consider the following code:
```javascript
function foo(str, a) {
	eval( str ); // cheating!
	console.log( a, b );
}

var b = 2;

foo( "var b = 3;", 1 ); // 1 3
```
The string "var b = 3;" is treated, at the point of the eval(..) call, as code that was there all along. Because that code happens to declare a new variable b, it modifies the existing lexical scope of foo(..). In fact, as mentioned above, this code actually creates variable b inside of foo(..) that shadows the b that was declared in the outer (global) scope.

### `with(..)`
The with statement takes an object, one which has zero or more properties, and treats that object as if it is a wholly separate lexical scope, and thus the object's properties are treated as lexically defined identifiers in that "scope".

```javascript
var obj = {
	a: 1,
	b: 2,
	c: 3
};

// more "tedious" to repeat "obj"
obj.a = 2;
obj.b = 3;
obj.c = 4;

// "easier" short-hand
with (obj) {
	a = 3;
	b = 4;
	c = 5;
}

function foo(obj) {
	with (obj) {
		a = 2;
	}
}

var o1 = {
	a: 3
};

var o2 = {
	b: 3
};

foo( o1 );
console.log( o1.a ); // 2

foo( o2 );
console.log( o2.a ); // undefined
console.log( a ); // 2 -- Oops, leaked global!
```

#### Performance
But if the Engine finds an eval(..) or with in the code, it essentially has to assume that all its awareness of identifier location may be invalid, because it cannot know at lexing time exactly what code you may pass to eval(..) to modify the lexical scope, or the contents of the object you may pass to with to create a new lexical scope to be consulted.

## TL;DR
Lexical scope means that scope is defined by author-time decisions of where functions are declared. The lexing phase of compilation is essentially able to know where and how all identifiers are declared, and thus predict how they will be looked-up during execution.

Two mechanisms in JavaScript can "cheat" lexical scope: eval(..) and with. The former can modify existing lexical scope (at runtime) by evaluating a string of "code" which has one or more declarations in it. The latter essentially creates a whole new lexical scope (again, at runtime) by treating an object reference as a "scope" and that object's properties as scoped identifiers.

The downside to these mechanisms is that it defeats the Engine's ability to perform compile-time optimizations regarding scope look-up, because the Engine has to assume pessimistically that such optimizations will be invalid. Code will run slower as a result of using either feature. *Don't use them*.

# Chapter 3: Function versus Block Scope
## TL;DR
Functions are the most common unit of scope in JavaScript. Variables and functions that are declared inside another function are essentially "hidden" from any of the enclosing "scopes", which is an intentional design principle of good software.

But functions are by no means the only unit of scope. Block-scope refers to the idea that variables and functions can belong to an arbitrary block (generally, any { .. } pair) of code, rather than only to the enclosing function.

Starting with ES3, the try/catch structure has block-scope in the catch clause.

In ES6, the let keyword (a cousin to the var keyword) is introduced to allow declarations of variables in any arbitrary block of code. if (..) { let a = 2; } will declare a variable a that essentially hijacks the scope of the if's { .. } block and attaches itself there.

Though some seem to believe so, block scope should not be taken as an outright replacement of var function scope. Both functionalities co-exist, and developers can and should use both function-scope and block-scope techniques where respectively appropriate to produce better, more readable/maintainable code.

## Scopes from Functions
Function scope encourages the idea that all variables belong to the function, and can be used and reused throughout the entirety of the function (and indeed, accessible even to nested scopes). This design approach can be quite useful, and certainly can make full use of the "dynamic" nature of JavaScript variables to take on values of different types as needed.

## Hiding in Plain Scope
Hide code in scope. 
There's a variety of reasons motivating this scope-based hiding. They tend to arise from the software design principle "Principle of Least Privilege" [^note-leastprivilege], also sometimes called "Least Authority" or "Least Exposure". This principle states that in the design of software, such as the API for a module/object, you should expose only what is minimally necessary, and "hide" everything else.

## Blocks as Scopes
That's what block-scoping is all about. Declaring variables as close as possible, as local as possible, to where they will be used. Another example:

# Chapter 4: Hoisting
## TL;DR
We can be tempted to look at var a = 2; as one statement, but the JavaScript Engine does not see it that way. It sees var a and a = 2 as two separate statements, the first one a compiler-phase task, and the second one an execution-phase task.

What this leads to is that all declarations in a scope, regardless of where they appear, are processed first before the code itself is executed. You can visualize this as declarations (variables and functions) being "moved" to the top of their respective scopes, which we call "hoisting".

Declarations themselves are hoisted, but assignments, even assignments of function expressions, are not hoisted.

Be careful about duplicate declarations, especially mixed between normal var declarations and function declarations -- peril awaits if you do!

# Chapter 5: Scope Closure
## TL;DR
Closure seems to the un-enlightened like a mystical world set apart inside of JavaScript which only the few bravest souls can reach. But it's actually just a standard and almost obvious fact of how we write code in a lexically scoped environment, where functions are values and can be passed around at will.

Closure is when a function can remember and access its lexical scope even when it's invoked outside its lexical scope.

Closures can trip us up, for instance with loops, if we're not careful to recognize them and how they work. But they are also an immensely powerful tool, enabling patterns like modules in their various forms.

Modules require two key characteristics: 1) an outer wrapping function being invoked, to create the enclosing scope 2) the return value of the wrapping function must include reference to at least one inner function that then has closure over the private inner scope of the wrapper.

## Closure
Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.
```javascript
function foo() {
	var a = 2;

	function bar() {
		console.log( a ); // 2
	}

	bar();
}

foo();
```

Now we can see closures all around our existing code, and we have the ability to recognize and leverage them to our own benefit!
