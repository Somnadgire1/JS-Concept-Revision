# JS-Concept-Revision
1. What are the primitive data types in JS?
Number
String
Boolean
Undefined
Null
Symbol
BigInt

2. What's the difference between a variable that is: null, undefined or undeclared?
a)undefined is a variable that has been declared but no value exists and is a type of itself ‘undefined’.
b)null is a value of a variable and is a type of object.
c)undeclared variables is a variable that has been declared without ‘var’ keyword.
testVar = ‘hello world’;
as opposed to
var testVar = ‘hello world’;
When former code is executed, undeclared variables are created as global variable and they are configurable (ex. can be deleted).
We use ‘console.log();’ and ‘type of’ to check if a variable is undefined or null.

3. What is the difference between while and do-while loops in JavaScript?
while loop: If the condition of a loop is always runs on condition, the loop runs for condition statement times(until memory is full).
for example-
while(<condition>){
//code
}
do-while loop: It is similar to the while loop.The only difference is that do..while loop, the cod of loop is executed at least Ones.
for example-
do{
console.log("hi");
} while(<condition>);

4. What language constructions do you use for iterating over object properties and array items?
Object Properties:
for...in statement

for (const property in obj) {
  console.log(property);
}
The for...in statement iterates over all the object's enumerable properties (including inherited enumerable properties). Hence most of the time you should should check whether the property exists on directly on the object via Object.hasOwn(object, property) before using it.

for (const property in obj) {
  if (Object.hasOwn(obj, property)) {
    console.log(property);
  }
}
obj.hasOwnProperty() is not recommended because it doesn't work for objects created using Object.create(null). It is recommended to use Object.hasOwn() in newer browsers, or use the good old Object.prototype.hasOwnProperty.call(object, key).

Object.keys()
Object.keys(obj).forEach((property) => {
  console.log(property);
});
Object.keys() is a static method that will return an array of all the enumerable property names of the object that you pass it. Since Object.keys() returns an array, you can also use the array iteration approaches listed below to iterate through it.

Reference: Object.keys() - JavaScript | MDN

Object.getOwnPropertyNames()
Object.getOwnPropertyNames(obj).forEach((property) => {
  console.log(property);
});
Object.getOwnPropertyNames() is a static method that will lists all enumerable and non-enumerable properties of the object that you pass it. Since Object.getOwnPropertyNames() returns an array, you can also use the array iteration approaches listed below to iterate through it.

