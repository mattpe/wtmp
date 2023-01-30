# JavaScript - Week 3

## Contents

- Light intro to design patterns
- Asynchronous code
- Events
- Progressive Web Apps

---

## Design Patterns

[Wikipedia: Software design pattern](https://en.wikipedia.org/wiki/Software_design_pattern)

> In software engineering, a software design pattern is a general, reusable solution to a commonly occurring problem within a given context in software design. It is not a finished design that can be transformed directly into source or machine code. Rather, it is a description or template for how to solve a problem that can be used in many different situations. Design patterns are formalized best practices that the programmer can use to solve common problems when designing an application or system.

> Design patterns may be viewed as a structured approach to computer programming intermediate between the levels of a programming paradigm and a concrete algorithm.

### Design patterns and JS

- [Module pattern](https://en.wikipedia.org/wiki/Module_pattern): [ES modules](./02-javascript-week-2.md#modules)
- [Factory pattern](https://en.wikipedia.org/wiki/Factory_method_pattern): covered later (with JS classes)
- [Observer pattern](https://en.wikipedia.org/wiki/Observer_pattern): publish - subscribe, events
- [Adapter pattern](https://en.wikipedia.org/wiki/Adapter_pattern): makes two incompatible interfaces to work together, see [task 2](#task-2---lunch-menu---adapter-pattern)
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
- [`EventTarget.addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) method is used to bind certain type of an event to a function that will be called whenever the specified event is delivered to the target
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

## Progressive Web App (PWA)

[MDN Progressive web apps](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)

- "Types" of applications used in mobile devices
  - Web site -> web app (single page app, SPA) -> **PWA** -> hybrid app -> native app

- Deliver app-like experiences on the web
- Umbrella term for new technologies that allow delivering fast and engaging user experiences with a website
- In order to call a Web App a PWA, it should have the following technical features:
  - **Secure contexts**: The application must be served over a secure network. Most of the features related to a PWA such as geolocation and even service workers are available only once the app has been loaded using HTTPS.
  - **Service workers**: A script that allows intercepting and control of how a web browser handles its network requests and asset caching. Provides faster loading times and offline features.
  - **Manifest file**:  A JSON file that describes the name of the app, the start URL, icons, and all of the other details necessary to transform the website into an app-like format.

- Key features:
  - _Progressive_ - Work for every user, regardless of browser choice because they’re built with progressive enhancement as a core tenant.
  - _Responsive_ - Fit any form factor, desktop, mobile, tablet, or whatever is next.
  - _Connectivity independent_ - Enhanced with service workers to work offline or on low quality networks.
  - _App-like_ - Use the app shell model to provide app-style navigations and interactions.
  - _Fresh_ - Always up-to-date thanks to the service worker update process.
  - _Safe_ - Served via TLS to prevent snooping and ensure content hasn’t been tampered with.
  - _Discoverable_ - Are identifiable as “applications” thanks to W3C manifests and service worker registration scope allowing search engines to find them.
  - _Re-engageable_ - Make re-engagement easy through features like push notifications.
  - _Installable_ - Allow users to keep apps they find most useful on their home screen without the hassle of an app store.
  - _Linkable_ - Easily shared via URL and no complex installation required

### The manifest file

[MDN Web app manifests](https://developer.mozilla.org/en-US/docs/Web/Manifest)

- provides information about a web application, including:
  - application name, author, icon(s), version, description, list of all the necessary resources, etc.
- uses JSON text file format
- necessary for the web app to be downloaded and be presented to the user similarly to a native app: icon installed on the homescreen of a device

```json
{
  "name": "HackerWeb",
  "short_name": "HackerWeb",
  "start_url": ".",
  "display": "standalone",
  "background_color": "#fff",
  "description": "A readable Hacker News app.",
  "icons": [{
    "src": "images/touch/homescreen48.png",
    "sizes": "48x48",
    "type": "image/png"
  }, {
    "src": "images/touch/homescreen72.png",
    "sizes": "72x72",
    "type": "image/png"
  }, {
    "src": "images/touch/homescreen96.png",
    "sizes": "96x96",
    "type": "image/png"
  }, {
    "src": "images/touch/homescreen144.png",
    "sizes": "144x144",
    "type": "image/png"
  }, {
    "src": "images/touch/homescreen168.png",
    "sizes": "168x168",
    "type": "image/png"
  }, {
    "src": "images/touch/homescreen192.png",
    "sizes": "192x192",
    "type": "image/png"
  }],
  "related_applications": [{
    "platform": "play",
    "url": "https://play.google.com/store/apps/details?id=cheeaun.hackerweb"
  }]
}
```

Example usage in HTML `<head>` element:

```html
<link rel="manifest" href="manifest.json">
<!-- or -->
<link rel="manifest" href="/app.webmanifest" crossorigin="use-credentials">
```

### Workbox

<https://developer.chrome.com/docs/workbox/>

Workbox is a library that bakes in a set of best practices and removes the boilerplate every developer writes when working with service workers.

- Precaching
- Runtime caching
- Strategies
- Request routing
- Background sync
- Helpful debugging

[A plugin](https://developer.chrome.com/docs/workbox/modules/workbox-webpack-plugin/) for webpack available.

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
1. Develop your Sodexo & Fazer menu modules further
1. When importing menu data from Sodexo / Fazer menu module the data should look similar from application (_index.js_) point of view -> no need to use different functions for manipulating data in _index.js_. (It's basically an adapter design pattern implementation.)
    - in other words: both modules should provide similar menu data interface for your main app
1. Push task branch to Github and return direct link to Oma.
1. Deploy your lunch menu website to your home folder (_users.metropolia.fi_) and return link to Oma.

### Task 3 - Lunch menu - first PWA

1. Develop you lunch menu web app further, continue previous task 2
1. Create a new branch called _week3-task3_ and checkout it
1. Follow the instructions at to add pwa support to webpack config: <https://webpack.js.org/guides/progressive-web-application/>
    - Install Workbox webpack plugin `npm install workbox-webpack-plugin --save-dev`
    - Add the plugin into the `plugins: [ ... ]` array in your webpack config file (common or production only):

      ```js
      const WorkboxPlugin = require('workbox-webpack-plugin');
      ...
        new WorkboxPlugin.GenerateSW({
          // these options encourage the ServiceWorkers to get in there fast
          // and not allow any straggling "old" SWs to hang around
          clientsClaim: true,
          skipWaiting: true,
        }),
      ...
      ```

    - Register the generated service worker in your `index.js`:

      ```js
      if ('serviceWorker' in navigator) {
        window.addEventListener('load', () => {
          navigator.serviceWorker.register('./service-worker.js').then(registration => {
            console.log('SW registered: ', registration);
          }).catch(registrationError => {
            console.log('SW registration failed: ', registrationError);
          });
        });
      }
      ```

1. Test the service worker: create a build, copy the `dist/` folder to your `public_html/` and test that your web app/site works offline too
    - open the url address using _https_ protocol
    - use dev tools offline mode to simulate network failure
    - refresh the page

1. Create a manifest file
    - install [webpack plugin](https://www.npmjs.com/package/webpack-pwa-manifest) `npm install --save-dev webpack-pwa-manifest`
    - Add the plugin into the `plugins: [ ... ]` array in your webpack config file (common or production only):

      ```js
      const WebpackPwaManifest = require('webpack-pwa-manifest');
      ...
      ...
      new WebpackPwaManifest({
        name: 'Lunch Progressive Web App',
        short_name: 'LunchPWA',
        description: 'Describe your Progressive Web App here',
        background_color: '#ffffff',
        crossorigin: 'use-credentials',
        publicPath: '.',
        icons: [
          {
            src: path.resolve('src/assets/icon.png'),
            sizes: [96, 128, 192, 256, 384, 512]
          },
        ]
      })
      ...
      ```

1. Add `icon.png` file into `assets/` folder. Any png with size 512x512 pixels will do
1. Use your responsive/mobile html/css layout
1. Build and publish your app: (`npm run build`), upload contents of the `dist/` folder to the web server (e.g. _users.metropolia.fi_) and test home screen installation with a mobile device
    - use _https_ protocol and a PWA capable browser (e.g.Chrome)
    - NOTE: check that the relative paths of _manifest.xxxxx.json_ in _dist/index.html_ and icon files in _manifest.xxxxx.json_ are injected/generated correctly before uploading

**Note:** When you want to continue developing your app's features without the PWA functionality e.g. in other branch, you must clear browser's cache and unregister the service worker. Check Chrome dev tools docs: [Debug Progressive Web Apps](https://developer.chrome.com/docs/devtools/progressive-web-apps/)

**Tip:** If you need to pass values from Webpack config to your application code (e.g. for conditional registering of service workers), use [DefinePlugin](https://webpack.js.org/plugins/define-plugin/).

Check also: [Learn PWA](https://web.dev/learn/pwa/)
