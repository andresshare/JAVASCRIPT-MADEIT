# FUNCTIONS 🔥

> function is a piece of program wrapped in a value

```
function calculateCircumference(radius) {
  return 2 * Math.PI * radius;
}

console.log(Math.PI);
// expected output: 3.141592653589793

//https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Math/PI
```

## JavaScript/Anonymous functions
An anonymous function is a function that was declared without any named identifier to refer to it


```
// returns the factorial of 10.
alert((function(n) {
  return !(n > 1)
    ? 1
    : arguments.callee(n - 1) * n;
})(10));

//https://en.wikibooks.org/wiki/JavaScript/Anonymous_functions

```

## ARROW FUNCTIONS

Are a more concise syntax for writing function expressions. They utilize a new token
Arrow functions make our code more concise, and simplify function scoping and the this keyword

```

// (param1, param2, paramN) => expression

// ES5
var multiplyES5 = function(x, y) {
  return x * y;
};

// ES6
const multiplyES6 = (x, y) => { return x * y };

//https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions

```

### Immediately Invoked Function Expression aka IIFE

An anonymous function is a function without a name. For e.g.

```
function () {
  let message = "I don't have a name";
  console.log(message);
}
```

#### Where can IIFE be used?
It can be used in scenarios where you need to run the function only once, 
like fetching some initial data, setting some configuration values, 
checking system status on startup etc.

> The beauty of IIFE is that it can only be invoked once.


### Closures

The closure is an inner function that has the access to the outer function’s variable. 
And it is a combination of function and lexical environment within which that function was declared.

The closure has three scope chains

1.    It has access to its own scope. For example(that variable that is declared between its curly brackets).
2.    It has access to its outer function ’s variables. For example(Closure parent function).
3.    And it has access to the global variables.


see [Example CLOUSURE](src="https://gist.github.com/andresshare/544090a7953774242fb3baa4fbba4c21.js")

see [Example CLOUSURETWO](src="https://gist.github.com/andresshare/7cfdfb2b512f337ec6006e07d313dd06.js")

we have defined a function createAdding(x), which takes a single parameter x and returns a new function. 

The function it returns takes a single parameter y and returns the sum of x and y.

The createAdding is a function factory — it creates functions which can add a specific value to their parameter. 
In the above example, we use our function factory to create two new functions — one that adds 5 to its argument, 
and one that adds 10.

adding5 and adding10 are both closures. They share the same function body definition but store different 
lexical environments.


* lexical environment: When a function is declared, the lexical environment for that function is 
  created to store the variables and functions that are declared within it. 
  The lexical environment for a function can be thought of as a map between the variable names and their values in memory. 
  So when I write.

 ```
 function foo() {
  var a = 10;
 }

 ```


# FUNCTIONAL PROGRAMING


> In Javascript, primitive data types (string, number, boolean, null and undefined) are passed by value,
  and objects are passed by reference. This means primitive data types are immutable, 
  and objects are mutable.


Functional programming is a paradigm that treats computation as the manipulation of value, and avoids changing state 
and mutable data.

In functional code, the output value of a function depends ONLY on the arguments that are passed to the function. 
In other words, calling a function f twice with the same value for an argument x produces the same result f(x) each time.



This is in contrast procedural and object-oriented programs, which can often depend on a local or global state, 
which may produce different results at different times when called with the same arguments but a different program state.

By eliminating such side effects, the functional program is made predictable, simple, easy-to-understand, 
loosely-coupled and extensible.

## PURE FUNCIONS

Here is a very strict definition of purity:

It returns the same result if given the same arguments (it is also referred as deterministic)

It does not cause any observable side effects

> It returns the same result if given the same arguments

Imagine we want to implement a function that calculates the area of a circle. 
An impure function would receive radius as the parameter, and then calculate radius * radius * PI:

```
let PI = 3.14;

const calculateArea = (radius) => radius * radius * PI;

calculateArea(10); // returns 314.0

```

Why is this an impure function? 

Simply because it uses a global object that was not passed as a parameter to the function.

Now imagine some mathematicians argue that the PI value is actually 42 and change the value of the global object.


Our impure function will now result in 10 * 10 * 42 = 4200. 
For the same parameter (radius = 10), we have a different result. Let's fix it!

