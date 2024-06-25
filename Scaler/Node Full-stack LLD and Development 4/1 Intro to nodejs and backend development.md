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









