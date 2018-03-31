# Samenvatting Frontend

## YDKJS: Up & Going

### Chapter 1: Into Programming

#### Statements

In a computer language, a group of words, numbers, and operators that performs a specific task is a statement. In JavaScript, a statement might look as follows:

`a = b * 2;`

`=` & `*` are _operators_

#### Expressions

Statements are made up of one or more expressions. An expression is any reference to a variable or value, or a set of variable\(s\) and value\(s\) combined with operators.

For example:

`a = b * 2;`  
This statement has four expressions in it:

* `2` is a _literal value expression_
* `b` is a _variable expression_, which means to retrieve its current value
* `b * 2` is an _arithmetic expression_, which means to do the multiplication
* `a = b * 2` is an _assignment expression_, which means to assign the result of the b \* 2 expression to the variable a \(more on assignments later\)

A more common expression statement is a call expression statement \(see "Functions"\), as the entire statement is the function call expression itself:

`alert( a );`

#### Executing a Program

Most people think that JS is interpreted, since the code is processed every single time the progam runs. This is not completely true. The JS engine actually compiles the program on the fly and then immediately runs the compiled code.

##### Interpreting the code

For some computer languages, this translation of commands is typically done from top to bottom, line by line, every time the program is run, which is usually called interpreting the code.

##### Compiling the code

For other languages, the translation is done ahead of time, called compiling the code, so when the program runs later, what's running is actually the already compiled computer instructions ready to go.

#### Input

Most forms of input are from form elements in HTML. There are other \(easier\) ways to do this. Such as the `prompt(...);` function.

```javascript
age = prompt( "Please tell me your age:" );
console.log( age );
```

#### Operators

Operators are how we perform actions on variables and values. While not technically an operator, you'll need the keyword var in every program, as it's the primary way you declare \(aka create\) variables \(see "Variables"\). Here are the most used operators

* Assignment: = as in a = 2.

* Math: + \(addition\), - \(subtraction\), _ \(multiplication\), and / \(division\), as in a _ 3.

* Compound Assignment: +=, -=, \*=, and /= are compound operators that combine a math operation with assignment, as in a += 2 \(same as a = a + 2\).

* Increment/Decrement: ++ \(increment\), -- \(decrement\), as in a++ \(similar to a = a + 1\).

* Object Property Access: . as in console.log\(\).

* Objects are values that hold other values at specific named locations called properties. obj.a means an object value called obj with a property of the name a. Properties can alternatively be accessed as obj\["a"\]. See Chapter 2.

* Equality: == \(loose-equals\), === \(strict-equals\), != \(loose not-equals\), !== \(strict not-equals\), as in a == b.

See "Values & Types" and Chapter 2.

* Comparison: &lt; \(less than\), &gt; \(greater than\), &lt;= \(less than or loose-equals\), &gt;= \(greater than or loose-equals\), as in a &lt;= b.

See "Values & Types" and Chapter 2.

* Logical: && \(and\), \|\| \(or\), as in a \|\| b that selects either a or b.

These operators are used to express compound conditionals \(see "Conditionals"\), like if either a or b is true.

#### Values & Types

The primitive values of JS are:

* `number`
* `string`
* `boolean` \(decision making\)

There are also _arrays, objects & functions_, but we'll get there later.

#### Converting Between Types

* _Coercion_ is turning a type into another type, like turning a `number` into a `string` or vice versa. JS provides several different ways for forcibly coercing between types. For example: 

```javascript
var a = "42";
var b = Number( a );

console.log( a );    // "42"
console.log( b );    // 42
```

Using Number\(..\) \(a built-in function\) as shown is an _explicit_ coercion from any other type to the number type. That should be pretty straightforward. But a controversial topic is what happens when you try to compare two values that are not already of the same type, which would require implicit coercion.

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

##### Type Enforcement \(Static typing\)

In some programming languages, you declare a variable \(container\) to hold a specific type of value, such as number or string. Static typing, otherwise known as type enforcement, is typically cited as a benefit for program correctness by preventing unintended value conversions.

##### Weak typing

Other languages emphasize types for values instead of variables. Weak typing, otherwise known as dynamic typing, allows a variable to hold any type of value at any time. It's typically cited as a benefit for program flexibility by allowing a single variable to represent a value no matter what type form that value may take at any given moment in the program's logic flow.

JavaScript uses the latter approach, dynamic typing, meaning variables can hold values of any type without any type enforcement. This is an example of how the var `amount` changes through the program from a `number` to a `string`.

```javascript
var amount = 99.99;

amount = amount * 2;

console.log( amount );        // 199.98

// convert `amount` to a string, and
// add "$" on the beginning
amount = "$" + String( amount );

console.log( amount );        // "$199.98"
```

The newest version of JavaScript at the time of this writing \(commonly called "ES6"\) includes a new way to declare_constants_, by using`const`instead of`var`:

```js
//
 as of ES6:
const TAX_RATE = 0.08;


var amount = 99.99;
//
 ..
```

Constants are useful just like variables with unchanged values, except that constants also prevent accidentally changing value somewhere else after the initial setting. If you tried to assign any different value to`TAX_RATE `after that first declaration, your program would reject the change \(and in strict mode, fail with an error -- see "Strict Mode" in Chapter 2\).

#### Blocks

In JavaScript, a block is defined by wrapping one or more statements inside a curly-brace pair

`{ .. } `. Consider:

```js
var amount = 99.99;


// a general block
{
	amount = amount * 2;
	console.log ( amount ); //199.98
}
```