```
let PI = 3.14;

const calculateArea = (radius, pi) => radius * radius * pi;

calculateArea(10, PI); // returns 314.0


```

TA-DA 🎉! Now we’ll always pass the PI value as a parameter to the function. 
So now we are just accessing parameters passed to the function. No external object.

    For the parameters radius = 10 & PI = 3.14, we will always have the same the result: 314.0
    For the parameters radius = 10 & PI = 42, we will always have the same the result: 4200


### Reading Files

If our function reads external files, it’s not a pure function — the file’s contents can change.

const charactersCounter = (text) => `Character count: ${text.length}`;

function analyzeFile(filename) {
  let fileContent = open(filename);
  return charactersCounter(fileContent);
}


### Random number generation

```
function yearEndEvaluation() {
  if (Math.random() > 0.5) {
    return "You get a raise!";
  } else {
    return "Better luck next year!";
  }
}
```


> It does not cause any observable side effects

Examples of observable side effects include modifying a global object or a parameter passed by reference.

Now we want to implement a function to receive an integer value and return the value increased by 1.


```
let counter = 1;

function increaseCounter(value) {
  counter = value + 1;
}

increaseCounter(counter);
console.log(counter); // 2

```

We have the counter value. Our impure function receives that value and re-assigns 
the counter with the value increased by 1.

Observation: mutability is discouraged in functional programming.

We are modifying the global object. But how would we make it pure? Just return the value increased by 1. Simple as that.

```
let counter = 1;

const increaseCounter = (value) => value + 1;

increaseCounter(counter); // 2
console.log(counter); // 1

```

See that our pure function increaseCounter returns 2, but the counter value is still the same. The function returns the 
incremented value without altering the value of the variable.

If we follow these two simple rules, it gets easier to understand our programs. 
Now every function is isolated and unable to impact other parts of our system.

Pure functions are stable, consistent, and predictable. Given the same parameters, 
pure functions will always return the same result. 
We don’t need to think of situations when the same parameter has different results — because it will never happen.

### Pure functions benefits

The code’s definitely easier to test. We don’t need to mock anything. 
So we can unit test pure functions with different contexts:

Given a parameter A → expect the function to return value B
Given a parameter C → expect the function to return value D

A simple example would be a function to receive a collection of numbers and expect it to increment 
each element of this collection.

```
let list = [1, 2, 3, 4, 5];

const incrementNumbers = (list) => list.map(number => number + 1);

```

We receive the numbers array, use map incrementing each number, and return a new list of incremented numbers.

```
incrementNumbers(list); // [2, 3, 4, 5, 6]}
``` 

For the input [1, 2, 3, 4, 5], the expected output would be [2, 3, 4, 5, 6].

# Immutability

> Unchanging over time or unable to be changed.


When data is immutable, its state cannot change after it’s created. If you want to change an immutable object, 
you can’t. Instead, you create a new object with the new value.

In Javascript we commonly use the for loop. This next for statement has some mutable variables.


```
var values = [1, 2, 3, 4, 5];
var sumOfValues = 0;

for (var i = 0; i < values.length; i++) {
  sumOfValues += values[i];
}

sumOfValues // 15

```

For each iteration, we are changing the i and the sumOfValue state. But how do we handle mutability in iteration? Recursion!

```
let list = [1, 2, 3, 4, 5];
let accumulator = 0;

function sum(list, accumulator) {
  if (list.length == 0) {
    return accumulator;
  }

  return sum(list.slice(1), accumulator + list[0]);
}

sum(list, accumulator); // 15
list; // [1, 2, 3, 4, 5]
accumulator; // 0

```



So here we have the sum function that receives a vector of numerical values. 
The function calls itself until we get the list empty (our recursion base case). 
For each "iteration" we will add the value to the total accumulator.

With recursion, we keep our variables immutable. The list and the accumulator 
variables are not changed. It keeps the same value.

> Observation: Yes! We can use reduce to implement this function. 
  We will cover this in the Higher Order Functions topic.

It is also very common to build up the final state of an object. 
Imagine we have a string, and we want to transform this string into a url slug.

In OOP in Ruby, we would create a class, let’s say, UrlSlugify. 
And this class will have a slugify! method to transform the string input into a url slug.

