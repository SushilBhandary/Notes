Nodejs
Why use nodejs for web serve
Node.js is a popular choice for web servers, especially in scenarios involving heavy I/O operations and small server requirements. Here's why Node.js is a suitable option for such use cases:

Non-Blocking I/O Model: a. Node.js is designed around a non-blocking, event-driven architecture. This means it can efficiently handle multiple I/O operations concurrently without blocking the execution of other tasks. b. When performing heavy I/O operations, such as reading and writing files, making network requests, or interacting with databases, Node.js can initiate these operations and continue executing other code while waiting for the I/O operations to complete. This asynchronous approach is highly advantageous for scenarios with many concurrent I/O tasks.

Scalability: a. In situations involving heavy I/O, it's common for multiple clients to make simultaneous requests to the server. Node.js's non-blocking model allows it to handle a large number of concurrent connections efficiently, making it a suitable choice for scalable applications. It can process incoming requests as soon as they arrive, rather than waiting for each request to complete before moving on to the next one.

Low Overhead: a. Node.js has a relatively small memory footprint compared to some other web server technologies. This makes it well-suited for small server applications where resource utilization needs to be efficient. You can run multiple Node.js instances on a single server without consuming excessive system resources

Rich Ecosystem:

Ports
a. What is a Port Number?

b. In the context of computer networking, a port number is a way to identify a specific process or service within a device that uses the Internet Protocol (IP).

c. Why the concept of ports was invented if it is not a physical thing?

d. Think of ports as a mechanism for multitasking. While an IP address identifies a machine, many different applications and services run on that machine at once. Each of these services needs a way to receive the appropriate data packets without confusion. Ports allow a single machine with one IP address to efficiently manage multiple services simultaneously. By assigning different services to different ports, a computer can easily determine

which application should handle incoming data. 2. Valid Range: Port numbers can range from 0 to 65535. Well-Known Ports: 0-1023, reserved for system and standard services. 4. Registered Ports: 1024-49151, typically used for user applications. 5. Dynamic/Private Ports: 49152-65535, used for dynamic purposes.

Nodemon
npm intsall -g nodemon npm install --save-dev nodemon

# FS module

fs module is used tp read write in files.

we can import `fs` as 


```
const fs = require("fs")
```

or 

```
import * as fs from 'node:fs'
```
## Function 

### `readFile`

To Read the data in the file

```
fs.readFile('file.txt', 'utf-8', (err, data) => {
    if (err) {
        console.log("Error: ", err);
    } else {
        console.log("Data in file:", data);
    }
}); 
```
OUTPUT: 
```
Data in file: ABC
```

### `writeFile`

To write the data in the file

```
const content = "Hello, World!";
fs.writeFile('example.txt', content, "utf-8", ((err)=>{
    if(err){
        console.log("Error: ", err);
    }else{
        console.log("File is created");
    }
}));
```
OUTPUT: 
```
File is created
```

### `rename`

To rename the file

```
fs.rename("example.txt", "example2.txt", (err)=>{
    if(err){
        console.log("Error: ", err);
    }else{
        console.log("File renamed");
    }
});
```
OUTPUT: 
```
File renamed
```

### `unlink`

To delete the file

```
fs.unlink("example2.txt", (err)=>{
    if(err){
        console.log("Error: ", err);
    }else{
        console.log("File deleted");
    }
});
```
OUTPUT: 
```
File deleted
```

### `stat`

To find the details of file or folder

```
fs.stat("dir", (err, stats)=>{
    if(err){
        console.log("Error: ", err);
    }else{
        console.log("File size: ", stats.size);
        console.log("Is Directory: ", stats.isDirectory());
        console.log("stats -->", stats);
    }
});
```
OUTPUT: 
```
File size:  0
Is Directory:  true
stats --> Stats {
  dev: 2488716441,
  mode: 16822,
  nlink: 1,
  uid: 0,
  gid: 0,
  rdev: 0,
  blksize: 4096,
  ino: 1125899910160966,
  size: 0,
  blocks: 0,
  atimeMs: 1719325642503.6504,
  mtimeMs: 1719325642500.6526,
  ctimeMs: 1719325642500.6526,
  birthtimeMs: 1719325642500.6526,
  atime: 2024-06-25T14:27:22.504Z,
  mtime: 2024-06-25T14:27:22.501Z,
  ctime: 2024-06-25T14:27:22.501Z,
  birthtime: 2024-06-25T14:27:22.501Z
}
```
### `mkdir`

