# Deep and Shallow Copy

array , object -> two types of values -> primitves(values) , non primitives(reference)
  Assignment ->
 *  values ->  are not copied, only main address copied.
 *  reference -> they are also not copied, only main address copied.

---
shallow copy: shallow copy of an object/Array is a copy whose properties share the same references (point to the same underlying values) as those of the source object from which copied object is formed
### shallow copy :
 *  value -> values will be copied and they have diff mem
 * references -> new references will be created but the values inside the reference will be pointing towards same location

For Array using Spread

```js
let arr = [1, 2, 3, 4, [10, 12], 5, 6];
let spreadArray = [...arr];
spreadArray[2] = 100;
spreadArray[4][1] = 200;
console.log("outputs ", spreadArray, arr);

```
For Object using Spread
```js
let person = {
    firstName: 'John',
    lastName: 'Doe',
    address: {
        street: 'North 1st street',
        city: 'San Jose',
        state: 'CA',
        country: 'USA'
    },
};
let copiedObject = { ...person };
```
For Object using Object.assign
```js
let person = {
    firstName: 'John',
    lastName: 'Doe',
    address: {
        street: 'North 1st street',
        city: 'San Jose',
        state: 'CA',
        country: 'USA'
    },
};
let copiedObject = Object.assign({}, person);
```

Deep Copy -> JSON.stringify and JSON.parse

```js
let person = {
    firstName: 'John',
    lastName: 'Doe',
    address: {
        street: 'North 1st street',
        city: 'San Jose',
        state: 'CA',
        country: 'USA'
    },
};
let stringSyntaxOfobject = JSON.stringify(person);
let deepClonedObj = JSON.parse(stringSyntaxOfobject);
console.log("copiedObject", deepClonedObj);
```

# Function to create Deap copy for Array and Object

```js
function superCloneEffective(input) {
    if (!Array.isArray(input) && typeof input !== "object") {
        return input; // Function or wither primitive data type.
    }

    // Create new container to clone values.
    let clone = Array.isArray(input) ? [] : {};

    // copy all the keys and values.
    for (let key in input) {
        const values = input[key];
        clone[key] = superCloneEffective(values);
    }

    return clone;
}
```

# Flattening Array

```js
let input = [1, 2, 3, [4, 5], [6, 7, 8, [9, 10, 110]]];
// Array contains only Integers. 
function flatten(srcArr) {
    let newArr = [];
    for (element of srcArr) {
        if (Array.isArray(element)) {
            let flatterdArrayUsingRecursion = flatten(element);
            newArr.push(...flatterdArrayUsingRecursion);
            // for(ele of flatterdArrayUsingRecursion){
            //     newArr.push(ele);
            // }
        } else {
            newArr.push(element);
        }
    }

    return newArr;
}

let flattenedArr = flatten(input);
```

# Array

## slice

slice cut a sub-array from orgenal array and returns

slice(start)
slice(start, end-1)

```js
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
console.log(animals.slice(2));
console.log(animals.slice(2, 4));
console.log(animals.slice(1, 5));
```
output:
```js
["camel", "duck", "elephant"]
["camel", "duck"]
["bison", "camel", "duck", "elephant"]
```

slice(start)
slice(start, end-1)

## Splice
modifies the original array

splice(start)
splice(start, deleteCount)
splice(start, deleteCount, item1)
splice(start, deleteCount, item1, item2)
splice(start, deleteCount, item1, item2, /* …, */ itemN)

```js
arr=[1,2,3,4,5,6]
let splicedArr = arr.splice(2, 5);
console.log("arr: ", arr)
```
output 
```js
arr:  [ 1, 2 ]
```

## concat
It concat a two array without chainging the original array
First parent array get copied followed by concat array so Time: O(N + M), , S: O(N + M)

```js

arr=[1,2,3,4,5,6]
let concatArr = arr.concat([100, 200, 300]);
console.log("concatArr: ", concatArr);
console.log("arr",arr)
```
output :
```js
concatArr:  [
  1,   2,   3,   4, 5,
  6, 100, 200, 300
]
arr [ 1, 2, 3, 4, 5, 6 ]
```
## trim

remove the whitespace in frount and back of the string

```js
let str = "      Hi i am google     ".trim();
console.log(str)
```
output:
```js
Hi i am google
```

## split
split the string by the given char value

```js
let arrStr = str.split(" ");
console.log("splitted Array: ",arrStr);
```
output:

```js
splitted Array:  [ 'Hi', 'i', 'am', 'google' ]
```

## join

join the array of string by the given value
```js
let joinedStr = arrStr.join("+");
console.log("joinedStr",joinedStr);
```
output:

```js
joinedStr Hi+i+am+google
```


# Function

functions are also `object`
so, it have key, value pair


Let take this code 

