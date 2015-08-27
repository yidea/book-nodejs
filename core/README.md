# Node Fundamental

### How Node.js works

![nodejs core](https://dl.dropboxusercontent.com/u/14122618/imgs/nodejs_core.jpg)

```
Node.js Core lib API http://nodejs.org/api/ (e.g. module,stream,fs)
                  |
Node.js Binding (C++ glue js cmd to V8/libuv)
                  |
libuv(C binary lib for Async I/O File/Network) + V8 (C++ for js compiler)
```

Async File I/O example 
   
```js
var fs = require("fs");
// fs.readFile -> Binding for I/O related -> libuv -> create a thread w callback in Event Queue(async read file to a buffer, once done it exe the callback)
// callback func runs via v8 + buffer as context
fs.readFile(".gitignore", function (err, data) {
  if (err) throw err;
  console.log(data);
});
```  

### Node.js Core modules
- Global objects

  process, console, timer
 
- I/O modules
 
 fs, net, http modules

- EventEmitter 
- an entity that handles and processes external events and converts them into callback invocations- allow Async, pub/sub system, Node.js extend dom event to server I/O
- cb is register to event queue, cb is executed when I/O is done
- event loop = event emitter + callback
- event loop is only used to determine what do next when the execution of your code finishes (not good for CPU-bound task)

How event loop works https://www.youtube.com/watch?v=8aGhZQkoFbQ
*/
setTimeout(function () { //async
  console.log(new Date().getTime());
});

/* 2 Callback
- At an I/O call, your code saves the callback and returns control to the runtime environment. The callback will be called later when the data actually is available.
- Callback hell: nested callback depends on each other
- Callback hell Solution:
code in a small modular way
series, parallel http://book.mixu.net/node/ch7.html
event emitter
generator (thunk - a function that returns a callback)
promise: use Q, rsvp
http://spion.github.io/posts/why-i-am-switching-to-promises.html

# 2.5 Promise
ES6 have Promise as control flow for async to solve callback hell by add .then



