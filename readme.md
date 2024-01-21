# Scopes and Closures

## Scopes

Scope is like a look up list for variables. It enforces a set of rules related to the accessibility of these variables for the currently executing code.

When JavaScript code is being compiled, variables are allocated memory. However when execution of code happens, the variable assignments are performed.

Consider this code:

```js
var a = 2;
```

In compilation, `a` is allocated memory. In exection, JS Engine finds `a` in the current scope. If it is present there, it will assign the value `2` to it. If its not found, JS Engine will look for the variable in the outercontaining scopes. The process of looking in the outercontaining scopes is continued until the global scope is reached.

If the variable is not found in global scope, a `Reference Error` is thrown, if we are in `strict mode`. However, in `non-strict mode`, the variable is created in the global scope.

About the errors, the `Reference Error` is related to scopes. If scope resolution fails for a variable, we will encounter this error. On the other hand, with `Type Error`, scope resolution was successful. But we are trying to perform some illegal operation on the result of scope resolution. For example, invoking a primitive as a function, trying to reference a property on an `undefined` or `null` value.

Lexical Scope is the scope that is defined at lexing time. No matter where a function is invoked from or how it is invoked, its lexical scope is always defined by where the function was declared. To cheat the lexical scope at runtime:

- eval funcion
- with

## Closures

A closure is mechanism through which a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

When a function is called in JavaScript, a new local memory, also known as a variable environment or state, is created for that function's execution context. This local memory stores the function's variables parameters, and references to any variables from outer scopes that the function needs. After the function completes its execution, its local memory is typically deleted or garbage-collected, except for the returned value (if any).

Closures allow functions to retain access to variables from the outercontaining scopes even after the outer function has finished executing. This means that a function returned from another function (creating a closure) can still access and manipulate the variables of its parent function, even if the parent function's local memory was deleted.

This allows for powerful patterns like encapsulation, data hiding, and maintaining state across function calls.

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

Scopes are determined at compile time. And at compile time it is determined by examining the location where the function is declared. Lexical scope means that no matter where a function is invoked from or how it is invoked, its lexical scope is always defined by where the function was declared.

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

## What is the importance of Scopes?

Scopes are important because:

- **Avoiding Variable Name Collisions**: Different parts of a program might use the same variable names for different purposes. By using scopes, you can create local variables that are only accessible within a specific scope, preventing unintentional collisions with variables in other parts of the program. This helps in maintaining code integrity and readability.

- **Encapsulation**: Scopes contribute to the concept of encapsulation, where the internal details of a function or block of code are hidden from the outside world. This is important in preventing unintended data corruption or unauthorized access.

## What are Closures in JavaScript?

Closures in JavaScript is a concept which allows a function to gain access to all the variables declared outside of its own scope. A function bundled with the surrounding scopes forms a closure. Now this is powerful because it let our function definitions have an associated cache/persistent memory.

Closure acts as a permanent memory for the function. The local memory of the function is deleted when the function execution is completed. However, the closure which is like a backpack of data, is permanent. It persists in subsequent function calls.

The closrue contains all the variables used by the function. The variables not used inside the function are not linked to its closure. JavaScript does this optimization for us. JavaScript Engine sees inside the function definition to get an idea of what variables are being used by it. It then adds only the variable used inside the function definition to the closure.

The key thing to remember is that when a function gets declared, it contains a function definition and a closure.

## Read More Sources

- [Scopes of Variables](https://stackoverflow.com/questions/500431/what-is-the-scope-of-variables-in-javascript#:~:text=Javascript%20uses%20scope%20chains%20to,linked%20to%20the%20outer%20function.)
- [UnderStanding Scope and Scope Chain in JavaScript](https://blog.bitsrc.io/understanding-scope-and-scope-chain-in-javascript-f6637978cf53#:~:text=JavaScript%20uses%20lexical%20scope%20which,every%20JavaScript%20developer%20should%20understand.)
- [Closures](https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8)