```js
function fn() {
    let a = 10;
    console.log("Hi I am an fn");
    fn.count++;
}
fn.count = 0;
fn();  // count 0 -> 1
fn(); // count 1 -> 2
```
now when we run 

```js
// /** method to a fn */
fn.showCount = function () {
    console.log("count on fn  is ", this.count);
}
fn.showCount();
```
then the output: will be 
```js
count on fn  is  2
```

now, when we Iterate this fumction then 

```js
for (let key in fn) {
    console.log("key: ", key, " vaue: ", fn[key]);
}
```

output will be 
```js
key:  count  vaue:  2
key:  showCount  vaue:  [Function (anonymous)]
```

from this we can say "function are the object that implements callable constructor"
or in Layman fn is an object that can also be called.

- function are first class citizens in JS ->
- functions also behave as variables in JS
- Assign a variable , pass a variable as a parameter, return a variable

## pass a variable as a parameter


```js
function fn(param) {
    console.log(" I recived a param", param);
    if (typeof param === "function") {
        param()
    }
}

function smallerfn() {
    console.log("I am smaller");
}

fn({name: "Gibran"})
fn(smallerfn);
```

## HOF (Higher order Function ) - function that accepts or returns a function

example :

```js
function HOF(cb) {
    console.log("Inside HOF");
    cb();
}

function smallerfn() {
    console.log("I am smaller");
}

HOF(smallerfn);
```
output :
```js
Inside HOF
I am smaller
You both are wasted
```
---
there is a bug in the below code 

```js
real();
// this can cause a bug 
function real() { console.log("I am real. Always run me"); }
function real() { console.log("No I am real one "); }
function real() { console.log("You both are wasted"); }
```

output ;

```js
You both are wasted
```
this can be fixed by 

```js
const real = function () { console.log("I am real. Always run me"); }
const real = function () { console.log("No I am real one "); }
const real = function () { console.log("You both are wasted"); }
```

so, now when ypou run then you will get error 
showing that this should not be done 

```js
SyntaxError: Identifier 'real' has already been declared
```

# HOF

HOF are the function that accepts a fn as a parameter or returns a function.
`Callbacks` -> function that are passed as a paramtere to another are known as cb fns.
They usually be called by Higher order functios(HOFns)

HOF -> majorly available on arrays, these fn doesn't change the source array
- foreach
- Map
- filter
- reduce
- sort


## forEach

forEach travesal throw the array and does not return the value

```js
let arr = [2, 3, 4, 5];
const printElem = function (ele) {
    console.log(ele * ele);
    return ele * ele;
}

let rVal = arr.forEach(printElem);   // In forEach returned value get ignored.
console.log(rVal);

arr.forEach((ele) => {
    console.log(ele * ele);
});
```
output :
```js
4
9
16
25
undefined
4
9
16
25
```

## Map

Map travesal throw the array and return the value

```js
let arr = [2, 3, 4, 5];
function squarer(elem) {
    return elem * elem;
}
function cuber(elem) {
    return elem * elem * elem;
}
let squaredArr = arr.map(squarer);
console.log("squaredArr", squaredArr);
// let cubedArr = arr.map(cuber);
let cubedArr = arr.map((ele) => ele * ele * ele)
console.log("cubedArr", cubedArr);
```
output:
```js
squaredArr [ 4, 9, 16, 25 ]
cubedArr [ 8, 27, 64, 125 ]

```
### Polyfill of Map

```js
Array.prototype.myMap = function (logic) {
    let newArray = [];

    for (ele of this) {
        newArray.push(logic(ele));
    }

    return newArray;
}
```

## Filter

filter traverse through every elem -> elem to call back function if call back function function returns true
it will add that elem to a new Arr at the end ,it returns the new Arr

```js
let elems = [1, 2, 3, 11, 4, 5, 34, 12];
function isOdd(elem) {
    return elem % 2 == 1;
}
let oddvaluesArr = elems.filter(isOdd);
console.log("oddvaluesArr: ", oddvaluesArr);
```

output: 
```js
oddvaluesArr:  [ 1, 3, 11, 5 ]
```

### Polyfill of filter

```js
Array.prototype.myFilter = function (logic) {
    let newArray = [];

    for (ele of this) {
        if (logic(ele)) {
            newArray.push(ele);
        }
    }

    return newArray;
}
```

## Reduce


```js
let elems = [1, 2, 3, 4, 5];
function sum(sumSoFar, elem) {
    return sumSoFar + elem;
}
console.log("sum: ", elems.reduce(sum));
```

output:
```js
sum:  15
```

### PLyfill of Reduce 

```js
Array.prototype.myReduce = function (fun, initialValue) {
    if( this.length == 0) {
        return 
    }
    let val = initialValue ? initialValue : this[0]
    for(let i= initialValue? 0:1; i<this.length; i++) {
        val = fun(val, this[i])
    }
    return val
}
```