Array Properties:
for loop
for (var i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
A common pitfall here is that var is in the function scope and not the block scope and most of the time you would want block scoped iterator variable. ES2015 introduces let which has block scope and it is recommended to use let over var.

for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
Array.prototype.forEach()
arr.forEach((element, index) => {
  console.log(element, index);
});
The Array.prototype.forEach() method can be more convenient at times if you do not need to use the index and all you need is the individual array elements. However, the downside is that you cannot stop the iteration halfway and the provided function will be executed on the elements once. A for loop or for...of statement is more relevant if you need finer control over the iteration.

Reference: Array.prototype.forEach() - JavaScript | MDN

for...of statement
for (let element of arr) {
  console.log(element);
}

5. What are the promises and how do they work?
Promises are used to handle asynchronous operations in JavaScript. They are easy to manage when dealing with multiple asynchronous operations where callbacks can create callback hell leading to unmanageable code. 
A Promise has four states:-
fulfilled: Action related to the promise succeeded
rejected: Action related to the promise failed
pending: Promise is still pending i.e. not fulfilled or rejected yet
settled: Promise has fulfilled or rejected
Parameters:-
Promise constructor takes only one argument which is a callback function (and that callback function is also referred as anonymous function too).
Callback function takes two arguments, resolve and reject
Perform operations inside the callback function and if everything went well then call resolve.
If desired operations do not go well then call reject.
for example:-
var promise = new Promise(function(resolve, reject){
const x = "Hi";
const y = "Hi";
if(x==y){
	resolve();
       } else {
	reject();
       }
      });
     promise.then(function(){
	console.log("success");
}).
             catch(function(){
	console.log("Error");
});

6. What are IIFEs and explain with an example where they can be used?

An immediately invoked function expression (IIFE for short) is a JavaScript design pattern that declares an anonymous function and immediately executes it.

// Prints "Hello, World!"
  1)(function() {
  console.log('Hello, World!');
  2)(() => {
  console.log('Hello, World!');
})();
In that two example function and arrow it is a IIFE.

7. Explain event delegation.
Event delegation is a technique involving adding event listeners to a parent element instead of adding them to the descendant elements. The listener will fire whenever the event is triggered on the descendant elements due to event bubbling up the DOM. The benefits of this technique are:

Memory footprint goes down because only one single handler is needed on the parent element, rather than having to attach event handlers on each descendant.
There is no need to unbind the handler from elements that are removed and to bind the event for new elements.

8. Explain how this works in JavaScript.
If the new keyword is used when calling the function, this inside the function is a brand new object.
If apply, call, or bind are used to call/create a function, this inside the function is the object that is passed in as the argument.
If a function is called as a method, such as obj.method() — this is the object that the function is a property of.
If a function is invoked as a free function invocation, meaning it was invoked without any of the conditions present above, this is the global object. In a browser, it is the window object. If in strict mode ('use strict'), this will be undefined instead of the global object.
If multiple of the above rules apply, the rule that is higher wins and will set the this value.
If the function is an ES2015 arrow function, it ignores all the rules above and receives the this value of its surrounding scope at the time it is created.

a.Can you give an example of one of the ways that working with this has changed in ES6?
ES6 allows you to use arrow functions which uses the enclosing lexical scope. This is usually convenient, but does prevent the caller from controlling context via .call or .apply—the consequences being that a library such as jQuery will not properly bind this in your event handler functions. Thus, it's important to keep this in mind when refactoring large legacy applications.

9. Explain how prototypal inheritance works.
This is an extremely common JavaScript interview question. All JavaScript objects have a __proto__ property with the exception of objects created with Object.create(null), that is a reference to another object, which is called the object's "prototype". When a property is accessed on an object and if the property is not found on that object, the JavaScript engine looks at the object's __proto__, and the __proto__'s __proto__ and so on, until it finds the property defined on one of the __proto__s or until it reaches the end of the prototype chain. This behavior simulates classical inheritance, but it is really more of delegation than inheritance.
Example-
function Parent() {
  this.name = 'Parent';
}

Parent.prototype.greet = function () {
  console.log('Hello from ' + this.name);
};

function Child() {
  Parent.call(this);
  this.name = 'Child';
}

Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;

const child = new Child();

child.greet();
// hello from Child

child.constructor.name;
// 'Child'

10. What is a closure, and how/why would you use one?
A closure is the combination of a function and the lexical environment within which that function was declared. The word "lexical" refers to the fact that lexical scoping uses the location where a variable is declared within the source code to determine where that variable is available. Closures are functions that have access to the outer (enclosing) function's variables—scope chain even after the outer function has returned.

Why would you use one?
Data privacy / emulating private methods with closures. Commonly used in the module pattern.
Partial applications or currying.

11. Can you describe the main difference between the Array.forEach() loop and Array.map() methods and why you would pick one versus the other?
forEach

Iterates through the elements in an array.
Executes a callback for each element.
Does not return a value.
const a = [1, 2, 3];
const doubled = a.forEach((num, index) => {
  // Do something with num and/or index.
});

// doubled = undefined

map

Iterates through the elements in an array.
"Maps" each element to a new element by calling the function on each element, creating a new array as a result.
const a = [1, 2, 3];
const doubled = a.map((num) => {
  return num * 2;
});

// doubled = [2, 4, 6]

The main difference between .forEach and .map() is that .map() returns a new array. If you need the result, but do not wish to mutate the original array, .map() is the clear choice. If you simply need to iterate over an array, forEach is a fine choice.

12. What's a typical use case for anonymous functions?
They can be used in IIFEs to encapsulate some code within a local scope so that variables declared in it do not leak to the global scope.

(function () {
  // Some code here.
})();

As a callback that is used once and does not need to be used anywhere else. The code will seem more self-contained and readable when handlers are defined right inside the code calling them, rather than having to search elsewhere to find the function body.

setTimeout(function () {
  console.log('Hello world!');
}, 1000);

Arguments to functional programming constructs or Lodash (similar to callbacks).

const arr = [1, 2, 3];
const double = arr.map(function (el) {
  return el * 2;
});
console.log(double); // [2, 4, 6]

13. What's the difference between host objects and native objects?
Native objects are objects that are part of the JavaScript language defined by the ECMAScript specification, such as String, Math, RegExp, Object, Function, etc.
Host objects are provided by the runtime environment (browser or Node), such as window, XMLHTTPRequest, etc.

14. Explain the difference between: function Person(){}, var person = Person(), and var person = new Person()?
function Person(){} is just a normal function declaration. The convention is to use PascalCase for functions that are intended to be used as constructors.

var person = Person() invokes the Person as a function, and not as a constructor. Invoking as such is a common mistake if the function is intended to be used as a constructor. Typically, the constructor does not return anything, hence invoking the constructor like a normal function will return undefined and that gets assigned to the variable intended as the instance.

var person = new Person() creates an instance of the Person object using the new operator, which inherits from Person.prototype. An alternative would be to use Object.create, such as: Object.create(Person.prototype).

function Person(name) {
  this.name = name;
}

var person = Person('John');
console.log(person); // undefined
console.log(person.name); // Uncaught TypeError: Cannot read property 'name' of undefined

var person = new Person('John');
console.log(person); // Person { name: "John" }
console.log(person.name); // "john"

15. Explain the differences on the usage of foo between function foo() {} and var foo = function() {}
The former is a function declaration while the latter is a function expression. The key difference is that function declarations have its body hoisted but the bodies of function expressions are not (they have the same hoisting behavior as variables). For more explanation on hoisting, refer to the question above on hoisting. If you try to invoke a function expression before it is defined, you will get an Uncaught TypeError: XXX is not a function error.

Function Declaration

foo(); // 'FOOOOO'
function foo() {
  console.log('FOOOOO');
}

Function Expression

foo(); // Uncaught TypeError: foo is not a function
var foo = function () {
  console.log('FOOOOO');
};

16. Can you explain what Function.call and Function.apply do? What's the notable difference between the two?
function - A function is a set of statements that take inputs, do some specific computation, and produce output. The idea is to put some commonly or repeatedly done tasks together and make a function so that instead of writing the same code again and again for different inputs, we can call that function.
call and apply - The difference is that apply lets you invoke the function with arguments as an array; call requires the parameters be listed explicitly.
Pseudo syntax:
theFunction.apply(valueForThis, arrayOfArgs)

theFunction.call(valueForThis, arg1, arg2, ...)

17. Explain Function.prototype.bind.
The JavaScript Function bind() method is used to create a new function. When a function is called, it has its own this keyword set to the provided value, with a given sequence of arguments.
Syntax:
function.bind(thisArg [, arg1[, arg2[, ...]]]  
Parameter
thisArg - The this value passed to the target function.
arg1,arg2,....,argn - It represents the arguments for the function.
Return Value:
It returns the replica of the given function along provided this value and initial arguments.
Example: 
var website = {  
  name: "Javatpoint",  
  getName: function() {  
    return this.name;  
  }  
}  
var unboundGetName = website.getName;  
var boundGetName = unboundGetName.bind(website);  
document.writeln(boundGetName());  

18. What's the difference between feature detection, feature inference, and using the UA string?
Feature Detection:-
Feature detection is just a way of determining if a feature exists in certain browsers. A good example is a modern HTML5 feature ‘Location’.

if (navigator.geolocation) {
// detect users location here B-) and do something awesome
}
Feature Inference:-
Feature Inference is when you have determined a feature exists and assumed the next web technology feature you are implementing unto your app exists as well. Its usually bad practice to assume, so its better to explicitly specify features you want to detect and plan a fallback action.
UA String:-
UA String or User Agent String is a string text of data that each browsers send and can be access via navigator.userAgent. These “string text of data” contains information of the browser environment you are targeting.
If you open your console and run:
navigator.userAgent
19. Explain "hoisting".
Hoisting is a term used to explain the behavior of variable declarations in your code. Variables declared or initialized with the var keyword will have their declaration "moved" up to the top of their module/function-level scope, which we refer to as hoisting. However, only the declaration is hoisted, the assignment (if there is one), will stay where it is.

Note that the declaration is not actually moved - the JavaScript engine parses the declarations during compilation and becomes aware of declarations and their scopes. It is just easier to understand this behavior by visualizing the declarations as being hoisted to the top of their scope. Let's explain with a few examples.

console.log(foo); // undefined
var foo = 1;
console.log(foo); // 1

Function declarations have the body hoisted while the function expressions (written in the form of variable declarations) only has the variable declaration hoisted.

// Function Declaration
console.log(foo); // [Function: foo]
foo(); // 'FOOOOO'
function foo() {
  console.log('FOOOOO');
}
console.log(foo); // [Function: foo]

// Function Expression
console.log(bar); // undefined
bar(); // Uncaught TypeError: bar is not a function
var bar = function () {
  console.log('BARRRR');
};
console.log(bar); // [Function: bar]

Variables declared via let and const are hoisted as well. However, unlike var and function, they are not initialized and accessing them before the declaration will result in a ReferenceError exception. The variable is in a "temporal dead zone" from the start of the block until the declaration is processed.

x; // undefined
y; // Reference error: y is not defined

var x = 'local';
let y = 'local';

20. Describe event bubbling.
When an event triggers on a DOM element, it will attempt to handle the event if there is a listener attached, then the event is bubbled up to its parent and the same thing happens. This bubbling occurs up the element's ancestors all the way to the document. Event bubbling is the mechanism behind event delegation.

21. Describe event capturing.
An alternative form of event propagation is event capture. This is like event bubbling but the order is reversed: so instead of the event firing first on the innermost element targeted, and then on successively less nested elements, the event fires first on the least nested element, and then on successively more nested elements, until the target is reached.

Event capture is disabled by default. To enable it you have to pass the capture option in addEventListener().
Example:
const output = document.querySelector('#output');
function handleClick(e) {
  output.textContent += `You clicked on a ${e.currentTarget.tagName} element\n`;
}

const container = document.querySelector('#container');
const button = document.querySelector('button');

document.body.addEventListener('click', handleClick, { capture: true });
container.addEventListener('click', handleClick, { capture: true });
button.addEventListener('click', handleClick);

22. What's the difference between an "attribute" and a "property"?
Attributes are defined on the HTML markup but properties are defined on the DOM. To illustrate the difference, imagine we have this text field in our HTML: <input type="text" value="Hello">.

const input = document.querySelector('input');
console.log(input.getAttribute('value')); // Hello
console.log(input.value); // Hello

But after you change the value of the text field by adding "World!" to it, this becomes:

console.log(input.getAttribute('value')); // Hello
console.log(input.value); // Hello World!

23. What are the pros and cons of extending built-in JavaScript objects?
Extending a built-in/native JavaScript object means adding properties/functions to its prototype. While this may seem like a good idea at first, it is dangerous in practice. Imagine your code uses a few libraries that both extend the Array.prototype by adding the same contains method, the implementations will overwrite each other and your code will break if the behavior of these two methods is not the same.

The only time you may want to extend a native object is when you want to create a polyfill, essentially providing your own implementation for a method that is part of the JavaScript specification but might not exist in the user's browser due to it being an older browser.

24. What is the difference between == and ===?
== is the abstract equality operator while === is the strict equality operator. The == operator will compare for equality after doing any necessary type conversions. The === operator will not do type conversion, so if two values are not the same type === will simply return false. When using ==, funky things can happen, such as:

1 == '1'; // true
1 == [1]; // true
1 == true; // true
0 == ''; // true
0 == '0'; // true
0 == false; // true

My advice is never to use the == operator, except for convenience when comparing against null or undefined, where a == null will return true if a is null or undefined.

var a = null;
console.log(a == null); // true
console.log(a == undefined); // true

25. Explain the same-origin policy with regards to JavaScript.
The same-origin policy prevents JavaScript from making requests across domain boundaries. An origin is defined as a combination of URI scheme, hostname, and port number. This policy prevents a malicious script on one page from obtaining access to sensitive data on another web page through that page's Document Object Model.
function duplicate(arr) {
  return arr.concat(arr);
}
example-
duplicate([1, 2, 3, 4, 5]); // [1,2,3,4,5,1,2,3,4,5]

Or with ES6:

const duplicate = (arr) => [...arr, ...arr];

duplicate([1, 2, 3, 4, 5]); // [1,2,3,4,5,1,2,3,4,5]

26. Why is it called a Ternary operator, what does the word "Ternary" indicate?
"Ternary" indicates three, and a ternary expression accepts three operands, the test condition, the "then" expression and the "else" expression. Ternary expressions are not specific to JavaScript and I'm not sure why it is even in this list.

27. What is strict mode? What are some of the advantages/disadvantages of using it?
'use strict' is a statement used to enable strict mode to entire scripts or individual functions. Strict mode is a way to opt into a restricted variant of JavaScript.
Advantages:
Makes it impossible to accidentally create global variables.
Makes assignments which would otherwise silently fail to throw an exception.
Makes attempts to delete undeletable properties throw an exception (where before the attempt would simply have no effect).
Requires that function parameter names be unique.
this is undefined in the global context.
It catches some common coding bloopers, throwing exceptions.
It disables features that are confusing or poorly thought out.

Disadvantages:
Many missing features that some developers might be used to.
No more access to function.caller and function.arguments.
Concatenation of scripts written in different strict modes might cause issues.

28. What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?
Advantages:

Fixes some of the longstanding problems in JavaScript and discourages JavaScript anti-patterns.
Enables you to write shorter code, by providing some syntactic sugar on top of JavaScript, which I think ES5 lacks, but ES2015 is awesome.
Static types are awesome (in the case of TypeScript) for large projects that need to be maintained over time.

Disadvantages:
Require a build/compile process as browsers only run JavaScript and your code will need to be compiled into JavaScript before being served to browsers.
Debugging can be a pain if your source maps do not map nicely to your pre-compiled source.
Most developers are not familiar with these languages and will need to learn it. There's a ramp up cost involved for your team if you use it for your projects.
Smaller community (depends on the language), which means resources, tutorials, libraries, and tooling would be harder to find.
IDE/editor support might be lacking.
These languages will always be behind the latest JavaScript standard.
Developers should be cognizant of what their code is being compiled to — because that is what would actually be running, and that is what matters in the end.

29. What tools and techniques do you use debugging JavaScript code?
a)React and Redux
i)React Devtools
ii)Redux Devtools
b)Vue
i)Vue Devtools
c)JavaScript
i)Chrome Devtools
ii)debugger statement
iii)Good old console.log debugging

