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
1. _Compiler_ will start lexing the code to break it down into tokens, then it will parse it into a tree. 
  1. Encountering `var a`, _compiler_ will ask _scope_ to see if a variable `a` already exists for that particular scope. If that's the case, _compiler_ igonres this declaration and moves on. Otherwhise, _compiler_ asks scope to declare the new variable called `a` for that scope collection.
  2. _Compiler_ then produces code for _engine_ to later execute, to handle the `a = 2` assignment. The code _engine_ runs will first ask _scope_ if there is a variable `a` accessible in the current scope. If this is the case, _engine_ uses that variable. If not, _engine_ looks further elsewhere for a variable called `a`. 
2. If _engine_ finds a variable, it assigns the value `2` to it. If not _engine_ will give you and error. 
