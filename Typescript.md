# Typescript 3 Fundamentals, v2
https://frontendmasters.com/courses/typescript-v2/

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

### Intersection

let otherContactInfo: HasEmail & HasPhoneNumber = {
  // we _must_ initialize it to a shape that's asssignable to HasEmail _and_ HasPhoneNumber
  name: "Mike",
  email: "mike@example.com",
  phone: 3215551212
};


## Type Systens & Type Equivalents
* Nominal type systems: the type depends on whether X is an instance of a class/type named Y (e.g. Java; JS is not necessarily written in classes)
* Structural type systems: only care about the shape of an object

*Shape*: the names of the properties and the types of their values 

### Widener vs Narrower
* Describes the relative differences in range of a type's allowable values
* Never: it is nothing

![image](https://user-images.githubusercontent.com/11481046/110376878-05550c00-8054-11eb-91ff-97449091e97d.png)

## Function basics
* `to`: argument 
* `: { recipient: string; body: string }`: return type
* return types can be inferred, but things can easily slip up

```js
function sendEmail(to: HasEmail): { recipient: string; body: string } {
  return {
    recipient: `${to.name} <${to.email}>`, // Mike <mike@example.com>
    body: "You're pre-qualified for a loan!"
  };
}
```

Arrow function

```js 
const sendTextMessage = (
  to: HasPhoneNumber
): { recipient: string; body: string } => {
  return {
    recipient: `${to.name} <${to.phone}>`,
    body: "You're pre-qualified for a loan!"
  };
};
```

### Overload signatures

```js
// "overload signatures"
function contactPeople(method: "email", ...people: HasEmail[]): void;
function contactPeople(method: "phone", ...people: HasPhoneNumber[]): void;
```

```js
// "function implementation"
function contactPeople(
  method: "email" | "phone",
  ...people: (HasEmail | HasPhoneNumber)[]
): void {
  if (method === "email") {
    (people as HasEmail[]).forEach(sendEmail);
  } else {
    (people as HasPhoneNumber[]).forEach(sendTextMessage);
  }
}
```

### Lexical scope - handling this in functions
```js
function sendMessage(
  this: HasEmail & HasPhoneNumber,
  preferredMethod: "phone" | "email"
) {
  if (preferredMethod === "email") {
    console.log("sendEmail");
    sendEmail(this);
  } else {
    console.log("sendTextMessage");
    sendTextMessage(this);
  }
}
const c = { name: "Mike", phone: 3215551212, email: "mike@example.com" };
```

```js
function invokeSoon(cb: () => any, timeout: number) {
  setTimeout(() => cb.call(null), timeout);
}
```

```js
// ✅ creating a bound function is one solution
const bound = sendMessage.bind(c, "email");
invokeSoon(() => bound(), 500);
```

```js
// ✅ call/apply works as well
invokeSoon(() => sendMessage.apply(c, ["phone"]), 500);
```


## Interfaces & type aliases
### Type aliases
* Type aliases allow us to give a type a name
 ```js
type StringOrNumber = string | number;
```

```js
// // this is the ONLY time you'll see a type on the RHS of assignment
type HasName = { name: string };
```

```js
// NEW in TS 3.7: Self-referencing types!
type NumVal = 1 | 2 | 3 | NumVal[];
```

### Interfaces; Call and construct signatures
* Interfaces can extend from other interfaces
* *exteds* is used for inheriting like-things (e.g. a class extends a class)

```js
export interface HasInternationalPhoneNumber extends HasPhoneNumber {
  countryCode: string;
}
```

* Type aliases can handle primitive types as well, interfaces can extend ONLY the JS Object and some types (arrays and functions)
* Interfaces can describe objects, arrays
* Interfaces can describe call signature

```js
interface ContactMessenger1 {
  (contact: HasEmail | HasPhoneNumber, message: string): void;
}
```

```js
type ContactMessenger2 = (
  contact: HasEmail | HasPhoneNumber,
  message: string
) => void;
```

* contextual inference of functions => we don't need type annotations for contact or message

```js
const emailer: ContactMessenger1 = (_contact, _message) => {
  /** ... */
};
```

* construct signatures 
```js
interface ContactConstructor {
  new (...args: any[]): HasEmail | HasPhoneNumber;
}
```

### Interfaces; dictionary objects and index signatures
* index signatures => describe how a type will respond to property access

* if u access a property of a `PhoneNumberDict` and give it a `string`, it will either not be there (`undefined`) or it will have the form of `{
        areaCode: number;
        num: number;
      };`
      
 
```js
interface PhoneNumberDict {
  // arr[0],  foo['myProp']
  [numberName: string]:
    | undefined
    | {
        areaCode: number;
        num: number;
      };
}
```
