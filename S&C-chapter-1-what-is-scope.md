# What is Scope?
Without such a concept, a program could perform some tasks, but they would be extremely limited and not terribly interesting. But, where and how do these Scope rules get set?

## Compilter Theory
Despite the fact that Javascript is a _dynamic_ or _interpreted_ language, it is in fact a compiled language. It is not compiled well in advance tho. In a traditional compiled-langauge process, a chunk of source code will undergo three stepts _before_ it is executed. This process is called _compilation_:
1. *Tokenizing/Lexing*: breaking up a string of characters into chunks (_tokens_) that are meaningful to the language. (e.g. `var a = 2;` would become `var`, `a`, `=`, `2` and `;`. Whitespace may or may not be a token depending on if it's meaningful or not. 
2. *Parsing*: Taking a stream (array) of tokens and turning it into a tree of nested elements. This tree collectively represents the grammatical structure of the program. The tree is called an Abstract Syntax Tree (_AST_). 
> The tree for `var a = 2;` might start with a top-level node called `VariableDeclaration`, with a child node called `Identifier` (whose value is `a`), and another child called `AssignmentExpression` which itself has a child called `NumericLitera`l (whose value is `2`).
3. *Code-Generation*: The proces of taking an AST and turning into executable code. This part differs a lot for every language. 


The JS engine is way complexer. JS engines don't have the luxury of having plenty of time to optimize, b/c JS compilation doesn't happen in a build step ahead of time, as with other languages. For JS the compilation occurs in a few microseconds before the code is executed. So, long story short, the JS compiler takes `var a = 2;` and compiles it first, and then it's ready to be executed. 

## Understanding Scope
The way we will approach learning about scope is to think of the process in terms of a conversation. But, who is having the conversation?

### The Cast
Let's meet the cast of characters that interact to process the program `var a = 2;`, so we understand their conversations that we'll listen in on shortly:
1. *Engine*: responsible for start-to-finish compilation and execution of our Javascript program.
2. *Compiler*: one of _engine's_ friends, handles all the dirty work of parsing and code-generation.
3. *Scope*: another friend of _engine_; collects and maintains a look-up list of all the declared identifiers(variables) and enforces a strict set of rules as how tese are accessible to currently executing code. 

### Back & Forth
So, how does this work. _Engine_  sees two distinct statements, one which _compiler_ will handle during the compilation and one which _engine_ will handle during the execution. Let's break down how this goes:
* _Compiler_ will start lexing the code to break it down into tokens, then it will parse it into a tree. 
  1. Encountering `var a`, _compiler_ will ask _scope_ to see if a variable `a` already exists for that particular scope. If that's the case, _compiler_ igonres this declaration and moves on. Otherwhise, _compiler_ asks scope to declare the new variable called `a` for that scope collection.
  2. _Compiler_ then produces code for _engine_ to later execute, to handle the `a = 2` assignment. The code _engine_ runs will first ask _scope_ if there is a variable `a` accessible in the current scope. If this is the case, _engine_ uses that variable. If not, _engine_ looks further elsewhere for a variable called `a`. 
* If _engine_ finds a variable, it assigns the value `2` to it. If not _engine_ will give you and error. 

*To summarize:* two distinct actions are taken for a variable assignment: 
1. Compiler declares a variable (if not previously declared in the current scope).
2. When executing, Engine looks up the variable in Scope and assigns to it, if found.

### Compiler Speak
When Engine executes the code that Compiler produced for step (2), it has to look-up the variable a to see if it has been declared, and this look-up is consulting Scope. But the type of look-up Engine performs affects the outcome of the look-up. There are two kinds of look-ups. _Left-hand Side_ & _Right-hand Side_. 

* RHS is for when you want to retrieve a value and pass it down to another operator/variable.
* LHS is for when you don't care what the current value is and you just want a variable to target an assignment. 

Exmaples:
This is a RHS reference. Because nothing is being assigned to `a`. We're just looking to retrieve the value of `a` and pass it down to `console.log();`. 
```javascript
console.log(a);
```
On the other hand (_huehuehue_), this here is a LHS. This is because we don't really care what the current value is, we simply want to find the variable as a target of the `= 2` assignment operation. 
```javascript
a = 2;
```
However, the subtle but important difference is that Compiler handles both the declaration and the value definition during code-generation, such that when Engine is executing code, there's no processing necessary to "assign" a function value to foo. Thus, it's not really appropriate to think of a function declaration as an LHS look-up assignment in the way we're discussing them here.

### Engine/Scope Conversation
```javascript
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```
Let's imagine the above exchange (which processes this code snippet) as a conversation. The conversation would go a little something like this:

> Engine: Hey Scope, I have an RHS reference for foo. Ever heard of it?

> Scope: Why yes, I have. Compiler declared it just a second ago. He's a function. Here you go.

> Engine: Great, thanks! OK, I'm executing foo.

> Engine: Hey, Scope, I've got an LHS reference for a, ever heard of it?

> Scope: Why yes, I have. Compiler declared it as a formal parameter to foo just recently. Here you go.

> Engine: Helpful as always, Scope. Thanks again. Now, time to assign 2 to a.

> Engine: Hey, Scope, sorry to bother you again. I need an RHS look-up for console. Ever heard of it?

> Scope: No problem, Engine, this is what I do all day. Yes, I've got console. He's built-in. Here ya go.

> Engine: Perfect. Looking up log(..). OK, great, it's a function.

> Engine: Yo, Scope. Can you help me out with an RHS reference to a. I think I remember it, but just want to double-check.

> Scope: You're right, Engine. Same guy, hasn't changed. Here ya go.

> Engine: Cool. Passing the value of a, which is 2, into log(..).


## Nested Scope
Just as a block or function is nested inside another block or function, scopes are nested inside other scopes. So, if a variable cannot be found in the immediate scope, Engine consults the next outer containing scope, continuing until found or until the outermost (aka, global) scope has been reached.
```javascript
function foo(a) {
	console.log( a + b );
}

var b = 2;

foo( 2 ); // 4
```
The RHS reference for `b` cannot be resolved inside the function foo, but it can be resolved in the Scope surrounding it (in this case, the global).

## Errors
Why does it matter if it's a LHS or RHS? Well, they both behave differently in the circumstance where the variable has not yet been declared.
```javascript
function foo(a) {
	console.log( a + b );
	b = a;
}

foo( 2 );
```
Both LHS and RHS reference look-ups start at the currently executing Scope, and if need be (that is, they don't find what they're looking for there), they work their way up the nested Scope, one scope (floor) at a time, looking for the identifier, until they get to the global (top floor) and stop, and either find it, or don't.

If RHS fails the look-up, it results in a `ReferenceError`. 
If LHS fails the look-up, it makes va riable in the global scope. 

Unfulfilled RHS references result in `ReferenceError`s being thrown. Unfulfilled LHS references result in an automatic, implicitly-created global of that name (if not in "Strict Mode" [^note-strictmode]), or a ReferenceError (if in "Strict Mode" [^note-strictmode]).
