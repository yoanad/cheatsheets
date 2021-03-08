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
### arrays
```js
let aa: number[] = [];
aa.push(33);
```

### arrays (tuples)
```js
let bb: [number, string, string, number] = [
  123,
  "Fake Street",
  "Nowhere, USA",
  10110
];
```

### Objects

```js
let cc: { houseNumber: number; streetName: string };
cc = {
  streetName: "Fake Street",
  houseNumber: 123
};
```

*optional operator
```js 
let dd: { houseNumber: number; streetName?: string };
dd = {
  houseNumber: 33,
  streetName: ''
};
```

## Interfaces 
- name for a structure that we are creating

```js
interface Address {
  houseNumber: number;
  streetName?: string;
}
// // and refer to it by name
let ee: Address = { houseNumber: 33 };
```

### Union

```js
export interface HasPhoneNumber {
  name: string;
  phone: number;
}

export interface HasEmail {
  name: string;
  email: string;
}

let contactInfo: HasEmail | HasPhoneNumber =
  Math.random() > 0.5
    ? {
        // we can assign it to a HasPhoneNumber
        name: "Mike",
        phone: 3215551212
      }
    : {
        // or a HasEmail
        name: "Mike",
        email: "mike@example.com"
      };

contactInfo.name; // NOTE: we can only access the .name property  (the stuff HasPhoneNumber and HasEmail have in common)
```
