

# Synchronous & Asynchronous

- Synchronous code-> the code that excutes line by line
- Asynchronous code -> code excutes parlelly

## Synchronous code

```js
console.log("Before");
function fn(){
    console.log("I'm in function.")
}
fn();
console.log("After");
```
output:
```js
Before
I'm in function.
After
```

## Asynchronous code 

```js
let timeFuture = Date.now() + 5000;
while(Date.now() < timeFuture){}

console.log("Before");
const cb2 = () => {
    console.log("set timeout 1");
    let timeInfuture = Date.now() + 5000;

    console.log("Before while loop: ", Date.now());
    while (Date.now() < timeInfuture) {
    }
    console.log("After while loop: ", Date.now());

}

const cb1 = () => {
    console.log("hello");
    console.log("After cb2: ", Date.now());
}
setTimeout(cb2, 1000)

setTimeout(cb1, 2000)

console.log("After");
```

Output:
```js
Before
I'm in function.
After
Before
After
set timeout 1
Before while loop:  1721759720147
After while loop:  1721759725147
hello
After cb2:  1721759725148
```

# Task

- Serial Tasks -> Dependedend Work
- Parallel Tasks -> task that are independent

## Read or write

- synchronous function
    ```js
    console.log("Before");
    const buffer = fs.readFileSync("./f1.txt");
    console.log("" + buffer);
    console.log("After");
    ```
    Output:
    ```js
    Before
    <---f1--->
    After
    ```
- Asynchrouns function
    ```js
    console.log("Before");
    fs.readFile("./f1.txt", function (err, data) {
        console.log("" + data);
    });
    console.log("After");
    ```
    Output:
    ```js
    Before
    After
    <---f1--->
    ```
- Reading 2 files

    ```js
    console.log("Before");
    const content1 = fs.readFileSync("./f1.txt");
    const content2 = fs.readFileSync("./f2.txt");
    console.log("concated result: " + content1 + " & " + content2);
    console.log("After");
    ```
    Output:
    ```js
    Before
    concated result: <---f1---> & <--- f2 values--->
    After
    ```
- Not Blocking the main thread and reading 2 files
    ```js
    console.log("Before");

    fs.readFile("./f1.txt", function (err, data) {
        console.log("" + data);
    });

    fs.readFile("./f2.txt", function (err, data) {
        console.log("" + data);
    });

    console.log("After");
    ```
    Output:
    ```js
    Before
    After
    <---f1--->
    <--- f2 values--->
    ```
- Not block the main thread and value in smae line( or some operation with the 2 file like compair or concat etc... )

    ```js
    console.log("Before");

    fs.readFile("./f1.txt", f1cb);

    function f1cb(err, content_1) {
        fs.readFile("./f2.txt", f2cb);
        
        function f2cb(err, content_2) {
            console.log("concated result: " + content_1 + " & " + content_2)
        }
    }

    console.log("After")
    ```
    Output:
    ```js
    Before
    After
    concated result: <---f1---> & <--- f2 values--->
    ```