``` 
lass UrlSlugify
  attr_reader :text
  
  def initialize(text)
    @text = text
  end

  def slugify!
    text.downcase!
    text.strip!
    text.gsub!(' ', '-')
  end
end

UrlSlugify.new(' I will be a url slug   ').slugify! # "i-will-be-a-url-slug"

```

Beautiful! It’s implemented! Here we have imperative programming saying exactly 
what we want to do in each slugify process — first lower case, 
then remove useless white spaces and, finally, replace remaining white spaces with hyphens.

But we are mutating the input state in this process.

We can handle this mutation by doing function composition, or function chaining. 
In other words, the result of a function will be used as an input for the next function, 
without modifying the original input string.

``` 
const string = " I will be a url slug   ";

const slugify = string =>
  string
    .toLowerCase()
    .trim()
    .split(" ")
    .join("-");

slugify(string); // i-will-be-a-url-slug

```

Here we have:

    toLowerCase: converts the string to all lower case
    trim: removes whitespace from both ends of a string
    split and join: replaces all instances of match with replacement in a given string

We combine all these 4 functions and we can "slugify" our string.


# Referential transparency


Let’s implement a square function:

```
const square = (n) => n * n;

```


This pure function will always have the same output, given the same input.

```
square(2); // 4
square(2); // 4
square(2); // 4

```

Passing 2 as a parameter of the square function will always returns 4. So now we can replace the square(2) with 4. 
That's it! Our function is referentially transparent.


Basically, if a function consistently yields the same result for the same input, it is referentially transparent.


> pure functions + immutable data = referential transparency


With this concept, a cool thing we can do is to memoize the function. Imagine we have this function:

``` 
const sum = (a, b) => a + b;

```
And we call it with these parameters:

```
sum(3, sum(5, 8));

```
The sum(5, 8) equals 13. This function will always result in 13. So we can do this:

```
sum(3, 13);
``` 

And this expression will always result in 16. 
We can replace the entire expression with a numerical constant and memoize it.


# Functions as first-class entities


The idea of functions as first-class entities is that functions are also treated as values and used as data.

Functions as first-class entities can:

    refer to it from constants and variables
    pass it as a parameter to other functions
    return it as result from other functions

The idea is to treat functions as values and pass functions like data. This way we can combine different functions to create new functions with new behavior.

Imagine we have a function that sums two values and then doubles the value. Something like this:

```
const doubleSum = (a, b) => (a + b) * 2;
```

Now a function that subtracts values and the returns the double:

```
const doubleSubtraction = (a, b) => (a - b) * 2;
```

These functions have similar logic, but the difference is the operators functions. 
If we can treat functions as values and pass these as arguments, 
we can build a function that receives the operator function and use it inside our function. Let’s build it!

```
const sum = (a, b) => a + b;
const subtraction = (a, b) => a - b;

const doubleOperator = (f, a, b) => f(a, b) * 2;

doubleOperator(sum, 3, 1); // 8
doubleOperator(subtraction, 3, 1); // 4
```

Done! Now we have an f argument, and use it to process a and b. 
We passed the sum and subtraction functions to compose with the doubleOperator function and create a new behavior.

# Higher-order functions

When we talk about higher-order functions, we mean a function that either:

    takes one or more functions as arguments, or
    returns a function as its result

The doubleOperator function we implemented above is a higher-order function because it takes an operator function as an argument and uses it.

You’ve probably already heard about filter, map, and reduce. Let's take a look at these.


# Filter

Given a collection, we want to filter by an attribute. The filter function expects a true or false value to determine 
if the element should or should not be included in the result collection. 
Basically, if the callback expression is true, the filter function will include the element 
in the result collection. Otherwise, it will not.

A simple example is when we have a collection of integers and we want only the even numbers.

### Imperative approach


An imperative way to do it with Javascript is to:

    create an empty array evenNumbers
    iterate over the numbers array
    push the even numbers to the evenNumbers array



``` 
var numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
var evenNumbers = [];

for (var i = 0; i < numbers.length; i++) {
  if (numbers[i] % 2 == 0) {
    evenNumbers.push(numbers[i]);
  }
}

console.log(evenNumbers); // (6) [0, 2, 4, 6, 8, 10]
```