30. Explain the difference between mutable and immutable objects.
Immutability is a core principle in functional programming, and has lots to offer to object-oriented programs as well. A mutable object is an object whose state can be modified after it is created. An immutable object is an object whose state cannot be modified after it is created.
a. What is an example of an immutable object in JavaScript?
In JavaScript, some built-in types (numbers, strings) are immutable, but custom objects are generally mutable.
Some built-in immutable JavaScript objects are Math, Date.
b. What are the pros and cons of immutability?
Pros-
Easier change detection - Object equality can be determined in a performant and easy manner through referential equality. This is useful for comparing object differences in React and Redux.
Programs with immutable objects are less complicated to think about, since you don't need to worry about how an object may evolve over time.
Defensive copies are no longer necessary when immutable objects are returning from or passed to functions, since there is no possibility an immutable object will be modified by it.
Easy sharing via references - One copy of an object is just as good as another, so you can cache objects or reuse the same object multiple times.
Thread-safe - Immutable objects can be safely used between threads in a multi-threaded environment since there is no risk of them being modified in other concurrently running threads.
Using libraries like ImmutableJS, objects are modified using structural sharing and less memory is needed for having multiple objects with similar structures.
Cons-
Naive implementations of immutable data structures and its operations can result in extremely poor performance because new objects are created each time. It is recommended to use libraries for efficient immutable data structures and operations that leverage on structural sharing.
Allocation (and deallocation) of many small objects rather than modifying existing ones can cause a performance impact. The complexity of either the allocator or the garbage collector usually depends on the number of objects on the heap.
Cyclic data structures such as graphs are difficult to build. If you have two objects which can't be modified after initialization, how can you get them to point to each other?

