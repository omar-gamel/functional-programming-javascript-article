# What is functional programming?

- Functional programming is a paradigm of building computer programs using expressions and functions without mutating state and data.
- By respecting these restrictions, functional programming aims to write code that is clearer to understand and more bug resistant. This is achieved by avoiding using flow-control statements (<b>for</b>, (<b>while</b>, (<b>break</b>, (<b>continue</b>) which make the code harder to follow. Also, functional programming requires us to write pure, deterministic functions which are less likely to be buggy.

Before getting into functional programming, though, one needs to understand the difference between pure and impure functions.

<h3>Pure vs. Impure Functions</h3>

Pure functions take some input and give a fixed output. Also, they cause no side effects in the outside world.

``` javascript
const add = (a, b) => a + b;
```

Here, <b>add</b> is a pure function. This is because, for a fixed value of <b>a</b> and <b>b</b>, the output will always be the same.

``` javascript
const SECRET = 42;  
const getId = (a) => SECRET * a;
```

<b>getId</b> is not a pure function. The reason being that it uses the global variable <b>SECRET</b> for computing the output. If <b>SECRET</b> were to change, the getId function will return a different value for the same input. Thus, it is not a pure function.

This can be troublesome if we had to debug this code.

Because of these reasons, we only use pure functions in functional programming.

Another benefit of pure functions is that they can be parallelized and memoized. Have a look at the previous two functions. It’s impossible to parallelize or memoize them. This helps in creating performant code.

<h3>The Tenets of Functional Programming</h3>

So far, we have learned that functional programming is dependent on a few rules. They are as follows.

1. Don’t mutate data
2. Use pure functions: fixed output for fixed inputs, and no side effects
3. Use expressions and declarations

When we satisfy these conditions, we can say our code is functional.

<h3>Functional Programming in JavaScript</h3>

JavaScript already has some functions that enable functional programming. Example: <b>String.prototype.slice</b>, <b>Array.protoype.filter</b>, <b>Array.prototype.join</b>.

On the other hand, <b>Array.prototype.forEach</b>, <b>Array.prototype.push</b> are impure functions.

One can argue that <b>Array.prototype.forEach</b> is not an impure function by design but think about it—it’s not possible to do anything with it except mutating non-local data or doing side effects. Thus, it’s okay to put it in the category of impure functions.

Also, JavaScript has a <b>const</b> declaration, which is perfect for functional programming since we won’t be mutating any data.

<h3>Pure Functions in JavaScript</h3>

- Filter
- Map
- Reduce
- Concat
- Object.assign

<h3>Creating Your Own Pure Function</h3>

We can create our pure function as well. Let’s do one for duplicating a string <b>n</b> number of times.

``` javascript
const duplicate = (str, n) =>  
	n < 1 ? '' : str + duplicate(str, n-1);
```

This function duplicates a string <b>n</b> times and returns a new string.

``` javascript
duplicate('hooray!', 3)  
// hooray!hooray!hooray!
```

<h3>Higher-order Functions</h3>

Higher-order functions are functions that accept a function as an argument and return a function. Often, they are used to add to the functionality of a function.

``` javascript
const withLog = (fn) => {  
	return (...args) => {  
		console.log(`calling ${fn.name}`);  
		return fn(...args);  
	};  
};
```
In the above example, we create a <b>withLog</b> higher-order function that takes a function and returns a function that logs a message before the wrapped function runs.

``` javascript
const add = (a, b) => a + b;  
const addWithLogging = withLog(add);  
addWithLogging(3, 4);  
// calling add  
// 7
```

<b>withLog</b> HOF can be used with other functions as well and it works without any conflicts or writing extra code. This is the beauty of a HOF.

``` javascript
const addWithLogging = withLog(add);  
const hype = s => s + '!!!';  
const hypeWithLogging = withLog(hype);  
hypeWithLogging('Sale');  
// calling hype  
// Sale!!!
```
One can also call it without defining a combining function.

``` javascript
withLog(hype)('Sale'); 
// calling hype  
// Sale!!!
```

<h3>Currying</h3>

Currying means breaking down a function that takes multiple arguments into one or multiple levels of higher-order functions.

Let’s take the <b>add</b> function.

``` javascript
const add = (a, b) => a + b;
```

When we are to curry it, we rewrite it distributing arguments into multiple levels as follows.

``` javascript
const add = a => {
	return b => {
		return a + b;
	};
};
add(3)(4);  
// 7
```

The benefit of currying is memoization. We can now memoize certain arguments in a function call so that they can be reused later without duplication and re-computation.

``` javascript
// assume getOffsetNumer() call is expensive
const addOffset = add(getOffsetNumber());
addOffset(4);
// 4 + getOffsetNumber()
addOffset(6);
```

This is certainly better than using both arguments everywhere.

``` javascript
// (X) DON"T DO THIS  
add(4, getOffsetNumber());  
add(6, getOffsetNumber());  
add(10, getOffsetNumber());
```

<h3>Composition</h3>

In mathematics, composition is defined as passing the output of one function into input of another so as to create a combined output. The same is possible in functional programming since we are using pure functions.

To show an example, let’s create some functions.

The first function is range, which takes <b>a</b> starting number <b>a</b> and an ending number <b>b</b> and creates an array consisting of numbers from <b>a</b> to <b>b</b>.

``` javascript
const range = (a, b) => a > b ? [] : [a, ...range(a+1, b)];
```
Then we have a function multiply that takes an array and multiplies all the numbers in it.

``` javascript
const multiply = arr => arr.reduce((p, a) => p * a);
```
We will use these functions together to calculate factorial.

``` javascript
const factorial = n => multiply(range(1, n));  
factorial(5);  
// 120  
factorial(6);  
// 720
```

The above function for calculating factorial is similar to <b>f(x) = g(h(x))</b>, thus demonstrating the composition property.

# Is JavaScript a functional programming language or object-oriented?

Thanks to new developments in ES6, we can say that JavaScript is both a functional as well as object-oriented programming language because of the various first-class features it provides.

# What is the advantage of functional programming?

Functional programming ensures simpler flow control in our code and avoids any surprises in the form of variable and state changes. All this helps us avoid bugs and understand our code easily.

<b>Linke:</b> https://www.toptal.com/javascript/functional-programming-javascript



