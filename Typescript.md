# Typescript useful stuff

## Pick
```js
type RoleNamesNeeded = Pick<TagListProps, 'tags'>;
const roleNamesNeeded: RoleNamesNeeded['tags'] = project.rolesNeeded.map((tag) => tag.name);

type RoleNamesNeeded = TagListProps['tags'];
const roleNamesNeeded: RoleNamesNeeded = project.rolesNeeded.map((tag) => tag.name);
```

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
* enforcing a type check; NARROWING a type

const d: PhoneNumberDict = {};
if (d.abc) { // we are checking if the property exists at all
  d.abc = ...;
}

const d: PhoneNumberDict = {};
if (d.abc === 'object') {
  d.abc = ...;
}

* a type may have one string and one number index signature 


### Combining interfaces 

interface PhoneNumberDict {
  home: {
    /**
     * (7) interfaces are "open", meaning any declarations of the
     * -   same name are merged
     */
    areaCode: number;
    num: number;
  };
  office: {
    areaCode: number;
    num: number;
  };
}

// phoneDict.home;   // definitely present
// phoneDict.office; // definitely present
// phoneDict.mobile; // MAYBE present

* interfaces are hoisted (they are like functions), types are not


### Type tests
-> dts lint

## Classes
* Fields
* access modifier keywords (private, public, protecteD)
* `implements`: a class aligning with a particular interface

```js
// For every class that implements that interface, we need to make sure that these properties, that conform to that interface (email, name) are available and stated upfront.
export class Contact implements HasEmail {
  email: string;
  name: string; // member fields
  constructor(name: string, email: string) { // we accept two parameters
    this.email = email;
    this.name = name;  // we pass the things that the constructor recieves on to the instance (this)
  }
}
```

### Access modifiers and initialization
* Parameter properties: a shortcut to specify parameters 

```js
class ParamPropContact implements HasEmail {
  constructor(
  public name: string,
  public email: string = "no email") {
    // nothing needed
  }
}
```

* class fleids can have initializers 
* public, private, protected and readonly(!)

```js
class OtherContact implements HasEmail, HasPhoneNumber {
  protected age: number = 0;
  // private password: string;
  constructor(public name: string, public email: string, public phone: number) {
    this.age = 35;
    // () password must either be initialized like this, or have a default value
    // this.password = Math.round(Math.random() * 1e14).toString(32);
  }
}
```

### definite assignment and lazy initialization 
* Definite assignment operator `!`: trust my TS, I will make sure that this field gets initialized properly
```js
class OtherContact implements HasEmail, HasPhoneNumber {
  protected age: number = 0;
  private password!: string;
  constructor(public name: string, public email: string, public phone: number) {
    // () password must either be initialized like this, or have a default value
  }
  
  async init() {
    this.password = Math.round(Math.random() * 1e14).toString(32);
  }
}
```

* What if we don't know if the field is going to be there at initialization time? 
```js
class OtherContact implements HasEmail, HasPhoneNumber {
  protected age: number = 0;
  private password: string | undefined;
  constructor(public name: string, public email: string, public phone: number) {
    // () password must either be initialized like this, or have a default value
  }
  private get password():string { // if the password value doesn't exists, we are gonna create it lazily
    if (!this.passwordVal;) {
      this.passwordVal = Math.round(Math.random() * 1e14).toString(32);
    }
    
    return passwordVal;
  }
}
```

### Abstract classes 
* cannot be instantiated
* can have implementations
* the abstract method must be implemented by any abstract class

```js
abstract class AbstractContact implements HasEmail, HasPhoneNumber {
  public abstract phone: number; // must be implemented by non-abstract subclasses

  constructor(
    public name: string,
    public email: string // must be public to satisfy HasEmail
  ) {}

  abstract sendEmail(): void; // must be implemented by non-abstract subclasses
}
```


