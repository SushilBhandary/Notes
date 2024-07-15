# This 

In GEC this = window.

Input :
```
var firstVar = "steve";
let secondVal = "loki";
console.log("first: ", window.firstVar);
console.log("second: ", secondVal);
console.log("Hello from: ", this);   // In GEC this = window.

```
output :
```
first:  steve
second:  loki
Hello from:  Window {window: Window, self: Window, document: document, name: '', location: Location, …}
```

---
Input :
```
let cap = {
    // property
    firstName: "Steve",
    // method
    sayHi: function () {
        console.log("Hi from ", this.firstName);
    }
}

cap.sayHi();
let sayHiAdd = cap.sayHi;

var firstName = "loki";
sayHiAdd();
```
output :
```
Hi from  Steve
Hi from  loki
```

- GEC -> this -> window object
- EC is created with -> method call -> this will be that object, Example: cap.sayHi()
- EC is created with -> function call -> this will be that window, Example: let a =  cap.sayHi and then a(). 
---

Input :
```
let cap2 = {
    firstName: "Steve",
    sayHi: function (param) {
        console.log("47", this.firstName);
        const iAmInner = function (param) {
            console.log("49", this.firstName);
        }
        // EC by this kind of call -> window
        iAmInner(20);
    }
}

// EC by this -> cap
cap2.sayHi(10);
```
output :
```
47 Steve
49 loki
```
- iamInner -> this =window, param=20
- cap.saHI -> param=10, this = cap2
---

Input :
```
llet cap3 = {
    firstName: "Steve",
    sayHi: function () {
        console.log("53", this.firstName);
        // arrow ->  does not have it's own this. I am going to cheat it from outside 
        const iAmInner = () => {
            console.log("55", this.firstName);
        }
        iAmInner();
    }
}
cap3.sayHi();
```
output :
```
53 Steve
55 Steve
```
- GEC -> this -> window object
- EC is created with -> method call -> this will be that object
- EC is created with -> function call -> this will be that window
- Arrow function doesn't bother about above rules -> it takes this from outside(nearest scope)
---


### Example 

Input
```
let cap5 = {
    firstName: "Steve",
    sayHi: function () {
        console.log("109", this.firstName);
        // arrow ->  does not have it's own this. I am going to cheat it from outside 
        const subInner = () => {
            console.log("94", this.firstName);
            const iAmInner = function () {
                console.log("114", this.firstName);

                // const iAmSuperInner = function () {
                //     console.log("117", this.firstName);
                // }

                const iAmSuperInner = () => {
                    console.log("117", this.firstName);
                }
                iAmSuperInner();
            }
            iAmInner();
        }
        subInner();
    }
}
cap5.sayHi();
```
Output
```
109 Steve
94 Steve
114 loki
117 loki
```
---

Input
```
let cap4 = {
    firstName: "Steve",
    sayHi: function () {
        console.log("91", this.firstName);
        // arrow ->  does not have it's own this. I am going to cheat it from outside 
        const subInner = () => {
            // console.log("94", this.firstName);
            const iAmInner = () => {
                console.log("95", this.firstName);
            }
            iAmInner();
        }
        subInner();
    }
}
cap4.sayHi();
```
Output
```
91 Steve
95 Steve
```
# This ( with `use strict`)


Input
```
"use strict";
let cap = {
    // property
    firstName: "Steve",
    // method 
    sayHi: function () {
        console.log("Hi from ", this.firstName);
    }
}

cap.sayHi();
let sayHiAdd = cap.sayHi;
var firstName = "loki";
sayHiAdd();  // you will hit by error
```
Output
```
Hi from  Steve
TypeError: Cannot read properties of undefined (reading 'firstName')
```

---

Input :
```
"use strict";
let cap2 = {
    firstName: "Steve",
    sayHi: function () {
        console.log("47", this.firstName);
        const iAmInner = function () {
            console.log("49", this.firstName);
        }
        // EC by this kind of call -> undefined
        iAmInner();
    }
}

// EC by this -> ?? -> cap
cap2.sayHi();
```
Output :
```
47 Steve
TypeError: Cannot read properties of undefined (reading 'firstName')
```

---

Input :
```
"use strict";
let cap = {
    firstName: "Steve",
    sayHi: function () {
        console.log("53", this.firstName);
        // arrow ->  does not have it's own this. I am going to cheat it from outside 
        const iAmInner = () => {
            console.log("55", this.firstName);
        }
        iAmInner();
    }
}
cap.sayHi();
```
Output :
```
53 Steve
55 Steve
```

