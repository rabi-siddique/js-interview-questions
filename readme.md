# Scopes

## What are Scopes in JavaScript?

Scopes are sections of our code where a variable is defined and can be accessed.

There are 4 main types of Scopes in JavaScript:

- **Global Scope**: All variables are visible to the entire program.
- **Function Scope**: All variables are visible inside of a function.
- **Block Scope**: All variables are visible inside of a block.
- **Module Scope**: All variables are visible within the module.

There are three factors that determine the scope of a variable:

- How is it was declared? i.e. Using `let`, `const`, `var`, `function expression`, `function declaration` etc.
- Where it was declared? i.e. `Function`, `Class`, `Block` etc.
- Are we in strict mode or non strict mode.

## What is Lexical Scope?

Scopes are determined at compile time. And at compile time it is determined by examining the location a function is declared. Instead of where it is being invoked. Hence, Lexical scope means that scope is defined where the functions are declared by looking the structure of the code.

In other words, lexical scope refers to the ability of a function scope to access variables from the parent scope. When there is lexical scope, the innermost and outermost functions may access all variables from their parent scopes all the way up to the global scope.

## How does JavaScript implicitly declare variables, and what impact does the use of strict mode have on this behavior?

Consider this piece of code:

```js
var a = b;
```

In this piece of code, when JS engine encounters the line of code, it will search for the variable `b` in the scope chain. It starts searching for `b` in the currently executing scope. If it's not found there, the searching is shifted to the scopes outside the currently executing scopes. This process of searching keeps on running scope by scope, until we reach the global scope. And when it is not found, JavaScript implicitly creates `b` in the global scope.

However, if we were in `strict mode`, this won't happen. In that case, a `ReferenceError` is produced.

## What is Shadowing in JavaScript?

In nested scopes, we can have variables with the same name. In that case, JS engine stops searching for the variable upon finding the first match. This is called Shadowing in JavaScript.

A thing to note here is that in JavaScript, variables in the global scope are part of the window object. So if we were to access these variables in a lexical scope, we can use `window.variableName`. This way we can access variables declared in global scope that would have been otherwise inaccessbile due to shadowing. However, its important to note, that non-global shadowed variables can't be accessed.

## What are Closures in JavaScript?

Closures in JavaScript is a concept which allows a function to gain access to all the variables declared outside of its own scope. A function bundled with the surrounding scopes forms a closure.
Now this is powerful because because of this functions can persist state values.We can acheive encapsulation via closures. Using closures you can not manipulate variables directly. For any such operation you have to make use of the function which has a closure.
