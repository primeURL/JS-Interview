# JS-Interview

[1. What is map, filter and reduce](#1-what-is-map-filter-and-reduce)

[2. What is Closures ?](#2-what-is-closures)


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




