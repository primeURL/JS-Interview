# JS-Interview

[1. What is map, filter and reduce](#1-what-is-map-filter-and-reduce)

[2. What is Closures ?](#2-what-is-closures)

Code Step by step
Yahho baba
Akshay Saiini
Color Code

## 1. What is map, filter and reduce ? 
### A. Map Method
    * creates a new array from calling a function for every array element.
    * does not execute the function for empty elements.
    * does not change the original array.
```Javascript
const nums = [2,3,4,5]
const newNums = nums.map((curEle,index,arr)=>{
    return (curEle * 2) + index;
})
console.log(newNums) // [5,7,9,11]
```
#### PolyFills for Map
```Javascript
Array.prototype.mymap = function (cb){
    console.log(this); // [1,2]
    let temp = [];
    for(let i=0;i<this.length;i++){
        temp.push(cb(this[i],i,this))
    }
    return temp;
}

const nums = [1,2]
const newNums = nums.mymap((curvalue,index,arr)=>{
    return 2 * curvalue
})
console.log(newNums) // [2,4]
```
> Note : If we replace mymap to arrow function it will not work, bcz this keyword does not exits in arrow functions 

### map vs forEach
|map()|foreach()|   
|----|-----|   
|Return newArray after performing opertion on elements of an array. Does not modify existing array|Does not return newArray, and modifies existing array|
|As it return new Array we can chain other methods like filter or reduce|As it does not return new Array we cannot chain methods|


### B. Filter Method
    * method creates a new array filled with elements that pass a test provided by a function.
    * method does not execute the function for empty elements.
    * method does not change the original array.
```Javascipt
const nums = [2,3,4,5]
const newNums = nums.filter((curEle,index,arr)=>{
    return curEle > 2;
})
console.log(newNums) // [3,4,5]
```
#### PolyFills for Filter
```Javascript
Array.prototype.myfilter = function(cb){
    console.log(this);
    let temp = [];
    for(let i=0;i<this.length;i++){
        if(cb(this[i],i,this)) 
            temp.push((this[i]))
    }
    return temp;
}

const nums = [1,2,3,4,5]
const newNums = nums.myfilter((curvalue,index,arr)=>{
    return curvalue > 2
})
console.log(newNums) // [3,4,5]
```
### C. Reduce Method (tricky)
    * method executes a reducer function for array element.
    * method returns a single value: the function's accumulated result.
    * method does not execute the function for empty array elements.
    * method does not change the original array.
```Javascript
const nums = [1,2,3,4]
const sum = nums.reduce((acc,curvalue,index,arr)=>{
    return acc + curvalue
},0)
console.log(sum) // 10

// Important Example
// Input
let students = [
    {name : 'Utkarsh1', age: 23,marks : 20},
    {name : 'Utkarsh2', age: 23,marks : 30},
    {name : 'Utkarsh3', age: 21,marks : 40},
    {name : 'Utkarsh4', age: 21,marks : 50},
    {name : 'Utkarsh5', age: 22,marks : 60},
]
// Output
// {23 : 2, 21: 2, 22 : 1}

const newNums = students.reduce((acc,item)=>{
    if(acc[item.age.toString()]){
        acc[item.age.toString()]++;
    }else{
        acc[item.age.toString()] =  1
    }
    return acc;
},{})
console.log(newNums);
```
> Note : At the first callback, there is no return value from the previous callback.

> Normally, array element 0 is used as initial value, and the iteration starts from array element 1.

> If an initial value is supplied, this is used as accumulator first value, and the iteration starts from array element 0.

#### PolyFills for Reduce
```Javascript
Array.prototype.myreduce = function(cb,initialValue){
   let accumulator = initialValue
    for(let i=0;i<this.length;i++){
        accumulator = accumulator ? cb(accumulator,this[i],i,this) : this[i]
    }
    return accumulator;
}

const nums = [1,2,3,4]
const sum = nums.myreduce((acc,curvalue,index,arr)=>{
    return acc + curvalue
},0)
console.log(sum) // 10
```
## Questions on map, filter and reduce 
1. Return total marks for the students with marks greater than 40, after 20 marks have been addded to those who scored less than 50.
```Javascript
let students = [
    {name : 'Utkarsh1', age: 23,marks : 20},
    {name : 'Utkarsh2', age: 233,marks : 30},
    {name : 'Utkarsh3', age: 232,marks : 40},
    {name : 'Utkarsh4', age: 231,marks : 50},
    {name : 'Utkarsh5', age: 231,marks : 60},
]
const newNums = students.map((item,index,arr)=>{
    if(item.marks < 50){
        return {...item, marks : item.marks + 20}
    }else{
        return item
    }
}).filter((item)=>{
    return item.marks > 40;
}).reduce((acc,item)=>{
    return acc + item.marks
},0)

console.log(newNums);
```

## 2. What is Closures ? 
> a.  When we have a function defined inside of the another function, that inner function have access to variables and scopes of outer function even if that outer function is being executed

> b. Clouser is created when we defined a function, not when we executed

##### Important Interviews Quetions
1. Concept of Clousers
```Javascript
function a(){
    for(var i=0;i<4;i++){
        setTimeout(()=>{
            console.log(i);
        },i*1000)
    }
}
a()  // 4,4,4,4
//  Note : var have function scope, so in every iteration it value get updated to new value 

function a(){
    for(let i=0;i<4;i++){
        setTimeout(()=>{
            console.log(i);
        },i*1000)
    }
}
a()  // 0,1,2,3
//  Note : let have block scope, so in every iteration new block is created and value is preserved.



// Question : Keep variable var only and give output as 0, 1, 2, 3
function a(){
    for(var i=0;i<4;i++){
        function inner(i){
            setTimeout(()=>{
                console.log(i);
            },i*1000)
        }
        inner(i)
    }
}
a()  // 0,1,2,3
```
2. Module Pattern (Explore More)
```Javascript
var Module = (function(){
    function privateMethod(){
        console.log('Private');
    }

    return {
        publicMethod : function(){
            console.log('public');
        },
        privateMethod
    }
})()
Module.publicMethod();
Module.privateMethod()
```
3. Execute function once only if called n times
```Javascript
function once(func,context){
    let ran;
    return function(){
        if(func){
            ran = func()
            func = null
        }
        return ran
    }
}

let hello = once(()=>console.log('Hello Wolrd'))
hello() // Hello World
hello() // No Output
```
3. Caching/Memoize important
```Javascript
function Cached(fn){
    let res = {}
    return function(...args){
        console.log(args);
        const argsString = JSON.stringify(args)
        console.log(argsString);
        if(!res[argsString]){
            res[argsString] = fn(...args)
        }
        return res[argsString]
    }
}
const clumysquare = function(num1,num2){
    for(let i=0;i<100000000;i++){
    }
    return num1*num2;
}

const cachedClumysquare = Cached(clumysquare)

console.time()
console.log(cachedClumysquare(500,500));
console.timeEnd()

console.time()
console.log(cachedClumysquare(500,501));
console.timeEnd()
```
4. **Encapsulation**

Closures can be used to encapsulate functionality within a function, making certain variables inaccessible from outside the function, thus achieving data encapsulation.
```JS
function Counter() {
  let count = 0; // Variable count is encapsulated within the Counter function
  return {
    increment: function() {
      count++;
    },
    decrement: function() {
      if (count > 0) {
        count--;
      }
    },
    getCount: function() {
      return count;
    }
  };
}
const counter = Counter();
counter.increment();
counter.increment();
console.log(counter.getCount()); // Output: 2

```
In this example, the count variable is encapsulated within the Counter function. The returned object provides methods (increment, decrement, and getCount) to interact with the count variable, but the count variable itself is not directly accessible from outside the Counter function.

5. **Private Data**

Closures can be used to create private variables that are inaccessible from outside the function, ensuring data privacy.
```JS
function Person(name, age) {
  let _name = name; // Private variable
  let _age = age;   // Private variable

  return {
    getName: function() {
      return _name;
    },
    getAge: function() {
      return _age;
    }
  };
}

const person = Person("John", 30);
console.log(person._name); // Output: undefined (private variable)
console.log(person.getName()); // Output: "John"
console.log(person.getAge());  // Output: 30
```
In this example, _name and _age are private variables encapsulated within the Person function. They cannot be accessed directly from outside the function, but getter methods (getName and getAge) are provided to access their values.

6. **Module Pattern**

[Watch for More Understanding](https://www.youtube.com/watch?v=pOfwp6VlnlM&t=572s)

Closures can be used to create modules, which are self-contained units of code that expose a public interface for interaction while keeping their internal implementation hidden.
```JS
let person = (function(){
  let myName = 'Utkarsh'; // Private 
  function sayName(){
    console.log(myName)
  }

  function setName(newName){
    myName = newName
  }
  return {
    sayName,
    setName
  }
})()

person.sayName()  // Utkarsh
person.setName('Hardik')
person.sayName()  // Hardik
```
## 3. What is Currying ? 

[Read this blog](https://roadsidecoder.hashnode.dev/javascript-interview-questions-currying-output-based-questions-partial-application-and-more)

## 4. Objects in Javascript ? 
> Objects are passed by reference.
```Javascript
//Dynamic Properties 
let key = 'firstname';
let value = 'utkarsh';
const user = {
    [key] : value
}
console.log(user);  // {firstname : 'utkarsh'}
```

```Javascript
//Looping in Objects

const user = {
    name : 'Utkarsh',
    age : 23,
    isflag : true
}
for(key in user){
    console.log(key);  // name, age, isflag
}
for(key in user){
    console.log(user[key]);  // Utkarsh, 23, true
}
```

```Javascript
// What is the Output of the code ? 
const a = {}
const b = {key : 'b'}
const c = {key : 'c'}
a[(b)] = 100;
a[(c)] = 200
console.log(a) // { '[object Object]': 200 }
console.log(a[(b)]); // 200

// Note : Keys are stored in [object Object] fashion, to resolve this need to stringify.

```
```Javascript
//Object Referencing
 let a = {fruit : 'mango'}
 let b = a;
 a.fruit = 'banana';
 console.log(b.fruit); // banana
//------------------------------------------

let person = {name : 'utkarsh'}
let array = [person]
person = null
console.log(array); // [ { name: 'utkarsh' } ]
//------------------------------------------

let a = {a:2}
let b = {a:2}
 console.log(a===b); // false
// Note : both a and b are pointing to diffrent memory location. Solution is to stringify both objects and compare
```

```Javascript
// Refrencing vs local copy

let value = {num : 10}
let newValue = value   // newValue is refrence to value i.e also know as shallow copy
newValue.num = 20
console.log(newValue); // { num: 20 }
console.log(value);   // { num: 20 }

let value = {num : 10}
let newValue = {...value} // newValue is local-copy of value i.e also know as deep copy
// let newValue = Object.assign({},value)    // another way to deep copy
// let newValue = JSON.parse(JSON.stringify(value)) // another way to deep copy
newValue.num = 20
console.log(newValue); // { num: 20 }
console.log(value);   // { num: 10 }





//Question
let value = {num : 10}
const multiply = (x = {...value})=>{
    console.log(x.num +=10)
}
multiply()    // 20
console.log(value); // { num: 10 }
multiply()    // 20
multiply(value)  // 20
console.log(value); // { num: 20 }
multiply(value) // 30

```
```Javascript
// Important Question
function changeAgeandRefrence(person){
    // think 'person' in kind of varibale which is pointing to 'person1' object
    person.age = 26;
    person = {   // here we re-assigning new Object to varibale
        name : 'Raj',
        age: 30
    }
    return person
}
let person1 = {
    name :'Utkarsh',
    age : 23
}
const result = changeAgeandRefrence(person1)
console.log(person1);   // { name: 'Utkarsh', age: 26 }
console.log(result);   // { name: 'Raj', age: 30 }
```

## 5. Debouncing and Throttling

```Javascript
//HTML
<button onclick="advancedSimple()">Click Me</button>
//JS
let counter = 0
function simple(){
    console.log('API call made',++counter);
}
const throttle = (fn,limit)=>{
    let flag = true;
    return function(){
        if(flag){
            fn()
            flag = false;
            setTimeout(()=>{
                flag = true
            },limit)
        }
    }
}

const debounce = (fn,limit)=>{
    let timer;
    return function(){
        if(timer) clearTimeout(timer)
        timer = setTimeout(()=>{
            fn()
         },limit)
    }
}

let advancedSimple = debounce(simple,1000)
```

## 5. Memoization techniques, Pure functions and Pure components

### Pure Functions 
> Why ? Cleaner, Predictable, Consistent, Debuggable, Portable, Testable

> Pure Functions in JS are functions that does not have any Side-effects

> A Function that always return a exact same thing every single time you gave the same inputs

> Pure functions will not affect external thing's outside the function, it will take inputs and operate on those inputs(but will not manipulate that inputs)in order to create output.

> For same input's it will always return same output

> As it does not have any side-effects, we don't have to mock anything. It is easy to unit test Pure Functions.

> DownSides : Can't access Database, Can't access files, Can't access anything outside of it

[Watch for More Understanding- Kyle](https://www.youtube.com/watch?v=fYbhD_KMCOg)

[Watch for More Understanding - Color Code](https://www.youtube.com/watch?v=fYbhD_KMCOg)

#### Impure Functions Examples
1. DOM manipulation
2. Math.random()
3. new Date()
4. User Input
5. File IO
6. Network Request

## 6. Explain the concept of AJAX

> Asynchronous JavaScript and XML (AJAX) is a web development technique that allows for asynchronous communication between the client and server, enabling dynamic content updates without refreshing the entire page.

1. Event is occured in a webpage(button is clicked).
2. An XMLHttpRequest Object is created by JS.
3. XMLHttpRequest Object sends request to a web-server.
4. Server process the request and send back response to web-page.
5. The response is read by JS and Update the Page.
6. All this happens dynamically without page refresh.


## 6. Event Loop In JS ?  Call Stack, Call Queue, ? 
In Js, Event Loop is fundamental Mechanism that enables Asynchronous execution of code.The event loop is responsible for managing execution of code, handling events, and maintaining flow of control.

```Javascript
console.log('A')
console.log('B')

setTimeout(()=>console.log('C'),4000)

fetch('API').then(()=>console.log('Data Retrived'))

cosole.log('D')

//  Output
// ['A', 'B', 'D', 'Data Retrived', 'C']
```
How Event Loops Works ? 
1. JS uses call stack to keep track of code, JS have Global execution context to run the code.
2. For handling events and fecth API, JS put them in Web API's.
3. From Web API's it comes to call back queue, and when global-execution-context is over Event loop check in call-back queue and pushes it to Call Stack.

[Watch for More Understanding - Piyush Garg](https://www.youtube.com/watch?v=kl7ActKCQIU)


## 7. How can you handle errors in JavaScript?

> If we did not handle erros in JS, the line where erros has occured from that line to bottom of our code execution will not happen, means code will break. No more execution of code will happen.

> To run code gracefully even if errors has occured, we must caught errors in JS with try, catch block. So, if errors are occured then also code execution will not stop, because we are catching the errors in catch block.

1. try : The try statement defines a code block to run (to try)
2. catch : The catch statement defines a code block to handle any error.
3. finally : The finally statement defines a code block to run regardless of the result.
4. throw : The throw statement defines a custom error.

Note : We can also use nested try-catch block to handle errors.

[Watch for More Understanding](https://www.youtube.com/watch?v=rHQ1etbQpmQ)

## 8. Explain the concept of callback functions ?

In JS, callback function is a function that is passed as an argument to another function and is executed after completion of specific task or an event. 

```JS
// Function with a callback
// FetchData is Higher Order function
function fetchData(callback) {
  // Simulating asynchronous operation (e.g., fetching data from a server)
  setTimeout(function() {
    fetch('https://jsonplaceholder.typicode.com/users')
    .then(res=>res.json())
    .then((data)=>{
      callback(data); // Calling the callback function with the fetched data
    })
    .catch((err)=> console.log(err))
  }, 1000); 
}
// Callback function
function displayData(data) {
  console.log('Data', data);
}
// Calling the function with the callback
fetchData(displayData);
```

### Use Cases in JS

1. Event Handling:
   Callbacks are commonly used to handle events in web development.
```JS
// Event handling with callbacks
document.getElementById('myButton').addEventListener('click', function() {
  console.log('Button clicked!');
});
```
2. Array Iteration:
    Callbacks can be used with array iteration methods like forEach, map, filter, etc.
```JS
// Array iteration with callbacks
const numbers = [1, 2, 3, 4, 5];
numbers.forEach(function(number) {
  console.log(number * 2);
});
```
3. Custom Callbacks:
    Callbacks can also be defined as custom functions to be executed at a specific point in another function.
```JS
// Custom callback
function processInput(input, callback) {
  // Some processing logic
  const processedData = input.toUpperCase();
  // Execute the callback function with processed data
  callback(processedData);
}

// Usage of custom callback
processInput('hello', function(result) {
  console.log('Processed input:', result);
});
```

## 9. What is the purpose of the setTimeout function ?
The 'setTimeout' function in JS is used to schedule the execution of a function after a specifed delay. It's primary purpose is to introduce asychronous behaviour into JS code, allowing certain actions to be executed after certain period of time , without blocking the execution of other code.

### Use Cases : 

1 . **Delaying Execution:** Executing a function after a certain period of time has passed, which can be useful for animations, timed alerts, or scheduled tasks.

2. **Simulating Asynchronous Operations:** In scenarios where real asynchronous operations (like fetching data from a server) are not available, setTimeout can be used to mimic asynchronous behavior.

3. **Debouncing and Throttling:** Controlling the frequency of function calls in response to events, such as user input, to improve performance or prevent excessive resource consumption.

> Note : In a code whenever setTimeout is used, it get registred in Browser Web API's without blocking main thread of the code. In the call Stack when global execution context is finised event-loop will push into call stack from CallBack Queue.


## 10. What is the purpose of the "use strict" directive in JavaScript?

> Prevent Use of Undeclared variables

> To avoid syantax error

> To avoid dublicate Parametes in a function call


[Watch for More Understanding](https://www.youtube.com/watch?v=FiAKLb4qrvo)

## 11. Explain the differences between "shallow copy" and "deep copy" in JavaScript

> In Js, variables are copied by values and objects are by refrence

```JS
let person = {
    name : 'Utkarsh',
    age : 24,
    address : {
        locality : 'Pune'
    }
}
// This is shalllow copy, nested fields are mutable
let p1 = {...person}  

// This is deep copy, but if objects contains functions that will not be reflected. For Deep copy we can use lodash library.
let p2 = JSON.parse(JOSON.stringify(person)) 

// Recursive Function to Make Deep copy of Array/Object

function deepCopy(obj) {
    let result = Array.isArray(obj) ? [] : {};
    for (let key in obj) {
        if (typeof obj[key] === 'object' && obj[key] !== null) {
            result[key] = deepCopy(obj[key]);
        } else {
            result[key] = obj[key];
        }
    }
    return result;
}

let originalArray = [1, 2, [3, 4]];

// Deep copy using a custom function
let deepCopyArray = deepCopy(originalArray);

deepCopyArray[2][0] = 5; // This will not affect originalArray
console.log(originalArray); // Output: [1, 2, [3, 4]]

```
## 11. Explain Event Bubbling and Event Capturing/Trekeling ? 

[1. Watch for More Understanding - Akshay Saini](https://www.youtube.com/watch?v=aVSf0b1jVKk)

[2. Watch for More Understanding - Event Bubbling](https://www.youtube.com/watch?v=4XF1IVAH_dI)

[3. Watch for More Understanding - RoadSide Coder](https://www.youtube.com/watch?v=rS_4YfbEo2U&list=PLKhlp2qtUcSaCVJEt4ogEFs6I41pNnMU5&index=12)


### Event Delegation 
>Event delegation is a technique used in JavaScript to handle events efficiently, especially when dealing with a large number of elements. Instead of attaching event handlers to individual elements, event delegation involves attaching a single event handler to a parent element that listens for events bubbling up from its descendants. This way, you can handle events for multiple elements with less memory usage and better performance.


[Watch for More Understanding - Event Bubbling](https://www.youtube.com/watch?v=3KJI1WZGDrg)

## 13. Purpose of new Keyword In JS
If a function is called with new Keyword
> Creates an Empty Object

> Empty objects get assiged to this (this == {})

> Return object from the function automatically

## 13. Promises

```JS
console.log('Start');

function subscribe(msg) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(msg);
    }, 1000);
  });
}
function likeVideo(msg, cb) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject('Failed');
    }, 1000);
  });
}
function shareVideo(msg, cb) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(msg);
    }, 100);
  });
}
// Promise Combinators
// 1. Promise all
   // In this if one of the promise get rejeceted, then all of them are rejected
Promise.all([subscribe('subscribe my Channel'),
likeVideo('Like my Video'),
shareVideo('Share my Video')]).then((res)=>console.log(res)).catch((err)=>console.error(err))

// 2. Promise race
    // In this we get that promise that get rejected or   resolved first
    
Promise.race([subscribe('subscribe my Channel'),
likeVideo('Like my Video'),
shareVideo('Share my Video')]).then((res)=>console.log(res)).catch((err)=>console.error(err))

// 3. Promise allSettled
    // In this we get that all promise's either they are resolved or rejected
    
Promise.allSettled([subscribe('subscribe my Channel'),
likeVideo('Like my Video'),
shareVideo('Share my Video')]).then((res)=>console.log(res)).catch((err)=>console.error(err))

// 3. Promise any
    // In this we get the promise that gets fulfilled first. If all of the promises are rejected then we get reject error.
    
Promise.any([subscribe('subscribe my Channel'),
likeVideo('Like my Video'),
shareVideo('Share my Video')]).then((res)=>console.log(res)).catch((err)=>console.error(err))





// subscribe('Subscribe the video',function(msg){
//   console.log(msg)
//   likeVideo('Like my Video',function(msg){
//       console.log(msg)

//   })
// })

console.log('End');

```

