# 📦 Javascript Interview Questions

## Tricky coercions questions
```javascript
"5" - "2"  // → 3 (both strings are coerced to numbers)
1 + "2" + 3  // → "123" (1 + "2" = "12", then "12" + 3 = "123")
"10" - 4 + "2"  // → "62" ("10" - 4 = 6, then 6 + "2" = "62")
true + 1 + "3"  // → "23" (true → 1, 1 + 1 = 2, then 2 + "3" = "23")
null + 1        // → 1  (null coerces to 0)
undefined + 1   // → NaN (undefined → NaN)
[] == false   // → true  ([] → "" → 0, false → 0)
[] == 0       // → true  ([] → "" → 0)
"" == 0       // → true  ("" → 0)
[] + []       // → ""      (string coercion, empty arrays become "")
[] + {}       // → "[object Object]" ("" + "[object Object]")
{} + []       // → 0       (parsed as a block + array → +[] → 0) -> [] → 0 (via coercion: [].toString() → "" → Number("") → 0)

``` 

**❓What will be the output of the following code, and why?**

```javascript
console.log("1: script start");

setTimeout(() => {
  console.log("2: setTimeout"); // Macrotask
}, 0);

Promise.resolve().then(() => {
  console.log("3: promise 1");
});

async function asyncFunc() {
  console.log("4: async function start"); // synchronous
  await Promise.resolve();        // Behaves like then
  console.log("5: after await"); // After promise is resolved
}

asyncFunc();

Promise.resolve().then(() => {
  console.log("6: promise 2");
});

console.log("7: script end");

O/p ->

1: script start
4: async function start
7: script end
3: promise 1
5: after await
6: promise 2
2: setTimeout

```

**❓Convert promise to async/await**
```javascript
function loadJson(url) {
    return fetch(url).then((response) => {
        if (response.status == 200) {
            return response.json();
        } else {
            throw new Error(response.status);
        }
    });
}

async function loadJson(url) {
    const res = await fetch(url);
    if (res.status === 200) {
        return await res.json();
    } else {
        throw new Error(`HTTP Error: ${res.status}`);
    }
}
```

**❓What's the output?**

```javascript
console.log("start");
const fn = () => {
    return new Promise((resolve, reject) => {
        console.log(1);
        resolve("success");
    });
};

console.log("middle");

fn().then((res) => {
    console.log(res);
});

console.log("end");
```
O/p -> 
start
middle
1
end
success

// code inside promise will be executed synchronously but reolve will be asynchronous and console print at function execution

**❓What's the output?**

```javascript
function f(){
    console.log(this.name);
}

const val = f.bind({name:"John"}).bind({name:"Ann"})
```

O/p - "John" (Bind chaining does not exist)

**❓Find Maxnumber in array, numbers = [5,6,2,3,7]**

```Javascript

console.log(Math.max.apply(null, numbers));

```

**❓Call `printAnimals` such that the `this` reference is correct**

```javascript
const animals = [
    { species: "Lion", name: "Aslan" },
    { species: "Tiger", name: "Lucy" },
];

function printAnimals(i) {
    this.print = function () {
        console.log("a" + i + " " + this.species + ": " + this.name);
    };
    this.print();
}

O/p -> 
for (let i = 0; i < animals.length; i++) {
    printAnimals.call(animals[i], i);
}

```

**❓Analyze the output.**

```javascript
var status = "😊";

setTimeout(function () {
    this.status = "😎";

    const data = {
        status: "😍",
        getStatus() {
            return this.status;
        },
    };

    console.log(data.getStatus());           // Output 1
    console.log(data.getStatus.call(this));  // Output 2
    // incase of arrow function in setTimeout the output 2 will be 😊
}, 0);

O/p -> 😍
       😎

```

**❓Push elements of one array into another using `Array.prototype.push.apply`**

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

Array.prototype.push.apply(arr1, arr2);

console.log(arr1);
```

**❓What's the output?**

```Javascript
const user = {
    name: "Anukul",
    getName(){
        const name = "Rathore"
        return this.name;
    }
}

console.log(user.getName()); // Output is Anukul
```

**❓Modify function to make ref point to name inside function**

```Javascript
function User(){
    return {
        name: "John",
        ref: this,
    };
}

let user = User(); // ref is currently pointing to window object so ref is will not print anything

Solution -> 

function User(){
    return {
        name: "John",
        ref(){
            return this; // now points to John object 
        }
    };
}
```

**❓Fix the asynchronous call so that `this` refers to the correct object using an arrow function:**

```javascript
const user = {
  name: "Anukul",
  logMessage() {
    console.log(this.name);
  }
};