c. How can you achieve immutability in your own code?
The alternative is to use const declarations combined with the techniques mentioned above for creation. For "mutating" objects, use the spread operator, Object.assign, Array.concat(), etc., to create new objects instead of mutate the original object.

Examples:

// Array Example
const arr = [1, 2, 3];
const newArr = [...arr, 4]; // [1, 2, 3, 4]

// Object Example
const human = Object.freeze({race: 'human'});
const john = {...human, name: 'John'}; // {race: "human", name: "John"}
const alienJohn = {...john, race: 'alien'}; // {race: "alien", name: "John"}

31. Explain the difference between synchronous and asynchronous functions.
Synchronous functions are blocking while asynchronous functions are not. In synchronous functions, statements complete before the next statement is run. In this case, the program is evaluated exactly in order of the statements and execution of the program is paused if one of the statements take a very long time.

Asynchronous functions usually accept a callback as a parameter and execution continue on the next line immediately after the asynchronous function is invoked. The callback is only invoked when the asynchronous operation is complete and the call stack is empty. Heavy duty operations such as loading data from a web server or querying a database should be done asynchronously so that the main thread can continue executing other operations instead of blocking until that long operation to complete (in the case of browsers, the UI will freeze).

32. What is an event loop?
The event loop is a single-threaded loop that monitors the call stack and checks if there is any work to be done in the task queue. If the call stack is empty and there are callback functions in the task queue, a function is dequeued and pushed onto the call stack to be executed.
a. What is the difference between call stack and task queue?

