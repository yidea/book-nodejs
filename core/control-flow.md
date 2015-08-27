# Control Flow & Async Callback

The nature of Async callback in Node.js, makes the node.js code hard to read - [Callback Hell](http://callbackhell.com/)

```js
//example of callback hell
doAsync1(function () {
  doAsync2(function () {
    doAsync3(function () {
      doAsync4(function () {
    })
  })
})
```  

### Callback (Node.js core API by default use callback)

```js
var updateUser = function(id, attributes, callback) {
  User.findOne(id, function (err, user) {
    if (err) return callback(err);
    user.set(attributes);
    user.save(function(err, updated) {
      if (err) return callback(err);
      console.log("Updated", updated);
      callback(null, updated);
    });
  });
});
```  

### Promise (ES6/Bluebird)
Promise make your async code style like sync `doAsync().then((data)=>{})`

- Read: 
  * bluebird API https://github.com/petkaantonov/bluebird/blob/master/API.md
  * bluebird Cheatsheet http://ricostacruz.com/cheatsheets/bluebird.html
  * http://www.2ality.com/2014/10/es6-promises-api.html
  * https://www.promisejs.org/
  * https://strongloop.com/strongblog/node-js-callback-hell-promises-generators/

```js
var Promise = require("bluebird");
var request = Promise.promisify(require("request"));
var fs = Promise.promisify(require("fs"));//since Node.js core module is using callback by default, need wrap w bluebird promise
fs.readFileAsync("file.json").then(JSON.parse)
  .then(function(val) {
    return val;
  })
  .catch(SyntaxError, function(e) {
    console.error("invalid json in file");
  });
  
//parallel operation (use .all for dynamic array of promise, use .join for known # of promise)
Promise.all([asynchronousOperation(), asynchronousOperation()])
  .then(function(vals) {
      vals.forEach(console.log); //[{}, {}]
      return vals;
  });  
```  

### Generator 
Generator is introduced in ES6  

```js
var doAsyncOp = Q.async(function* () { //* indicate it's generator
  var val = yield asynchronousOperation(); //yield async fn = promise.then
  console.log(val);
  return val;
});
```    

### ES7 async/await 

Promise helped us get out of callback hell somewhat, but still need callback for `promise.then(funciton(val){})`, async can help you remove callback and return further. 

When you define a function `async`, it indicates it returns a promise. If the promise resolves `await`, we can immediately interact with it on the next line, . And if it rejects, then an error is thrown. So try/catch actually works again

- async function //will return a promise obj
- await func() //just like .then() but without callback, Use await to wait for a Promise and its resolution value, equal to yield in generator, or promise.then(function(data){})
- try/catch to replace then/catch
- still can use ls().then/catch, essentially async fn is returning promise

```js
async function doAsyncOp () { //= new Promise((resolve)=>{resolve()}))
  try {
    let filenames = await fs.promise.readdir(__dirname); //.then
    let data = await asynchronousOperation(filenames); //.then
    console.log(data)
  } catch (err) { //handling reject w try catch instead of .catch(function(err){})
    console.err(err);
  }    
};

//parallel operation
async function doAsyncOp () {
    var vals = await Promise.all([asynchronousOperation(), asynchronousOperation()]);
    vals.forEach(console.log.bind(console));
    return vals;
};
```  

### Reference
- http://www.sitepoint.com/simplifying-asynchronous-coding-es7-async-functions/
