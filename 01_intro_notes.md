## V8
V8 is designed so we can embed it in other C++ programs, and therefore make features (things we can do in C++) available to Javascript. So we are able to invoke or call these features through Javascript. This ability to add features is essentially what nodeJS is.

NodeJS is a C++ program with V8 embedded that adds a wealth of features that make it suitable to be a server technology. As a server technology, it is designed to be able to use JavaScript to write server code.


## Client-server Model of Computing
Server = Computer that performs services. It performs jobs that are requested of it.

Client = asks for the server or requests work from the server and the server responds (provides data, error messages, or other things).

Requests and responses are written (programmed) in a standard format that the client and server understands. The client and server are often two computers but could be one.

The internet model consists of the browser as the client and a web server. The standard format for making requests and responses on the internet is HTTP.

Google Chrome is built with C++ and it embeds V8. Features that don't exist in javascript but are added on by the browser (and can be used through JS) is already happening. Things like editing dom elements (dom manipulation) and ajax using http request object are outside the ecmascript specification. These features are made available by the browser.

The goal with node is to use javascript on the server to perform tasks ussually requiring an additional language. Just like on the browser we can add features on to javascript for server work.

While this is not the only reason nodejs exists, one great benefit is that you can share libraries of code between the browser and webserver.

-----

The construction of Node.js means we are trying to asks one important question?

"What does javascript need to manage a server?"

Here are some of the features that we need to have accessible in javascript in order to do the kinds of things that a webserver normally needs to do.

* NodeJS provides better ways to organize code into reusable pieces (es6 also offers improvements but this came later).
* We need to have ways to handle files
* We need ways to deal with databases. It is the webserver that talks to the database.
* ability to communicate over the internet.
* ability yo accept and send response and requests (in a standard format)
* Some times work on the server takes time, we need a way to allow people to continue to access the webserver even when it's fullfilling other tasks. 

## What are the features that have been added to javascript by NodeJs and how can we use them? 
Here we are quickly summarizing what is in some of the Node.js source code. 

**Node core** = features and utilities built in c++

* dep dir = Looking at the core of nodeJs we will see the dependancies (C++ libraries) that are built into and are a part of node (e.g. v8, npm, zlib)

* scr dir = shows v8 is embedded. 

**JavaScript Core** = javascript code written to make using the C++ features easier

* lib dir = contains the javascript files. These are often wrappers for the C++ features.

* util.js = provides useful features/function written in js that are ready for you to use.

Keep in mind that Node.js is not javascript rather it accepts javascript and allows us to write more features with it.


# Install and Run Some Javascript in Node
Install node.js on nodejs.org

The download includes npm and allows you to work in the command line.

If you type `node` in the command then you enter a program that allows you to type javascript in the console and get the output back after it is processed by the v8 engine.

Esentially this is what we do going forward we send javascript through node to be processed.

**control + c** to quit. 

You run your node applications by locating the directory your js file is in and then running `node app.js` (node + filename).

`node` executes and process a single file but there are ways to run multiple pages. `node` will run `console.log`.

## Visual Studio Code
VSC is built for node and offers a debugger

While this may change some, if you select **launch app** and push the play button a settings file will be generated for you.

VSC debug video = https://www.youtube.com/watch?v=6cOsxaNC06c

## Module
Reusable block of code whose existence does not accidentally impact other code. Js did not have this in an official way before es6. 

Node.js implements something called **commonJS modules** 
(this is an agreed upon standard for how code modules should be structured).

---
### Aside passing functions to functions
var greetme = function(fn){
  fn();
} //function expression

var hello = function(){
  console.log("hello")
};

greetMe(hello);

---

## Building Modules
We can add files to other files using node's `require` function. As discribed modules are designed to protect code from accidentally impacting other code. However, we can not run individual functions that exist within the module without using a special variable `exports`.

```
module.exports = greet; //exposes the greet function outside the file where it is defined

// In the file recieving the greet function we access the module function by creating a variable that will hold the module and that we can call.

var greet = require('./greet');
greet();
```

We can leave off the `.js` extention for require statement because node assumes we are requiring a js file.

## Object
An object is a collection of name/value pairs. A ma,e can be defined more than once but it can only have one value in a given context. A name can have a value that is another name value pair. An object sits in memory and points to other objects or values (primative, object "property", function "method").

An object literal is a format for creating an object that uses name value pairs seporated by commas and surrounded by curly braces.

we can use bracket notation when calling a value using a name. This is used when we create a name in qoutes that is more than one word.

```
greet['kip greet'](); // kip greet is the object name.
```

**Inheritance** one object gets access to the properties and methods of another object. JavaScript uses prototypal inheritance.

Every object has another object that it points to. This is the object's prototype. We also have access to the protoypes prototype in what is known as the prototype chain. Objects can also share the same prototype.

## Constructing Objects and Managing the Prototype Chain
By constructing we explicitly mean making an object that can be used as a prototype for other objects. The new way to do this and the old way:

* ES6 class keyword
* function constructors = a normal function used to create objects

With **function constructors** the `this` keyword points to a new empty object, and that object is returned from the function automatically. 

We do this with the `new` keyword. We use `new` then call a function. This executes the function 

```
function Person(firstname, lastname) {
  this.firstname = firstname;
  this.lastname = lastname;
}

Person.prototype.greet = function() {
  console.log(this.firstname + ' says "Hello"')
}

var john = new Person('John', 'Smith'); //the 'new' keyword turns 'this' into a new object and immediately returns it (i.e. john holds the new object)

john.greet();
```

`john` has a prototype and we access it using Person.prototype. Functions are special a type of object and .prototype is a built in property that is an object. We can access it and add to it. `Person.prototype` is not the prototype of `Person`, it is the prototype of any object created from `Person`.

To view the prototype you can console log `.__proto__`

## By Value and By Reference
* passing a primitive to a function creates a copy of the value. This means that if you alter the value it does not affect the original value.  
* passing a reference refers to passing an object to a function. When you do this you get a reference to the same object. This means that if you change the object in one place it is changed in the other.

Node.js takes advantage of pass by reference when using modules and require

## Scope
Scope = Where in code you have access to a particular variable or function (ie. function scope, global scope). IIFEs fake what a module does because it protects declarations inside it. IIFEs are also short hand instead of creating a function and the calling it.

```
// because 
var firstname = 'Jane';

(function (lastname) {

	var firstname = 'John';
	console.log(firstname);
	console.log(lastname);
	
}('Doe'));

console.log(firstname);
```

## Summary: How Modules Work Inside Node
**Require** is a function that you pass a path to.
**module.exports** is what the require function returns.

Modules work because your code is wrapped in a function expression (by node.js) that is given these things (the values we define for require or module.exports) as function parameters. For this reason when you are writing in node you always have access to these parameters  require, module, dirname, filename available to you. What you write is not the only thing given to V8. What you write is the body of a function.

## JSON
Standard for structuring data inspired by javascript object literals (names appear in "qoutes" functions can not be used). Javascript and Node is built to take JSON and convert it to a javascript object.

## Requiring Multiple Documents In One Module
Without the .js extention node will look for a js file if it does not find it, then it will look for matching directories and process the files within it.

When we require a json file we can immediately access the data through the variable we set to the required file.

## Module Patterns in Node
Sourcetree test