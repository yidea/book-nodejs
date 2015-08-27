# HTTP

## HTTP server obj
- is an EventEmitter, on, listen, close
- API http://nodejs.org/api/http.html

## http.ServerRequest obj
- ServerRequest are Readable Streams
- API request.method/url/headers/data(pass via POST)

## http.ServerResponse obj
- ServerResponse are Writable Streams
- API response.writeHead/end

```js
var http = require("http");
var server = http.createServer(function(req, res) {
  if (req.url === "parseGet" && req.method.toLowerCase() === "get") {
    /*## parse GET request (url module)*/
    var url = require("url");
// url.parse(urlStr, parseQueryString = false) return an object w url parts
    var urlParts = url.parse("http://user:pass@host.com:8080/category/?query=string#hash", true);
    console.log(urlParts); //host, port, hash, query, pathname
  }

  if (req.url === "parsePost" && req.method.toLowerCase() === "post") {
    /*## parse POST request (querystring module)
     - POST data is being encoded in 2 ways
     1 application/x-www-form-urlencoded (by default for form and textual data) encoded like GET request, e.g. name=john&other=abc%20ad
     2 multipart/form-data for binary files, e.g. upload img/files use npm formidable
     */
    var querystring = require("querystring");
    var data;
    req.on("data", function (chunk) { //stream
      data += chunk; //buffer
    });
    req.on("end", function () {
      var postData = querystring.parse(data); //decode url into data object, a=b&c=d -> {a:"b", c:"d"}
      // querystring.stringify() for convert object into encodedURL string
    });
  }

  /*## HTTP client for making HTTP request
    - http.get(options, callback)
  */
  if (req.url === "/" && req.method.toLowerCase() === "get") {
    var options = {
      host: "headers.jsontest.com",
      port: 80,
      path: "/"
    };
    var httpGet = http.get(options, function (response) {
      var data;
      response.on("data", function (chunk) {
        data += chunk;
      });
      response.on("end", function () {
        console.log(data);
      });
    })
  }

  //response.writeHead(statusCode, [reasonPhrase], [headers]).
  res.writeHead(200, {"content-type": "text/html"}); //MIME type
  res.write("<div>Test</div>"); // write to buffer
  //res.setHeader("Location", "/index.html"); //redirect URL
  res.end(); //finish buffer and close connection
});
server.listen(8080, "local");
```