33. What are the differences between variables created using let, var or const?
Variables declared using the var keyword are scoped to the function in which they are created, or if created outside of any function, to the global object. let and const are block scoped, meaning they are only accessible within the nearest set of curly braces (function, if-else block, or for-loop).

function foo() {
  // All variables are accessible within functions.
  var bar = 'bar';
  let baz = 'baz';
  const qux = 'qux';

  console.log(bar); // bar
  console.log(baz); // baz
  console.log(qux); // qux
}

console.log(bar); // ReferenceError: bar is not defined
console.log(baz); // ReferenceError: baz is not defined
console.log(qux); // ReferenceError: qux is not defined

var allows variables to be hoisted, meaning they can be referenced in code before they are declared. let and const will not allow this, instead throwing an error.

console.log(foo); // undefined

var foo = 'foo';

console.log(baz); // ReferenceError: can't access lexical declaration 'baz' before initialization

let baz = 'baz';

console.log(bar); // ReferenceError: can't access lexical declaration 'bar' before initialization

const bar = 'bar';


Redeclaring a variable with var will not throw an error, but let and const will.

var foo = 'foo';
var foo = 'bar';
console.log(foo); // "bar"

let baz = 'baz';
let baz = 'qux'; // Uncaught SyntaxError: Identifier 'baz' has already been declared
let and const differ in that let allows reassigning the variable's value while const does not.

