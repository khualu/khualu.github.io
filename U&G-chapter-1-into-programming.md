# Samenvatting Frontend

## YDKJS: Up & Going

### Chapter 1: Into Programming
#### Statements
In a computer language, a group of words, numbers, and operators that performs a specific task is a statement. In JavaScript, a statement might look as follows:

`a = b * 2;`

`=` & `*` are _operators_

#### Expressions
Statements are made up of one or more expressions. An expression is any reference to a variable or value, or a set of variable(s) and value(s) combined with operators.

For example:

`a = b * 2;`
This statement has four expressions in it:

* `2` is a _literal value expression_
* `b` is a _variable expression_, which means to retrieve its current value
* `b * 2` is an _arithmetic expression_, which means to do the multiplication
* `a = b * 2` is an _assignment expression_, which means to assign the result of the b * 2 expression to the variable a (more on assignments later)

A more common expression statement is a call expression statement (see "Functions"), as the entire statement is the function call expression itself:

`alert( a );`

#### Executing a Program
Most people think that JS is interpreted, since the code is processed every single time the progam runs. This is not completely true. The JS engine actually compiles the program on the fly and then immediately runs the compiled code.

##### Interpreting the code
For some computer languages, this translation of commands is typically done from top to bottom, line by line, every time the program is run, which is usually called interpreting the code.

##### Compiling the code
For other languages, the translation is done ahead of time, called compiling the code, so when the program runs later, what's running is actually the already compiled computer instructions ready to go.

#### Input
Most forms of input are from form elements in HTML. There are other (easier) ways to do this. Such as the `prompt(...);` function. 

```javascript
age = prompt( "Please tell me your age:" );
console.log( age );
```

#### Operators 
Operators are how we perform actions on variables and values. While not technically an operator, you'll need the keyword var in every program, as it's the primary way you declare (aka create) variables (see "Variables"). Here are the most used operators

* Assignment: = as in a = 2.

* Math: + (addition), - (subtraction), * (multiplication), and / (division), as in a * 3.

* Compound Assignment: +=, -=, *=, and /= are compound operators that combine a math operation with assignment, as in a += 2 (same as a = a + 2).

* Increment/Decrement: ++ (increment), -- (decrement), as in a++ (similar to a = a + 1).

* Object Property Access: . as in console.log().

* Objects are values that hold other values at specific named locations called properties. obj.a means an object value called obj with a property of the name a. Properties can alternatively be accessed as obj["a"]. See Chapter 2.

* Equality: == (loose-equals), === (strict-equals), != (loose not-equals), !== (strict not-equals), as in a == b.

See "Values & Types" and Chapter 2.

* Comparison: < (less than), > (greater than), <= (less than or loose-equals), >= (greater than or loose-equals), as in a <= b.

See "Values & Types" and Chapter 2.

* Logical: && (and), || (or), as in a || b that selects either a or b.

These operators are used to express compound conditionals (see "Conditionals"), like if either a or b is true.

#### Values & Types
The primitive values of JS are:
* `number`
* `string`
* `boolean` (decision making)

There are also _arrays, objects & functions_, but we'll get there later. 

#### Converting Between Types
* _Coercion_ is turning a type into another type, like turning a `number` into a `string` or vice versa. JS provides several different ways for forcibly coercing between types. For example: 

```javascript
var a = "42";
var b = Number( a );

console.log( a );	// "42"
console.log( b );	// 42
```
Using Number(..) (a built-in function) as shown is an _*explicit*_ coercion from any other type to the number type. That should be pretty straightforward. But a controversial topic is what happens when you try to compare two values that are not already of the same type, which would require implicit coercion.

When comparing the string "99.99" to the number 99.99, most people would agree they are equivalent. But they're not exactly the same, are they? It's the same value in two different representations, two different types. You could say they're "loosely equal," couldn't you? `==` is the loose equals operator. `"99.99 == 99.99` will give `true`. 


#### Comments

```javascript
// This is a single-line comment

/* But this is
       a multiline
             comment.
                      */
```

#### Variables

##### Type Enforcement (Static typing)
In some programming languages, you declare a variable (container) to hold a specific type of value, such as number or string. Static typing, otherwise known as type enforcement, is typically cited as a benefit for program correctness by preventing unintended value conversions.

##### Weak typing
Other languages emphasize types for values instead of variables. Weak typing, otherwise known as dynamic typing, allows a variable to hold any type of value at any time. It's typically cited as a benefit for program flexibility by allowing a single variable to represent a value no matter what type form that value may take at any given moment in the program's logic flow.

JavaScript uses the latter approach, dynamic typing, meaning variables can hold values of any type without any type enforcement. This is an example of how the var `amount` changes through the program from a `number` to a `string`. 
```javascript
var amount = 99.99;

amount = amount * 2;

console.log( amount );		// 199.98

// convert `amount` to a string, and
// add "$" on the beginning
amount = "$" + String( amount );

console.log( amount );		// "$199.98"
```

The newest version of JavaScript at the time of this writing (commonly called "ES6") includes a new way to declareconstants, by usingconstinstead ofvar:
```
//
 as of ES6:
const TAX_RATE = 0.08;


var amount = 99.99;
//
```

Constants are useful just like variables with unchanged values, except that constants also prevent accidentally changing value somewhere else after the initial setting. If you tried to assign any different value toTAX_RATEafter that first declaration, your program would reject the change (and in strict mode, fail with an error -- see "Strict Mode" in Chapter 2).

