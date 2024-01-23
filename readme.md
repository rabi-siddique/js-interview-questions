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

## What are the benefits of closures?

- **Encapsulation**: Closures allow for the creation of private variables and functions. Variables within a closure are not directly accessible from the outside, helping to maintain the integrity and security of the data.

- **Maintaining State**: Closures enable the creation of functions that can maintain and update their internal state across multiple invocations. This is particularly useful for scenarios where you need to keep track of some information between function calls without exposing it globally.

- **Module Pattern**: Closures are instrumental in implementing the module pattern in JavaScript. By creating a function that returns an object containing methods and variables, you can simulate private and public members, achieving a level of organization and encapsulation.

```js
function myModule() {
  // Private variables and functions
  var privateVar = 'I am private';

  function privateFunction() {
    console.log('This is a private function');
  }

  // Public API (methods and variables accessible from the outside)
  return {
    // Public variables
    publicVar: 'I am public',

    // Public function
    publicFunction: function () {
      console.log('This is a public function');
      // It can access private variables and functions
      console.log(privateVar);
      privateFunction();
    },
  };
}
```

- **Factory Functions**: Closures are often used in the creation of factory functions. These functions produce and return other functions with specific behaviors or configurations based on the arguments provided. This allows for the creation of reusable and customizable code.

```js
function createMathOperation(operationType) {
  return function (operand1, operand2) {
    switch (operationType) {
      case 'add':
        return operand1 + operand2;
      case 'subtract':
        return operand1 - operand2;
      case 'multiply':
        return operand1 * operand2;
      case 'divide':
        return operand1 / operand2;
      default:
        throw new Error('Invalid operation type');
    }
  };
}

// Create specific math operation functions using the factory
var addFunction = createMathOperation('add');
var subtractFunction = createMathOperation('subtract');
var multiplyFunction = createMathOperation('multiply');
var divideFunction = createMathOperation('divide');

// Use the created functions
console.log(addFunction(5, 3)); // Output: 8
console.log(subtractFunction(8, 2)); // Output: 6
console.log(multiplyFunction(4, 6)); // Output: 24
console.log(divideFunction(10, 2)); // Output: 5
```

## Read More Sources

- [Scopes of Variables](https://stackoverflow.com/questions/500431/what-is-the-scope-of-variables-in-javascript#:~:text=Javascript%20uses%20scope%20chains%20to,linked%20to%20the%20outer%20function.)
- [UnderStanding Scope and Scope Chain in JavaScript](https://blog.bitsrc.io/understanding-scope-and-scope-chain-in-javascript-f6637978cf53#:~:text=JavaScript%20uses%20lexical%20scope%20which,every%20JavaScript%20developer%20should%20understand.)
- [Closures](https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8)

# Promises

## What is a Promise?

A promise is a JavaScript object which will produce a single value some time in the future. It is a mechanism to perform async operations in JavaScript. It has three states. These are:

- `Pending`: Starting state of every promise, neither fulfilled nor rejected.
- `Fulfilled`: The operation completed successfully.
- `Rejected`: The operation failed.

If the promise is successful, it will produce a resolved value, but if something goes wrong then it will produce a reason why the promise was rejected.

After the promise is resolved or rejected, the following handler methods which take callbacks as arguments are called:

- `then`: When the promise is resolved the callback argument of this function will be called.
- `catch`: When the promise is rejected the callback argument of this function will be called.

## Explain the difference between Promise and Callback functions.

Both promises and callbacks are used to handle async operations in JavaScript.

Callbacks are functions passed as arguments to other functions, and they are executed after the completion of a certain asynchronous operation or event. Commonly used in event handling, AJAX requests, and other asynchronous tasks.

On the other hand, Promises are JavaScript objects that return a value sometime in the future. A promise after being resolved/rejected, will use `then` and `catch` handlers which take callbacks as arguments. They provide a cleaner way to handle async code compared to callbacks.

## How can you create a Promise that resolves after a specific time delay?

You can use the setTimeout function to create a delay and resolve the Promise after that delay. For example:

```js
const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
```

## What is the purpose of the Promise.all method?

Promise.all takes an array of promises as input and returns a single Promise that fulfills with an array of the fulfilled values when all of the input promises have been fulfilled. If any of the input promises is rejected, the whole Promise.all is rejected.

## Why forEach loops are not good for async operations?

`forEach` is not designed for asynchronous code. Consider this code:

```js
const players = await this.getWinners();

await players.forEach(async (player) => {
  await givePrizeToPlayer(player);
});
```

The promises returned by the iterator function are not handled. So if one of them throws an error, the error won't be caught. Also, forEach does not wait for each promise to resolve, all the prizes are awarded in parallel, not serial.

## Read More Sources

- [Promises vs Callbacks](https://stackoverflow.com/questions/22539815/arent-promises-just-callbacks#:~:text=Promises%20are%20not%20callbacks.,do%2C%20you%20get%20little%20benefit.)
- [Callbacks in JavaScript](https://stackoverflow.com/questions/9596276/how-to-explain-callbacks-in-plain-english-how-are-they-different-from-calling-o)
- [Async Loops](https://www.theanshuman.dev/articles/asynchronous-loops-in-javascript-using-foreach-vs-map-vs-for-loop-5020)
- [For loop vs ForEach](https://medium.com/@moucodes/exploring-the-difference-between-using-async-await-in-a-for-loop-and-foreach-739c9ebeb64a)
