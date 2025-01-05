********
### Introduction to the JavaScript execution context

So javascript is a `single-threaded` language. When javascript start executing a script it :-
- Create a `global object`  i.e. `window` in the web browser 
- Create a `this` object and bind it to the global object.
- Setup a memory heap for storing variables and functions.
- Store functions in memory heap and variables in global execution context with value `undefined`
```js
let x = 10;

function timesTen(a){
    return a * 10;
}

let y = timesTen(x);

console.log(y);
```
- First, store the variables `x` and `y` and function declaration `timesTen()` in the global execution context.
- Second, initialize the variables `x` and `y` to `undefined`.
- After creation it moves to execution phase.
![[Creation Phase.png]]
- During the execution phase, the JavaScript engine executes the code line by line, assigns the values to variables, and executes the function calls
![[execution phase.png]]
- For each function call, the JavaScript engine creates a new **function execution context**.
	![[function execution context.png]]
- The function execution context is similar to the global execution context. But instead of creating the global object, the JavaScript engine creates the `arguments` object that is a reference to all the parameters of the function
- To keep track of all the execution contexts, including the global execution context and function execution contexts, the JavaScript engine uses the call stack.