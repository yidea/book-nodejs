# FS - File System

https://nodejs.org/api/fs.html

Node's FS module can read/write file using async method, but provide sync method 

`fs` module allows:
- [POSIX - Portable Operating System Interface](http://pubs.opengroup.org/onlinepubs/9699919799/toc.htm) file I/O 
- File I/O streaming/watching/bulk 
- has both async/sync I/O method (different from net, http module), still need file sync method for require module cases (load config file) 

### POSIX file I/O methods

POSIX are low-level file API for most common file operations. Missing ones: glob

Most used 
- fs.readdir(path, callback); //read files/folder in a dir not recursively `callback(err,filenameArray)` 
- fs.stat(path, callback); //get stat of a file/folder, `callback(err, stat) {console.log(stat.isDirectory(), stat.isFile())}`
- fs.existsSync(path)

Table of Nodejs all POSIX API: https://nodejs.org/api/fs.html
```
rename(2) fs.rename Changes the name of a file
truncate(2) fs.truncate Truncates or extends a file to a specified length
ftruncate(2) fs.ftruncate Same as truncate but takes a file descriptor
chown(2) fs.chown Changes file owner and group
fchown(2) fs.fchown Same as chown but takes a file descriptor
lchown(2) fs.lchown Same as chown but doesn’t follow symbolic links
chmod(2) fs.chmod Changes file permissions
fchmod(2) fs.fchmod Same as chmod but takes a file descriptor
lchmod(2) fs.lchmod Same as chmod but doesn’t follow symbolic links
stat(2) fs.stat Gets file status
lstat(2) fs.lstat Same as stat but returns information about link if
provided rather than what the link points to
fstat(2) fs.fstat Same as stat but takes a file descriptor
link(2) fs.link Makes a hard file link
symlink(2) fs.symlink Makes a symbolic link to a file
readlink(2) fs.readlink Reads value of a symbolic link
realpath(2) fs.realpath Returns the canonicalized absolute pathname
unlink(2) fs.unlink Removes directory entry
rmdir(2) fs.rmdir Removes directory
mkdir(2) fs.mkdir Makes directory
readdir(2) fs.readdir Reads contents of a directory
close(2) fs.close Deletes a file descriptor
open(2) fs.open Opens or creates a file for reading or writing
utimes(2) fs.utimes Sets file access and modification times
futimes(2) fs.futimes Same as utimes but takes a file descriptor
fsync(2) fs.fsync Synchronizes file data with disk
write(2) fs.write Writes data to a file
read(2) fs.read Reads data from a file
```

### File Streaming API
File stream API: 
- `fs.createReadStream` 
-`fs.createWtriteStream` 
can connect to other streams with pipe
Good for when you want deal with a piece of data at a time and chain data source, for large file   
 
```js
//copy file A to B
copyFile("./package.json", "./test/copy.json");
function copyFile (filenameA, filenameB) {
  let readable = fs.createReadStream(filenameA);
  let writeable = fs.createWriteStream(filenameB);
  readable.pipe(writeable); //data pipe to writable stream, write piece by piece
  console.log("copied %s %s", filenameA, filenameB);
}
```
   
### File Bulk API (use Buffer)
File Bulk API (Async): 
- `fs.readFile` 
- `fs.writeFile`
- `fs.appendFile`
Good for when you want to load file(small file) into memory (using buffer) or write out data completely instead of piece (stream)
 
```js
let fs = require("fs");
let Promise = require("songbird");
fs.promise.readFile("./package.json", "utf8")
  .then(JSON.parse)
  .then((data) => {
    //console.log(data);
  })
  .catch((err) => { console.log(err);});
```

### File Watching API
File Watching API:
- `fs.watch`: efficient for internal file system, not work well w network drives
- `fs.watchFile`: less-efficient but work for network drives

### File Sync API
Good for load config file or module (require), better than async in this case
- readFileSync   
- readdirSync

```js
//read config.json on program startup w fs.readFileSync() you can change the config 
var config = JSON.parse(fs.readFileSync('./config.json').toString());
//when you are not going to change config use var confi = requrie("./config.json") - cached singleton
doThisThing(config);
```  

### Recursive File operation