We can also use the filter higher order function to receive the even function, and return a list of even numbers:


```
const even = n => n % 2 == 0;
const listOfNumbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
listOfNumbers.filter(even); // [0, 2, 4, 6, 8, 10]
``` 

One interesting problem I solved on Hacker Rank FP Path was the Filter Array problem. 
The problem idea is to filter a given array of integers and output only those values that are less than a specified value X.

an imperative Javascript solution to this problem is something like:

```
var filterArray = function(x, coll) {
  var resultArray = [];

  for (var i = 0; i < coll.length; i++) {
    if (coll[i] < x) {
      resultArray.push(coll[i]);
    }
  }

  return resultArray;
}

console.log(filterArray(3, [10, 9, 8, 2, 7, 5, 1, 3, 0])); // (3) [2, 1, 0]

``` 

We say exactly what our function needs to do — iterate over the collection, 
compare the collection current item with x, and push this element to the resultArray if it pass the condition.

### Declarative approach

But we want a more declarative way to solve this problem, and using the filter higher order function as well.

A declarative Javascript solution would be something like this:

```
function smaller(number) {
  return number < this;
}

function filterArray(x, listOfNumbers) {
  return listOfNumbers.filter(smaller, x);
}

let numbers = [10, 9, 8, 2, 7, 5, 1, 3, 0];

filterArray(3, numbers); // [2, 1, 0]
```

Using this in the smaller function seems a bit strange in the first place, but is easy to understand.

this will be the second parameter in the filter function. In this case, 3 (the x) is represented by this. That's it.


We can also do this with maps. Imagine we have a map of people with their name and age.


```
let people = [
  { name: "TK", age: 26 },
  { name: "Kaio", age: 10 },
  { name: "Kazumi", age: 30 }
];

```
And we want to filter only people over a specified value of age, in this example people who are more than 21 years old.

``` 
const olderThan21 = person => person.age > 21;
const overAge = people => people.filter(olderThan21);
overAge(people); // [{ name: 'TK', age: 26 }, { name: 'Kazumi', age: 30 }]

```

Summary of code:

    we have a list of people (with name and age).
    we have a function olderThan21. In this case, for each person in people array, we want to access the age and see if it is older than 21.
    we filter all people based on this function.

# Map

The idea of map is to transform a collection.


> The map method transforms a collection by applying a function to all of its elements 
  and building a new collection from the returned values.


Let’s get the same people collection above. We don't want to filter by 
“over age” now. We just want a list of strings, something like TK is 26 years old. 
So the final string might be :name is :age years old where :name and :age are attributes 
from each element in the people collection.


In a imperative Javascript way, it would be:

```
var people = [
  { name: "TK", age: 26 },
  { name: "Kaio", age: 10 },
  { name: "Kazumi", age: 30 }
];

var peopleSentences = [];

for (var i = 0; i < people.length; i++) {
  var sentence = people[i].name + " is " + people[i].age + " years old";
  peopleSentences.push(sentence);
}

console.log(peopleSentences); // ['TK is 26 years old', 'Kaio is 10 years old', 'Kazumi is 30 years old']

```

In a declarative Javascript way, it would be:

```
const makeSentence = (person) => `${person.name} is ${person.age} years old`;

const peopleSentences = (people) => people.map(makeSentence);
  
peopleSentences(people);
// ['TK is 26 years old', 'Kaio is 10 years old', 'Kazumi is 30 years old']

```

The whole idea is to transform a given array into a new array.

Another interesting Hacker Rank problem was the update list problem. We just want to update the values of a given array with their absolute values.

For example, the input [1, 2, 3, -4, 5]needs the output to be [1, 2, 3, 4, 5]. The absolute value of -4 is 4.

A simple solution would be an in-place update for each collection value.


```
var values = [1, 2, 3, -4, 5];

for (var i = 0; i < values.length; i++) {
  values[i] = Math.abs(values[i]);
}

console.log(values); // [1, 2, 3, 4, 5]

```

We use the Math.abs function to transform the value into its absolute value, and do the in-place update.

This is not a functional way to implement this solution.

First, we learned about immutability. We know how immutability is important to make our functions more consistent and predictable. The idea is to build a new collection with all absolute values.

Second, why not use map here to "transform" all data?

