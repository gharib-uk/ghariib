---
title: "How to Use JavaScript Fetch API Step-by-Step Guide with Examples"
date: 2025-02-07T03:28:48.325Z
draft: false
type: posts
categories: 
- 
---
# How to Use JavaScript Fetch API Step-by-Step Guide with Examples

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

There was a time when `XMLHttpRequest` was used to make API requests. It didn’t include Promises, and it didn’t make for clean JavaScript code. Using jQuery, you could use the cleaner syntax of `jQuery.ajax()`.

Now, JavaScript has its own built-in way to make API requests. This is the Fetch API, a new standard to make server requests with Promises, but which also includes additional features.

In this tutorial, you will create both GET and POST requests using the Fetch API.

Deploy your frontend applications from GitHub using [DigitalOcean App Platform](/products/app-platform). Let DigitalOcean focus on scaling your app.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

To complete this tutorial, you will need the following:

-   A local development environment for Node.js. Follow [How to Install Node.js and Create a Local Development Environment](/community/tutorial_series/how-to-install-node-js-and-create-a-local-development-environment).
-   A basic understanding of coding in JavaScript, which you can learn more about from the [How to Code in JavaScript](/community/tutorial_series/how-to-code-in-javascript) series.
-   An understanding of Promises in JavaScript. Read the [Promises section](/community/tutorials/understanding-the-event-loop-callbacks-promises-and-async-await-in-javascript#promises) of this article on [the event loop, callbacks, Promises, and async/await](/community/tutorials/understanding-the-event-loop-callbacks-promises-and-async-await-in-javascript) in JavaScript.

[Step 1 — Getting Started with Fetch API Syntax](#step-1-getting-started-with-fetch-api-syntax)[](#step-1-getting-started-with-fetch-api-syntax)
------------------------------------------------------------------------------------------------------------------------------------------------

One approach to using the Fetch API is by passing `fetch()` the URL of the API as a parameter:

```
fetch(url)
```

The `fetch()` method returns a Promise. After the `fetch()` method, include the Promise method `then()`:

```
fetch(url)
  .then(function() {
    // handle the response
  })
```

If the Promise returned is `resolve`, the function within the `then()` method is executed. That function contains the code for handling the data received from the API.

After the `then()` method, include the `catch()` method:

```
fetch(url)
  .then(function() {
    // handle the response
  })
  .catch(function() {
    // handle the error
  });
```

The API you call using `fetch()` may be down or other errors may occur. If this happens, the `reject` promise will be returned. The `catch` method is used to handle `reject`. The code within `catch()` will be executed if an error occurs when calling the API of your choice.

With an understanding of the syntax for using the Fetch API, you can now move on to using `fetch()` on a real API.

[Step 2 — Using Fetch to get Data from an API](#step-2-using-fetch-to-get-data-from-an-api)[](#step-2-using-fetch-to-get-data-from-an-api)
------------------------------------------------------------------------------------------------------------------------------------------

The following code samples will be based on the [JSONPlaceholder API](https://jsonplaceholder.typicode.com/). Using the API, you will get ten users and display them on the page using JavaScript. This tutorial will retrieve data from the JSONPlaceholder API and display it in list items inside the author’s list.

Begin by creating an HTML file and adding a heading and unordered list with the `id` of `authors`:

authors.html

```
<h1>Authors</h1>
<ul id="authors"></ul>
```

Now add `script` tags to the bottom of your HTML file and use a DOM selector to grab the `ul`. Use [`getElementById`](/community/tutorials/how-to-access-elements-in-the-dom) with `authors` as the argument:

authors.html

```
<h1>Authors</h1>
<ul id="authors"></ul>

<script>
  const ul = document.getElementById('authors');
</script>
```

Remember, `authors` is the `id` for the previously created `ul`.

Next, create a `list` that is a [`DocumentFragment`](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment):

authors.html

```
<script>
  // ...

  const list = document.createDocumentFragment();
</script>
```

All the appended list items will be added to `list`. A `DocumentFragment` is not part of the active document tree structure. This has the benefit of not causing performance-affecting redraws when the Document Object Model is changed.

Create a constant variable called `url` which will hold the API URL that will return ten random users:

authors.html

```
<script>
  // ...

  const url = 'https://jsonplaceholder.typicode.com/users';
</script>
```

Now using the Fetch API, call the JSONPlaceholder API using `fetch()` with `url` as the argument:

authors.html

```
<script>
  // ...

  fetch(url)
</script>
```

You are calling the Fetch API and passing in the URL to the JSONPlaceholder API. Then a response is received. However, the response you get is not JSON, but an object with a series of methods that can be used depending on what you want to do with the information. To convert the object returned into JSON, use the `json()` method.

Add the `then()` method which will contain a function with a parameter called `response`:

authors.html

```
<script>
  // ...

  fetch(url)
    .then((response) => {})
</script>
```

The `response` parameter takes the value of the object returned from `fetch(url)`. Use the `json()` method to convert `response` into JSON data:

authors.html

```
<script>
  // ...

  fetch(url)
    .then((response) => {
      return response.json();
    })
</script>
```

The JSON data still needs to be processed. Add another `then()` statement with a function that has an argument called `data`:

authors.html

```
<script>
  // ...

  fetch(url)
    .then((response) => {
      return response.json();
    })
    .then((data) => {})
</script>
```

Within this function, create a variable called `authors` that is set equal to `data`:

authors.html

```
<script>
  // ...

  fetch(url)
    .then((response) => {
      return response.json();
    })
    .then((data) => {
      let authors = data;
    })
</script>
```

For each author in `authors`, you will want to create a list item that displays their name. The [`map()` method](/community/tutorials/list-processing-with-map-filter-and-reduce) is suited for this pattern:

authors.html

```
<script>
  // ...

  fetch(url)
    .then((response) => {
      return response.json();
    })
    .then((data) => {
      let authors = data;

      authors.map(function(author) {

      });
    })
</script>
```

Within your `map` function, create a variable called `li` that will be set equal to [`createElement`](/community/tutorials/how-to-make-changes-to-the-dom) with `li` (the HTML element) as the argument. Also, create an `h2` for `name` and a `span` for `email`:

authors.html

```
<script>
  // ...

  fetch(url)
    .then((response) => {
      return response.json();
    })
    .then((data) => {
      let authors = data;

      authors.map(function(author) {
        let li = document.createElement('li');
        let name = document.createElement('h2');
        let email = document.createElement('span');
      });
    })
</script>
```

The `h2` element will contain the `name` of the `author`. The `span` element will contain the email of the `author`. The `innerHTML` property and string interpolation will allow you to do this:

authors.html

```
<script>
  // ...

  fetch(url)
    .then((response) => {
      return response.json();
    })
    .then((data) => {
      let authors = data;

      authors.map(function(author) {
        let li = document.createElement('li');
        let name = document.createElement('h2');
        let email = document.createElement('span');

        name.innerHTML = `${author.name}`;
        email.innerHTML = `${author.email}`;
      });
    })
</script>
```

Next, connect these DOM elements with [`appendChild`](/community/tutorials/how-to-make-changes-to-the-dom):

authors.html

```
<script>
  // ...

  fetch(url)
    .then((response) => {
      return response.json();
    })
    .then((data) => {
      let authors = data;

      authors.map(function(author) {
        let li = document.createElement('li');
        let name = document.createElement('h2');
        let email = document.createElement('span');

        name.innerHTML = `${author.name}`;
        email.innerHTML = `${author.email}`;

        li.appendChild(name);
        li.appendChild(email);
        list.appendChild(li);
      });
    })

  ul.appendChild(list);
</script>
```

Note that each list item is being appended to the `DocumentFragment` `list`. Once the `map` is complete, the `list` is appended to the `ul` unordered list element.

With both `then()` functions completed, you can now add the `catch()` function. This function will log the potential error to the console:

authors.html

```
<script>
  // ...

  fetch(url)
    .then((response) => {
      // ...
    })
    .then((data) => {
      // ...
    })
    .catch(function(error) {
      console.log(error);
    });

  // ...
</script>
```

This is the full code of the request you created:

authors.html

```
<h1>Authors</h1>
<ul id="authors"></ul>

<script>
  const ul = document.getElementById('authors');
  const list = document.createDocumentFragment();
  const url = 'https://jsonplaceholder.typicode.com/users';

  fetch(url)
    .then((response) => {
      return response.json();
    })
    .then((data) => {
      let authors = data;

      authors.map(function(author) {
        let li = document.createElement('li');
        let name = document.createElement('h2');
        let email = document.createElement('span');

        name.innerHTML = `${author.name}`;
        email.innerHTML = `${author.email}`;

        li.appendChild(name);
        li.appendChild(email);
        list.appendChild(li);
      });
    }).
    .catch(function(error) {
      console.log(error);
    });

  ul.appendChild(list);
</script>
```

You just successfully performed a GET request using the JSONPlaceholder API and the Fetch API. In the next step, you will perform POST requests.

[Step 3 — Handling POST Requests](#step-3-handling-post-requests)[](#step-3-handling-post-requests)
---------------------------------------------------------------------------------------------------

Fetch defaults to GET requests, but you can use all other types of requests, change the headers, and send data. Let’s create a POST request.

First, include a constant variable that holds the link to the JSONPlaceholder API:

new-author.js

```
const url = 'https://jsonplaceholder.typicode.com/users';
```

Next, you need to set your object and pass it as the second argument of the fetch function. This will be an object called `data` with the key `name` and value `Sammy` (or your name):

new-author.js

```
// ...

let data = {
  name: 'Sammy'
}
```

Since this is a POST request, you will need to state that explicitly. Create an object called `fetchData`:

new-author.js

```
// ...

let fetchData = {

}
```

This object needs to include three keys: `method`, `body`, and `headers`:

new-author.js

```
// ...

let fetchData = {
  method: 'POST',
  body: JSON.stringify(data),
  headers: new Headers({
    'Content-Type': 'application/json; charset=UTF-8'
  })
}
```

The `method` key will have the value `'POST'`. `body` will be set equal to the [`JSON.stringify()`](/community/tutorials/how-to-work-with-json-in-javascript) format of the `data` object that was just created. `headers` will have the value of `'Content-Type': 'application/json; charset=UTF-8'`.

The `Headers` interface is a property of the Fetch API, which allows you to perform actions on HTTP request and response headers. This article called [How To Define Routes and HTTP Request Methods in Express](/community/tutorials/nodejs-express-routing) can provide you with more information.

With this code in place, the POST request can be made using the Fetch API. You will include `url` and `fetchData` as arguments for your `fetch` POST request:

new-author.js

```
// ...

fetch(url, fetchData)
```

The `then()` function will include code that handles the response received from the JSONPlaceholder API:

new-author.js

```
// ...

fetch(url, fetchData)
  .then(function() {
    // Handle response you get from the API
  });
```

This is the full code of the request you created:

new-author.js

```
const url = 'https://jsonplaceholder.typicode.com/users';

let data = {
  name: 'Sammy'
}

let fetchData = {
  method: 'POST',
  body: JSON.stringify(data),
  headers: new Headers({
    'Content-Type': 'application/json; charset=UTF-8'
  })
}

fetch(url, fetchData)
  .then(function() {
    // Handle response you get from the API
  });
```

Alternatively, you can pass `fetch()` a [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request) object.

new-author-request.js

```
const url = 'https://jsonplaceholder.typicode.com/users';

let data = {
  name: 'Sammy'
}

let request = new Request(url, {
  method: 'POST',
  body: JSON.stringify(data),
  headers: new Headers({
    'Content-Type': 'application/json; charset=UTF-8'
  })
});

fetch(request)
  .then(function() {
    // Handle response you get from the API
  });
```

With this approach, `request` can be used as the sole argument for `fetch()`, replacing `url` and `fetchData`.

Now you know two methods for creating and executing POST requests with the Fetch API.

[Comparison with Axios and Framework Integrations](#comparison-with-axios-and-framework-integrations)[](#comparison-with-axios-and-framework-integrations)
----------------------------------------------------------------------------------------------------------------------------------------------------------

The Fetch API is a modern and flexible interface for making network requests in JavaScript. It is promise-based, making it easier to handle asynchronous operations efficiently. However, it is not the only option for making network requests in JavaScript.

[Axios](https://axios-http.com/docs/intro) is a popular library for making HTTP requests in JavaScript. It is promise-based and has a simple and clean API. It also provides the ability to intercept requests and responses, transform data, and cancel requests.

Many JavaScript frameworks, such as [React](https://reactjs.org/), [Vue.js](https://vuejs.org/), and [Angular](https://angular.io/), have their own built-in methods for making network requests. These methods are often based on the Fetch API or Axios, but they may have additional features or be more tightly integrated with the framework’s ecosystem.

If you’re working on a simple project and prefer a lightweight, native solution, use Fetch API. However, for projects requiring automatic JSON parsing, interceptors, and better error handling, Axios is the better choice.

Feature

Fetch API

Axios

Native Support

Built-in in JavaScript

Requires installation (`npm install axios`)

Response Handling

Needs response.json() to parse JSON

Auto-parses JSON responses

Error Handling

Requires manual error handling

Better built-in error handling

Request Cancellation

Not built-in (needs AbortController)

Supports request cancellation

Interceptors

Not supported

Supports request/response interceptors

You can check out [How to Use Vue.js and Axios to Display Data from an API](/community/tutorials/how-to-use-vue-js-and-axios-to-display-data-from-an-api) for an Axios-based approach.

[Using Fetch API in JavaScript Frameworks](#using-fetch-api-in-javascript-frameworks)[](#using-fetch-api-in-javascript-frameworks)
----------------------------------------------------------------------------------------------------------------------------------

### [1\. Fetch API in React](#1-fetch-api-in-react)[](#1-fetch-api-in-react)

React applications often use Fetch API inside useEffect() to fetch data when a component mounts:

```
import { useState, useEffect } from 'react';

function App() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(json => setData(json))
      .catch(error => console.error('Error fetching:', error));
  }, []);

  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}

export default App;
```

For better performance in React, consider using [JavaScript Performance API](/community/tutorials/js-js-performance-api).

### [2\. Fetch API in Vue.js](#2-fetch-api-in-vue-js)[](#2-fetch-api-in-vue-js)

In Vue.js, Fetch API is commonly used inside the `mounted()` lifecycle hook:

```
<script>
export default {
  data() {
    return { data: null };
  },
  async mounted() {
    try {
      const response = await fetch('https://api.example.com/data');
      this.data = await response.json();
    } catch (error) {
      console.error('Error fetching:', error);
    }
  }
};
</script>
```

Alternatively, many Vue.js projects prefer using Axios for its simplicity, as shown in [How to Use Vue.js and Axios to Display Data from an API](/community/tutorials/how-to-use-vue-js-and-axios-to-display-data-from-an-api).

### [3\. Fetch API in Angular](#3-fetch-api-in-angular)[](#3-fetch-api-in-angular)

In Angular, Fetch API can be used within services using `HttpClient`, but if using native Fetch API, you can implement it inside a component:

```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-data',
  template: '<p>{{ data | json }}</p>'
})
export class DataComponent implements OnInit {
  data: any;

  async ngOnInit() {
    try {
      const response = await fetch('https://api.example.com/data');
      this.data = await response.json();
    } catch (error) {
      console.error('Error fetching:', error);
    }
  }
}
```

For large applications, Angular’s built-in `HttpClientModule` is recommended for better scalability.

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. What does Fetch API do in JavaScript?](#1-what-does-fetch-api-do-in-javascript)[](#1-what-does-fetch-api-do-in-javascript)

The **Fetch API** provides a modern and flexible interface for making network requests in JavaScript. It allows you to fetch resources like JSON data, HTML, images, and more from a server. Unlike older methods like XMLHttpRequest, Fetch API is **[promise-based](https://java-design-patterns.com/patterns/promise/)**, making it easier to handle asynchronous operations efficiently.

### [2\. What is an example of Fetch API?](#2-what-is-an-example-of-fetch-api)[](#2-what-is-an-example-of-fetch-api)

A simple example of using Fetch API to request JSON data from an API:

```
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error fetching data:', error));
```

This fetches a sample post from a placeholder API and logs it to the console. You can also check out [How to Use Vue.js and Axios to Display Data from an API](/community/tutorials/how-to-use-vue-js-and-axios-to-display-data-from-an-api) for another way to retrieve and display API data.

### [3\. How to fetch JSON data from an API in JavaScript?](#3-how-to-fetch-json-data-from-an-api-in-javascript)[](#3-how-to-fetch-json-data-from-an-api-in-javascript)

Fetching JSON data using Fetch API follows a simple pattern:

```
fetch('https://api.example.com/data')
  .then(response => response.json()) 
  .then(jsonData => console.log(jsonData))
  .catch(error => console.error('Error fetching data:', error));
```

This converts the response to JSON using `.json()` and then processes the data. If you’re working with performance optimizations, you may also find [JavaScript Performance API](/community/tutorials/js-js-performance-api) useful.

### [4\. How to fetch data from an API with JavaScript?](#4-how-to-fetch-data-from-an-api-with-javascript)[](#4-how-to-fetch-data-from-an-api-with-javascript)

o fetch data asynchronously, use `fetch()` inside an `async` function with `await`:

```
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}

fetchData();
```

This ensures cleaner code and better error handling. For advanced API integrations, consider learning about [GraphQL API](/community/tutorials/an-introduction-to-graphql) as an alternative to REST APIs.

### [5\. What is the difference between REST API and Fetch API?](#5-what-is-the-difference-between-rest-api-and-fetch-api)[](#5-what-is-the-difference-between-rest-api-and-fetch-api)

Feature

REST API

Fetch API

Architecture

REST API is an architectural style used for designing networked applications.

Fetch API is a JavaScript interface used to make HTTP requests.

HTTP Methods

REST API relies on HTTP methods (GET, POST, PUT, DELETE, etc.) to access resources.

Fetch API also supports these HTTP methods.

Resource Access

REST API is used to access resources.

Fetch API can be used to access a REST API or other network resources.

In simpler terms, Fetch API is a tool used to interact with a REST API or any other data source available over the web.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

While the Fetch API is not yet supported by all the browsers, it is a great alternative to `XMLHttpRequest`.

This tutorial provides a step-by-step guide on using Fetch API in JavaScript. However, if you’re working on a larger project, you may want to explore Axios for better error handling or GraphQL for more efficient data fetching.

### [Next Steps](#next-steps)[](#next-steps)

-   Learn how to optimize API performance with [JavaScript Performance API](/community/tutorials/js-js-performance-api).
-   Explore [GraphQL](/community/tutorials/an-introduction-to-graphql) for an alternative to REST APIs.
-   Read [How to Use Vue.js and Axios to Display Data from an API](/community/tutorials/how-to-use-vue-js-and-axios-to-display-data-from-an-api) for a comparison with Axios.

By integrating these concepts, you can efficiently fetch and manage data in any JavaScript project.

If you would like to learn how to call Web APIs using React, [check out this article](/community/tutorials/how-to-call-web-apis-with-the-useeffect-hook-in-react) on this very topic.

#### [Source](https://www.digitalocean.com/community/tutorials/how-to-use-the-javascript-fetch-api-to-get-data)

<br/>
---