####Blocks
In JavaScript, a block is defined by wrapping one or more statements inside a curly-brace pair

`{ .. }`. Consider:
```javascript
var amount = 99.99;


// a general block
{
    amount = amount * 2;
    console.log ( amount ); //199.98
}
```

#### Conditionals 
There are quite a few ways we can express _conditionals_ (aka decisions) in our programs.
The most common one is the if statement. Essentially, you're saying, "If this condition is true, do the following...". For example:

```javascript
var bank_balance = 302.13;
var amount = 99.99;

if (amount < bank_balance) {
	console.log( "I want to buy this phone!" );
}
```

As we discussed in "Values & Types" earlier, values that aren't already of an expected type are often coerced to that type. The if statement expects a `boolean`, but if you pass it something that's not already boolean, coercion will occur.

JavaScript defines a list of specific values that are considered "falsy" because when coerced to a boolean, they become `false` -- these include values like `0` and `""`. Any other value not on the "falsy" list is automatically "truthy" -- when coerced to a boolean they become true. Truthy values include things like 99.99 and "free". See "Truthy & Falsy" in Chapter 2 for more information.


#### Loops
For example, the `while` loop and the `do..while` loop forms illustrate the concept of repeating a block of statements until a condition no longer evaluates to true:

```javascript
while (numOfCustomers > 0) {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
}

// versus:

do {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
} while (numOfCustomers > 0);
```
The only practical difference between these loops is whether the conditional is tested _*before*_ the first iteration (`while`) or _*after*_ the first iteration (`do..while`).

We can use JavaScript's `break` statement to stop a loop. Also, we can observe that it's awfully easy to create a loop that would otherwise run forever without a breaking mechanism.

```javascript
var i = 0;

// a `while..true` loop would run forever, right?
while (true) {
	// stop the loop?
	if ((i <= 9) === false) {
		break;
	}

	console.log( i );
	i = i + 1;
}
// 0 1 2 3 4 5 6 7 8 9
```

While a `while` (or `do..while`) can accomplish the task manually, there's another syntactic form called a `for` loop for just that purpose:
```javascript
for (var i = 0; i <= 9; i = i + 1) {
	console.log( i );
}
// 0 1 2 3 4 5 6 7 8 9
```

#### Functions

Functions can optionally take arguments (aka parameters) -- values you pass in. And they can also optionally return a value back.
```javascript
function printAmount(amt) {
	console.log( amt.toFixed( 2 ) );
}

function formatAmount() {
	return "$" + amount.toFixed( 2 );
}

var amount = 99.99;

printAmount( amount * 2 );		// "199.98"

amount = formatAmount();
console.log( amount );			// "$99.99"
```
The function printAmount(..) takes a parameter that we call amt. The function formatAmount() returns a value. Of course, you can also combine those two techniques in the same function.

Functions are often used for code that you plan to call multiple times, but they can also be useful just to organize related bits of code into named collections, even if you only plan to call them once.

Consider:
```javascript
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}

var amount = 99.99;

amount = calculateFinalPurchaseAmount( amount );

console.log( amount.toFixed( 2 ) );		// "107.99"
```

#### Scope
In JavaScript, each function gets its own scope. Scope is basically a collection of variables as well as the rules for how those variables are accessed by name. Only code inside that function can access that function's scoped variables. A variable name has to be unique within the same scope -- there can't be two different `a` variables sitting right next to each other. But the same variable name `a` could appear in different scopes.

```javascript
function one() {
	// this `a` only belongs to the `one()` function
	var a = 1;
	console.log( a );
}

function two() {
	// this `a` only belongs to the `two()` function
	var a = 2;
	console.log( a );
}

one();		// 1
two();		// 2
```

Also, a scope can be nested inside another scope, just like if a clown at a birthday party blows up one balloon inside another balloon. If one scope is nested inside another, code inside the innermost scope can access variables from either scope.

Consider:
```javascript
function outer() {
	var a = 1;

	function inner() {
		var b = 2;

		// we can access both `a` and `b` here
		console.log( a + b );	// 3
	}

	inner();

	// we can only access `a` here
	console.log( a );			// 1
}

outer();
```
_*Lexical scope*_ rules say that code in one scope can access variables of either that scope or any scope outside of it.

So, code inside the inner() function has access to both variables a and b, but code in outer() has access only to a -- it cannot access b because that variable is only inside inner()

#### REVIEW
Learning programming doesn't have to be a complex and overwhelming process. There are just a few basic concepts you need to wrap your head around.

These act like building blocks. To build a tall tower, you start first by putting block on top of block on top of block. The same goes with programming. Here are some of the essential programming building blocks:

* You need operators to perform actions on values.
* You need values and types to perform different kinds of actions like math on numbers or output with strings.
* You need variables to store data (aka state) during your program's execution.
* You need conditionals like if statements to make decisions.
* You need loops to repeat tasks until a condition stops being true.
* You need functions to organize your code into logical and reusable chunks.

Code comments are one effective way to write more readable code, which makes your program easier to understand, maintain, and fix later if there are problems.

Finally, don't neglect the power of practice. The best way to learn how to write code is to write code.

I'm excited you're well on your way to learning how to code, now! Keep it up. Don't forget to check out other beginner programming resources (books, blogs, online training, etc.). This chapter and this book are a great start, but they're just a brief introduction.

The next chapter will review many of the concepts from this chapter, but from a more JavaScript-specific perspective, which will highlight most of the major topics that are addressed in deeper detail throughout the rest of the series.

