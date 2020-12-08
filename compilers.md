**Compilers Cheatsheet**

**Babel**
Javascript compiler
  not your traditional compiler. Doesn't generate assembly
  **input:**Javascript
  **output:**Javascript
  javascript has the same meaning, just in a different form
  it is plugin dependant, one pluggin is covers a specific use case or small scope, use presets! (includes all the specific plugins for a specific scope)

Babel compiles the human readable code: Javascript, to machine friendly format named **AST** Abstract Syntax Tree.

Babel expresses its AST as a normal JSON object

![Introduction to Babel. Babel is a JavaScript compiler. Babel… | by Imran  Sayed | Medium](https://miro.medium.com/max/839/1*GSjQeeWwVl5_tLZBOrNSSA.png)

Babel is famous for transpiling ES6 Javascript code to ES5 Javascript code so developers can write code in ES6 which is more intuitive meanwhile clients consume transpiled ES5 code which actuallyl implements the same logic with older syntax but with better compatibility.

Wrapped **inside @babel/core** API is
**Parse:**@babel/parser 
Babel parses source code to AST which is and abstraction of your code for the machine.
continue watching here[5:12](https://www.youtube.com/watch?v=UeVq_U5obnE)

  Lexical analysis:
    transform input source code into a list of tokens
    reports errors about invalid literal or characters
  Syntax analysis:
    transform the list of tokens into an AST
    automatic semicolon insertion ASI
    reports errpors about misplaced tokens
  Semantic analysis
    check the AST respects all static ECMAscript rules
    reports errors about inbalid variables, using a scope tracker
**Transform:**@babel/traverse 
Babel traverses AST to explore, analyse or modify AST programmatically

provides a declarative way to visit every node in the AST, uses Depth-first search

performs scope analysis
  collects different scopes
    collects all the declarations within each scope
      collects all the useages of each variable

@babel/traverse provides many utilities to transform / analyze the AST

**Generate:**@babel/generator 
Babel generates actual runtime code based on AST

It is fast and opinionated. It cares about speed not readability, meant for computer readability. It is not a pretty printer

**small notes**

babel plugins are usually applied at bundling systems such as Webpack (with babel-loader) to resolve Javascript version compatibility issue.