---
Input :
```
"use strict";
let cap2 = {
    firstName: "Steve",
    sayHi: function () {
        console.log("91", this.firstName);
        // arrow ->  does not have it's own this. I am going to cheat it from outside 
        const subInner = () => {
            console.log("94", this.firstName);
            const iAmInner = () => {
                console.log("95", this.firstName);
                
                const iAmSupperInner = function () {
                    console.log("49", this.firstName);
                }

                iAmSupperInner();
            }
            iAmInner();
        }
        subInner();
    }
}
cap2.sayHi();
```
Output :
```
91 Steve
91 Steve
91 Steve
TypeError: Cannot read properties of undefined (reading 'firstName')
```

---

# Chaining

Input :
```
let ladder2 = {
    step: 0,
    up() {
        this.step++;
        return this;
    },
    down() {
        this.step--;
        return this;
    },
    showStep: function () {
        console.log(this.step);
        return this;
    }
};

// var thisRef = ladder2.up();
// thisRef.showStep();
ladder2.up().up().up().up().up().up().down().showStep();
```
Output:
```
5
```

# Inheritance


Run this on broser 
```
let arr = [1, 2, 3, 4];
console.log(arr);
```
then you can see the inheritance oof the `arr`  
arr ->[p] Array ->[p] Object->[p] null

advantage  of  inheritance -> 
1. Reuse of code
2. Multiple child objects can access the single method of parent.
3. Save memory space


### Example of creating a prototype (inheritance)

```
let arr1 = [1, 2, 3, 4];
let arr2 = [1, 2, 3, 4, 5, 6];
Array.prototype.sum = function () {
    let sum = 0;
    for (let i = 0; i < this.length; i++) {
        sum = sum + this[i];
    }
    console.log(sum);
}

// usecase of this and prototype
arr1.sum();
arr2.sum();
```
output
```
10
21
```
# Bind , Call , Apply

let take this as a starter code for all the bellow 
```
let cap = {
    name: "Steve",
    team: "Cap",

    petersTeam: function (mem1, mem2, ...otherMem) {
        console.log(`Hey ${this.name} I am your neighborhood's  spiderman and i belong to ${this.team}'s team`);

        console.log(`I am working with ${mem1} & ${mem2} with ${otherMem}`);
    }
}

let ironMan = {
    name: "Tony",
    team: "Iron Man"
}
```

if we run the code 
```
cap.petersTeam("black panther", "Winter soldier", "Thor");
```
output:
```
Hey Steve I am your neighborhood's  spiderman and i belong to Cap's team
I am working with black panther & Winter soldier with Thor
```

## call

borrow a fn from another object 

```
cap.petersTeam.call(ironMan, "Natsha", "WarMachine", "Groot", "Thor");
```
output
```
Hey Tony I am your neighborhood's  spiderman and i belong to Iron Man's team
I am working with Natsha & WarMachine with Groot,Thor
```

## apply 

borrow for n number of paramters

```
cap.petersTeam.apply(ironMan, ["Natsha", "WarMachine", "doctor strange", "loki", "thor"]);
```
output:
```
Hey Tony I am your neighborhood's  spiderman and i belong to Iron Man's team
I am working with Natsha & WarMachine with doctor strange,loki,thor
```
## bind

copies function that you can call later with the same this

```
let ironManStolenMem = cap.petersTeam.bind(ironMan);
ironManStolenMem("Natsha", "WarMachine", "doctor strange");
ironManStolenMem("loki", "thor")
```
output: 
```
Hey Tony I am your neighborhood's  spiderman and i belong to Iron Man's team
I am working with Natsha & WarMachine with doctor strange
Hey Tony I am your neighborhood's  spiderman and i belong to Iron Man's team
I am working with loki & thor with 
```

# Shadowing 

The problem is 

```
var fruits = "apple";
console.log(fruits); // apple
{
    console.log(fruits);  // apple
    var fruits;
    console.log(fruits); // apple
    fruits = "orange";
    {
        console.log(fruits) // orange
    }
    console.log(fruits); // orange
}
console.log(fruits); // orange
```

Legal way to slove this is 

```
let fruits = "apple";
console.log("21",fruits); // apple
{ 
    let fruits;
    console.log("25",fruits);
    fruits = "orange";
    {
        let fruits;
        console.log("28",fruits)
    }
    console.log(fruits);
}
console.log(fruits);
```
```
var fruits = "apple";
console.log("21",fruits); // apple
{ 
    let fruits;
    fruits = "orange";
    console.log("25",fruits);
    {
        let fruits;
        console.log("28",fruits)
    }
    console.log(fruits);
}
console.log(fruits);
```

illegal shadowing way is 

```
let fruits = "apple";
console.log("21",fruits); // apple
{ 
    let fruits;
    fruits = "orange";
    console.log("25",fruits);
    {
        var fruits;
        console.log("28",fruits)
    }
    console.log(fruits);
}
console.log(fruits);
```