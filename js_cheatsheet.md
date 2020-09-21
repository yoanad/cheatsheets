# JS cheatsheet

# ES6 vs ES5
## Arrow functions
    // ES5
    var multiplyES5 = function(x, y) {
      return x * y;
    };
    // ES6
    const multiplyES6 = (x, y) => { return x * y };
    
    //ES5
    var phraseSplitterEs5 = function phraseSplitter(phrase) {
      return phrase.split(' ');
    };
    //ES6
    const phraseSplitterEs6 = phrase => phrase.split(" ");
    
    //ES5
    var docLogEs5 = function docLog() {
        console.log(document);
    };
    //ES6
    var docLogEs6 = () => { console.log(document); };
# OOP principles 
- Easy to add features and functionality
- Performant
- Clear Structure
- Encapsulation 
## Objects
- notation
1. *Dot Notation:* Call `objectName.propertyName`.
2. *Bracket Notation:* Call `objectName['propertyName']`. 
-  **Initializer Notation**
    var a = 3;
    var b = 'Rome';
    var c = false;
    var o = {a, b, c};
    
    var p = {
        a: 3, 
        b: 'Rome', 
        c: false
    };
    
    var q = {};
    console.log('Object \'q\' (Initial):', q);
    q.a = a;
    q.b = b;
    q.c = c;
    - `**new Object()**`
      var o = new Object();
      o.a = 4;
      o.b = 'Rome';
      o.c = true;
    - `**Object.create()**` → creating an empty object
    - Object.create(*proto*, [*propertiesObject*])
        - proto- > prototype of the newly created object
        - _ _ **proto_ _**  → JS knows to look at the proto if the obj property we sre looking for is not on the object
    var x = {
        a: 5, 
        foo: function() {
            return this.a * this.a;
        }
    };
    var o = Object.create(x);
    
    
- Constructor
    function Actor(firstName, lastName, Age) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.Age = Age;
    }
    
    var a1 = new Actor('Julia', 'Roberts', 48);
    var a2 = new Actor('Kate', 'Winslet', 40);

→ create() vs new()
**Create**

    - with create you initialize a new empty object and you can sepcify some functionality in the proto property. You can then fill the object and assign it properties manually
    function userCreator (name, score) { 
      let newUser = Object.create(userFunctionStore);
      newUser.name = name;
      newUser.score = score; 
      return newUser;
    }; 
    let userFunctionStore = { 
      increment: function(){this.score++;},
      login: function(){console.log("You're loggedin");}
    };
    let user1 = userCreator("Will", 3);
    let user2 = userCreator("Tim", 5);
    user1.increment()
    

**New**

    - with new you create an object and you can directly assign values to the properties. it returns it’s own object as this 
    - When we call the constructor function with new in front we automate 2 things 1. Create a new user object 2. return the new user object
    function User(name, score){
      this.name = name; this.score = score;
    } 
      User.prototype.increment = function(){ this.score++; };
      User.prototype.login = function(){ console.log("login"); };
      
      let user1 = new User(“Eva”, 9);
      user1.increment();
## Class syntactic sugar
- same as above
- Emerging as a new standard
- Feels more like style of other languages (e.g. Python)
    class User { 
      constructor (name, score){
      this.name = name;
      this.score = score;
      }
      increment (){
        this.score++;
      }
      login (){
        console.log("login");
      }
    }
    let user1 = new User("Eva", 9);
    user1.increment();
## Constructor
    class Polygon {
        constructor(height, width) {
            this.height = height;
            this.width = width;
        }
        getArea() {
            return this.height * this.width;
        }
    }


      var testObj = {
        foo: "tball"
      };
     
      // prints: 'We got ourselves a foo!'
      if (testObj.hasOwnProperty('foo')) {
        console.log('We got ourselves a foo!');
      } else {
        console.log('No foo for you!');
      }
## Work with objects

**get key**

    var keys = Object.keys(example);
    Object.values(example)[0].
    
