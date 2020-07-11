# sa-axios
//Article with explanations
https://stackabuse.com/making-asynchronous-http-requests-in-javascript-with-axios/

///Text format

Making Asynchronous HTTP Requests in JavaScript with Axios
By  Janith Kasun • 0 Comments
Making Asynchronous HTTP Requests in JavaScript with Axios
Introduction
Axios is a Promised-based JavaScript library that is used to send HTTP requests. You can think of it as an alternative to JavaScript's native fetch() function.

We will be using features such as Promises, async/await, and other modern JavaScript design patterns in this tutorial. If you'd like to get up to speed or refresh your memory, you be interested in reading these articles before continuing:

This article uses the arrow notation introduced in ES2015 to define functions. You can read more about it on Arrow Functions in JavaScript article.
Axios is a Promised-based library. If you need to learn more about Promises, you can read our Promises in Node.js guide.
To improve our experience with Promises, we'll use Node.js async/await syntax. You can read our Node.js Async Await in ES7 article to master this feature!
In this tutorial, we will make GET, POST, PUT, and DELETE requests to a REST API using Axios. Let's learn a bit more about this library.

What is Axios?
Axios is a modern, Promise-based HTTP client library. This means that Axios is used to send an HTTP request and handle their responses, all using JavaScript's promises. Axios supports both Node.js and JavaScript in the browser.

Axios is also free and open-source. You can visit its GitHub Repository to see its code and documentation.

It comes built-in with some web security by protecting users against attacks such as Cross-Site Request Forgery (CSRF).

As a result of its features and ease of use, it's become a popular choice for JavaScript developers to use when making HTTP calls. Let's get started by setting up Axios.

Setting up Axios
Let's first create a new folder and initialize NPM with the default settings:

$ mkdir axios-tutorial
$ cd axios-tutorial
$ npm init -y
Next, we can use NPM to install the library:

$ npm i --save axios
Note: If you're using TypeScript in your project (for example with an Angular app), the Axios library comes bundled with its types definitions. You don't have to take an extra step to install types!

If you are on the browser, you can use a CDN to import the script as well.

<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
This tutorial uses Node.js with CommonJS to load our libraries. CommonJS is a standard for loading modules, in particular, it specifies the require() keyword to do so. The examples should be working regardless of the platform without any changes.

Now that we've set up Axios in our development environment, let's head straight into making HTTP requests.

Writing Asynchronous Requests With Axios
In Node.js, input and output activities like network requests are done asynchronously. As Axios uses Promises to make network requests, callbacks are not an option when using this library. We interact with Axios using Promises, or the async/await keywords which are an alternative syntax for using Promises.

Importing Axios
If you are using CommonJS, there are two methods in Node.js to import the library.

You can import the module in your code like this:

const axios = require('axios')
However, many IDE and code editors can offer better autocompletion when importing like this:

const axios = require('axios').default;
This works while using CommonJS to import modules. We recommend you use the second method as autocompletion and seeing code documentation in your IDE can make the development process easier.

With the library imported, we can start making HTTP requests.

Sending GET Requests
Let's send our first request with Axios! It will be a GET request, typically used to retrieve data.

We will make an HTTP request to an external API that sends us a list of blog posts. Upon receiving the data, we'll log it's contents to the console. If we encounter an error, we'll log that too.

Let's see how to make one using the default Promise syntax. In a new file called getRequestPromise.js, add the following code:

const axios = require('axios').default;

axios.get('https://jsonplaceholder.typicode.com/posts')
    .then(resp => {
        console.log(resp.data);
    })
    .catch(err => {
        // Handle Error Here
        console.error(err);
    });
To make a GET request, we pass the URL of the resource as the argument in the axios.get() method.

If you run this code with node getRequestPromise.js, you would see the following output:

[ { userId: 1,
    id: 1,
    title:
     'sunt aut facere repellat provident occaecati excepturi optio reprehenderit',
    body:
     'quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum
 est autem sunt rem eveniet architecto' },
  { userId: 1,
    id: 2,
    title: 'qui est esse',
    body:
     'est rerum tempore vitae\nsequi sint nihil reprehenderit dolor beatae ea dolores neque\nfugiat blanditiis voluptate porro ve
l nihil molestiae ut reiciendis\nqui aperiam non debitis possimus qui neque nisi nulla' },
...
Now let's see how we can rewrite the same code this using async/await keywords. In a new file getRequestAsyncAwait.js, add the following code:

const axios = require('axios');

const sendGetRequest = async () => {
    try {
        const resp = await axios.get('https://jsonplaceholder.typicode.com/posts');
        console.log(resp.data);
    } catch (err) {
        // Handle Error Here
        console.error(err);
    }
};

sendGetRequest();
To use the async/await syntax, we need to wrap the axios.get() function call within an async function. We encase the method call with a try...catch block so that we can capture any errors, similar to the catch() method we used in the Promise version. The variable that received the HTTP data had to use the await keyword to ensure the asynchronous data was received before continuing. From here on out, we'll only use the async/await syntax in our examples.

Running this code will print the same output to the console as the original Promise example.

An Axios response for an HTTP request (the resp object in the example) will contain the following information about the HTTP response:

data - The response body provided by the server. If the response from the server is a JSON, Axios will automatically parse data into a JavaScript object.
status - The HTTP status code from the response e.g. 200, 400, 404.
statusText - The HTTP status message from the server response e.g. OK, Bad Request, Not Found.
headers - The HTTP headers accompanying the response.
config - The configuration that was provided to the Axios API for the request.
request - The native request that generated the response. In Node.js this will be a ClientRequest object. In the browser, this will be an XMLHTTPRequest object.
Now that we've seen how to make a GET request with Axios, let's look at how to make a POST request.

Subscribe to our Newsletter
Get occassional tutorials, guides, and jobs in your inbox. No spam ever. Unsubscribe at any time.

Newsletter Signup
Enter your email...
Sending POST Requests
We send POST requests to create a new resource in a REST API. In this case, we will make a POST request with Axios to make a new blog post for a user.

Create a new file called postRequest.js and enter the following code:

const axios = require('axios').default;

const newPost = {
    userId: 1,
    title: 'A new post',
    body: 'This is the body of the new post'
};

const sendPostRequest = async () => {
    try {
        const resp = await axios.post('https://jsonplaceholder.typicode.com/posts', newPost);
        console.log(resp.data);
    } catch (err) {
        // Handle Error Here
        console.error(err);
    }
};

sendPostRequest();
To send a POST with axios.post() you must first supply the URL, and then provide the request data in the second argument. In this case, we are sending the data in the newPost variable, which will be sent to our API as JSON.

Running this with node postRequest.js produces the following successful result:

{ userId: 1,
  title: 'A new post',
  body: 'This is the body of the new post',
  id: 101 }
Let's move on to see how we can send PUT requests.

Sending PUT Requests
PUT requests are used to replace data at an endpoint. You can use the axios.put() method to send a PUT request in a similar fashion to how we send POST requests.

To see it in action, let's create a PUT request that updates the properties of the first blog post. Create a new file called putRequest.js with the code below:

const axios = require('axios').default;

const updatedPost = {
    id: 1,
    userId: 1,
    title: 'A new title',
    body: 'Update this post'
};

const sendPutRequest = async () => {
    try {
        const resp = await axios.put('https://jsonplaceholder.typicode.com/posts/1', updatedPost);
        console.log(resp.data);
    } catch (err) {
        // Handle Error Here
        console.error(err);
    }
};

sendPutRequest();
Like with POST, we provide the URL and the data we want to be uploaded. Running this with node putRequest.js gives us:

{ id: 1, userId: 1, title: 'A new title', body: 'Update this post' }
Now that we've covered two ways to upload data, let's look at how we can remove data.

Sending DELETE Requests
You can send an HTTP DELETE request using the axios.delete() method to remove data from a RESTful API.

Let's remove a blog post by sending a DELETE request with Axios. In a new file called deleteRequest.js, enter the following:

const axios = require('axios').default;

const sendDeleteRequest = async () => {
    try {
        const resp = await axios.delete('https://jsonplaceholder.typicode.com/posts/1')
        console.log(resp.data);
    } catch (err) {
        // Handle Error Here
        console.error(err);
    }
};

sendDeleteRequest();
The axios.delete() function only needs the URL of the resource we want to remove. Executing this program with node putRequest.js displays this in the terminal:

{}
This means no data was returned, which is fine when a resource is removed. However, as no error was thrown by Axios, we're pretty sure that it was processed correctly.

Let's have a look at an alternative way of sending Axios requests using configurations,

Configuring Requests
As an alternative to specifying the function to make the request, we can provide a JavaScript object that configures how Axios sends a request. For example, if I wanted to send a PUT request without using axios.put(), we can configure Axios like :

const axios = require('axios').default;

const sendRequest = async () => {
    try {
        const resp = await axios({
            method: 'PUT',
            url: 'https://jsonplaceholder.typicode.com/posts/1',
            data: {
                id: 1,
                userId: 1,
                title: 'A new title',
                body: 'Update this post'
            }
        });

        console.log(resp.data);
    } catch (err) {
        // Handle Error Here
        console.error(err);
    }
}

sendRequest();
In this case, we use axios as a function directly. We pass it a JavaScript function that contains the HTTP method being used in the method, the API endpoint in the url and any data in the request in the data property.

The end result is the same, so you can use this way of making requests if it appeals to you more.

Now that we have a handle on sending requests, let's modify them by setting custom headers.

Set Custom Headers
For certain APIs, a raw request needs to have additional data in headers to be processed. A common example would be to set headers that authenticate the HTTP request.

If we used JWTs for Authentication and Authorization, we would have to add it to our requests so it won't be rejected by the API server.

Let's see how we can add custom headers to a axios.get() method call:

const axios = require('axios').default;

const sendGetRequest = async () => {
    try {
        const resp = await axios.get('https://jsonplaceholder.typicode.com/posts', {
            headers: {
                'authorization': 'Bearer YOUR_JWT_TOKEN_HERE'
            }
        });

        console.log(resp.data);
    } catch (err) {
        // Handle Error Here
        console.error(err);
    }
};

sendGetRequest();
As you can see in this code example, we can pass the configuration with the headers property to set custom headers for the request. The headers property is a JavaScript object with string keys and values.

You can add this property to the other Axios methods such as axios.post(), axios.put(), axios.delete(). The headers property should be entered after the data object in axios.post() and axios.put().

Next, let's see how we can set a custom header using the Axios API configuration:

const axios = require('axios').default;

axios({
    method: 'GET',
    url: 'https://jsonplaceholder.typicode.com/posts',
    headers: {
        'authorization': 'Bearer YOUR_JWT_TOKEN_HERE'
    }
}).then(resp => {
    console.log(resp.data);
}).catch(err => {
    // Handle Error Here
    console.error(err);
});
In this case, the headers are just another property of the JavaScript object!

Conclusion
In this article, you learned how to create asynchronous HTTP requests with Axios in Node.js and browser JavaScript. You made requests with Axios methods - axios.get(), axios.post(), axios.put() and axios.delete(). You also used the Axios API to send HTTP requests by configuring a JavaScript object with the request details. Finally, you added custom headers in your requests.