To create a new directory(folder)

```
const directoryName = "my-directory";
fs.mkdir(directoryName, (err)=>{
    if(err){
        console.log("Error: ", err);
    }else{
        console.log("Directory is created");
    }
});
```
OUTPUT: 
```
Directory is created
```

### `rmdir`

To delete a new directory(folder)

```
const directoryName = "my-directory";
fs.rmdir(directoryName, (err)=>{
    if(err){
        console.log("Error: ", err);
    }else{
        console.log("Directory is deleted");
    }
}); 
```
OUTPUT: 
```
Directory is deleted
```


### `existsSync`

To check if the parth is present are not .

```
const directiryPath = "./dir2";
if(fs.existsSync(directiryPath)){
    console.log("Directory Exist");
}else{
    console.log("The directory does not exist!");
}
```
OUTPUT: 
```
The directory does not exist!
```

## TO Read from one file and write on the other file with using pipe

```
const sourceFilePath = "./dir/file1.txt";
const destinationFilePath = "./file2.txt";

// Create a redable stream.
const readStream = fs.createReadStream(sourceFilePath);


// Create a writable stream.
const writeStream = fs.createWriteStream(destinationFilePath);

// pipe the stream data flow.
readStream.pipe(writeStream);

readStream.on("error", (err) => {
    console.log("Error while reading the file: ", err.message);
});

readStream.on("end", () => {
    console.log("file reading and writing is completed.");
})

writeStream.on("error", (err) => {
    console.log("Error while writing in the file: ", err.message);
});
```

OUTPUT:
```
file reading and writing is completed.
```




# PATH Module

Path module is used to ?....

we can import `path` as 


```
const path = require("path");
```
## Function 

### `join`

To build a path 

```
const fullPath = path.join("folder", "subfolder", "file.txt");
console.log("full path of file is: ", fullPath);
```
OUTPUT: 
```
full path of file is:  folder\subfolder\file.txt
```

### `resolve`

To build a absolute Path 

```
const absolutePath = path.resolve("folder", "subfolder", "file.txt");
console.log("Absolute Path: ", absolutePath);
```
OUTPUT: 
```
Absolute Path:  D:\dev\Exprement\Scaler\Node.js\folder\subfolder\file.txt
```

### `basename`

To find the base file name

```
const fileName = path.basename('./dir/dir/dir/dir/file.txt');
console.log("BaseName is: ", fileName);
```
OUTPUT: 
```
BaseName is:  file.txt
```

### `dirname`

To find the base folder(directory) name

```
const dirName = path.dirname('/path/to/file.txt');
console.log("Directory name: ", dirName);
```
OUTPUT: 
```
Directory name:  /path/to
```
### `parse`

To find the details of the path 

```
const pathInfo = path.parse('/path/to/file.txt');
console.log("Path Info: ", pathInfo);
```
OUTPUT: 
```
Path Info:  {
  root: '/',
  dir: '/path/to',
  base: 'file.txt',
  ext: '.txt',
  name: 'file'
}
```

### `normalize`

The path.normalize() method is used to normalize the given path. Normalization resolves the (.) and (..) segments of the path to their correct form. If the method encounters multiple path separators, it replaces all of them by a single platform-specific path separator. 

```
const normalizedPath = path.normalize("/path/to/../file.txt");
console.log(normalizedPath);
```
OUTPUT: 
```
\path\file.txt
```

### `normalize`

The path.normalize() method is used to normalize the given path. Normalization resolves the (.) and (..) segments of the path to their correct form. If the method encounters multiple path separators, it replaces all of them by a single platform-specific path separator. 

```
const normalizedPath = path.normalize("/path/to/../file.txt");
console.log(normalizedPath);
```
OUTPUT: 
```
\path\file.txt
```