## get key and value
    var key = Object.keys(person)[0];
    var value = person[key];


# Class
## Prototype
- objects inherit from other objects
- When I call a method on an object, i look in the object to find the method, if it’s not there, I look up the next object in the prototypal chain and we can use the functionality there


    Rectangle.prototype.area = function () {
        return this.w * this.h;
    }


## Inheritance 
    class Rectangle {
        constructor(w, h) {
            this.w = w;
            this.h = h;
        }
    }
    /*
     *  Write code that adds an 'area' method to the Rectangle class' prototype
     */
    Rectangle.prototype.area = function (w, h) {
        return this.w * this.h;
    }
    /*
     * Create a Square class that inherits from Rectangle and implement its class constructor
     */
    class Square extends Rectangle {
        constructor(h) {
            super(h, h);
        }
    }
## Iterate over object
- `for … in`

The *for...in* statement iterates over the enumerable properties of an object in an arbitrary order

    const o = {
        a: 1,
        b: 2,
        c: 3,
        d: 4
    };
    
    console.log('property: value');
    // 'p' is the property
    for (p in o) {
        console.log(p + ': ' + o[p]);
    }
    
    const o = ['first', 'second', false];
    
    // 'p' is the index
    for (let p in o) {
        console.log(p + ' ' + o[p]);
    }


- `***forEach***`

The *forEach* method iterates through an array and, for each element, it executes a function once.

    const arr = ['a', 'b', 'c', 'd'];
    
    arr.forEach((value, index, array) => {
        console.log('index', index, 'has a value of', value,
        'which correlates to array[' + index + ']:', array[index]);
    });
    
    arr.forEach((value, index) => {
        console.log('index', index, 'has a value of', value);
    });
    
    arr.forEach((value) => {
        console.log('value:', value);
    });
    
    this.sides.forEach(function (s) {
        sum += s;
    });


# JS map
    var myMap = new Map();
    var keyString = "a string",
        keyObj = {},
        keyFunc = function () {};
    
    // setting the values
    myMap.set(keyString, "value associated with 'a string'");
    myMap.set(keyObj, "value associated with keyObj");
    myMap.set(keyFunc, "value associated with keyFunc");
    
    myMap.size; // 3
    
    // getting the values
    myMap.get(keyString);    // "value associated with 'a string'"
    myMap.get(keyObj);       // "value associated with keyObj"
    myMap.get(keyFunc);      // "value associated with keyFunc"
    
    //deleting
    myMap.delete(key);
    
     map1.keys(); // The keys() method returns a new Iterator object that contains the keys for each element in the Map object in insertion order.
     for (var [key, value] of myMap) {
      console.log(key + ' = ' + value);
    }


# JS Set

