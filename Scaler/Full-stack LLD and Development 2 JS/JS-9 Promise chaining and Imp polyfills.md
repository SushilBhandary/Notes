
# Creating a Custom Promise 

```js
const PENDING = "pending";
const RESOLVED = "resolved";
const REJECTED = "rejected";

// function constructor 
function CustomPromise(executorFn) {
    // Add required properties and methods
    let State = PENDING;
    let Value = undefined;
    let scbArr = [];  // It can be a queue.
    let fcbArr = [];  // It can be a queue.

    // attach resolve.
    const resolve = (value) => {
        if (State !== PENDING) {
            return;
        }

        State = RESOLVED;
        Value = value;

        // call your all success from call back array.
        scbArr.forEach((cb) => {
            cb(value);
        });
    }

    // attach reject.
    const reject = (value) => {
        if (State !== PENDING) {
            return;
        }

        State = REJECTED;
        Value = value;

        // call your all success from call back array.
        fcbArr.forEach((cb) => {
            cb(value);
        });
    }

    // thread then with resolve.
    this.then = function (cb) {
        if (State === RESOLVED) {
            cb(Value);
        } else {
            scbArr.push(cb);
        }
    }

    // thread catch with reject.
    this.catch = function (cb) {
        if (State === REJECTED) {
            cb(Value);
        } else {
            fcbArr.push(cb);
        }
    }

    // Most Important: Don't forget to call your executor function. 
    executorFn(resolve, reject);
}
```

```js
const myPromise = new CustomPromise(executorFn);

const cb = (data) => {
    console.log(data);
}

myPromise.then(cb);

myPromise.then((data) => {
    console.log("I am the second then");
})

myPromise.catch((err) => {
    console.log("Error: ", err);
})

myPromise.catch((data) => {
    console.log("I am the second catch");
})
```

# Promise.all

```js
const promise = fs.promises.readFile("f1.txt");
const promise2 = fs.promises.readFile("f2.txt");

const combinedpromise_all = Promise.all([promise, promise2]);

combinedpromise_all.then(function (resultArr) {
    console.log("" + resultArr)
})
```
Output:
```js
<---f1--->,<--- f2 values--->
```

## Polyfill of Promise All

```js
Promise.myPromiseAll = function (arrOfPromises) {
    return new Promise(function (resolve, reject) {
        if (!Array.isArray(arrOfPromises)) {
            reject("Expected a array of Promise.")
            return;
        }

        let unresolvedPromise = arrOfPromises.length;
        const resolvedPromiseRes = [];

        if (unresolvedPromise === 0) {
            resolve(resolvedPromiseRes);
        }

        arrOfPromises.forEach(async (promise) => {
            try {
                const value = await promise;
                resolvedPromiseRes.push(value);

                unresolvedPromise--;
                if (unresolvedPromise === 0) {
                    resolve(resolvedPromiseRes);
                }
            } catch (err) {
                reject(err);
            }
        });
    });
}
```

# Promise.any

```js
const promise = fs.promises.readFile("f1.txt");
const promise2 = fs.promises.readFile("f2.txt");

const combinedpromise_any = Promise.any([promise, promise2]);

combinedpromise_any.then(function (resultArr) {
    console.log("" + resultArr)
})
```


# Resolve and Reject 

```js
const promise1 = Promise.resolve(100);
const promise2 = Promise.resolve(200);
const promise3 = Promise.reject(300);
const promise4 = Promise.reject(400);
```

## Example 1

```js
const promiseAll = async () => {
    console.log("1: ");
    const group1 = await Promise.all([promise1, promise2]);
    console.log("2: ", group1);
    const group2 = await Promise.all([promise3, promise4])

    console.log("3: ", group2);
    return [group1, group2]
}

promiseAll().then(console.log).catch(function (err) {
    console.log("Hit by error: ", err);
});
```
Output:
```js
1: 
2:  [ 100, 200 ]
Hit by error:  300
```