// This is fine.
let foo = 'foo';
foo = 'bar';

// This causes an exception.
const baz = 'baz';
baz = 'qux';

34. What are the differences between ES6 class and ES5 function constructors?
The main difference in the constructor comes when using inheritance. If we want to create a Student class that subclasses Person and add a studentId field, this is what we have to do in addition to the above.
// ES5 Function Constructor
function Student(name, studentId) {
  // Call constructor of superclass to initialize superclass-derived members.
  Person.call(this, name);

  // Initialize subclass's own members.
  this.studentId = studentId;
}

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;

// ES6 Class
class Student extends Person {
  constructor(name, studentId) {
    super(name);
    this.studentId = studentId;
  }
}

35. Can you offer a use case for the new arrow => function syntax? How does this new syntax differ from other functions?
One obvious benefit of arrow functions is to simplify the syntax needed to create functions, without a need for the function keyword. The this within arrow functions is also bound to the enclosing scope which is different compared to regular functions where the this is determined by the object calling it. Lexically-scoped this is useful when invoking callbacks especially in React components.

36. What advantage is there for using the arrow syntax for a method in a constructor?
The main advantage of using an arrow function as a method inside a constructor is that the value of this gets set at the time of the function creation and can't change after that. So, when the constructor is used to create a new object, this will always refer to that object. For example, let's say we have a Person constructor that takes a first name as an argument has two methods to console.log that name, one as a regular function and one as an arrow function:
const Person = function (firstName) {
  this.firstName = firstName;
  this.sayName1 = function () {
    console.log(this.firstName);
  };
  this.sayName2 = () => {
    console.log(this.firstName);
  };
};

