# CallBack Hell

Callback hell is a phenomenon that happens when multiple callbacks are nested on top of each other. The two common ways of escaping the callback heare are by using promises and async/await.

Example :
```js
fs.readFile(".././f1.txt", (err, data) => {
    if (err) {
        console.log("Error is: " + err);
    } else {
        console.log("Content is: " + data);
        fs.readFile(".././f2.txt", (err, data) => {
            if (err) {
                console.log("Error is: " + err);
            } else {
                console.log("Content is: " + data);
                fs.readFile(".././f3.txt", (err, data) => {
                    if (err) {
                        console.log("Error is: " + err);
                    } else {
                        console.log("Content is: " + data);
                        fs.readFile(".././f4.txt", (err, data) => {
                            if (err) {
                                console.log("Error is: " + err);
                            } else {
                                console.log("Content is: " + data);
                            }
                        })
                    }
                })
            }
        })
    }
});
```

```js
const list = [".././f4.txt",".././f3.txt",".././f2.txt",".././f1.txt"];

function recursiveWay(list){
    if(list.length === 0){
        return;
    }

    fs.readFile(list.pop(), smallCbFunction);
    function smallCbFunction(err, data){
        if (err) {
            console.log("Error is: " + err);
        } else {
            console.log("Content is: " + data);
            recursiveWay(list);
        }
    }
}
```
# Promise 

In JavaScript, a Promise is an object that will produce a single value some time in the future.
It has 3 status
- Resolved
- Pending
- Rejected

Promise based function do not take a call back function

Example with `readFile`
```js
const promise = fs.promises.readFile("./f1.txt");
```
The task has started when fn is called

```js
promise.then((data) => {
    console.log("My content is: " + data);
});
```
`then` is  an event listener over promise.
Promise gives us a method named `then` which is an event listener over promise and it triggers when the promise is resolved(the task is done).

```js
promise.catch((err) => {
    console.log("We hit by error: " + err);
});
```
`catch` is also an event listener over promise and it triggers when the promise is rejected(you get an error).


---
### chaining your then and catch

```js

fs.promises.readFile("./f1.txt")
    .then(function (futureValue) {
        console.log("Data inside the file is " + futureValue);
    })
    .catch(function (err) {
        console.log("err: ", err);
    });
```

---
when your call back code lloks like this 

```js
fs.readFile(".././f1.txt", (err, data) => {
    if (err) {
        console.log("Error is: " + err);
    } else {
        console.log("Content is: " + data);
        fs.readFile(".././f2.txt", (err, data) => {
            if (err) {
                console.log("Error is: " + err);
            } else {
                console.log("Content is: " + data);
                fs.readFile(".././f3.txt", (err, data) => {
                    if (err) {
                        console.log("Error is: " + err);
                    } else {
                        console.log("Content is: " + data);
                        fs.readFile(".././f4.txt", (err, data) => {
                            if (err) {
                                console.log("Error is: " + err);
                            } else {
                                console.log("Content is: " + data);
                            }
                        })
                    }
                })
            }
        })
    }
});
```
let slove it with `Nested then`


```js
fs.promises.readFile("./f1.txt")
    .then(function (data1) {
        console.log("My Content is: " + data1);
        fs.promises.readFile("./f2.txt")
            .then(function (data1) {
                console.log("My Content is: " + data1);
                fs.promises.readFile("./f3.txt")
                    .then(function (data1) {
                        console.log("My Content is: " + data1);
                        fs.promises.readFile("./f4.txt")
                            .then(function (data1) {
                                console.log("My Content is: " + data1);

                            })
                            .catch(function (err) {
                                console.log("ohh! I hit by error: " + err);
                            })
                    })
                    .catch(function (err) {
                        console.log("ohh! I hit by error: " + err);
                    })
            })
            .catch(function (err) {
                console.log("ohh! I hit by error: " + err);
            })
    })
    .catch(function (err) {
        console.log("ohh! I hit by error: " + err);
    });
```

now , with `chaning with then`

```js
fs.promises.readFile("./f1.txt")
    .then(function (data) {
        console.log("My Content is: " + data);
        return fs.promises.readFile("./f2.txt");
    }).then(function (data) {
        console.log("My Content is: " + data);
        return fs.promises.readFile("./f3.txt");
    }).then(function (data) {
        console.log("My Content is: " + data);
        return fs.promises.readFile("./f4.txt");
    }).then(function (data) {
        console.log("My Content is: " + data);
    })
    .catch(function (err) {
        console.log("ohh! I hit by error: " + err);
    });
```

## Create Our own promise.ReadFile()

```js
function promiseReadFile(filePath) {
    return new Promise((resolve, reject) => {
        fs.readFile(filePath, (err, data) => {
            if (err) {
                reject(err);
            } else {
                resolve(data);
            }
        })
    });
}
```
```js
const promise = promiseReadFile("./f1.txt");
```
```js
promise.then((data) => {
    console.log("My content is: " + data);
}).catch((err) => {
    console.log("ohh! I hit by error: " + err);
})
```

```js
promiseReadFile("./f1.txt")
    .then(function (data) {
        console.log("My Content is: " + data);
        return promiseReadFile("./f2.txt");
    }).then(function (data) {
        console.log("My Content is: " + data);
        return promiseReadFile("./f3.txt");
    }).then(function (data) {
        console.log("My Content is: " + data);
        return promiseReadFile("./f4.txt");
    }).then(function (data) {
        console.log("My Content is: " + data);
    })
    .catch(function (err) {
        console.log("ohh! I hit by error: " + err);
    });
```