The `**Set**` object lets you store unique values of any type, whether [primitive values](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) or object references.

    const set1 = new Set([1, 2, 3, 4, 5]);
    
    console.log(set1.has(1));
    // expected output: true
    
    set1.add('Jane').add('John).add('Luke'); 
    set1.delete('Luke');
    
    for (let name of names) {  
        console.log(name);
    }
    name.forEach(name => console.log(name))  
    
    const names = new Set(['Jane', 'John', 'Luke']);
    //filter, map and reduce
    [...names] // the set is now an array
    [...names].map(name => {firstname: name});
    new Set([...names].map(name => {firstname: name})); // -> convert back to set
    
# String manipulation
## Split
    let arr = [...str];
    let arr = str.split(/(?!$)/u;
    let arr = Array.from(str);
## Splice

At position 2, add the new items, and remove 1 item:

    var fruits = ["Banana", "Orange", "Apple", "Mango"];
    fruits.splice(2, 1, "Lemon", "Kiwi");

At position 2, remove 2 items:

    fruits.splice(2, 2);
## Replace

`*string*``.replace(``*searchvalue, newvalue*``)`

## Matrix range
    var array1 = [1, 2, 3, 4];
    // fill with 0 from position 2 until position 4
    
    console.log(array1.fill(0, 2, 4));
    // expected output: [1, 2, 0, 0]
    
    // fill with 5 from position 1
    console.log(array1.fill(5, 1));
    // expected output: [1, 5, 5, 5]
    
    console.log(array1.fill(6));
    // expected output: [6, 6, 6, 6]
# Prototype

**Function.prototype.apply()**


     Math.max.apply( null, arr );
## Spreading iterables

The spread operator (`...`) turns an iterable into the arguments of a function or parameter call. For example, `Math.max()` accepts a variable amount of parameters. With the spread operator, you can apply that method to iterables.

    > let arr = [2, 11, -1];
    > Math.max(...arr)
    11
# Closure

A *closure* is the combination of a function and the lexical environment within which that function was declared. 
Memoizing


    function outer() {
      let counter = 0;
      function incrementCounter() {
        counter++;
      }
      return incrementCounter;
    }
    let myNewFunction = outer();
    
    myNewFunction(); //actually equals to outer()'s return which is incrementCounter()
    //incrementCounter() is not just a function. function PLUS a reference to the surrounding data. so counter = 0 is available; Like a backpack!
    // IT SAVES THE DATA IN THE BACKPACK.


## Lexical scope → the backpack

the position of the function definition is what determines what data will be available when the function gets invoked.

## [[scope]] 

-> where the live storage of data is stored

    //When a fct is defined, it gets a [[scope]] property that references the Local Memory/Variable Environment in which it has been defined.


# Functions
## Higher order function
- a function that can take or return another function
## Callback function
- a function that can be passed in a higher order function
## Map, reduce, filter
- The `**map()**` method creates a new array with the results of calling a provided function on every element in the calling array.

map is used when you have an array of *stuff* ( scientific term ) and you want to*do something* ( another scientific term ) for every item in that array.

    const numbers = [2, 4, 8, 10];
    const halves = numbers.map(x => x / 2);
    // halves is [1, 2, 4, 5]
- The `**filter()**` method creates a new array with all elements that pass the test implemented by the provided function.
    const words = ["spray", "limit", "elite", "exuberant", "destruction", "present"];
    
    const longWords = words.filter(word => word.length > 6);
    // longWords is ["exuberant", "destruction", "present"]
- The `**reduce()**` method applies a function against an accumulator and each element in the array (from left to right) to reduce it to a single value.
    const total = [0, 1, 2, 3].reduce((sum, value) => sum + value, 1);
    // total is 7


# Asynchronous JS

Allows us to defer our actions until the work (e.g. API request) is completed and continue running our code line by line in the meantime.

- nonblocking
- made possible through browser APIs
## Promise

This is what the standard class `Promise` is for. A *promise* is an asynchronous action that may complete at some point and produce a value. It is able to notify anyone who is interested when its value is available.

    let fifteen = Promise.resolve(15);
    fifteen
      .then(function (value) { console.log(`Got ${value}`)});
      .catch(function (err) {
        console.log("Caught failure " + reason);
        return "nothing";
      })
    // → Got 15


# ES6 maps vs objects
| What            | Map                                                                                                                                                   | Object                                                                                                                                                                                                                   |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Definition      | Map is a data collection type, in which, data is stored in a form of **pairs**, which contains a **unique** **key** and **value mapped to that key**. | ***Regular*** **Objec**t is *dictionary type* of data collection — which means it also **follows key-value stored concept** like Map. Each **key** in Object  is also **unique** and **associated with a single value**. |
| *Key field*     | Any data type - object, array                                                                                                                         | must be simple types - integer, string                                                                                                                                                                                   |
| *Element order* | Order is preserved                                                                                                                                    | Order is not preserved                                                                                                                                                                                                   |
| *Inheritance*   | Map is an instance of Object                                                                                                                          |                                                                                                                                                                                                                          |
| Iterating       | built-in iterable <br>`for … of` , `map.forEach`                                                                                                      | no<br>`for…in` (property) or `Object.keys(obj).forEach((key)`                                                                                                                                                            |
| Performance     | Lookups O(1); Also performs better when keys are unknown until runtime and when all keys are the same type and all values are the same type           | Lookups could be O(n)                                                                                                                                                                                                    |
| When to use     |                                                                                                                                                       |                                                                                                                                                                                                                          |

    const fn = function() {}
    const m = new Map([[document.body, 'stackoverflow'], [fn, 'redis']]);
    
    m.get(document.body) // 'stackoverflow'
    m.get(fn) //'redis'
    
    myMap.set(keyString, "value associated with 'a string'");
    myMap.set(keyObj, 'value associated with keyObj');
    myMap.set(keyFunc, 'value associated with keyFunc');
    


# ES6 sets vs arrays
| What               | Set                                                                                                                                                                                       | Array                                                                                                                                                         |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                    | Abstract data type with only disinct elements, without the need of being allocated by index<br><br>- keyed collection                                                                     | A Structure representing block of data allocated in consecutive memory<br><br>- indexed collection                                                            |
| Acessing elements  |                                                                                                                                                                                           | Faster, because stored in consecutive memory                                                                                                                  |
| Check if el exists | ***Set.prototype.has(value)***<br>set.has(0) → O(1)                                                                                                                                       | ***Array.prototype.indexOf(value)***<br>arr.indexOf(1) !== -1; → O(n)                                                                                         |
| Add/insert         | set.add() → O(1) because hashtable approach                                                                                                                                               | **O(1)** → end with ``arr.push(el)`<br> **O(n)**  → beggining with `arr.unshift(3)`                                                                           |
| Remove             | set.delete(4)<br>set.clear()                                                                                                                                                              | arr.pop() → O(1)<br>arr.shift() → //removes first el → O(n)                                                                                                   |
| Support            | less built in operations, but delete                                                                                                                                                      | no delete, but reduce, reverse, sort…                                                                                                                         |
| When to use        | -  *avoid saving duplicate data to our structure*<br>-  **union(), intersect(), difference()**, etc… are easily implemented effectively based on the native built-in operations provided. | -  *keep elements ordered for quick access*<br>- *heavy modification* (removing and adding elements) or *any action required direct index access to elements* |
| Iteration          |                                                                                                                                                                                           |                                                                                                                                                               |

> **Indexed collections** are collections of data which are ordered by an index value
> **Keyed collections** are collections which use keys; these contain elements which are iterable in the order of insertion.
# CSS
## Border box

Include padding and border in the element's total width and height:
#example1 {
  box-sizing: border-box;
}

## Cascading

**How is priority determined in assigning styles (a few examples)? How can you use this system to your advantage?**
CSS priority is determined by [specificity and inheritance](https://www.smashingmagazine.com/2010/04/css-specificity-and-inheritance/).

- Specificity: ID > class, psuedo-class > element, psudo-element
- Inheritence: specified value → computed value → used value → actual value


# Vue
| Props                                                       | Data                                                                             |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------- |
| data that the ancestor can pass down to the child component | Data is the memory of each component. This is where you would store *your* data; |
|                                                             | data() →  computations only in init() <br>data: {} → object map                  |

| Computed                                                                                          | Methods                       | Watch                                                                              |
| ------------------------------------------------------------------------------------------------- | ----------------------------- | ---------------------------------------------------------------------------------- |
| calculate a value on the fly (behaves as it is a `data` value; it’s *computed* when it’s accessed | you want to execute something | keeps track of any data that changed (you can observe data, props, maybe computed) |
| values are cached                                                                                 | values are not cached         |                                                                                    |

# map, reduce, filer
## map
* takes 2 arguments, a callback and an optional context (will be considered as this in the callback) which I did not use in the previous example. The callback runs for each value in the array and returns each new value in the resulting array.
`const officersIds = officers.map(officer => officer.id);`

# Code style
## Safe mapping
```js
arr 
&& arr.length
&& arr.map((el) =>(
    <Option> {el} </Option>
))
}
```

