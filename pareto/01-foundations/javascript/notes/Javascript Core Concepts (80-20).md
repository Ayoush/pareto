### **Core Concepts**

These are the foundation of JavaScript and essential for any project.

#### **a. Variables and Scope (15 mins)**

- `var`, `let`, `const`: Differences and when to use each.
	- `var` is globally scoped when declared outside the function. 
	- It is initialized with `undefined` and function-scoped if declared inside the function.
	- `var` variables can be re-declared and updated, and this is where the problem comes
	- **Hositing is a JavaScript mechanism where variables declaration move to the top of there scope**
	- The problem with `var` is if you have a large codebase you unknowingly might re-declare the variable and it will retain that value and other parts of the code might break because of this action.
		```javascript
		var greeting = "Hello";
		console.log(greeting); // Output: Hello
		var greeting = "Hi"; // Re-declared without error
		console.log(greeting); // Output: Hi
		// The issue arises in loops or block scopes
		for (var i = 0; i < 3; i++) {
		  setTimeout(() => console.log(i), 1000); // Output: 3, 3, 3
		}
		// 'i' is not block-scoped and retains the final value after the loop.
		```
	- `let` is introduced to resolve this issue. 
	- `let` is also block scoped(anything under `{}`) but it can't be re-declared, it can only be updated and if you try to refer the variable even before declaration it will give you reference error but in case of var it will give you `undefined`
	- So in case of `let` also declaration are hoisted to the top but in case of `let` its not initialized.
	- `const` is simply to remove the updation functionality of `let`. It works similar to `let` but it restricts update.

- Block, function, and global scope.
	- Scope is noting but a environment where variables are declared and can be accessed
	- Global scope is like putting billboard in a small town which can be seen and accessible to everyone
	- Since they are available they make your code less modular and organised.
	- Local scope is like a private room in a building. When you declare a code inside local scope like block, function or conditional statement, its scope is limited to that room only.
	- Local variables are like furnitures inside a private room which need permission to access
	- `Shadowing` is a concept when you declare a variable with same name but inside different scope.
	 ```javascript
	 var message = "global message";
	 function showMessage() {
		 var message = "local message";
		 console.log(message); // local message
	 }
	 showMessage(); // local message
	 console.log(message); // global message
	 ```
	- Benefits of using local scope is isolation, modularity and reusability.
	- Block scope is like series of nested block within large container each with its own set of variables and majorly comes into picture for condition statements and loops.

- Hoisting: How it affects variables and functions.
	- Hoisting is a concept in javascript where the declaration of a function, variable or class goes to the top of the scope they are defined in.
	```javascript
		printHello()
		// hello

		function printHello() {
		  console.log("hello")
		}
    ```
	- Here is one more interesting example
	```javascript	
		printHello()
		printDillion() // ReferenceError: printDillion is not defined
		function printHello() {
		  console.log('hello')
		  function printDillion() {
		    console.log("dillion")
		  }
		}
		
	 // does this mean printDillion is not hosted, It did but inside the function scope. To resolve this :-
		function printHello() {
		  printDillion()
		  console.log('hello')
		  function printDillion() {
		    console.log("dillion")
		  }
		}
    ```
    - Different variable type are hoisted differently as discussed, `var` gives undefined, `let` gives reference error. It does get hoisted but not initialised. Compiler knows about the variable but not the value. So these are only accessible after the line they are initialised and this is what we call **`Temporal Dead Zone`**
	- Same is the case with `const`


#### **b. Functions (15 mins)**

- Function declarations vs. expressions.
	- Somewhat similar but have some notable difference.
	- For function declaration you use `function` keyword and specify a name for the function.
	- Function expression is when you assign a function to a variable and call that variable.
	- If we use a expression and doesn't assign any variable to it, then we would get error.
	- Difference is when you declare a function you can use it even before initialisation because of hoisting but in case of function expression depending on which variable type you assigned it you will get different error.
	 ```javascript
		const result = sum(20, 50)
		const sum = function(num1, num2) {
		  return num1 + num2
		}
		//different code
		const result = sum(20, 50)
		var sum = function(num1, num2) {
		  return num1 + num2
		}
		// Reference error in first
		// sum is not a function since hoisted value will be undefined which is          not a function
     ```