setTimeout(user.logMessage, 1000); 
//You're passing it by reference, not calling it immediately. SetTimeout will call the function without any object context after 1 sec.

Solution -> 

setTimeout(() => user.logMessage(), 1000); // arrow function takes this reference from wher
setTimeout(user.logMessage.bind(user), 1000);
//You're not using this inside the arrow function — you're calling user.logMessage() directly. So the arrow function isn't relying on this at all. It's just calling a method from the object user.
```

**❓What will be the output and behavior of `this` in the following code?**

```javascript
const obj = {
  name: "Anukul",
  outer() {
    console.log("outer:", this.name); 

    function inner() {
      console.log("inner:", this.name); 
    }

    const inner2 = () => {
      console.log("inner:", this.name);
    };

    inner();
    inner2();
  }
};

obj.outer();

O/p - > 
outer: Anukul
inner: undefined
inner: Anukul
```

**❓What will be the output of the following code?**

```javascript
var length = 4;

function callback() {
  console.log(this.length);
}

const object = {
  length: 5,
  method() {
    arguments[0](); // == arguments[0].call(arguments); this refers to arguments
  }
};

object.method(callback, 2, 3);
```

Ans -> 3

**❓What will be the output of the following code?**

```javascript
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]);

O/p -> 456 

//a["[object Object]"] = 123;
//a["[object Object]"] = 456;
```

**❓What does the following `callOnce` function do?**

```javascript
function callOnce(fn) {
  let called = false; // this variable is "remembered" by the closure

  return function (...args) {
    if (!called) {
      called = true;
      return fn.apply(this, args); // call the original function
    } else {
      console.log("Already called!");
    }
  };
}

