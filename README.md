<img src="https://s3.amazonaws.com/devmountain/readme-logo.png" width="250" align="right">

# Project Summary

In this project we're going to build the backend service (API) for an ecommerce site. We'll be able to display products, search for products, and retrieve a specific product. There will be some accelerator content at the end to help add additional challenges.

The goal is to better understand how the client (frontend application) and server (backend application) interact with each other in the `request -> response cycle`. This includes generating a request on the client, receiving and parsing it on the server, and sending the response back to the client.

## Setup

* `Fork` this repository
* `Clone` your `fork`

## Step 1

### Summary

We're starting from a clean slate. We'll be building our backend service from start to finish. Often, you'll have some boilerplate code to start with. `Boilerplate` is a term that describes starter or framework code. For instance, `create-react-app` generates boiler plate code for us, things like our `package.json`, `node_modules`, the `src` directory, etc. Here we won't have any of that so we'll need to create it ourselves.

### Instructions

* Start this project by running `npm init -y`
    * Review the `package.json` that was created
    * We'll now be able to utilize packages from the NPM store
* Create a new directory called `server`
    * Inside server create an `index.js`
    * This is where we'll architect our server

## Step 2

### Summary

Now that we have our foundation set up, let's start building out our ecommerce API. In this step, we'll setup and test the initial framework for our server.

### Instructions

* Start by installing Express
    * `npm install express`
    * This command will generate a `node_modules` folder
        * node_modules shouldn't be committed to github because they're something we'll download again later if needed
        * create a `.gitignore` file in the root of the directory
            * add `node_modules/` to your .gitignore
* Next, build the skeleton of your server
    * This should include:
        * Requiring express
        * Declaring your `app` variable and setting it equal to express invoked
        * Declaring the port you want your server to `listen` on
        * Invoking `app.listen`:
            * Provide it the port variable from above
            * And a callback with a console log so we know our server is running and ready to receive requests
        * Finally, go to your terminal/git bash
            * Type `pwd` to make sure you're in the root directory
                * It should look something like this `/Users/yourname/DevMountain/node/node-1-afternoon`
                * If you're not, navigate back to the root of the project directory
            * From there, run the following command `nodemon server/index.js`
            * You should see the `console.log` message from your `listen` method in the console

    ### Solution
    <details>

    <summary><code> server/index.js </code></summary>

    ```js
    const express = require('express');

    const app = express();

    const port = 3001;

    app.listen(port, () => {
        console.log(`Server listening on port: ${port}`);
    });
    ```
    </details>

## Step 3

### Summary

Now that we know our server is able to receive requests, let's get started adding our first endpoint and sending our first piece of data. The data we'll be using is provided for you in the `root` directory called `products.json`.

### Instructions

* Open `server/index.js`
* Require the `products.json` file and store it to a variable called `products`
* Write an endpoint that will send our array of products up to the client (note: make sure to do this below our `app` declaration)
    * An endpoint consists of a `Method`, `Path`, and `Handler`
    * The method for this endpoint should be a `GET` request
    * The path should be `/api/products`
    * The handler should be a function that accepts `req` and `res` as parameters and sends our `products` back to the client with a status of `200`
* Check your console to make sure your server is still running
* Open your browser and type `http://localhost:[your-port]/api/products`
    * You should see your array of products printed to the browser

### Solution
<details>

<summary><code> server/index.js </code></summary>

```js
const express = require('express');
const products = require('../products.json');

const app = express();

const port = 3001;

app.get('/api/products', (req, res) => {
    res.status(200).send(products)
});

app.listen(port, () => {
    console.log(`Server listening on port: ${port}`);
});
```
</details>


## Step 4

### Summary

In this step, we'll make a small adjustment to our `index.js`. A couple of aspects that are important to development are organization and clarity. You can see that as we start to add more and more endpoints our index file will quickly become cluttered. In this step we'll refactor (change) our index file so that all of our business logic (the Handlers for our endpoints) are contained in their own files.

### Instructions

* Create a new file in `./server` called `getProducts.js`
* Copy the handler from our endpoint into the `getProducts` file
* Store it to a variable called `getProducts`
* Export the function
    * Remember, in node we use `module.exports`
* Remember to require `products.json` into our `getProducts` file so it's in scope
* Require the file into `index.js` as a variable called `getProducts`
* Replace the Handler inside of `index.js` with our `getProducts` function we just required
    * Remember, any function that's used on an express method (represented as `app`) will receive `req` and `res` by default
* Test it in the browser like the previous step
    * The same content should appear in the browser window
    * Some changes don't affect how the application operates, but they do affect how easy it is to _work_ as a developer in the application

### Solution

<details>

<summary><code> server/index.js </code></summary>

```js
const express = require('express');
const getProducts = require('./getProducts');

const app = express();

const port = 3001;

app.get('/api/products', getProducts);

app.listen(port, () => {
    console.log(`Server listening on port: ${port}`);
});
```
</details>

<details>
<summary><code> server/getProducts.js </code></summary>

```js
const products = require('../products.json');

const getProducts = (req, res) => {
    res.status(200).send(products);
}

module.exports = getProducts;
```
</details>

## Step 5

### Summary

One of the benefits of using Express is that once our foundation is established, we can quickly scale our application by adding more endpoints. In this step we'll introduce an endpoint that allows us to request a single item from our dataset.

### Instructions