- Arrow functions: Syntax and `this` behavior.
	- `this` always refers to some object. What this object refers to will vary based on what and where this keyword is being called.
	- `this` is a keyword, not a variable so it can't be changed or reassigned.
	- If we call `this` by itself meaning not within function, object or whatever then it refers to global object.
	- If we call `this` from object then it refers to object itself.
	- In case of regular function it will again refer to `global Window`.
	- Inside a Constructor function or Class, `this` refers to the object.
	- In case of EventListners, `this` refers to the element that receives the event.
	```js
		console.log(this); // Browser: window, Node.js: {}
		
		function regularFunction() { 
			console.log(this); 
		} 
		regularFunction(); // Non-strict: window (browser), Strict: undefined
		
		const obj = { 
			name: "JavaScript", 
			greet() { 
				console.log(this.name); 
				}, 
			}; 
		obj.greet(); // "JavaScript"
		
		function Person(name) { 
			this.name = name; 
		} 
		const person = new Person("John"); console.log(person.name); // "John"
		
		const button = document.querySelector("button");
		button.addEventListener("click", function () { console.log(this); // The button element });
		
		setTimeout(function () { console.log(this); // Global object (window in browsers) }, 1000);
    ```
	- `Arrow Function` is nothing but short format of normal function.
	- The main difference comes on how the `this` keyword when associated with `Arrow function` behaves.
	- Arrow functions do not have their own `this`. Instead, they inherit `this` from the **surrounding lexical scope**.
	- Arrow functions cannot be used with the `new` keyword.
	- In a regular function, `this` is dynamically determined based on **how the function is called**.
	- In an arrow function, `this` is lexically bound. It inherits `this` from the surrounding scope where it is defined, not where it is called.
	```js
		const obj = {
		  name: "JavaScript",
		  greet: () => {
			console.log(this.name); // Arrow functions inherit from outer scope
		  },
		};
		obj.greet(); // undefined (Inherits `this` from global scope)

		button.addEventListener("click", () => {
			console.log(this); // Depends on outer scope (likely `window`) 
		});

		setTimeout(() => { 
			console.log(this); // Lexical scope 
		}, 1000);		
	```

- Closures: How they work and practical use cases.
	- `Closures` are functions that remembers the environment in which it was created.
	-  Before understanding closures we have to have some basic understanding of [[Execution Context]]
	- In javascript, functions are executed in context of the scope in which they were defined.
	- When function is created, it captures the environment at the time of definition and even if the outer function ended inner function retains the access as javascript keep them as long as they are referenced.
	```js
	function outerFunction(outerVariable) {
		return function innerFunction(innerVariable) {
	    console.log(`Outer: ${outerVariable}, Inner: ${innerVariable}`);
	  };
	}

	const closureFunc = outerFunction("outside");
	closureFunc("inside");
	// Output: Outer: outside, Inner: inside
	```

#### **c. Event Loop and Asynchronous JavaScript (30 mins)**
- Understand the **call stack**, **event loop**, and **task queue**.
	- `call stack` is a data structure which operates on LIFO. It manages function calling
	```js
	function firstFunction(){
	  throw new Error('Stack Trace Error');
	}

	function secondFunction(){
	  firstFunction();
	}

	function thirdFunction(){
	  secondFunction();
	}

	thirdFunction();
	```
	 ![[callstackoutput.png]]
	 - Its single threaded and synchronous.
	 - A function invocation creates a stack frame that occupies a temporary memory.
	 - For `event loop`, `task queue` and `microtask queue` refer to this [[Event Loop and Task Queues]]
- Promises: `then`, `catch`, `finally`.
	- Its a big topic hence explaining it in a separate [[Promises and then, catch, finally]]
- `async/await`: Syntax and handling errors.
- Example: Write a simple `async/await` function to fetch data using `fetch`.
