# Node.js Essential (Gitbook)

While coding and learning Node.js, always want to document what I learned and contribute back to community. Node.js Essential is published w Gitbook for free. 

### Node.js Intro
- built on V8 - Chrome’s JavaScript runtime for easily building fast, scalable app
- Key feature: 
  * Non-blocking async I/O (file/service)
  * NPM module system & Fullstack JS
  * Stream everything 
  * Single thread Event driven(callback)

Node.js server vs. Traditonal web server
![node.js vs. traditonal web server](http://i.stack.imgur.com/MirQF.png)  
  
### Node.js Pros vs. Cons
- __Pros__: 
  * Best for Large I/O, real-time, high performance need. Nonblocking async I/O data-intensive real-time app(Nasdaq, Chat, Websocket, Ads distribution, Game server), mobile mid layer(wrapping data source api, proxy), RESTful web api/scraping, socket.io, node-webkit. 
  * Event-driven programming style. 
  * Isomophic, NPM, FE javascript developer can quick grasp 

- __Cons__: single thread, callback hell w event loop, CPU bound (image encoding, e.g. calculate fibonacci(57) without memorize)

### Reference
- Book Node.js in Practice by Alex Young
- Mixu's Node book http://book.mixu.net/node/ good all-rounded intro
- http://javascriptissexy.com/learn-node-js-completely-and-with-confidence/?WPACFallback=1&WPACRandom=1417403555636
