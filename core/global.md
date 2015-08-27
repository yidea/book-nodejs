# Node.js Global Objects

Global objects are available in all nodejs modules. http://nodejs.org/api/globals.html#globals_global 

- module - organize components in modular way e.g. npm module
- process - communicate with OS 
- console - logging message
- Buffer - support for binary data

### Module & Require
- `require("module")` and cached singleton
 Commonjs Modules are cached after first time required(), second call will return the same object (singleton).
 Rare cases you might want to deleted module cache by `delete require.cache[require.resolve('./myclass')]; //require.resolve() will return a fully expanded filename`  
 
 ```js
 var express = require("express"); //without ./, node will search module  in local./node_modules first, then global/node_modules, if find create cached singleton
 var foo = require("./lib/foo"); //relative path, can ignore .js extension
```

- module.exports
you can export object/function/class as module API

```js
funciton MyClass () {}
module.exports = MyClass; //new require("./module")
module.exports = {get: function() {return "ok";}}; //require("./module").get()
module.exports = function () { //require("./module")()
  var obj = {};
  return obj;
};
```

### Process

`process` object is an `EventEmitter` can help you get data into or out of a nodejs program, communicate your nodejs program with OS in a standard I/O stream manner

- process.cwd(), `__filename`, `__dirname` to get location of current file in OS

  ```js
  console.log("__filename", __filename); ///Users/ycao2/walmart/github/codepath-nodejs/index.js
  console.log("__dirname", __dirname); ///Users/ycao2/walmart/github/codepath-nodejs
  var template = path.join(__dirname, "views", "view.html"); //=dir/views/view.html, use path.join for 
  cross platform url support
  ```    

- Read/Wrtie w OS command-line

  __Standard streams__: process.stdin, process.stdout & process.stderr
  
  `process.stdout` & `process.stdin` are readable & writable stream to stdin/stdout 
  
  ```js
  //@ Example 1: 
  //Pipe command-line content as input for node program, then output to command-line
  //cat mock.txt| babel-node index.js
  process.stdin.resume(); //start reading stream & prvent node exit
  process.stdin.setEncoding("utf8");
  process.stdin.on("data", function (text) {
    process.stdout.write(text.toUpperCase()); //transform data chunck to uppercase output 
  });
  ```

- process.argv parse command-line argument

  Your nodejs CLI util need parse command-line arguments? use `process.argv` array or npm yargs package (transfor an object)
  
  ```js
  //babel-node args.js -r test.js
  console.log(process.argv); //=[ 'node','/Users/ycao2/walmart/github/codepath-nodejs/args.js','-r','test.js' ]
  console.log(require('yargs').argv); //={_: [], r: 'test.js', '$0': 'args.js'}
  ```

- Respond to Signal from background program 

  Make nodejs program repond to single from program that runs in OS background based on the nature that `process` is an `EventEmitter` object
  
  ```js
  process.stdin.resume();
  process.on('SIGHUP', function () { //listen to signal from other process
  console.log('Reloading configuration...');
  });
  console.log('PID:', process.pid);
  ```      
  
- progress.exit

  exit a program with exit code in async

  ```js
  process.exit(0); //exit correctly
  process.exit(1); //exit w error (any code not 0 means error)
  ```
  
- process.nextTick(cb)  
  
  process.nextTick(cb) is Node.js's way to delay callback to head of next cycle of run loop, same as `setTimout(fn, 0)`
  
  > Node’s documentation recommends that APIs should always be 100% asynchronous
  or synchronous. That means if you have a method that accepts a callback and may call it asynchronously, then you should wrap the synchronous case in process.nextTick
  so users can rely on the order of execution.
  
  ```js
  var EventEmitter = require('events').EventEmitter;
  events = new EventEmitter();
  process.nextTick(function() {
    events.emit('success');
  });  
  events.on("success", function(){});
  ```  
- prcess.env 

- process.arch & process.platform

  Get OS platform info   
 
### Console
The easiest way to log node.js info/error, also can use `console.time()` for benchmarking nodejs function. (More advanced benchmarking https://www.npmjs.com/package/benchmark)
```js
var name = "test";
console.log("hi %s", name); //formatting placeholder, %s - string, %d - Number, %j-JSON
```   

### Timer
- 4 Methods: setTimeout, setInterval, clearTimeout, clearInterval
- JS Timings are not necessarily accurate if you have a long-running cpu blocking task
- in timer method, context this -> global

```js
//setTimeout() & clearTimeout(id) - delay a function
var timeoutId = setTimeout(function () { // run later
  console.log("setTimeout");
  clearTimeout(timeoutId); //no need
}, 100);
//setInterval() & clearInterval(intervalId) - repeat function
var counter = 0;
var intervalId = setInterval(function () {
  console.log("setInterval");
  counter++;
  if (counter > 0) {
    clearInterval(intervalId);
  }
}, 100);
```    