* Open `server/index.js`
* Write an endpoint that will:
    * use the `GET` method
    * has a path that uses a `parameter` called `id`
        * Remember, this parameter will give us access to `req.params` in our handler
            * The path should be `/api/product`
            * To tell the path to expect a parameter, append `/:id` to the end
        * We'll use it to capture a specific id for one of our products
    * The handler should be required from a file called `getProduct.js` 
        * Remember to require the `products.json` file so it's in scope
        * It should use `req.params` to find the item with the matching `id` in our products array
            * If the item is in the array, send it back to the client
            * If it is not in the array, it should send a status of 500 with a message `Item not in list`
            * Note: `req.params` will be a string and the id we're checking against is a number
    * Test the endpoint in your browser by entering `http://localhost:[your-port]/api/product/2`
        * It should print the content from the item with an id of 2 from our dataset
        * Try it with an id that doesn't exist (1334)
            * It should print `Item not in list` to the screen
<details>

<summary><code> server/index.js </code></summary>

```js
const express = require('express');
const getProducts = require('./getProducts');
const getProduct = require('./getProduct);

const app = express();

const port = 3001;

app.get('/api/products', getProducts);
app.get('/api/product/:id', getProduct);

app.listen(port, () => {
    console.log(`Server listening on port: ${port}`);
});
```
</details>

<details>
<summary><code> server/getProduct.js </code></summary>

```js
const products = require('../products.json');

const getProduct = (req, res) => {
    // find returns the item if it finds it, or undefined if not
    const item = products.find(val => val.id === parseInt(req.params.id));
    if (!item) {
        return res.status(500).send("Item not in list");
    }
    res.status(200).send(item);
}

module.exports = getProduct;
```
</details>

## Step 6

### Summary

At this point, we have a fairly solid API. We can retrieve all of our data from `/api/products` and we can retrieve specific items from our dataset with `/api/product/:id`. Let's add some flexibility to our `/api/products` endpoint. Currently, it will only retrieve all the products or a product by a specific ID, but we can have it serve an additional use case by utilizing `req.query`. Let's enable a request that allows us to filter based on a value. Remember, this is your API with your data, you get to set the rules and that includes what kind of filtering users are allowed to do.

### Instructions

* Open `server/getProducts.js`
    * Let's allow the user to filter the products based on price
    * Add a conditional before sending the products that checks if there's a property called `price` on the `req.query` object
        * If there is, filter through the `products` array and send any items that cost _more_ or the same as the provided price
            * Remember, query values are received as strings and the prices in our objects are numbers
        * If there is no `price` property on `req.query`, send all the products
    * Test in the browser
        * You can attach queries to the request by appending them to the url
        * `http://localhost:[your-port]/api/products?price=10.99`
        * Try it with various prices and make sure only products that cost the same or more than the price are received.
    * Query parameters are always optional and should default to a request for all the data if no query is provided

<details>
<summary><code> server/getProducts.js </code></summary>

```js
const products = require('../products.json');

const getProducts = (req, res) => {
    if (req.query.price) {
        const items = products.filter(val => val.price >= parseInt(req.query.price));
        return res.status(200).send(items);
    }
    res.status(200).send(products);
}

module.exports = getProducts;
```
</details>

## Wrap-up

We have a great start to a `read-only API`. This means it only supports getting data but has no functionality for updating, adding, or deleting data. Many of the API's you'll use on your first project will be read only. The next segment will cover how we bring that data in and make it our own so we can build a full CRUD (Create, Read, Update, Delete) API.

This is a great opportunity to practice building API's. Start from scratch with a new server and see how much you can do on your own. If you want to work with different data, [mockaroo](https://www.mockaroo.com) is a great tool for creating json files full of data. Download the file and place it in the directory you're working in.

Items to reinforce and get comfortable with are:

* Node Fundamentals
    * Using node to exectue JavaScript files
    * Require/Module.exports
* NPM
    * npm init
    * installing modules
* Express Fundamentals
    * Endpoints
        * Method
        * Path
        * Handler
            * Req
                * Params
                * Queries
            * Res
                * Status
                * Send

* Additionally, pair up and take turns answering the following:
    * What is Node?
    * What is the request-response cycle?
    * What is the purpose of a server?
    * What's the `package.json` for?
    * What are `node_modules`?
    * What is Express?
    * What does the `listen` method do?
    * What are some common status codes and what do they mean?
    * What's the difference between `npm install package-name` and `npm install -g package-name`?
    * When should I use a `query` vs. a `param` and vice-versa?

If your pair needs help answering these, utilize your available resources, then get with a mentor for further clarification. If needed, come back to the question later.

## Resources

<details>

<summary> <code> Setting up Your Server </code> </summary>

```
There are several steps for setting up a basic Express Server:

1. Run npm init -y
2. Install your dependencies (express)
3. Create a .gitignore file
4. Add node_modules to your .gitignore
5. Create the server directory
6. Create your index file
7. Require your dependencies
8. Declare your app variable
9. Declare your listen port
10. invoke the listen method and add a console log to the callback
11. run nodemon server/index.js in your terminal
12. success

```

</details>

<details>

<summary> <code> Express </code> </summary>

* [Express Documentation](https://expressjs.com/en/4x/api.html)
* [Common Status Codes](http.cat)

</details>


## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.

## Copyright

© DevMountain LLC, 2019. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.

<p align="center">
<img src="https://s3.amazonaws.com/devmountain/readme-logo.png" width="250">
</p>
