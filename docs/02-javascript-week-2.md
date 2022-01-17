# JavaScript - Week 2

## Contents

- Basics of algorithms
- Data structures & data manipulation
- Regular expressions
- Modules

---

## Algorithms

- An algorithm is a mechanical "recipe" for the steps needed to take to get the desired outcome.
- Algorithms can be implemented with all(?) programming languages.
- The concept of an algorithm is more general than the concept of programming language.
- Study and design of algorithms is in the core of computer science
- [Algorithms + Data Structures = Programs](https://en.wikipedia.org/wiki/Algorithms_%2B_Data_Structures_%3D_Programs)
- [JavaScript based examples of many popular algorithms and data structures](https://github.com/trekhleb/javascript-algorithms/blob/master/README.md)

### Euclidean algorithm

[Background info, Wikipedia](https://en.wikipedia.org/wiki/Euclidean_algorithm)

- Efficient algorithm for computing [greatest common divisor](https://en.wikipedia.org/wiki/Greatest_common_divisor) of two numbers
- For example, _GCD_ for numbers 12 and 15 is 3.

![Flowchart of Euclidean algorithm](https://upload.wikimedia.org/wikipedia/commons/d/db/Euclid_flowchart.svg) ([Source: Wikimedia](https://en.wikipedia.org/wiki/Algorithm#/media/File:Euclid_flowchart.svg))

[Example implementation](https://www.tutorialspoint.com/euclidean-algorithm-for-calculating-gcd-in-javascript) in JavaScript:

```js
const num1 = 252;
const num2 = 105;
const findGCD = (num1, num2) => {
   let a = Math.abs(num1);
   let b = Math.abs(num2);
   while (a && b && a !== b) {
      if(a > b){
         [a, b] = [a - b, b];
      }else{
         [a, b] = [a, b - a];
      };
   };
   return a || b;
};
console.log(findGCD(num1, num2));
```

### Randomness

- How random is random?

>The [`Math.random()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random) function returns a floating-point, pseudo-random number in the range 0–1 (inclusive of 0, but not 1) with approximately uniform distribution over that range - which you can then scale to your desired range. The implementation selects the initial seed to the random number generation algorithm; it cannot be chosen or reset by the user.

More reading: [Randomness is hard: learning about the Fisher-Yates shuffle algorithm & random number generation](https://medium.com/@oldwestaction/randomness-is-hard-e085decbcbb2)

---

## Array data manipulation

Note: `myArray = yourArray;` creates a reference to existing array, does not copy the array ([more info](https://www.samanthaming.com/tidbits/35-es6-way-to-clone-an-array))

### Sorting

- Built-in [`sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) method returns an ascending array based on string values.
  - Optional parameter (compare function) for other sorting ordering
  - Execution time and effiency depends on the browser engine implementation (different algorithms for different purposes?)

```js
const numbers = [70, 8, 5, 666];
console.log(numbers.sort());
// [5, 666, 70, 8]

let sorterdNumbers = numbers.sort((a, b) => {
  return a - b;
});
console.log(sorterdNumbers);
// [5, 8, 70, 666]

const persons = [
  { name: 'Ines', age: 37 },
  { name: 'Zorro', age: 45 },
  { name: 'Edward', age: 21 },
  { name: 'Silvia', age: 1 },
];
console.log(persons.sort()); // order doesn't change
let sortedPersons = persons.sort((a, b) => {
  return b.age - a.age;
});
console.log(sortedPersons);
// {name: "Zorro", age: 45}
// {name: "Ines", age: 37}
// {name: "Edward", age: 21}
// {name: "Silvia", age: 1}
```

Some common sorting algorithms:

- [Insertion sort](https://github.com/trekhleb/javascript-algorithms/tree/master/src/algorithms/sorting/insertion-sort)
- [Bubble sort](https://github.com/trekhleb/javascript-algorithms/tree/master/src/algorithms/sorting/bubble-sort)
- [Quicksort](https://github.com/trekhleb/javascript-algorithms/tree/master/src/algorithms/sorting/quick-sort)
- [Merge sort](https://github.com/trekhleb/javascript-algorithms/tree/master/src/algorithms/sorting/merge-sort)
- [Radix sort](https://github.com/trekhleb/javascript-algorithms/tree/master/src/algorithms/sorting/radix-sort)

### Queue

- [FIFO - First In, First Out](https://github.com/trekhleb/javascript-algorithms/tree/master/src/data-structures/queue)
- [`array.push()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) - adds one or more elements to the end of an array and returns the new length of the array
- [`array.shift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift) - removes the first element from an array and returns that removed element

### Stack

- [LIFO - Last In, First Out](https://github.com/trekhleb/javascript-algorithms/tree/master/src/data-structures/stack)
- [`array.push()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) - adds one or more elements to the end of an array and returns the new length of the array
- [`array.pop()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop) - removes the last element from an array and returns that element
- example: mobile app navigation?

### Abstract data types

[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map), [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set), stack and queue are called also [abstract data types (ADT)](https://en.wikipedia.org/wiki/Abstract_data_type).

>a mathematical model for data types, where a data type is defined by its behavior (semantics) from the point of view of a user of the data, specifically in terms of possible values, possible operations on data of this type, and the behavior of these operations.

More about Map & Set if interested:

- <https://flaviocopes.com/javascript-data-structures-map/>
- <https://flaviocopes.com/javascript-data-structures-set/>

### Spread operator

- [ES6 spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) allows iterables like arrays or strings to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected

```js
console.log(1, 2, 3); // 1 2 3
const numbers = [1, 2, 3];
console.log(...numbers); // 1 2 3
const moreNumbers = [...numbers, 4, 5, 6];
console.log(moreNumbers); // [1, 2, 3, 4, 5, 6]
```

---

### Built-in filter(), map() & reduce()

[Eloquent JavaScript, Chapter 5](https://eloquentjavascript.net/05_higher_order.html#h_MM7RF32uzF)

- [`filter()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) filters out the elements in an array that don’t pass a test and builds up a new array with only the elements passing the test function

```js
const numbers = [1, 2, 3, 4, 5, 6];
const filtered = numbers.filter(number => number%3 === 0);
console.log(filtered);
// [3, 6]
```

- The [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) method transforms an array by applying a function to all of its elements and building a new array from the returned values

```js
const numbers = [1, 2, 3, 4, 5, 6];
const multiplied = numbers.map(number => number*3);
console.log(multiplied);
// [3, 6, 9, 12, 15, 18]
```

- [`reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) computes a single value from an array

```js
const numbers = [1, 2, 3, 4, 5, 6];
const sum = numbers.reduce((acc, current) => acc + current);
console.log(sum);
// 21
```

- Reading: [Simplify your JavaScript – Use .map(), .reduce(), and .filter()](https://medium.com/poka-techblog/simplify-your-javascript-use-map-reduce-and-filter-bd02c593cc2d)

---

## More functions

### Recursion

[Eloquent JavaScript, Chapter 3](https://eloquentjavascript.net/03_functions.html#h_jxl1p970Fy)

- Function calls itself repeatedly until it arrives at a result -> loop-like iteration
- No need to keep track of the state by using local variables
- Most loops can be rewritten in a recursive style, and in some functional languages this approach to looping is the default.
- In pure functional programming there is no concept of state of a program.

Example: traditional function for calculating [factorial](https://en.wikipedia.org/wiki/Factorial) (`5! = 5*4*3*2*1 = 120`) uses a loop:

```js
const factorial = number => {
  let result = 1;
  for (let count = number; count > 1; count--) {
    result *= count;
  }
  return result;
};
console.log(factorial(5)); // 120
```

same function with recursion:

```js
const factorial = number => {
  if (number <= 0) {
    return 1;
  } else {
    return (number * factorial(number - 1));
  }
};
console.log(factorial(5)); // 120
```

### Higher-order functions

[Eloquent JavaScript, Chapter 5](https://eloquentjavascript.net/05_higher_order.html)

- A higher-order function is a function that does at least one of the following:
  - takes one or more functions as arguments
  - returns a function as its result
- [David Green: Higher-Order Functions in JavaScript](https://www.sitepoint.com/higher-order-functions-javascript/)
- [Damien Cosset: Higher-order functions in Javascript](https://dev.to/damcosset/higher-order-functions-in-javascript-4j8b)
- e.g. built-in array functions: `sort()`, `map()`, `filter()`...

### Function parameters

- ES6 functions are able to use [default parameter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameter) values if no value or `undefined` is passed

```js
const hello = (user = 'Anonymous') => {
  return `Hello ${user}!`;
};
console.log(hello()); // Hello Anonymous!
console.log(hello('Peter')); // Hello Peter!
```

- Using [rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) allows to represent an indefinite number of arguments as an array

```js
const doSomething = (firstArg, ...theRest) => {
  return theRest;
};
console.log(doSomething('first param', 'second', 3, [4, 5]));
// ["second", 3, [4, 5]]
```

---

## Regular expressions, regexp

[Eloquent JavaScript, Chapter 9](https://eloquentjavascript.net/09_regexp.html)

[MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

- describe string patterns effectively
- powerful tool for many string operations (search, test, match, replace, etc.), for example:
  - validation of user inputs (must be done on the server-side too!)
  - cleaning/fixing strings: replace/remove unwanted characters, add styling, etc.
- two ways to construct
  - regexp literal: `const re = /<MY REGULAR EXPRESSION PATTERN>/;`
  - RegExp object constructor: `const re = new RegExp('<MY REGULAR EXPRESSION PATTERN>');`
  - using regular expression literal is preferred if there is no need to change the pattern later at runtime
- [One Cheat Sheet](https://devinduct.com/cheatsheet/10/regex)

### Example patterns

```js
// string containing a three digits number
/^[0-9]{3}$/

// String size of 2-16 letters (a-z) starting with a capital letter
/^[A-ZÖÄÅ]{1}[a-zöäå]{2,16}$/

// DOS style filename ('filename.ext', max size 8.3 letters, no whitespaces or special chars, case insensitive)
/^[\w-]{1,8}\.\w{1,3}$/i

// Email validation function
const validateEmailAddress = email => {
    const regexpPattern = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return regexpPattern.test(email.toLowerCase());
}

// Some phonenumber patterns
/^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/
/^[(]{0,1}[0-9]{3}[)]{0,1}[-\s\.]{0,1}[0-9]{3}[-\s\.]{0,1}[0-9]{4}$/
/^[+]*[(]{0,1}[0-9]{1,3}[)]{0,1}[-\s\./0-9]*$/
/^[+]?(1\-|1\s|1|\d{3}\-|\d{3}\s|)?((\(\d{3}\))|\d{3})(\-|\s)?(\d{3})(\-|\s)?(\d{4})$/
```

---

## Modules

[Eloquent JavaScript, Chapter 10](https://eloquentjavascript.net/10_modules.html)

- split large code files into smaller, reusable modules of code
- helps in maintaining well organised project and file structure
- CommonJS modules (used in Node.js apps)
  - `module.exports = ...` and `require()` statements
- ECMAScript modules, built-in feature in ES6
  - supported in Webpack based projects
  - used with [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) & [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) statements

### ES Module examples

Folder structure:

```dir
src/
  | index.js
  | modules/
    | my-module.js
```

Using export for multiple variables:

_index.js:_

```js
import {myVariable, myFunction, myObject} from './modules/my-module';

console.log(myVariable);
// Output: Hello!
console.log(myFunction('Tina'));
// Output:  Hello Tina
console.log(myObject.prop2[3]);
// Output: values
```

_modules/my-module.js:_

```js
const myVariable = 'Hello';
const myFunction = (name) => {
  return 'Hello ' + name;
};
const myObject = {
  prop1: 'Prop1 value',
  prop2: ['Array', 'of', 'prop2', 'values']
};
export {myVariable, myFunction, myObject};
```

Using default export:

_index.js:_

```js
import Tools from './modules/my-module';

console.log(Tools.myVariable);
// Output: Hello!
console.log(Tools.myFunction('Tina'));
// Output:  Hello Tina
console.log(Tools.myObject.prop2[3]);
// Output: values
```

_modules/my-module.js:_

```js
const myVariable = 'Hello';
const myFunction = (name) => {
  return 'Hello ' + name;
};
const myObject = {
  prop1: 'Prop1 value',
  prop2: ['Array', 'of', 'prop2', 'values']
};
const Tools = {myVariable, myFunction, myObject};
export default Tools;
```

---

## Exercises - Week 2

Recap the [generic instructions](./01-javascript-basics.md#generic-instructions-for-all-programming-tasks) from last week.

Use the [WTMP Starter](https://github.com/mattpe/wtmp-starter) boilerplate as a starting point for all tasks.

### Task 1 - Number guessing algorithm

Add computer player functionality to the number guessing game.

1. Continue [task 2](01-javascript-basics.md#task-2---number-guessing-game) from previous week
1. Create a new branch called _week2-task1_ and checkout it (`git checkout -b week2-task1`)
1. Describe the best strategy in playing the number guessing game and design an algorithm for it
1. Implement your algorithm:
   - explain the strategy in code comments
   - display all the numbers guessed (save guess history to an array)
   - display the total number of guesses needed for correct answer
1. Test your algorithm by running it multiple times (1000+)
   - what is the average number of total guess counts?
   - what is the maximum number of total guess counts? What is the theoritical max?
   - what is the minimum number of total guess counts?
1. Push task branch to Github and return direct link to Oma.

### Task 2 - Data manipulation

Create a new branch called _week2-task2_ and checkout it.

#### A

Implement functions, use the following data array when needed.

```js
[
  {name: 'Lingonberry jam', price: 4.00},
  {name: 'Mushroom and bean casserole', price: 5.50},
  {name: 'Chili-flavoured wheat', price: 3.00},
  {name: 'Vegetarian soup', price: 4.80},
  {name: 'Pureed root vegetable soup with smoked cheese', price: 8.00}
]
```

1. Create a function that validates a name for a meal
    - takes input string as a parameter, returns true/false
    - example of valid title: "Mushroom and bean casserole"
    - use regexp (`.test()`), rules for the string:
      - starts with a capital letter
      - min length 4, max 64
      - can contain letters, numbers, whitespaces, hyphens, slashes, commas & parentheses
1. Sort the menu based on price
1. Display only items costing less than 5 € (filtering)
1. Raise all prices 15 % (use map)
1. How much does it cost to eat the whole menu (use reduce)
    - tip: <https://stackoverflow.com/questions/5732043/javascript-reduce-on-array-of-objects>

Execute all the statements and call all the functions (with test parameters) and print output to console (or document).

#### B - More advanced

1. Get [Fazer lunch menu json file](https://github.com/mattpe/wtmp/blob/master/assets/fazer-week-example.json)
1. Implement a function that displays vegan dishes only. (Choosing just one day is ok.)

**Push task branch to Github and return direct link to Oma.**

### Task 3 - Dummy lunch menu 3 - with modules

1. Continue [task 4](./01-javascript-basics.md#task-4---dummy-lunch-menu-2) from previous week
1. Create a new branch called _week2-task3_ and checkout it
1. Split JS code to reasonable ES6 modules (use `import`/`export`)
    - Create a subfolder called `modules/` and place all your module files there
    - Move code related to Sodexo data usage to a new module called e.g. `SodexoData`
    - use PascalCase for module names and kebab-case for filenames ([Case styles](https://medium.com/better-programming/string-case-styles-camel-pascal-snake-and-kebab-case-981407998841))
1. Add Fazer's lunch menu to your site, create a new module for Fazer data, json files:
    - Finnish: <https://raw.githubusercontent.com/mattpe/wtmp/master/assets/fazer-week-example.json>
    - English: <https://raw.githubusercontent.com/mattpe/wtmp/master/assets/fazer-week-example-en.json>
1. Bonus: Add some of the data manipulation features created in the previous task or develop new ones
1. Push task branch to Github and return direct link to Oma.
1. Deploy your lunch menu website to your home folder (_users.metropolia.fi_) and return link to Oma.