const john = new Person('John');
const dave = new Person('Dave');

john.sayName1(); // John
john.sayName2(); // John

37. What is the definition of a higher-order function?
A higher-order function is any function that takes one or more functions as arguments, which it uses to operate on some data, and/or returns a function as a result. Higher-order functions are meant to abstract some operation that is performed repeatedly. The classic example of this is map, which takes an array and a function as arguments. map then uses this function to transform each item in the array, returning a new array with the transformed data. Other popular examples in JavaScript are forEach, filter, and reduce. A higher-order function doesn't just need to be manipulating arrays as there are many use cases for returning a function from another function. Function.prototype.bind is one such example in JavaScript.

Map

Let say we have an array of names which we need to transform each string to uppercase.

const names = ['irish', 'daisy', 'anna'];

The imperative way will be as such:

const transformNamesToUppercase = function (names) {
  const results = [];
  for (let i = 0; i < names.length; i++) {
    results.push(names[i].toUpperCase());
  }
  return results;
};
transformNamesToUppercase(names); // ['IRISH', 'DAISY', 'ANNA']
Use .map(transformerFn) makes the code shorter and more declarative.
const transformNamesToUppercase = function (names) {
  return names.map((name) => name.toUpperCase());
};
transformNamesToUppercase(names); // ['IRISH', 'DAISY', 'ANNA']

38. Can you give an example for destructuring an object or an array?
Array destructuring

// Variable assignment.
const foo = ['one', 'two', 'three'];

const [one, two, three] = foo;
console.log(one); // "one"
console.log(two); // "two"
console.log(three); // "three"

// Swapping variables
let a = 1;
let b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1

Object destructuring

// Variable assignment.
const o = {p: 42, q: true};
const {p, q} = o;

console.log(p); // 42
console.log(q); // true

39. Can you give an example of generating a string with ES6 Template Literals?
Template literals help make it simple to do string interpolation, or to include variables in a string. Before ES2015, it was common to do something like this:

var person = {name: 'Tyler', age: 28};
console.log(
  'Hi, my name is ' + person.name + ' and I am ' + person.age + ' years old!',
);
// 'Hi, my name is Tyler and I am 28 years old!'

With template literals, you can now create that same output like this instead:

const person = {name: 'Tyler', age: 28};
console.log(`Hi, my name is ${person.name} and I am ${person.age} years old!`);
// 'Hi, my name is Tyler and I am 28 years old!'

Note that you use backticks, not quotes, to indicate that you are using a template literal and that you can insert expressions inside the ${} placeholders.

