******
Javascript runs on Web-browsers which comes with there own api's and XMLHttpRequests. Since **javascript is a single threaded synchronous language** but features like `event loop`, `task queues` made it handle `asynchronous tasks` also.
![[eventloop.png]]

**Heap**: It’s mostly the place where objects are allocated.

**Callback Queue**: It’s a data structure that stores all the callbacks. Since it’s a queue, the elements are processed based on FIFO which is First in First Out.

**Event Loop**: This is where all these things come together. The event loop simply checks the call stack, and if it is empty (which means there are no functions in the stack) it takes the oldest callback from the callback queue and pushes it into the call stack which eventually executes the callback.

To visualise how `event loop` , `web api` and `call stack` are work in sync go through this [gist](https://gist.github.com/jesstelford/9a35d20a2aa044df8bf241e00d7bc2d0)
