# Stream

https://github.com/substack/stream-handbook

Node.js Stream is great for processing data (read/write/transform) e.g. transform output from database to CSV, Stream is the basis for scalable I/O 

Stream inherits from `EventEmitter`. Using Node’s stream API allows you to create an object that receives events about the connection: `data` for when new data comes in, `end` when there’s no more data, and `error` when errors occur.


p11 example code for stream - counting specific words on a given webpage

/* 5 Streams
- read https://github.com/substack/stream-handbook
- Streams are partially buffered & eventEmitter, return smaller parts of the data (using a Buffer), and trigger a callback when new data is available for processing
- fully buffered data access: readFileSync(), readFile()
allocate buffer memory same as file size), bad if large file e.g. 1GB file,
- partially buffered(stream) access: readSync(), read()
1 allocate small buffer -> 2 read and return small buffer -> repeat 1&2 untill done

- Stream method
 fs.createReadStream(), http.ServerRequest(), child.stdout();
- Stream events
 "data", "end", "error"
*/
