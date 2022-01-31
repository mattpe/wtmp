# JavaScript - Week 4

## Contents

- Networking
- Promises
- SOP & CORS
- Error handling

---

## Networking

[Eloquent JavaScript, Chapter 18](https://eloquentjavascript.net/18_http.html)

[Prerequisities: Basics of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)

### Fetch API

[MDN web docs](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

- [`fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) method provides a logical way to fetch resources asynchronously across the network
  - replacement functionality for the old `XMLHttpRequest`
- [Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) interface
- Note: the Promise returned from `fetch()` won’t reject on HTTP error status even if the response is an HTTP 404 or 500. Instead, it will resolve normally (with ok status set to false), and it will only reject on network failure or if anything prevented the request from completin

```js
fetch(`https://api.github.com/users/mattpe`);
```

### Promises

[MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)

- `Promise` objects are used to handle asynchronous operations in JavaScript.
- e.g. calling `fetch()` returns a Promise that represents the eventual completion or failure of the asynchronous operation
- The state of promise is one of the following:
  - _pending_: initial state, neither fulfilled nor rejected
  - _fulfilled_: meaning that the operation completed successfully
  - _rejected_: meaning that the operation failed
- [`Promise.all()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) method returns a single Promise that fulfills when all of the promises passed as an iterable parameter have been fulfilled

```js
// Basic fetch using promises
fetch(`https://api.github.com/users/mattpe`)
  .then(data => data.json())
  .then(data => console.log(data));

// Nested promises: 1. get user and url for repos. 2. get repos
fetch(`https://api.github.com/users/mattpe`)
  .then(data => data.json())
  .then(data => {
    fetch(data.repos_url)
      .then(data => data.json())
      .then(data => console.log(data));
  });

// Fetching user and repos endpoints simultaneously with Promise.all()
Promise.all([
  fetch(`https://api.github.com/users/mattpe`),
  fetch(`https://api.github.com/users/mattpe/repos`)
]).then(responses => {
  Promise.all(responses.map(response => response.json()))
    .then(responses => console.log(responses));
});
```

### Async - await

- [`async`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/async_function) functions and [`await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) operator provide a modern way to avoid long callback chains or deep nesting in asynchronous operations with promises a.k.a "Callback Hell" a.k.a "The Pyramid of Doom"
- Introduced in ES8 spec. (Set `"ecmaVersion": 8,` in `.eslintrc` config if using ESLint)

Reading:

- [Dev.to: JavaScript Fetch API and using Async/Await](https://dev.to/shoupn/javascript-fetch-api-and-using-asyncawait-47mp)
- [Hackernoon: Should I use Promises or Async-Await](https://hackernoon.com/should-i-use-promises-or-async-await-126ab5c98789)
- [Hello.js: Asynchronous JavaScript: From Callback Hell to Async and Await](https://blog.hellojs.org/asynchronous-javascript-from-callback-hell-to-async-and-await-9b9ceb63c8e8)

```js
// Basic fetch as async function
const getGithubReposOfUser = async (userName) => {
  let response = await fetch(`https://api.github.com/users/${userName}/repos`);
  let repos = await response.json();
  return repos;
};
getGithubReposOfUser('mattpe')
  .then(data => console.log(data));
```

### Same-origin policy (SOP)

[MDN web docs](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)

- Is a security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin
- Implemented in the browser, follows the rules (cors) obtained from the server

Example source url: http://www.example.com/dir/page.html

| (`fetch()`) Target URL  | Outcome | Reason |
|--------------|---------|--------|
| http://www.example.com/dir/page2.html	| Success | Same protocol, host and port |
| http://www.example.com/dir2/other.html | Success | Same protocol, host and port |
| http://username:password@www.example.com/dir2/other.html | Success | Same protocol, host and port |
| http://www.example.com:81/dir/other.html | Failure | Same protocol and host but different port |
| https://www.example.com/dir/other.html | Failure | Different protocol |
| http://en.example.com/dir/other.html | Failure | Different host |
| http://example.com/dir/other.html | Failure | Different host (exact match required) |
| http://v2.www.example.com/dir/other.html | Failure | Different host (exact match required) |
| http://www.example.com:80/dir/other.html | Depends | Port explicit. Depends on implementation in browser. |

### Cross-Origin Resource Sharing (CORS)

[MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

- a mechanism that uses additional HTTP headers to tell browsers to give a web application running at one origin, access to selected resources from a different origin.
- standard that extends HTTP with a Origin request header and a Access-Control-Allow-Origin response header

### Bypassing SOP

- If hosting the server or serverside application by yourself, just [enable cors](https://enable-cors.org/)
- Use a proxy server
  - setup your own (google for simple php/node http proxy, [one example](./99-php-http-proxy.md))
  - test with public ones (e.g. <https://cors-anywhere.herokuapp.com/>)
  - think about data security
- [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)

- Example of unwanted SOP Bypass: [Security Boulevard: Bypassing SOP Using the Browser Cache](https://securityboulevard.com/2019/04/bypassing-sop-using-the-browser-cache/)

---

## Error/exception handling

[Eloquent JavaScript, Chapter 8](https://eloquentjavascript.net/08_error.html)

- [`try ... catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)
  - `try {...}` block: statements to be executed.
  - `catch {...}` block: statements to be executed if an exception is thrown in the try-block.
  - `finally {...}` block: statements to be executed after the try-block and catch-block execute. The statements in the finally-block execute even if no catch-block handles the exception.
- [Error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error) objects are thrown when runtime errors occur and they hold data of the exception (name, message, source file, linenumber...)
- [`throw`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/throw) can be used to generate user-defined exceptions
- in JS, terms _Error_ and _Exception_ are basically synonymous
  - in some other languages _Errors_ are considered more serious problems that should occur at all -> should not be handled with _try-catch_

```js
// Async function with error handling
const getGithubReposOfUser = async (userName) => {
  let response;
  try {
    response = await fetch(`https://api.github.com/users/${userName}/repos`);
    if (!response.ok) {
      throw new Error(`HTTP ${response.status} ${response.statusText}`);
    }
  } catch (error) {
    console.error('getGithubReposOfUser error', error.message);
  }
  let repos = await response.json();
  return repos;
};
getGithubReposOfUser('mattpe')
  .then(data => console.log(data));