My first idea was to test the Math.abs function to handle only one value.


```
Math.abs(-1); // 1
Math.abs(1); // 1
Math.abs(-2); // 2
Math.abs(2); // 2
``` 



We want to transform each value into a positive value (the absolute value).

Now that we know how to do absolute for one value, we can use this function 
to pass as an argument to the map function. Do you remember that a higher order 
function can receive a function as an argument and use it? Yes, map can do it!


```
let values = [1, 2, 3, -4, 5];

const updateListMap = (values) => values.map(Math.abs);

updateListMap(values); // [1, 2, 3, 4, 5]
```



Wow. So beautiful! 😍



# Reduce

The idea of reduce is to receive a function and a collection, and return a value created by combining the items.

A common example people talk about is to get the total amount of an order. Imagine you were at a shopping website. You’ve added Product 1, Product 2, Product 3, and Product 4 to your shopping cart (order). Now we want to calculate the total amount of the shopping cart.

In imperative way, we would iterate the order list and sum each product amount to the total amount.

```
var orders = [
  { productTitle: "Product 1", amount: 10 },
  { productTitle: "Product 2", amount: 30 },
  { productTitle: "Product 3", amount: 20 },
  { productTitle: "Product 4", amount: 60 }
];

var totalAmount = 0;

for (var i = 0; i < orders.length; i++) {
  totalAmount += orders[i].amount;
}

console.log(totalAmount); // 120
```

Using reduce, we can build a function to handle the amount sum and pass it as an argument to the reduce function.


```
let shoppingCart = [
  { productTitle: "Product 1", amount: 10 },
  { productTitle: "Product 2", amount: 30 },
  { productTitle: "Product 3", amount: 20 },
  { productTitle: "Product 4", amount: 60 }
];

const sumAmount = (currentTotalAmount, order) => currentTotalAmount + order.amount;

const getTotalAmount = (shoppingCart) => shoppingCart.reduce(sumAmount, 0);

getTotalAmount(shoppingCart); // 120
```


Here we have shoppingCart, the function sumAmount that receives the current currentTotalAmount , and the order object to sum them.

The getTotalAmount function is used to reduce the shoppingCart by using the sumAmount and starting from 0.

Another way to get the total amount is to compose map and reduce. What do I mean by that? We can use map to transform the shoppingCart into a collection of amount values, and then just use the reduce function with sumAmount function.

``` 
const getAmount = (order) => order.amount;
const sumAmount = (acc, amount) => acc + amount;

function getTotalAmount(shoppingCart) {
  return shoppingCart
    .map(getAmount)
    .reduce(sumAmount, 0);
}

getTotalAmount(shoppingCart); // 120

``` 

The getAmount receives the product object and returns only the amount value. So what we have here is [10, 30, 20, 60]. And then the reduce combines all items by adding up. Beautiful!

We took a look at how each higher order function works. I want to show you an example of how we can compose all three functions in a simple example.

Talking about shopping cart, imagine we have this list of products in our order:

```
let shoppingCart = [
  { productTitle: "Functional Programming", type: "books", amount: 10 },
  { productTitle: "Kindle", type: "eletronics", amount: 30 },
  { productTitle: "Shoes", type: "fashion", amount: 20 },
  { productTitle: "Clean Code", type: "books", amount: 60 }
]
```
We want the total amount of all books in our shopping cart. Simple as that. The algorithm?

    filter by book type
    transform the shopping cart into a collection of amount using map
    combine all items by adding them up with reduce


```
let shoppingCart = [
  { productTitle: "Functional Programming", type: "books", amount: 10 },
  { productTitle: "Kindle", type: "eletronics", amount: 30 },
  { productTitle: "Shoes", type: "fashion", amount: 20 },
  { productTitle: "Clean Code", type: "books", amount: 60 }
]

const byBooks = (order) => order.type == "books";
const getAmount = (order) => order.amount;
const sumAmount = (acc, amount) => acc + amount;

function getTotalAmount(shoppingCart) {
  return shoppingCart
    .filter(byBooks)
    .map(getAmount)
    .reduce(sumAmount, 0);
}

getTotalAmount(shoppingCart); // 70
```

///https://medium.com/the-renaissance-developer/concepts-of-functional-programming-in-javascript-6bc84220d2aa