Answer -> The callOnce function ensures that the passed function fn is executed only once.
```

**❓Write a function replaceEscapeSequences that returns the modified string with the escape sequences replaced.**

``` javascript

  function replaceEscapeSequences(str) {
    return str
        .replace(/\\n/g, '\n')    // Replace \n with new line where regex -> // with global g
        .replace(/\\t/g, '\t')    // Replace \t with tab
        .replace(/\\"/g, '"')     // Replace \" with double quote
        .replace(/\\\\/g, '\\');  // Replace \\ with backslash
}
```

**❓What happens when we assign an object to another variable and then change its property?**

```javascript
const obj1 = { value: 10 };
const obj2 = obj1;

obj1.value = 20;

console.log(obj2.value); // O/p -> 20
```

**❓What happens when we compare two identical-looking objects using `==` and `===`?**

```javascript
const obj1 = { key: "value" };
const obj2 = { key: "value" };

console.log(obj1 == obj2);  // false
console.log(obj1 === obj2); // false
```

**❓What happens when we assign an object to an array, then set the original variable to `null`, and later modify the object's properties?**

```javascript
let person = { name: "Anukul", age: 25 };

const arr = [person];

person = null;

console.log(arr[0]); // Still accessible

// Modify the object via the array reference
arr[0].age = 30;

console.log(arr[0]); // Modified object

O/p -> 
{ name: 'Anukul', age: 25 }
{ name: 'Anukul', age: 30 }

```

**❓What's the output?**

```javascript
const value = { number: 10 };
const multiply = (x = { ...value }) => {
  console.log(x.number *= 2);
};

multiply();  // 20
multiply();  // 20
multiply(value);  // 20
multiply(value);  // 40
```

**❓What will be the output of the following code involving object mutation and reassignment?**

```javascript
function changeAgeAndReference(person) {
    person.age = 25;
    person = {
        name: "John",
        age: 50,
    };
    return person;
}

const personObj1 = {
    name: "Alex",
    age: 30
};

const personObj2 = changeAgeAndReference(personObj1);

console.log(personObj1);
console.log(personObj2);

O/p => 
{ name: 'Alex', age: 25 }
{ name: 'John', age: 50 }
```

**❓How do you implement an infinite currying sum function?**

```javascript
function sum(a) {
    return function (b) {
        if (b !== undefined) {
            return sum(a + b);
        }
        return a;
    };
}

// Usage
console.log(sum(1)(2)(3)(4)()); // O/p => 10
```

**❓ Illegal shadowing and variable declaration in JavaScript?**

```javascript
let x = 10;

{
    var x = 20; // ❌ SyntaxError: Identifier 'x' has already been declared
    console.log(x);
}

let and const cannot be redeclared and const cannot be reinitialized
```

**Javascript Execution Context**
🧠 Explanation:

In JavaScript, code runs inside an execution context. When a script runs, JavaScript creates a Global Execution Context (GEC) which has two phases:

    Memory Creation Phase (Hoisting Phase)

        All var declarations are hoisted to the top and initialized with undefined.

        All function declarations are hoisted with their full definitions.

        let and const are hoisted too, but not initialized (they remain in a temporal dead zone).

    Execution Phase
        Code runs line by line, assigning values and executing functions. Every function creates a new execution context
        
Map returns a new array whereas forEach modifies the array and all these ,methods allow chaining

**❓What's the output?**

```Javascript
var x = 10;
function random(){
    console.log(x)
    var x = 5;
}

O/p -> undefined
```

**❓What's the output?**
```Javascript
let count = 0;
function printCount(){
    if(count===0){
        let count=1;
        console.log(count)   // prints 1
    }
    console.log(count)       // prints 0
}
```

**❓What is the difference between `let` and `var` in a `for` loop with `setTimeout`?**

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log("var i:", i);
  }, 1000);
}

o/p -> 333

for (let j = 0; j < 3; j++) {
  setTimeout(() => {
    console.log("let j:", j);
  }, 1000);
}

o/p -> 012

function showNums() {
  for (var i = 0; i < 3; i++) {
    (function(n) {
      setTimeout(function () {
        console.log(n);
      }, 1000);
    })(i);
  }
}

showNums();

o/p -> 012
```

**❓How do you implement the Module Pattern in JavaScript with private and public methods?**

```javascript
const CounterModule = (function () {
  let count = 0;
  function logCount() {
    console.log("Current count (private):", count);
  }

  function increment() {
    count++;
    logCount();
  }

  return {
    increment
  };
})();

CounterModule.increment(); // Output: Current count (private): 1
CounterModule.logCount(); // ❌ Error: logCount is not a function
```

**❓What does the following code print?**

```javascript
function f() {
  console.log(this);
}

let user = {
  g: f.bind(null)
};

user.g();

O/p -> null ( window object in browser)
```

**❓What will be the output of the following code when using `bind()` on normal vs arrow functions?**

```javascript
const obj = {
  name: "Original Object",
  normal: function () {
    console.log("Normal:", this.name);
  },
  arrow: () => {
    console.log("Arrow:", this.name);
  }
};

const anotherObj = {
  name: "Another Object"
};

const boundNormal = obj.normal.bind(anotherObj);
const boundArrow = obj.arrow.bind(anotherObj);

boundNormal(); // ✅ Output: Normal: Another Object
boundArrow();  // ✅ Output: Arrow: undefined (or window/globalThis.name in browsers)
// Arrow function don't define their own this inherit it from surrounding lexical scope 
```

**❓ What are Promise combinators in JavaScript and how do they behave?**

```javascript
const p1 = new Promise((resolve) => setTimeout(() => resolve("One"), 100));
const p2 = new Promise((resolve) => setTimeout(() => resolve("Two"), 200));
const p3 = new Promise((_, reject) => setTimeout(() => reject("Error in Three"), 150));

// ✅ Promise.all
Promise.all([p1, p2])
  .then(console.log)        // ["One", "Two"]
  .catch(console.error);

// ✅ Promise.race
Promise.race([p1, p2])
  .then(console.log)        // "One"
  .catch(console.error);

// ✅ Promise.allSettled
Promise.allSettled([p1, p2, p3])
  .then(console.log);       
/* Output:
[
  { status: "fulfilled", value: "One" },
  { status: "fulfilled", value: "Two" },
  { status: "rejected", reason: "Error in Three" }
]
*/

// ✅ Promise.any
Promise.any([p3, p2, p1])
  .then(console.log)        // "Two"
  .catch(console.error);    // Only if all promises reject

**🧠 Summary:**
- `Promise.all` → Waits for all promises to fulfill, or rejects if any fail.
- `Promise.race` → Resolves or rejects as soon as the first promise settles..
- `Promise.allSettled` → Waits for all promises to settle (fulfilled or rejected), returns an array of outcomes.
- `Promise.any` → Returns the first fulfilled promise, or rejects if all are rejected.
```

**❓What will be the output of the following code when a Promise is created but never resolved or rejected?**

```javascript
console.log("1: Start");

const promise = new Promise((resolve, reject) => {
  console.log("2: Inside promise (sync part)");
  // No resolve or reject
});
console.log("Random)
promise.then(() => {
  console.log("3: This will NOT run");
});

console.log("4: End");

// ✅ Output:
1: Start
2: Inside promise (sync part)
Random
4: End
```
**🧠 Note:**  
Since the promise is never resolved or rejected, the `.then()` callback is never executed.


**❓What is the output order of this asynchronous code involving a Promise and `setTimeout`?**

```javascript
console.log("A: Script start");

function tricky() {
  console.log("B: Function called");

  return new Promise((resolve) => {
    console.log("C: Promise executor running");
    setTimeout(() => {
      console.log("D: Timeout done, resolving promise");
      resolve("Done");
    }, 1000);
  });
}

console.log("E: Before calling tricky()");

tricky().then((result) => {
  console.log("F: Promise resolved with:", result);
});

console.log("G: After calling tricky()");
```

// ✅ Output order:
A: Script start
E: Before calling tricky()
B: Function called
C: Promise executor running
G: After calling tricky()
D: Timeout done, resolving promise
F: Promise resolved with: Done

**❓What is the output of this Promise chain with resolves, throws, and catches?**

```javascript
function myPromiseFunc(value) {
  return new Promise((resolve, reject) => {
    if (value === 1) {
      resolve("Resolved with 1");
    } else if (value === 2) {
      reject("Rejected with 2");
    }
  });
}

myPromiseFunc(1)
  .then((res) => {
    console.log("then1:", res);       // Logs: Resolved with 1
    return "Return a string";          // Resolves next step with this string
  })
  .then((res) => {
    console.log("then2:", res);       // Logs: then:Return a string
    throw new Error("Error thrown!"); // Throws error, skips next then, goes to catch
  })
  .then(() => {
    console.log("then3: skipped");    // This is skipped because of thrown error
  })
  .catch((err) => {
    console.log("catch1:", err);      // Logs: catch 1:Error thrown!
    return "Recovered from error";    // Resolves the chain again
  })
  .then((res) => {
    console.log("then4:", res);       // Logs: then4:Recovered from error
    return Promise.reject("Rejected again");
  })
  .catch((err) => {
    console.log("catch2:", err);      // Logs: catch2:Rejected again
  });

// ✅ Output:
then1: Resolved with 1  
then2: Return a string  
catch1: Error: Error thrown!  
then4: Recovered from error  
catch2: Rejected again  
```

## Prototypes

```javascript
Array.prototype.mymap = function (cb) {
  let temp = [];
  for (let i = 0; i < this.length; i++) {
    temp.push(cb(this[i], i, this));
  }
  return temp;
};

Array.prototype.myfilter = function(cb) {
    let temp = [];
    for(let i=0;i< this.length;i++){
        if(cb(this[i], i, this)){
            temp.push(this[i])
        }
    }
    return temp;
}

Array.prototype.myreduce = function(cb, initial) {
    let accumulator = initial;
    for(let i=0;i< this.length; i++){
        accumulator = accumulator? cb(accumulator,this[i], i, this) : this[i];
    }
    return accumulator;
}

Function.prototype.mycall = function (context = {}, ...args) {
    if(typeof this !== "function"){
        throw new Error(this + "is not a function");
    }

    const fnkey = Symbol(); // not overwriting any other property mistakenly
    context[fnkey] = this;
    const result = context[fnkey](...args)
    delete context[fnkey]; // to unbind the property from object
    return result;
}

Function.prototype.myapply = function (context = {}, args = []){
    if(typeof this !== "function"){
        throw new Error(this + "is not a function");
    }

    if(!Array.isArray(args)){
        throw new Error(this + "expects array argument");
    }

    const fnkey = Symbol();
    context[fnkey] = this;
    const result = context[fnkey](...args)
    delete context[fnkey]
    return result;
}

Function.prototype.mybind = function (context = {}, ...args){
    if(typeof this !== "function"){
        throw new Error(this + "is not a function");
    }

    const fnkey = Symbol();
    context[fnkey] = this;
    return function(){
        return context[fnkey](...args)
    }
}

function mymemoize(cb, context) {
    const res = {};

    return function(...args) {
        const argsStr = JSON.stringify(args); 

        if (!res[argsStr]) {
            res[argsStr] = cb.call(context || this, ...args);
        }

        return res[argsStr];
    };
}

const mydebounce = function debounce (cb, delay){
    let timer;
    return function(...args){
        if(timer) clearTimeout(timer);
        timer = setTimeout(()=>{
            cb(...args)
        }, delay)
    }
}

function throttle(cb, delay) {
    let last = 0;

    return function (...args) {
        const now = Date.now().getTime(); 
        if (now - last >= delay) {
            last = now;
            return cb(...args);
        }
    };
}

```

**❓Deep compare two objects**

```javascript
function deepCompare(obj a, obj b){
  if(a===b) return true;
  if(typeof a!== 'object' || typeof b!== 'object' || a===null || b===null) return false;
  if(Object.keys(a).length!==Object.keys(b).length) return false;
  for(let key of a){
    if(!Object.keys(b).includes(key) || !deepCompare(a[key], b[key])) return false;
  }
  return false;
}
```

