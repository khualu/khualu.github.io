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