## Example 2

```js
const promiseAny = async () => {
    console.log("1: ");
    const group1 = await Promise.any([promise1, promise2]);
    console.log("2: ", group1);
    const group2 = await Promise.any([promise3, promise4])

    console.log("3: ", group2);
    return [group1, group2]
}

promiseAny().then(console.log).catch(function (err) {
    console.log("Hit by error: ", err);
});
```

# Promise.race

```js
const firstPromise = new Promise((res, rej) => {
    setTimeout(res, 2000, 'one');
});
const secondPromise = new Promise((res, rej) => {
    // setTimeout(res, 1000, 'two'); 
    setTimeout((arg) => {
        res(arg)
    }, 1000, "two")
});


// from wherever you get the answer
Promise.race([firstPromise, secondPromise]).then(res => console.log(res));
```
# Promise Chaining 

- chain -> then-> promise above is resolved
- catch -> promise of the above is rejected / throw an error
if you have mixture and either then returns a value / catch return -> then will executed with the recieved value
- finally -> finally works in both resolve or reject but  -> when you reject inside a finally then you upcoming catch will be called 
- finally -> doesnot take any input / if you retrun either error/ rejected promise -> you need a catch to consume

```js
Promise.resolve(1)
    .finally((data) => {
        console.log("32: ", data)
        return Promise.resolve('error');
    })
    .catch((error) => {
        console.log("36: ", error)
        throw 'error2'
    })
    .finally((data) => {
        console.log("40: ", data)
        let rProm = Promise.resolve(41)
        let thenProm = rProm.then(console.log)
        return thenProm;
    })
    .then(console.log)
    .catch(console.log);
```


# Question 
1. let take this as Starting code .
    ```js
    let p = new Promise(function (resolve, reject) {
        setTimeout(function () {  // this line is going tobe ignored after 2s
            reject(new Error("some value v1"));
        }, 2000);

        resolve("some error e1");

        setTimeout(function () {  // this line is going tobe ignored after 2s
            reject(new Error("some value v2"));
        }, 2000);

        resolve("some error e2");   // this line is going tobe ignored

        setTimeout(function () {  // this line is going tobe ignored after 2s
            reject(new Error("some value v3"));
        }, 2000);
    });
    ```
    if 

    ```js
    p.then(null, function (err) {
        console.log(18);
        console.log(err);
    });

    p.catch(function (err) {
        console.log(23);
        console.log(err);
    });

    p.finally(function () {
        console.log("28: ", 1);
    })
    ```
    Output:
    ```js
    28:  1
    ```
    if 
    ```js
    p.then(
        function (val) {
            console.log("34: ", val);
        },
        function (err) {
            console.log(err);
        }
    );
    ```
    Output:
    ```js
    34:  some error e1
    ```
    if 
    ```js
    p.finally(function () {
        console.log("finally: ", 2);
        // throw new Error("Hello");
        return Promise.resolve("some Error");
    }).finally(function () {
        console.log("finally: ", 3);
        return fs.promises.readFile("f1.txt");
    }).then(function (val) {
        console.log("then with finally:", val);
    }).catch(function (err) {
        console.log("catch with finally: ", err);
    })
    ```
    Output:
    ```js
    finally:  2
    finally:  3
    then with finally: some error e1
    ```
2. 

    ```js
    Promise.reject(1).
        catch((err) => {
            console.log("3: ", err);
            return 10;
        }).then((data) => {
            console.log("6: ", data)
        }).catch(console.log);
    ```

    output:

    ```js
    3:  1
    6:  10
    ```

3. 

    ```js
    Promise.reject(1)
        .finally((data) => {
            console.log("3", data)
            return Promise.reject('error')
        })
        .catch(console.log)
        .then((data) => {
            console.log("19: ", data)
        })
        .catch(console.log);
    ```
    Output:
    ```js
    3 undefined
    error
    19:  undefined
    ```