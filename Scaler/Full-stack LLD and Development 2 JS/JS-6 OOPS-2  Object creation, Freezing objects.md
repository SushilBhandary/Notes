# ways to create object in js

1. object literal
2. Object create 

## Object literal

Any properties and methods having Object as there prototype (parent)

- Arrays
    ```js
    console.log([1,2,3,4]);
    ```
    array -> Array(Object/class) -> Object
- Object
    ```js
    let obj = {
        name: "Steve",
        address: {
            state: "Newyork",
            city: "Manhatten"
        },
    };
    ```
    object -> Object 
- Function 
    ```js
    function Hi() {
        console.log(`${this.name} say's Hi `);
        return this;
    }
    ```
    function -> Function -> Object
- String 
    ```js
    console.log(new String("Hi My name is Rajneesh"));
    ```
    str -> String -> Object
- Boolean
    ```js
    console.log(new Boolean());
    ```
    boolean -> Boolean -> Object
- Number
    ```js
    console.log(new Number());
    ```
    num -> Number  -> Object
- null and undefined -> no parent
- char -> Character -> Object


whenever you want to access any method or property then that  primitive is typecased as a  children of there respective parent class and then that method is applied on that non primitive you are returned the answer. -> autoboxing

## Object create

### Create an object without any parent

```js
let obj = {
    name: "Steve",
    address: {
        state: "Newyork",
        city: "Manhatten"
    },
    sayHi: function () {
        console.log(`${this.name} say's Hi`);
        console.log("object this: ", this);
        return this;
    }
};

let obj3 = Object.create(obj);
console.log("obj3: ", obj3);
obj3.name = "Rajneesh";
obj3.lastName = "Kumar";
console.log("obj3: ", obj3);
```
Output:

```js
obj3:  {}
obj3:  { name: 'Rajneesh', lastName: 'Kumar' }
```

### Creating an object from another object

```js
let obj = {
    name: "Steve",
    address: {
        state: "Newyork",
        city: "Manhatten"
    },
    sayHi: function () {
        console.log(`${this.name} say's Hi`);
    }
};

let obj2 = Object.create(obj);
console.log(obj2);
```
This is how we create new object by the another object 


`Question`: Can you write a loop to print all members of my object but you need to ignore member of all parent and gParent class. And Can you optimize that operations.


```js
for(let key in obj3){
    let isMyKey = obj3.hasOwnProperty(key);
    if(isMyKey){
        console.log("Actual key's are: ", key);
    }else{
        break;
    }
}
```
Output:
```js
Actual key's are:  friends
Actual key's are:  fullName
Actual key's are:  age
Actual key's are:  friends
Actual key's are:  fullName
Actual key's are:  age
```
so, In this why we have else part is `First all the obj3 key will come then after we will start getting all the parent object key and then great-parent key will come`

One more Example 

```js
for(let key in obj3){
    if(obj3.hasOwnProperty(key)){
        console.log("My Keys are ", key);
    }else if(obj2.hasOwnProperty(key)){
        console.log("Parent Keys are ", key);
    }else{
        console.log("grandParent Keys are ", key);
    }
}
```
# Class

## function constructor before es6

```js
function Person(name, age) {
    // console.log(this);
    this.name = name;
    this.age = age;
    this.sayHi = function () {
        console.log(`I am ${this.name} and ${this.age} years old`);
        return this;
    }

    return this.sayHi();
}

console.log(Person("Rajneesh", 26));   // Treat this as a function.
const rajneesh = new Person("Rajneesh", 27);  // Treat this as a object.
console.log(rajneesh.sayHi());
```
Output :
```js
I am Rajneesh and 26 years old
<ref *1> Object [global] {
  global: [Circular *1],
  queueMicrotask: [Function: queueMicrotask],
  clearImmediate: [Function: clearImmediate],
  setImmediate: [Function: setImmediate] {
    [Symbol(nodejs.util.promisify.custom)]: [Getter]
  },
  structuredClone: [Getter/Setter],
  clearInterval: [Function: clearInterval],
  clearTimeout: [Function: clearTimeout],
  setInterval: [Function: setInterval],
  setTimeout: [Function: setTimeout] {
    [Symbol(nodejs.util.promisify.custom)]: [Getter]
  },
  atob: [Getter/Setter],
  btoa: [Getter/Setter],
  performance: [Getter/Setter],
  fetch: [AsyncFunction: fetch],
  name: 'Rajneesh',
  age: 26,
  sayHi: [Function (anonymous)]
}
I am Rajneesh and 27 years old
I am Rajneesh and 27 years old
Person { name: 'Rajneesh', age: 27, sayHi: [Function (anonymous)] }
```


## Class  after es6

```js
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    sayHi() {
        console.log(`I am ${this.name} and ${this.age} years old`);
    }

    sayBye(){
        console.log(`I am ${this.name} and ${this.age} years old, Have a nice day!`);
    }
}

class SuperHuman extends Person {
    constructor(name, age) {
        super(name, age)
    }
    sayHi() {
        console.log(`I am ${this.name} and ${this.age} years old, I want to say Hi!`);
    }
}

const rajneesh = new Person("Rajneesh", 26);
console.log(rajneesh);
rajneesh.sayHi();

const gibran = new SuperHuman("Gibran", 26);
gibran.sayHi();
gibran.sayBye();
```
Output:

```js
Person { name: 'Rajneesh', age: 26 }
I am Rajneesh and 26 years old
I am Gibran and 26 years old, I want to say Hi!
I am Gibran and 26 years old, Have a nice day!
```

inheritance  : code sharing , saving memory
Prototype is : In JavaScript, a prototype is a mechanism by which objects inherit properties and methods from other objects. Every object in JavaScript has a built-in property called prototype. It's essentially a blueprint or a template that other objects can inherit from
Prottype chain : Prototype chaining in JavaScript is a mechanism where objects inherit properties and methods from other objects through a linked chain. This chain is created by assigning an object's prototype property to another object, allowing for property and method lookups to traverse through the chain until the desired property is found or the end of the chain is reached





# setTimeout -> clearTimeout

```js
console.log("Before");
function cb() {
    console.log("Set-timouts cb has been called");
}
// setTimeout(cb, 1000);

const timerID = setTimeout(cb, 4000);
console.log(timerID);
function canceller() {
    console.log("Cancelling the timeout");
    clearTimeout(timerID);
}
// setTimeout(canceller, 2000); 
console.log("After");
```

# setInterval, clearInterval

```js
console.log("Before");
function cb() {
    console.log("Interval called " + Date.now());
}
// setInterval(cb, 2000)

const timerId = setInterval(cb, 1000);
// console.log(timerId);

function cancelInterval() {
    console.timeEnd();
    console.log("cancelling the interval timer");
    clearInterval(timerId);
}
console.time();
setTimeout(cancelInterval, 5000);

console.log("after");
```