A second helpful use case is in creating multi-line strings. Before ES2015, you could create a multi-line string like this:

console.log('This is line one.\nThis is line two.');
// This is line one.
// This is line two.

Or if you wanted to break it up into multiple lines in your code so you didn't have to scroll to the right in your text editor to read a long string, you could also write it like this:

console.log('This is line one.\n' + 'This is line two.');
// This is line one.
// This is line two.

Template literals, however, preserve whatever spacing you add to them. For example, to create that same multi-line output that we created above, you can simply do:

console.log(`This is line one.
This is line two.`);
// This is line one.
// This is line two.

Another use case of template literals would be to use as a substitute for templating libraries for simple variable interpolations:

const person = {name: 'Tyler', age: 28};
document.body.innerHTML = `
  <div>
    <p>Name: ${person.name}</p>
    <p>Age: ${person.age}</p>
  </div>
`;

40. Can you give an example of a curry function and why this syntax offers an advantage?
Currying is a pattern where a function with more than one parameter is broken into multiple functions that, when called in series, will accumulate all of the required parameters one at a time. This technique can be useful for making code written in a functional style easier to read and compose. It's important to note that for a function to be curried, it needs to start out as one function, then broken out into a sequence of functions that each accepts one parameter.

function curry(fn) {
  if (fn.length === 0) {
    return fn;
  }

  function _curried(depth, args) {
    return function (newArgument) {
      if (depth - 1 === 0) {
        return fn(...args, newArgument);
      }
      return _curried(depth - 1, [...args, newArgument]);
    };
  }

  return _curried(fn.length, []);
}

function add(a, b) {
  return a + b;
}

var curriedAdd = curry(add);
var addFive = curriedAdd(5);

var result = [0, 1, 2, 3, 4, 5].map(addFive); // [5, 6, 7, 8, 9, 10]

41. What are the benefits of using spread syntax and how is it different from rest syntax?
ES6's spread syntax is very useful when coding in a functional paradigm as we can easily create copies of arrays or objects without resorting to Object.create, slice, or a library function. This language feature is used often in Redux and RxJS projects.

function putDookieInAnyArray(arr) {
  return [...arr, 'dookie'];
}

const result = putDookieInAnyArray(['I', 'really', "don't", 'like']); // ["I", "really", "don't", "like", "dookie"]

const person = {
  name: 'Todd',
  age: 29,
};

const copyOfTodd = {...person};


ES6's rest syntax offers a shorthand for including an arbitrary number of arguments to be passed to a function. It is like an inverse of the spread syntax, taking data and stuffing it into an array rather than unpacking an array of data, and it works in function arguments, as well as in array and object destructuring assignments.

function addFiveToABunchOfNumbers(...numbers) {
  return numbers.map((x) => x + 5);
}

const result = addFiveToABunchOfNumbers(4, 5, 6, 7, 8, 9, 10); // [9, 10, 11, 12, 13, 14, 15]

const [a, b, ...rest] = [1, 2, 3, 4]; // a: 1, b: 2, rest: [3, 4]

const {e, f, ...others} = {
  e: 1,
  f: 2,
  g: 3,
  h: 4,
}; // e: 1, f: 2, others: { g: 3, h: 4 }

42. How can you share code between files?
This depends on the JavaScript environment.

On the client (browser environment), as long as the variables/functions are declared in the global scope (window), all scripts can refer to them. Alternatively, adopt the Asynchronous Module Definition (AMD) via RequireJS for a more modular approach.

On the server (Node.js), the common way has been to use CommonJS. Each file is treated as a module and it can export variables and functions by attaching them to the module.exports object.

ES2015 defines a module syntax which aims to replace both AMD and CommonJS. This will eventually be supported in both browser and Node environments.

43. Why you might want to create static class members?
Static class members (properties/methods) are not tied to a specific instance of a class and have the same value regardless of which instance is referring to it. Static properties are typically configuration variables and static methods are usually pure utility functions which do not depend on the state of the instance.