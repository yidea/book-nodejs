# EventEmitter

EventEmitter - Node.js build-in class for exteand object eventing ability

```js
var EventEmitter = require("events").EventEmitter;
var util = require("util");
// create the class
var MyClass = function () {};
// augment the prototype using util.inherits
util.inherits(MyClass, EventEmitter);
MyClass.prototype.whatever = function() {
  this.emit("eventA", "test");
};
var obj = new MyClass();
obj.on("eventA", function () {
  console.log("eventA");
});
```
  
### Event loop (Single thread)
How event loop works https://www.youtube.com/watch?v=8aGhZQkoFbQ
- an entity that handles and processes external events and converts them into callback invocations- allow Async, pub/sub system, Node.js extend dom event to server I/O
- cb is register to event queue, cb is executed when I/O is done
- event loop = event emitter + callback
- event loop is only used to determine what do next when the execution of your code finishes (not good for CPU-bound task)

```js
setTimeout(function () { //async
  console.log(new Date().getTime());
});
```  

### Callback & Promise
- At an I/O call, your code saves the callback and returns control to the runtime environment. The callback will be called later when the data actually is available.
- Callback hell: nested callback depends on each other
- Callback hell Solution:
code in a small modular way
series, parallel http://book.mixu.net/node/ch7.html
event emitter
generator (thunk - a function that returns a callback)
promise: use Q, rsvp
http://spion.github.io/posts/why-i-am-switching-to-promises.html
