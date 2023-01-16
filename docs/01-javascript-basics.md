# JavaScript Basic Concepts & Getting Started

## Contents

- Variables
- Operators
- Types
- Scopes
- Control structures
- Functions
- Data structures (array & object)
- Exercise instructions
- Exercises for Week 1

---

## About JS and ES

- _ECMAScript_ is a specification standardized by _European Computer Manufacturers Association (ECMA)_ for _JavaScript_ language
- ES6 is short for ECMAScript 6, also known as ES2015
- ES8 is short for ECMAScript 8, also known as ES2017 and so on..
- ES6+ versions are often considered "modern JavaScript"
- some selected new features in "modern" ES versions:
  - `const`
  - `let`
  - arrow functions
  - JavaScript classes
  - function default parameter values
  - `Array.find()`
  - `Array.findIndex()`
  - `Object.values()`
  - for-of loop
  - promises
  - await-async

---

## Variables and values

- Read [Eloquent JavaScript, chapters 1 and 2](https://eloquentjavascript.net/01_values.html)
- A variable is a name for a location where a value is stored
- Three ways to declare a variable:
  - `let` - declare variable in block scope ≈ in closest enclosing `{ ... }` block. Declarations are not _hoisted_
  - `const` - like let but variable value can not be changed after initial assignment
  - `var` - the original Javascript way. Only two scopes in use: function or global.
Declarations are hoisted, ie. moved to the beginning of the scope. Can be confusing. Use only if you have a really good reason to do so.

---

## Variable types

Type system in Javascript is loose and dynamic. Type of a variable depends on the type of value currently assigned to it. There is no way to declare types and ensure type-safety (assign a Number to a variable that now holds a String) - it is up to the programmer to make sure total confusion and chaos is not created. Built-in types are:

- Boolean
- Number
- String
- ...and Object, undefined, null, Symbol

```js
> let a = 13;
> a
13
> typeof a
'Number'
> a = 14;
14
> let b = 'string 1';
> typeof b
'string'
> a = true;
true
> typeof a;
'boolean'
> let d
> d
Undefined
> const e = 123;
> e
123
> e = 124;
TypeError: Assignment to constant variable.
```

---

## Arithmetic, string and boolean operations

- Arithmetic: `+`, `-` (both [unary](https://scotch.io/tutorials/javascript-unary-operators-simple-and-useful) and binary), `*`, `/`, `%`, `++`, `--`, `+=`, `-=`, `*=`, `/=`, `%=`
- Boolean: `&&`, `||`, `!`, `==`, `!=`, `===`, `!==`, (`>`, `<`, `>=`, `<=`)
- String: `+`, etc
- Array: `.push()`, `.pop()`, etc.
- Precedence: "normal" mathematic rules (* and / before + and - etc) apply. However, relying on the precedence rules is not always a good idea - using parentheses often clarifies

```js
> a = 13; let b = 8;
> a + b;
21
> a - b;
5
> a * b;
104
> a / b;
1.625
> a % b;
5
> Math.floor(a / b);
1
> 0.1 * 0.2
0.020000000000000004
> 0.1 * 0.2 - 0.02
3.469446951953614e-18
> 1 / 0;
Infinity
> 0 / 0;
NaN
> 1 / 0 - 1 / 0;
NaN
> "onetwothree" * 2
NaN
> let h = true, k = false;
> h && true;
true
> !h && !k;
false
> k || "whatever";
'whatever'
> h || "whatever"
true
> h || whatever;
true
> k || whatever;
ReferenceError: whatever is not defined
> !(h && k)
true
> !h || !k
true
> !(h || k)
false
> !h && !k
false
```

---

## Equality

- `===` (strict equality): do equality testing without type conversion. This is the equality one should prefer.
- `==` (loose equality): perform type conversions according to rules in (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness), and then do strict equality comparison.
- For objects, if the content “sameness” is the target, one option is to compare stringified formats: `JSON.stringify(a) === JSON.stringify(b)`. Note that this does deep comparison (all values included in a and b down to primitive types are compared).
- There are also libraries that implement deep comparisons etc.
- Ternary operator `(cond) ? value1 : value2;` returns value1 if cond evaluates to true, value2 otherwise.
- All value types equate to [truthy or falsy](https://www.sitepoint.com/javascript-truthy-falsy/) values

```js
> 4 == 4
true
> 4 == "4"
true
> 4 != "4"
false
> 4 === "4"
false
> 4 !== "4"
true
> 4 === 4
true
> Infinity == Infinity
true
> NaN == NaN
false
> 1 > 3
false
> 3 > 1
true
> typeof (3 > 1)
'boolean'
let p = 5
undefined
> (p % 2 == 0) ? "even" : "odd"
'odd'
> p++
5
> p
6
> (p % 2 == 0) ? "even" : "odd"
'even'
> let a = 6, b = 2, c = 1;
> [a, b, c] = [c, a, b];
[ 1, 6, 2 ]
> [a, b, c];
[ 1, 6, 2 ]
```

---

## Type conversions

Javascript has a comprehensive implicit type conversion mechanism for basic types. Type conversions are often very handy and minimize the amount of code, however, they can be very dangerous in some cases.

```js
> let r = "String one", s = 'Another string', t = `One more string`;
> r + s;
'String oneAnother string'
> `value of a + b is ${a + b}`;
'value of a + b is 21'
> 'value of a + b is ' + (a + b);
'value of a + b is 21'
> 'value of a + b is ' + a + b;
'value of a + b is 138'
> 12 + 12 + "a"
'24a'
> "a" + 12 + 12
'a1212'
> "12" + "12"
'1212'
> "12" - "12"
0
> Number("12");
12
> String(5 + 6);
'11'
> Number(true);
1
> String(false);
'false'
> Boolean('Donald');
true
```

---

## Scoping

- Scope: rules for deciding what names are visible in program execution
- Lexical scoping: visibility is defined by the location of definitions in the program
code
- Global scope: bindings defined outside any function
- Local scope: bindings defined inside a block, including a function. In Javascript `let` and `const` bindings have a local scope. `var` behaves in different manner. Just don't use it.

```js
const a = 6;
let b = 3;
{
  const a = 9;
  const c = 4;
  {
    let c = 1;
    console.log(a + b + c); // --> 13
    b = 2;
  }
  console.log(a + b); // --> 11
}
console.log(a + b); // --> 8
```

---

## Control structures - loops

[Eloquent JavaScript, Chapter 2](https://eloquentjavascript.net/02_program_structure.html)

- `for(init; cond; step) { ... }` - four-part loop construct: initialization;
condition; step, and body
- (for .. of - later)
- (for .. in - later)
- `while(cond) { ... }` - keep executing body while cond is true
- `do { ... } while(cond)` - keep executing body while cond is true, note that cond is evaluated first time only after executing body once

### for loop

```js
const retirementAge = 65;
for (let age = 0; age <= 100; age += 10) {
  console.log(`${name} is ${age} years old.`);
  if (age >= retirementAge) {
    break; // break statement _can_ be used to jump immediately out from the loop
  }
}
```

### while loop

```js
age = -10;
while (age <= 100) {
  if (age > 65) {
    console.log(`${name}, age ${age} should retire.`);
  } else if (age >= 18) {
    console.log(`${name}, age ${age} is adult.`);
  } else if (age >= 0) {
    console.log(`${name}, age ${age} is not adult at all.`);
  } else {
    console.log(`${name}, age ${age} has a weird age.`);
  }
  age += 10;
}
```

### do - while loop

```js
age = 101;
do {
  console.log(`${name} is ${age} years old.`);
  age++;
} while (age <= 100);
```

---

## Control structures - conditional

### if - else

```js
if (age > 65) {
  console.log(`${name}, age ${age}, should retire.`);
} else if (age >= 18) {
  console.log(`${name}, age ${age}, is adult.`);
} else if (age >= 0) {
  console.log(`${name}, age ${age}, is not adult at all.`);
} else {
  console.log(`${name}, age ${age}, has a weird age.`);
}
```

### switch - case

```js
let result;
switch (name) {
  case 'Donald':
  case 'Sauli':
    result = 'President';
    break;
  case 'Angela':
    result = 'Chancellor';
    break;
  case 'John':
    result = 'Nobody';
    break;
  default:
    result = '';
}
```

`break` statement is needed if fall-through is not desired.

---

## Functions

[Eloquent JavaScript, Chapter 3](https://eloquentjavascript.net/03_functions.html)

A function can be used to give a name to a snippet of code to clarify its meaning. Functions can also be used to avoid repeating code multiple times. They serve as building blocks out of which whole applications can be constructed.

A function can be defined with keyword function and assigned to a variable (in this case const sqr). The argument(s) of the function are listed in parenthesis after keyword function. The value of the expression after return statement defines the value returned by function when it is called.

```js
const sqr = function(x) {
  return x * x
}
console.log(sqr(0)); // 0
console.log(sqr(3)); // 9
console.log(sqr(2 - 4)); // 4
console.log(sqr("five")); // NaN
```

### Defining a function

Three different ways. We prefer [the fat arrow expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

[Function declaration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function) (is hoisted):

```js
function powerIterative(base, exponent) {
  let result = 1;
  for(expo = 1; expo <= exponent; expo++) {
    result *= base;
  }
  return result;
};
```

[Function expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function) (is not hoisted):

```js
const powerIterative = function(base, exponent) {
  let result = 1;
  for(expo = 1; expo <= exponent; expo++) {
    result *= base;
  }
  return result;
};
```

[Arrow function expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions):

```js
const powerIterative = (base, exponent) => {
  let result = 1;
  for(expo = 1; expo <= exponent; expo++) {
    result *= base;
  }
  return result;
};
```

### Pure functions

- The function `sqr` defined earlier is a _pure function_: it only returns a value (doesn’t do anything else), and the value it returns depends only on its argument value(s). This means that whenever the function is called with same parameter value(s), it will return the same value.

- It makes sense to try and write pure functions whenever possible. Reasoning about pure functions is easier then with impure ones and contributes towards more correct programs.

A function can be impure in two ways:

- The return value depends on variables other than arguments, including values stored by the system.
- The function can have side-effects, it doesn’t just return a value.

---

## [Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

[Eloquent JavaScript, Chapter 4](https://eloquentjavascript.net/04_data.html)

- An array allows for storing together related values using an integer index for identification.
- In Javascript arrays are dynamic, ie. they can grow in shrink in size during program execution. (The are more like ArrayList than array in Java).
- An array in Javascript is also an object. It is possible to add/remove attributes to/from an array. The indexes in an array are technically property names, however a special syntax (which is familiar from other programming languages) must be used.

```js
> const myGrades = [ 3, 3, 4, 1, 5, 5 ];
undefined
> myGrades;
[ 3, 3, 4, 1, 5, 5 ]
> myGrades[0];
3
> myGrades[2];
4
> myGrades[6];
undefined
> myGrades.length;
6
> myGrades[2] = 3;
3
> myGrades;
[ 3, 3, 3, 1, 5, 5 ]
> myGrades[6] = 4;
4
> myGrades;
[ 3, 3, 3, 1, 5, 5, 4 ]
> myGrades.push(5);
8
> myGrades;
[ 3, 3, 3, 1, 5, 5, 4, 5 ]
> myGrades.pop();
5
> myGrades;
[ 3, 3, 3, 1, 5, 5, 4 ]
```

### Iterating using for-of loop

- [for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) is a new loop in ES6 that replaces both for-in and forEach() and supports the new iteration protocol. ([one comparison](https://codeburst.io/foreach-vs-for-of-vs-for-in-tug-of-for-d8f935396648))
- Use it to loop over iterable objects (Arrays, strings, Maps, Sets, etc.)

```js
const myGrades = [3, 3, 4, 1, 5, 5];
let topGradeCount = 0;
for (const grade of myGrades) {
  if (grade > 4) {
    topGradeCount++;
  }
}
// topGradeCount: 2
```

---

## [Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

[Eloquent JavaScript, Chapter 4](https://eloquentjavascript.net/04_data.html)

In Javascript object allows for grouping together several related values (properties). The values can be objects, or they can be any primitive values.

Javascript allows for object definitions “on-the-fly”, ie. it is not mandatory to declare a class (like in Java or C#) to keep together several related values. Object attributes can also be added and removed dynamically.

It probably makes sense to use Javascript class mechanism when objects are used in the application extensively. We will take a look at classes later on.

```js
let student = {
  name: 'Jill',
  credits: 90,
  active: true
};
{ name: 'Jill', credits: 90, active: true }

student.credits++;
student.active = false;
{ name: 'Jill', credits: 91, active: false }

student.address = {
  street: 'Learning Road',
  zip: 1010101
};
{
  name: 'Jill',
  credits: 91,
  active: false,
  address: {
    street: 'Learning Road',
    zip: 1010101
  }
}
console.log(student.address.zip);
1010101
```

### Iterating object properties using for-in loop

All enumerable string properties of an object can be iterated over using a [for...in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in) loop:

```js
for (const property in student) {
  console.log('avain:', property, 'arvo: ', student[property]);
}
```

---

## Generic instructions for all programming tasks

You can use the same GitHub repo for returning all programming tasks, just remember to create a specific branch for every task.

Following good coding practices makes your app a lot more easier to maintain, develop and understand:

- Use meaningful and descriptive names for variables, functions, etc.
- Be consistent with naming conventions, indentation, commenting style, application structure, file & folder organisation
- Avoid deep nesting
- [KISS](https://en.wikipedia.org/wiki/KISS_principle) - Keep It Simple, Stupid
- DRY - Don't Repeat Yourself
- Use proper commenting when needed
- Line length: avoid writing horizontally long lines of code
- Avoid large code files, split to smaller modules

Preferred coding conventions in course tasks, use:

- charset UTF-8
- indentation 2 spaces
- `const` & `let`, never `var`
- arrow functions
- single quotes for strings
- `async/await` instead of promise chaining/nesting
- [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) especially with long strings instead of concatenation
- [JSDoc](https://jsdoc.app/) style comments
- Check [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)

Some tips:

- Start small. Take small steps. Write something you can verify (a function, for example), and only after it works, build based on it.
- Trying to construct the application from multiple pieces whose correct functioning is unproven is a very risky strategy. Try out the functions you write using different inputs (even illegal) and check that they behave as expected.
- Separate UI from the application logic as much as possible. Avoid writing the whole application logic in event handlers.
- Use debugger. Or use alert events.
- If the application refuses to work, step back and try to isolate the problematic
part. Use debugger and breakpoints.

Some useful git commands:

- see the status of your repo: `git status`
- choose changed files to be committed: `git add <FILES>`, `git add .` chooses all changes in current folder
- commit (save) the changes chosen with `add`: `git commit -m "<DESCRIBE CHANGES HERE>"`
- list local branches: `git branch`
- list all (local & remotes) branches: `git branch --all`
- choose a branch: `git checkout <BRANCH-NAME>`
- push local branch to remote 'origin': `git push origin <BRANCH-NAME>`
- push all local branches to remote 'origin': `git push origin --all`
- view commit history of a branch: `git log`

Make sure that you are in the correct branch when committing changes!

## Exercises - Week 1

Use the [WTMP Starter](https://github.com/mattpe/wtmp-starter) boilerplate as a starting point for all tasks.

### Task 1 - Setup Enviroment

1. Setup your [toolchain](./00-tools.md)
1. Get the boilerplate project `git clone https://github.com/mattpe/wtmp-starter.git`
1. Setup a remote repository of your own
    - Create a new repository at GitHub
    - Change project's remote origin: `git remote set-url origin <NEW-REPO-URL>`
1. Adapt the layout that you created in HTML/CSS classes
    - All code files are in `src/` folder
    - By default, only files in `src/assets` are copied directly to build. Place static asset files there.
1. Run (`npm start`) and open in the browser
1. Create a new branch called _week1-task1_  (`git branch week1-task1` / `git checkout -b week1-task1`), push it to Github (`git push origin <BRANCH_NAME>`) and return a direct link to Oma.

### Task 2 - Number Guessing Game

1. Create a new branch called _week1-task2_ and checkout it (`git checkout -b week1-task2`)
1. Do the [number guessing game tutorial](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/A_first_splash)
    - Use wtmp-starter project, **DO NOT** use `<script>` tags inside html files.
    - Use arrow functions
1. modify it so that the lowest and the highest numbers (limits) and maximum guess count are defined as constants (and can be changed by modifying the values in code).
1. add a timer that calculates the total time spent guessing in seconds.
   - TIP: `Date.now()` returns the current timestamp in milliseconds
1. Show total number of guesses and the time spent when correct number is found
1. Add some css styles.
1. Push task branch to Github and return direct link to Oma.

### Task 3 - Dummy lunch menu

1. Create a new branch called _week1-task3_ based on your _main_ branch and checkout it (`git checkout main; git checkout -b week1-task3`)
1. Use the html/css layout you created on Ulla's classes (you can implement just the JS application logic first if you don't have the html-layout ready yet)
1. Write the contents of one of the following arrays into correct text box

    ```js
    const coursesEn = ["Hamburger, cream sauce and poiled potates",
                    "Goan style fish curry and whole grain rice",
                    "Vegan Chili sin carne and whole grain rice",
                    "Broccoli puree soup, side salad with two napas",
                    "Lunch baguette with BBQ-turkey filling",
                    "Cheese / Chicken / Vege / Halloum burger and french fries"];
    const coursesFi = ["Jauhelihapihvi, ruskeaa kermakastiketta ja keitettyä perunaa",
                    "Goalaista kalacurrya ja täysjyväriisiä",
                    "vegaani Chili sin carne ja täysjyväriisi",
                    "Parsakeittoa,lisäkesalaatti kahdella napaksella",
                    "Lunch baguette with BBQ-turkey filling",
                    "Juusto / Kana / Kasvis / Halloumi burgeri ja ranskalaiset"];
    ```

1. Add a button for changing the language of the menu
1. Add a button to sort courses alphabetically and write a function for sorting:
    - Takes two arguments: menu array and order (asc/desc)
    - Returns a sorted array
1. Add a button that picks a random dish from the array and displays it
1. Push task branch to Github and return direct link to Oma.

### Task 4 - Dummy lunch menu 2

1. Continue the previous task (3)
1. Create a new branch called _week1-task4_ and checkout it (`git checkout -b week1-task4`)
1. Replace hardcoded menu arrays with a static json file
    - copy [this json file](https://github.com/mattpe/wtmp/blob/master/assets/sodexo-day-example.json) into your project folder
    - use import syntax for loading the json data:

    ```js
    import LunchMenu from '<relative-path-to-json-file>';
    // Test
    console.log('lunch menu object', LunchMenu);
    ```

1. Implement all the features/requirements listed in the previous task
1. Push task branch to Github and return direct link to Oma.
1. Deploy your lunch menu website to your home folder (_users.metropolia.fi_) and return link to Oma.
    - create a "production" build: `npm run build` (you must stop the dev server first: _ctrl-c_)
    - copy contents of `dist/` folder to your `public_html` folder on _shell.metropolia.fi_
    - TIP: bash command for scp file copy: `scp -r dist/* <USERNAME>@shell.metropolia.fi:public_html/<FOLDER-NAME>/`

---
