# JavaScript - Week 3

## Contents

- Light intro to design patterns
- Asynchronous code
- Events

---

## Design Patterns

[Wikipedia: Software design pattern](https://en.wikipedia.org/wiki/Software_design_pattern)

> In software engineering, a software design pattern is a general, reusable solution to a commonly occurring problem within a given context in software design. It is not a finished design that can be transformed directly into source or machine code. Rather, it is a description or template for how to solve a problem that can be used in many different situations. Design patterns are formalized best practices that the programmer can use to solve common problems when designing an application or system.

> Design patterns may be viewed as a structured approach to computer programming intermediate between the levels of a programming paradigm and a concrete algorithm.

### Design patterns and JS

- [Module pattern](https://en.wikipedia.org/wiki/Module_pattern): [ES modules](./02-javascript-week-2.md#modules)
- [Factory pattern](https://en.wikipedia.org/wiki/Factory_method_pattern): covered later (with JS classes)
- [Observer pattern](https://en.wikipedia.org/wiki/Observer_pattern): publish - subscribe, events
- [Adapter pattern](https://en.wikipedia.org/wiki/Adapter_pattern): makes two incompatible interfaces to work together, task 2
- many JS frameworks/libraries/apps implement features based on design patterns

Some reading:

- [Dev Labs: ES6 Journey Through Design Patterns](https://notes.devlabs.bg/es6-journey-through-design-patterns-1970f5eaa9d6)
- [BetterProgramming: JavaScript Design Patterns](https://medium.com/better-programming/javascript-design-patterns-25f0faaaa15)

---

## Asynchronous code

[Eloquent JavaScript, Chapter 11](https://eloquentjavascript.net/11_async.html)

- single thread (vs. multi-threading)
- asynchronous model allows multiple operations to happen at the same time
- non-blocking code
- when async operation is done [callback function](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function) can be called

e.g. scheduling:

```js
// Wait for five secs before logging
setTimeout(() => console.log("The tick"), 5000);

// Log 1/sec for 5 times
let counter = 1;
let timer = setInterval(() => {
  console.log(`${counter++}. tick.`);
  if (counter > 5) {
    clearInterval(timer);
  }
}, 1000);

// Logging now not affected or blocked by previous statements
console.log('Hello week 3!');
```

---

## Events

[Eloquent JavaScript, Chapter 15](https://eloquentjavascript.net/15_event.html)

[MDN Event reference](https://developer.mozilla.org/en-US/docs/Web/Events)

- Event interface represents an event which takes place in the [DOM](https://developer.mozilla.org/en-US/docs/Glossary/DOM)
- [`EventTarget.addEventListener()`] method is used to bind certain type of an event to a function that will be called whenever the specified event is delivered to the target
- Common event targets are DOM _elements_, _document_ object, and browser _window_, but the target may be any object that supports events

```html
<html>
  <div id="theDiv">Click here</div>
</html>
```

```js
let targetElement = document.querySelector('#theDiv');
targetElement.addEventListener('click', (event) => {
  console.log('Clicking the div');
});

```

### Event object

- An [event](https://developer.mozilla.org/en-US/docs/Web/API/Event) object holds data about the occurring event
- Different kind of event objects are based on the main Event
- [List of Events and types](https://developer.mozilla.org/en-US/docs/Web/Events)

```js
// MouseEvent
document.querySelector('#theDiv').addEventListener('click', (event) => {
  console.log('event object', event);

// KeyboardEvent
document.addEventListener('keydown', event => {
  console.log('keydown:', event.key, event.keyCode);
  if (event.keyCode === 13) {
    console.log('You hit enter!');
  }
});

// FocusEvent
let inputField = document.querySelector('input');
inputField.addEventListener('focusout', event => {
  console.log('input event', event);
  if (inputField.value === '') {
    inputField.focus();
  }
});  
```

### Event propagation

![An event dispatched in a DOM tree using the DOM event flow](img/eventflow.svg)([Source: w3.org](https://www.w3.org/TR/DOM-Level-3-Events/#dom-event-architecture))

- Read: [Sitepoint: What Is Event Bubbling in JavaScript? Event Propagation Explained](https://www.sitepoint.com/event-bubbling-javascript/)

Manipulating the default behaviour:

- [`event.stopPropagation()`](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation)
- [`event.preventDefault()`](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault)

#### Event bubbling example

What is logged to the console when one of the nested divs get clicked?

```html
<div id="a">
  a div
  <div id="b">
    b div
    <div id="c">
      c div
    </div>
  </div>
</div>
```

```js
document.querySelector('#a').addEventListener('click', event => {
  console.log('click event a', event);
});
document.querySelector('#b').addEventListener('click', event => {
  console.log('click event b', event);
  event.stopPropagation();
});
document.querySelector('#c').addEventListener('click', event => {
  console.log('click event c', event);
});
```

### Creating events

- User actions can be simulated, e.g. [mouse click](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/click)
- [Custom events](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Creating_and_triggering_events) can be created and dispathed.

```js
// CustomEvent
document.addEventListener('myMessage', event => {
  console.log('got a message:', event.detail.msg);
});
const event = new CustomEvent('myMessage', { detail: {msg: 'Hello!'} });
document.dispatchEvent(event);
```

### Using 3rd party modules - Drag'n'drop

Modern browsers provide a native [HTML Drag and Drop API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API) for implementing user actions. HTML drag'n'drop is based on DOM event model and drag event but can be a bit complicated to work with.

That's where 3rd party libraries come in handy providing simplified interfaces for implementing complex UI features. For example, [draggable.js](https://shopify.github.io/draggable/) is easy to install using npm: `npm install --save @shopify/draggable`

But remember that relying on 3rd party code is not risk-free. For example: [How one developer just broke Node, Babel and thousands of projects in 11 lines of JavaScript](https://www.theregister.co.uk/2016/03/23/npm_left_pad_chaos/).

Using draggable.js as an ES module:

```js
import { Sortable } from '@shopify/draggable';

const sortable = new Sortable(document.querySelectorAll('ul'), {
  draggable: 'li'
});
```

```html
<!-- html -->
<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```

[More examples.](https://shopify.github.io/draggable/examples/)

Similar functionality with "[VanillaJS](http://vanilla-js.com/)":

```js
let selected;

const dragOver = e => {
  if (isBefore(selected, e.target)) {
    e.target.parentNode.insertBefore(selected, e.target);
  } else {
    e.target.parentNode.insertBefore(selected, e.target.nextSibling);
  }
};

const dragEnd = () => {
  selected = null;
};

const dragStart = e => {
  e.dataTransfer.effectAllowed = "move";
  e.dataTransfer.setData("text/plain", null);
  selected = e.target;
};

const isBefore = (el1, el2) => {
  let cur;
  if (el2.parentNode === el1.parentNode) {
    for (cur = el1.previousSibling; cur; cur = cur.previousSibling) {
      if (cur === el2) return true;
    }
  } else return false;
};
```

```html
<!-- html -->
<ul>
    <li draggable="true" ondragend="dragEnd()" ondragover="dragOver(event)" ondragstart="dragStart(event)">first</li>
    <li draggable="true" ondragend="dragEnd()" ondragover="dragOver(event)" ondragstart="dragStart(event)">second</li>
    <li draggable="true" ondragend="dragEnd()" ondragover="dragOver(event)" ondragstart="dragStart(event)">third</li>
</ul>
```

([source](https://stackoverflow.com/questions/10588607/tutorial-for-html5-dragdrop-sortable-list))

---

## Exercises - Week 3

Recap the [generic instructions](./01-javascript-basics.md#generic-instructions-for-all-programming-tasks) for coding tasks.

Use the [WTMP Starter](https://github.com/mattpe/wtmp-starter) boilerplate as a starting point for all tasks.

### Task 1 - Events

1. Create a new branch called _week3-task1_ and checkout it
1. Implement the following small tasks (use code comments to describe tasks)
    1. Create a "game cheat code" like secret code feature, activated by typing secret password (record letter key presses in certain sequence). When a user types e.g. "hello", launch a response alert or something like that. (TIP: think about queue data structure)
    1. Create a function that shows the x and y coordinates of mouse double-clicks on the page
    1. Create an element that reacts (e.g. console.log something) to touches but not clicks
    1. Create a timer that tells user to "hurry up" after 15 secs of browsing
       - the notification should appear on the web page
    1. Create a timer that tells user to "hurry up" after 15 secs of idling (= not doing anything: mouse hasn't been moving, keyboard keys haven't been pressed...)
       - the notification should appear on the web page

1. Push task branch to Github and return direct link to Oma.

### Task 2 - Lunch menu - adapter pattern

1. Develop you lunch menu page further, continue [task 3](./02-javascript-week-2.md#task-3---dummy-lunch-menu-3---with-modules) from previous week
1. Create a new branch called _week3-task2_ and checkout it
1. Develop your Sodexo & Fazer menu modules furter
1. When importing menu data from Sodexo / Fazer menu module the data should look similar from application (_index.js_) point of view -> no need to use different functions for manipulating data in _index.js_. (It's basically an adapter design pattern implementation.)
    - in other words: both modules should provide similar menu data interface for your main app
1. Push task branch to Github and return direct link to Oma.
1. Deploy your lunch menu website to your home folder (_users.metropolia.fi_) and return link to Oma.
