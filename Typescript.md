# Typescript 3 Fundamentals, v2

## Introduction
-  `.d.ts`: a type declaration file that layers right on top of the JS file it represents 

### `tsconfig`
![image](https://user-images.githubusercontent.com/11481046/110247755-1592bb80-7f6e-11eb-8efc-2731d3c7bf4c.png)

## Typscript basics

### string literal type 
`const y = "hello world";`
* the type is literally "hello world"
* y can never be reassigned since it's a const, so we can regard it as only ever holding a value that's literally the string 'hello world' and no other possible value

* Variable declarations
```js
let zz: number;
zz = 41;
zz = "abc"; // 🚨 ERROR Type '"abc"' is not assignable to type 'number'.
```


## Interfaces 
- name for a structure that we are creating