```js
class ConcreteContact extends AbstractContact {
  constructor(
    public phone: number, // must happen before non property-parameter arguments
    name: string,
    email: string
  ) {
    super(name, email);
  }
  sendEmail() {
    // mandatory!
    console.log("sending an email");
  }
}
```

## Converting TS to JS
*Don't do:*
* functional changes
* changes when u have low test coverage
* forget to add tests to your types
* type too strong too early
* publish types for the consumer while they are in a "weak" state


### Step 1: Compiling things in loose mode

1. Start with tests passing
2. Rename all `.js` to `.ts`, allowing implicit any (whenever the TS compiler cannot infer a more specific and useful type, e.g. a function argument)
```js
function foo(a) {
  a.split(', ');
}
```
=> it's not clear what type `a` has
3. Fix only things that are not type-checking, or causing compile errors (e.g. JS classes - you will have to go and state the fields)
4. Be careful about changing behavior!
5. Get tests passing!

### Step 2: Making `Anys` Explicit
1. Start with tests passing 
2. Ban implicit `Any` by setting a TS compiler option
```js
noImplicitAny: true
```
=> instead of TS falling back to Any in cases where it can't figure things out by inference, it will throw an compiler error

3. Where possible, provide a specific and appropriate type
  * Import types for dependancies from `DefinitelyTyped` - open source project
  * otherwise explicit any

4. Get test passing again


### Step 3: Squash explicit `Anys`, enable strict mode
Incrementally, in small chunks...
1. Enable strict mode => 

```js
"strictNullChecks": true, // null is not regarded as a valid value in a type
"strict": true, 
"strictFunctionTypes": true, // validates arguments and return types of callback types
"strictBindCallApply: true // the arguments passed to bind, call, apply and the lexical scope type check appropriately
```

2. Replace explicit anys w/ more appropriate types
3. Try really hard to avoid unsafe casts (e.g. `as`)


## Generics
* Generics are parameterized types
* Generics allow us to parameterize types in the same way that functions parameterize values

```js
// param determines the value of x
function wrappedValue(x: any) {
  return {
    value: x
  };
}
```

```js
// // type param determines the type of x
interface WrappedValue<X> {
  value: X;
}
```
```js
let val: WrappedValue<string[]> = { value: [] };
val.value;
```

### Type parameters

* Filter an array

```js
interface FilterFunction<T = any> {
  (val: T): boolean;
}
```
```js
const stringFilter: FilterFunction<string> = val => typeof val === "string";
stringFilter(0); // 🚨 ERROR
stringFilter("abc"); // ✅ OK
```

* fetch return a `Promise` that resolves to a `Response` and TS figures out that T in `resolveOrTimeout<T>` is a `Response

```js
function resolveOrTimeout<T>(promise: Promise<T>, timeout: number): Promise<T> {
  return new Promise<T>((resolve, reject) => {
    // start the timeout, reject when it triggers
    const task = setTimeout(() => reject("time up!"), timeout);

    promise.then(val => {
      // cancel the timeout
      clearTimeout(task);

      // resolve with the value
      resolve(val);
    });
  });
}
resolveOrTimeout(fetch(""), 3000);
```


### Constraints and scope
* `extends`: if we wanna access an object of type T (e.g. exampleObj.id), we must specify that the type T is at least an object with a property `id`, whose value is a `string`
```js
function arrayToDict<T extends { id: string }>(array: T[]): { [k: string]: T } {
  const out: { [k: string]: T } = {};
  array.forEach(val => {
    out[val.id] = val;
  });
  return out;
}
```
```js
const myDict = arrayToDict([
  { id: "a", value: "first", lisa: "Huang" },
  { id: "b", value: "second" }
]);
```


### Scope

function startTuple<T>(a: T) {
  return function finishTuple<U>(b: U) {
    return [a, b] as [T, U];
  };
}
const myTuple = startTuple(["first"])(42);
  
  
### When to use generics?
* when we want to describe a relationship between two or more types (i.e., a function argument and return type)
