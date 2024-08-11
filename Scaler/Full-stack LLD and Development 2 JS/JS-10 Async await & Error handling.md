# Async and Await

await only works inside a  function with async keyword

```js
const promise = fs.promises.readFile("./f1.txt");

async function fn() {
    try {
        const data = await promise;
        console.log("Data inside the file is " + data);
    } catch (err) {
        console.log("err: ", err);
    }
}

fn();
```

## Serial 

```js
(async function fn() {
    try {
        let data = await fs.promises.readFile("./f1.txt");
        console.log("content: " + data);

        let data1 = await fs.promises.readFile("./f2.txt");
        console.log("content: " + data1);

        let data2 = await fs.promises.readFile("./f3.txt");
        console.log("content: " + data2);

        return "rval from fn";
    } catch (err) {
        console.log("Error: " + err);
    }

})();
```

async keyword fn always return a pending promise so either await for or use then 

```js
async function fn() {
    try {
        let data = await fs.promises.readFile("./f1.txt");
        console.log("content: " + data);

        let data1 = await fs.promises.readFile("./f2.txt");
        console.log("content: " + data1);

        let data2 = await fs.promises.readFile("./f1.txt");
        console.log("content: " + data2);

        return "rval from fn";
    } catch (err) {
        console.log("Error: " + err);
    }
}

let rVal = fn();

rVal.then(function (data) {
    console.log("content: " + data);
})
```

### Question 

```js
function resolveAfterNSeconds(delay, x) {
    return new Promise(resolve => {
        setTimeout(() => {
            console.log("Value: " + x);
            resolve(x);
        }, delay);
    });
}

(function () {
    let a = resolveAfterNSeconds(1000, "data 1")
    a.then(async function (x) {
        // 1 Sec

        // It work in serial order.
        let y = await resolveAfterNSeconds(2000, "data 2") // total: 1 + 2
        let z = await resolveAfterNSeconds(1000, "data 3") // total: 1 + 2 + 1
        // 4 sec


        // let p = await resolveAfterNSeconds(2000, "data 4");  // Total : 
        // let q = await resolveAfterNSeconds(1000, "data 5");  // Total : 

        // They work in parallel
        let p = resolveAfterNSeconds(2000, "data 4"); // Total: 4 + max(2,1); 6 sec
        let q = resolveAfterNSeconds(1000, "data 5");

        console.log(x + y + z + await p + await q);


        /**
         * litreal meaning of await -> waiting for some result 
         * if result is pdening -> wait
         * if your result -> use 
         * **/
    })
})()
```


# Error Handling 

try catch only works with run time Error
Runtime error -> only get to know after exceuting the code and it will only fail on the where we have made the error
Errors 
-> TypeError
-> reference Error
-> Range Error

## Type of Error 
### Syntax error
Syntax error code does not execute at all
```js
le a;
```
### Illlegal Shadowing
Illlegal Shadowing : Identifier 'a' has already been declare
```js
console.log("a");
let a =10
{
   var a=20;
 console.log("Hello",a );  
}
```

### Reference Error
1. TDZ -> temporal Dead Zone, ReferenceError: Cannot access 'a' before initialization
```js
console.log(a);
let a;
```

2. ReferenceError: fn is not defined

```js
fn();
```

3. ReferenceError: a is not defined (in strict mode if a variable is not defined)

```js
"use strict";
a=10
console.log(a);
```
when you accessing a prop for which object does not exist
```js
console.log(obj.a);
```

### Range error

1. RangeError: Maximum call stack size exceeded stackoverflow infinite recursion

```js
function foo() {
    foo();
}
foo();
```

2. array size RangeError: Invalid array length

```js
let a = [];

a.length = 2 ** 32 - 1; // Max allowed len is: 2^32 - 1

a.length = 100 ** 100  // 100 ^ 100, Throw error.
console.log(a.length);
```

### Type error

- whenever a method is called and it does not exist
- when you accessing a prop for which  object is undefined

1. TypeError: num.toUpperCase is not a function
```js
let num = 10;
console.log(num.toUpperCase());
```

2. TypeError: Cannot read properties of undefined (reading 'a')
```js
"use strict";
function fn(){
    console.log(this.rajneesh);
};
fn();
```


# Try and Catch

syntax error can't be solved by try and catch

```js
try {
    
} catch (err) {

}
```

- runtime Error

    ```js
    try {
        console.log(a);
        let a;
        console.log("Before");
    } catch (err) {
        console.log("Rajneesh_Error", err);
    }
    ```
- try and catch are synchronous

    `wrong` way to use try catch in synchronous
    ```js
    try {
        setTimeout(() => {
            console.log("set timeout is executed");
            console.log(a);
        }, 1000);
    } catch (err) {
        console.log("Rajneesh_message_of_error: ", err.message);
        console.log("Rajneesh_name_of_error: ", err.name);
    }
    ```

    `Rigth` way to use 

    ```js
    setTimeout(() => {
        try {
            console.log("set timeout is executed");
            console.log(a);
        } catch (err) {
            console.log(" message: ", err.message);
            console.log("name of error: ", err.name);
        }
    }, 1000);
    ```



