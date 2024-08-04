# Spread , Rest , Default

## Default 

```js
function fn(param1, param2, param3="default") {
    console.log("Hi params are ", param1, param2, param3);
}
fn("Hi", "Hello");
fn("Hi", "Hello", "Hola");
```

## Spread

spread operator : copies value,ref from on array to another for only first level

```js
let arr = [1, 2, [3, 4], 4, 5];
et arr2 = [...arr];
```

## Rest

rest -> It is used as paremeter of fn
use you to collect remianing parameters numbers of params 

```js
function fn(param1, ...param2) {
    console.log(" params are ", param1);
    console.log("Rest paramateres",param2);
}
fn("Hi", "Hello");
fn("Hi", "Hello", "Naga", "Meena", "Gaurav", "shravya", "Adarsh");
```

# Polyfill of call, apply, bind

Let assume that this code is present 

```js
let cap = {
    name: "Steve",
    team: "Cap",
    petersTeam: function (mem1, mem2) {
        console.log(`Hey ${this.name} I am your neighborhood's  
        spiderman and i belong to ${this.team}'s team with members  ${mem1} ${mem2}`);
    }
}

let ironMan = {
    name: "Tony",
    team: "Iron Man",
}
```
## call

polyfill of call method

```js
Function.prototype.myCall = function (requiredObject, ...args) {
    // get your function.
    const functionToBeInvoked = this;

    // copy your function.
    requiredObject.tempFunction = functionToBeInvoked;

    // call your function.
    requiredObject.tempFunction(...args);

    // delete temp method.
    delete requiredObject.tempFunction;
}

cap.petersTeam.myCall(ironMan,"thor", "loki");
```

## Apply

polyfill of apply method

```js
Function.prototype.myApply = function (requiredObject, arrayAsArgu) {
    // get your function.
    const functionToBeInvoked = this;

    // copy your function.
    requiredObject.tempFunction = functionToBeInvoked;

    // call your function.
    requiredObject.tempFunction(...arrayAsArgu);

    // delete temp method.
    delete requiredObject.tempFunction;
}

cap.petersTeam.myApply(ironMan, ["thor", "loki"]);
cap.petersTeam.apply(ironMan, ["thor", "loki"]);
```

## bind

polyfill of bind method

```js
Function.prototype.myBind = function (requiredObject) {
    // get your function.
    const functionToBeInvoked = this;

    return function(...args){
        functionToBeInvoked.call(requiredObject, ...args);
    }
}

const boundFn = cap.petersTeam.myBind(ironMan);
boundFn(["thor", "loki", "rajneesh", "arti", "jagdish"], "Hema");
```

# Call By Value

what a reference is -> address -> pointer

call by value -> call by sharing
primitive -> value
no primitive -> val of the reference

```js
function modifier(a, b) {
    console.log("13", a, b)
    a[0] = 10;
    b[1] = 20;
    console.log("16", a, b)
}
let p = [4, 7, 9]
let q = [3, 6, 8]

console.log("20", p, q);
modifier(p, q)
console.log("23", p, q);
```