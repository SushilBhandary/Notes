

- At the Start Var variable is always `undefined`
- There is no difference between `single` and `double` quote string.
- for template string using `` to store multiple lines of string and '${}' to refer any value.
- typeof --> var a --> undefined 
- In Js number is treated as 64 bit floating number.
- typeof [] (Array) --> `object` , then how will you find thw value is array ar not ? Array.isArray([...])
- typeof null --> `object`

## Primitives

- number
- boolean 
- undefined 
- null
- symbol
- BigInt



## non Primitives

- object 
- Arrays
- function
- map
- set
- weak map 
- weak set

while accessing the key & value from object we use can use
obj["key"] or obj.key . both workes but in some carse obj["key"] does not work .so,
Best to use obj.key

## CodeExec

Code Excution : always exec in EC
    GLobal code -> GEC (Global execution context)
    insidee fn -> own EC (execution context)

code execution
1.  EC creation
    - Hoisting -> memory allocation
    - variable -> undefined;
    - fn -> get it's memory allocated
2. global -> browser -> window/nodejs-> global-> features
3. outer scope-> outer var
4. this-> always calculated
5. EC Code execution


## var

- var get Hoisting 
- before declaration let and const variables cannot be accessed -> temporal dead zone