```

---

## Exercises - Week 4

Recap the [generic instructions](./01-javascript-basics.md#generic-instructions-for-all-programming-tasks) for coding tasks.

Use the [WTMP Starter](https://github.com/mattpe/wtmp-starter) boilerplate as a starting point for all tasks.

### Task 1 - Lunch menu with real data

1. Develop your lunch menu page further
1. Create a new branch called _week4-task1_ and checkout it
1. Add a new module for network features
    - provides generic reusable network functions to other modules and main app (_index.js_)
    - use Fetch API
    - use try-catch for error handling
1. Fetch lunch menu data from web sources, example urls:
    - [Sodexo Leiritie](https://www.sodexo.fi/ravintolat/helsinki/metropolia-leiritie)
        - <https://www.sodexo.fi/ruokalistat/output/daily_json/152/YYYY-MM-DD>
        - <https://www.sodexo.fi/ruokalistat/output/weekly_json/152>
    - [Fazer Karaportti](https://www.foodandco.fi/ravintolat/Ravintolat-kaupungeittain/espoo/metropolia/)
        - <https://www.foodandco.fi/api/restaurant/menu/week?language=fi&restaurantPageId=270540&weekDate=2022-02-01>
        - simple version: subscribe -> json: <https://www.foodandco.fi/modules/json/json/Index?costNumber=3208&language=en>
    - use e.g. [your own PHP proxy](./99-php-http-proxy.md) (preferred) or open services like [allOrigins]()https://allorigins.win/ or [cors-anywhere](https://cors-anywhere.herokuapp.com/) proxy server for testing
1. Publish a demo at users.metropolia.fi/~_MY-ACCOUNT_/wtmp-week4-task-1
1. Push task branch to Github and return direct link to Oma.

### Task 2 - Lunch menu search bar

1. Develop your lunch menu page further
1. Create a new branch called _week4-task2_ and checkout it
1. Design and implement functionality for lunch menu page's search field.
1. Some ideas:
    - search from already downloaded content
    - provide autofill/autocomplete feature using keywords
    - fetch menu content for livesearch
    - use UI events from the `<input>` field
1. Write a short description about features you implemented
1. Publish a demo at users.metropolia.fi/~_MY-ACCOUNT_/wtmp-week4-task-2
1. Push task branch to Github and return direct link to Oma.
