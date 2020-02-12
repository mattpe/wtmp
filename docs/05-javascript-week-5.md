# JavaScript - Week 5

## Contents

- Objects
- Prototypes
- Classes
- Web Storage API

---

## JavaScript Objects

[Eloquent JavaScript, Chapter 6](https://eloquentjavascript.net/06_object.html)

[MDN web docs: JavaScript object basics](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics)

- Most things are objects in Javascript
- Even primitive values like numbers and string are treated as objects having methods and properties:
  - `"AbbaACDC".toLowerCase()` -> "abbaacdc"
  - `(6.16).toFixed()` -> 6
- Fundamentally objects are key-value pairs

More reading: [A Beginner's Guide to JavaScript's Prototype by Tyler McGinnis](https://tylermcginnis.com/beginners-guide-to-javascript-prototype/)

### Creating objects

Object literal notation:

```js
const someDog = {
  name: 'Maddy',
  weight: 7,
  breed: 'Cocker Spaniel',
  // NOTE: this keyword doesn't work with arrow functions
  bark: function() {
    if (this.weight < 8) {
      console.log('Wuf.');
    } else {
      console.log('WUFF!');
    }
  }
};

// Another object
const otherDog = {};
otherDog.name = "Victor";
otherDog.bark = function() {
  console.log("Bark!");
};
```

Constructor function:

```js
function Dog(name, weight, breed) {
  this.name = name;
  this.weight = weight;
  this.breed = breed;
  this.bark = () => {
    if (this.weight < 8) {
      console.log('Wuf.');
    } else {
      console.log('WUFF!');
    }
  };
}
const harryDog = new Dog('Harry', 7, 'bulldog');
const fifiDog = new Dog('Fifi', 12, 'beagle');
```

### Prototypes

[MDN web docs: Object prototypes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)

[MDN web docs: Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

- Javascript is a prototype-based language (cf. Object-oriented programming (OOP) languages: Java, C#, etc.)
- Prototypes are the mechanism by which JavaScript objects inherit features from one another.
- Almost all objects in JavaScript are instances of [`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
- Typical object inherits properties and methods from `Object.prototype`
- Overriding built-in prototypes by developer is possible but not (ever) recommended:

    ```js
    [1, 3, 5, 7].toString(); // -> '1,3,5,7'
    Array.prototype.toString = () => 'hello';
    [1, 3, 5, 7].toString(); // -> 'hello'
    ['first', 'second'].toString(); // -> 'hello'
    ```

- Adding methods to objects created by `Dog()` constructor:

    ```js
    Dog.prototype.speak = function() {
      console.log(`My name is ${this.name}`);
    };
    const smallDog = new Dog('Gunther', 6, 'beagle');
    harryDog.speak(); //-> My name is Harry
    smallDog.speak(); //-> My name is Gunther
    ```

>When it comes to inheritance, JavaScript only has one construct: objects. Each object has a private property which holds a link to another object called its prototype. That prototype object has a prototype of its own, and so on until an object is reached with null as its prototype. By definition, null has no prototype, and acts as the final link in this prototype chain.

```js
console.log(Object.getPrototypeOf(harryDog)); //-> Dog()
console.log(Object.getPrototypeOf(Object.getPrototypeOf(harryDogf))); //-> Object()
console.log(Object.getPrototypeOf(Object.getPrototypeOf(Object.getPrototypeOf(harryDogf)))); //-> null
```

### Classes

[MDN web docs: Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

- ES6+ classes are primarily _syntactical sugar_ over JavaScript's existing prototype-based inheritance.
- OOP like syntax but prototypes used under the hood
- Class declaration is not hoisted, class must be declared before using it

```js
class Dog {
  constructor(name, weight, breed) {
    this.name = name;
    this.weight = weight;
    this.breed = breed;
  }
  bark() {
    if (this.weight < 8) {
      console.log('Wuf.');
    } else {
      console.log('WUFF!');
    }
  }
}
const harryDog = new Dog('Harry', 10, 'bulldog');
const smallDog = new Dog('Gunther', 6, 'beagle');
harryDog.bark(); //-> WUFF!
smallDog.bark(); //-> Wuf.

// Inheritance

class SpecialDog extends Dog {
  constructor(name, weight, breed) {
    super(name, weight, breed);
  }
  speak() {
    console.log(`Hi, my name is ${this.name} and I can speak.`);
  }
}
const circusDog = new SpecialDog('Chanelle', 3, 'chihuahua');
circusDog.bark(); //-> Wuf.
circusDog.speak(); //-> Hi, my name is Chanelle and I can speak.

// Add a new method to Dog super class by using the prototype

Dog.prototype.sit = function() {
  console.log(`${this.name} sat down.`);
};
circusDog.sit(); //-> Chanelle sat down.
harryDog.sit();  //-> Harry sat down.
```

---

## Web Storage API

[MDN web docs](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)

- Store key/value pairs in the browser.
- Stored data is bind to URL address, origin.
- Storage capacity ~ 5 MB
- Two mechanisms:
  - `sessionStorage` if for short time storage, cleared when browser window is closed.
  - `localStorage` if for more permanent storage without expiration date, gets cleared only when browser cache/locally stored data is cleared manually.
- object data structures can be stored as values by using JSON representation:
  - [`JSON.stringify()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) - convert object data to JSON string
  - [`JSON.parse()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) - parse JSON back to object

```js
// Store username for later usage
let username = 'Will';
localStorage.setItem('user', username);
console.log(localStorage.getItem('user'));
// Output: "Will"

// Store some website user settings as a json string
let userSettings = {
  username: 'Emily',
  colorTheme: 'dark',
  textColumns: 2
};
localStorage.setItem('userConfig', JSON.stringify(userSettings));
username = JSON.parse(localStorage.getItem('userConfig')).username;
console.log(username);
// Output: "Emily"
```

---

## Exercises - Week 5

Recap the [generic instructions](./01-javascript-basics.md#generic-instructions-for-all-programming-tasks) for coding tasks.

Use the [WTMP Starter](https://github.com/mattpe/wtmp-starter) boilerplate as a starting point for all tasks.

### Task 1 - Personalizing lunch menu website

1. Develop your lunch menu page further
1. Create a new branch called _week5-task1_ and checkout it
1. Let user to personalize the site (e.g. change color theme) and save the settings "permanently" in the browser
1. **BONUS:** Design and implement "add a new menu/restaurant" box functionality, some ideas:
   - hard coded ready-made options are ok
   - user can change the order of the boxes (drag'n'drop maybe?)
   - user can hide/display menu boxes
1. Use localStorage to save user settings (theme, menu configuration: order, restaurants, etc.. depends on your implementation)
1. Write a short description about your implementation
1. Publish a demo at users.metropolia.fi/~_MY-ACCOUNT_/wtmp-week5-task-1
1. Push task branch to Github and return direct link to Oma.
