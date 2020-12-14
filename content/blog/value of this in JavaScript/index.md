---
title: Understanding "this"
date: "2020-12-13T22:40:32.169Z"
description: This blog will explain about different values of "this" keyword depending on where it is used in JavaScript code.
---

## "this" in JavaScript

One question which confuses every developer in JavaScript.
What is `this`?

Well there cannot be one specific quoted definiton or answer for the above question because the value of `this` is not fixed in JavaScript code.
It has different value based on where it is used in the code.

We will have a look at almost all the cases.

* `this` in global scope

```javascript
console.log(this)
```

The output of the above code will be the `window` object, because `this` in global scope points to the **global object** which is `window` in browser.

* `this` inside a function in global scope

```javascript
function f1(){
    console.log(this)
}

f1();
```

In a function the value of  `this` is determined by how a function is called (runtime binding). It can't be set by assignment during execution, 
ES5 features like `call`, `bind` and `apply` can be used to set value of `this`

Inside a function, the value of this depends on how the function is called.

Since the above code is not in strict mode, and because the value of `this` is not set by the `call`, `this` will default to the global object, which is `window` in a browser.

However in strict mode if the value of `this` is not set, it remains as `undefined`

```javascript
function f2() {
  'use strict'; // see strict mode
  return this;
}

f2() === undefined; // true
```

* `this` inside a method of an object

```javascript
var obj = {
    name : "objName",
    methodObj : function(){
        console.log(this)
    }
}

obj.methodObj();
```

One simple rule to know what a `this` is referring when called with object as a object method

The value of `this` is the object “before dot”, the one used to call the method.

* `this` inside a function which is inside a method of an object.

```javascript
var obj1 = {
    name: "objName1",
    methodObj1 : function(){
        function nestedFunc(){
            console.log(this)
        }
        nestedFunc();
    }
}

obj1.methodObj1();
```

The output of the above code will again be global object `window`,why?
because we call object method with the object so the value of `this` of object method is set, but the value of `this` for the nested function is not set so in that case it points to the **global object**.

This is a problem, to solve this problem we have got two solutions

```javascript
var obj2 = {
    name: "objName2",
    methodObj2 : function(){
        // the value of this is correct till this point
        var that = this;
        function nestedFunc(){
            console.log(that)
        }
        nestedFunc();
    }
}

obj2.methodObj2();
```
This solution can be considered as old school. This solution works by storing the correct value of `this` in a local function variable and using it inside the nested function

Another solution is using ES6 arrow functions inside the method of an object

```javascript
var obj3 = {
    name: "objName3",
    objMethod3 : function(){
        var nestedFunc = () => {
            console.log(this);
        }
        nestedFunc();
    }
}

obj3.objMethod3();
```

This solution works perfectly. Again why?

It's because ES6 arrow function have no value of `this` of their own. They take value of `this` from their outer lexical scope. In simple words the value of `this` for it's parent is copied by the arrow funtion.

But for this solution to work the outer function should be a "normal" function. If we make the outer function which is the object methoad also as a arrow function then the value of `this` will be set to `window` as the method will take the value from the outer context which is the object which lies in global context so again it will be `window`.


Phew..... this was the story of `this`, which is not over yet 😅 we have not discussed Classes yet.

Well that deserves a separate blog post. 

Will discuss in coming blog.....


