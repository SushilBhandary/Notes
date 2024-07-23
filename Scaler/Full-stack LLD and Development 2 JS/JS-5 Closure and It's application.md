# Closures

## Lexical scope

outer scope  -> every function has access to it's vars/lets as well as as any variable deaclred outside 
JS says that outer scope is `lexically scoped` -> your fn definiton

```
var varName = 10;
/**fn def*/
function b() {
    console.log("in b", varName);
}

function fn() {
    var varName = 20;
    /**fn call*/
    b();
    console.log("in fn: ", varName);  //20
}

fn();
```
Output:
```
in b 10
in fn:  20
```

## whats is closures 

In JavaScript, a closure is created when a function has access to variables from its outer (enclosing) function, even after the outer function has finished executing. This means the inner function "remembers" the environment in which it was created, allowing it to access variables that are no longer in scope


Example 1 :

```
function outerFunction() {
    console.log("first line of outerfunction: ", count);
    var count = 0;
    function innerFunction() {
        count++;
        return count
    }
    console.log("second line of outerfunction: ", count);
    return innerFunction
}
const innerFunc = outerFunction();  // Log -> Undefined, 0
console.log(innerFunc())  // 1
console.log(innerFunc())  // 2
const innerFunc2 = outerFunction();
```
output:
```
first line of outerfunction:  undefined
second line of outerfunction:  0
1
2
first line of outerfunction:  undefined
second line of outerfunction:  0
```
---
Example 2 :
```
function createCounter(init, delta) {
    function count() {
        init = init + delta;
        return init;
    }
    return count;
}
let c1 = createCounter(10, 5); 
let c2 = createCounter(5, 2);

console.log(c1())  
console.log(c2())

console.log(c1()) 
console.log(c2())
```
output:
```
15
7
20
9
```

## Nested closer
Nested closure : you will get access to outer variable even if the outer fn is not your direct parent

```
let iamINGEC = 200;
function getFirstName(firstName) {
    console.log("I have got your first Name");
    return function getLastName(lastName) {
        console.log("I have got Your last Name");
        return function greeter() {
            console.log(`Hi I am ${firstName} ${lastName}`);  // closure 
            console.log("Hi GEC", iamINGEC) // Lexical scope
            iamINGEC++;
        }
    }
}
const firstNameRtrn = getFirstName("Rajneesh");
const lastNameRtrn = firstNameRtrn("Kumar");
lastNameRtrn();  // iamINGEC=201

lastNameRtrn();  // iamINGEC=202
lastNameRtrn();
console.log("Final Value: ", iamINGEC);
```
outpur:

```
I have got your first Name
I have got Your last Name
Hi I am Rajneesh Kumar
Hi GEC 200
Hi I am Rajneesh Kumar
Hi GEC 201
Hi I am Rajneesh Kumar
Hi GEC 202
Final Value:  203
```

## Application of closures

1. `currying` : taking one input at a time and you use the input later 
2. `asynchrounous` code cannnot run without closure 


### Currying
Builder pattern: https://www.dofactory.com/javascript/design-patterns/builder

```
function getFirstName(firstName) {
    console.log("I have got your first Name");
    return function getLastName(lastName) {
        console.log("I have got Your last Name");
        return function greeter(deduction) {
            console.log(`Hi I am ${firstName} ${lastName}`);  // closure 
            console.log("here is your deduction: ", deduction)
        }
    }
}
getFirstName("Gibran")("Castillo")(100);
```
Output:

```
Hi I am Rajneesh kumar
I have got your first Name
I have got Your last Name
Hi I am Gibran Castillo
here is your deduction:  100
```

### Asynchrounous

```
let a = 100;
console.log("Before");

function cb() {
    console.log("I will explode", a);
}

setTimeout(cb, 2000);
for (let i = 0; i < 1000; i++) {
    a++;
}
console.log("After");   // Async + closures are responsible for such kind of behaviour.
```
Output:
```
Before
After
I will explode 1100
```

# Question on Closures

1. Question 
what is the output of this code

```
function outer() {
    let arrFn = [];
    for (var i = 0;i < 3; i++) {
        arrFn.push(function fn() {
            i++;
            console.log(i);
        })
    }
    return arrFn;
}

let arrFn = outer();
arrFn[0]();
arrFn[1]();
arrFn[2]();
```

output:

```
4
5
6
```

fn is taking value from closure -> i=3
then when arrFn[0] is called then -> i=4 , print
then when arrFn[1] is called then -> i=5 , print
then when arrFn[2] is called then -> i=6 , print

`importan` it is a var variable so that we are seeing this kind of behavier


2. Question 
what is the output of this code
Diff in this code is `var` is changed to `let` 
```
function outer() {
    /**
     * arrfns block scope refer to -> functions
     * */
    let arrFn = [];
    for (let i = 0; i < 3; i++) {
        arrFn.push(function fn() {
            i++;
            console.log(i);
        })
    }
    return arrFn;
}
let arrFn = outer();
arrFn[0]();
arrFn[1]();
arrFn[2]();
```
Output :
```
1
2
3
```

3. Question 
what is the output of this code
Diff in this code from question 2 is let i is defined out side the for loop block

```
function outer() {
    /**
     * arrfns block scope refer to -> functions
     * */
    let arrFn = [];
    let i=0;
    for (i = 0; i < 3; i++) {
        arrFn.push(function fn() {
            i++;
            console.log(i);
        })
    }
    return arrFn;
}
let arrFn = outer();
arrFn[0]();
arrFn[1]();
arrFn[2]();
```
Output:
```
4
5
6
```
fn is getting diffrent values of i because here block scope is different for every iteration

# Aplication of Closure

## infinite curry
Example 1
```
function counter(args){
    let count = 0;
    count++;
    if(args === 0){
        return count;
    }else{
        return function inner(args){
            count++;
            if(args === 0){
                return count;
            }else{
                return inner;
            }
        }
    }
}

console.log(counter(0)); // print -> 1
console.log(counter()(0)); // print ->2
console.log(counter()()()()(0)); // print -> 5

```
Output:
```
1
2
5
```
Example 2
```
function sum(val) {
    if (val === undefined) {
        return 0;
    } else {
        let res = val;
        return function smallerSumHelperMethod(val) {
            if (val === undefined) {
                return res;
            } else {
                res += val;
                return smallerSumHelperMethod;
            }
        }
    }
}

console.log(sum())  // 0
console.log(sum(1)());  // 1
console.log(sum(3)(4)());  // 7
console.log(sum(3)(7)(8)()); //18
```
Output:

```
0
1
7
18
```

## Private variables

```
function createEvenStack() {
    let items = [];
    return {
        push(item) {
            if (item % 2 == 0) {
                console.log("Is pushed");
                items.push(item);
            } else {
                console.log("Please input even value");
            }
        },
        pop() {
            return items.pop();
        },
        display() {
            return items;
        }
    };
}

const stack = createEvenStack();
stack.push(10);
stack.push(6);
stack.push(20);
console.log("pop ele: ", stack.pop());
console.log(stack.items);    // Undefined
stack.items = [10, 100, 1000]; // prevent this behaviour
console.log(stack.items);
console.log("value in the stack ", stack.display())
```

Output:

```
Is pushed
Is pushed
Is pushed
pop ele:  20
undefined
[ 10, 100, 1000 ]
value in the stack  [ 10, 6 ]
```



