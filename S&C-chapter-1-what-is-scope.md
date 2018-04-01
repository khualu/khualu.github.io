# What is Scope?
Without such a concept, a program could perform some tasks, but they would be extremely limited and not terribly interesting. But, where and how do these Scope rules get set?

## Compilter Theory
Despite the fact that Javascript is a _dynamic_ or _interpreted_ language, it is in fact a compiled language. It is not compiled well in advance tho. In a traditional compiled-langauge process, a chunk of source code will undergo three stepts _before_ it is executed. This process is called _compilation_:
1. *Tokenizing/Lexing*: breaking up a string of characters into chunks (_tokens_) that are meaningful to the language. (e.g. `var a = 2;` would become `var`, `a`, `=`, `2` and `;`. Whitespace may or may not be a token depending on if it's meaningful or not. 
2. *Parsing*: Taking a stream (array) of tokens and turning it into a tree of nested elements. This tree collectively represents the grammatical structure of the program. The tree is called an Abstract Syntax Tree (_AST_). 
  * The tree for `var a = 2;` might start with a top-level node called `VariableDeclaration`, with a child node called `Identifier` (whose value is `a`), and another child called `AssignmentExpression` which itself has a child called `NumericLitera`l (whose value is `2`).
