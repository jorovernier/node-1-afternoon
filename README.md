<img src="https://s3.amazonaws.com/devmountain/readme-logo.png" width="250" align="right">

# Project Summary

In this project we're going to build the backend service (API) for an ecommerce site. We'll be able to display products, search for products, and select a specific product for more info. There will be some accelerator content at the end to help add additional challenges.

The goal is to better understand how the client (frontend application) and server (backend application) interact with each other in the `request -> response cycle`. This includes generating a request on the client, receiving and parsing it on the server, and sending the response back to the client.

## Setup

* `Fork` this repository.
* `Clone` your `fork`.
* Run `npm install`.

## Step 1

### Summary

Let's get familiar with our exisiting code. It's good practice when entering a new application to scan some of the existing code to see what's already built. Don't worry about understanding all of it up front, you'll get to know it better as you interact with it.

### Instructions

* Review the `package.json` to see what packages (dependencies) are already being used in our application.
    * When you ran `npm install` earlier, npm checked our `package.json` for any packages listed as `dependencies` and installed them for us from the npm store to our `node_modules` folder in our local application.
    * We didn't have to re-run `create-react-app`, we just had to install the tools that the react portion of our application relies on.
* Next, review the exisiting code.
    * The takeaway here is to notice that we'll be making requests to the API we're going to build to populate our frontend with data.
    * A lot of our frontend is built, we'll need to fill in the pieces on some of the functionality.
    * None of our backend is built and that's where we'll spend most of our time.

## Step 2

### Summary

Now that we have a handle on the existing code that we're working with, let's start building out our ecommerce API. In this step, we'll setup and test the initial framework for our server.

### Instructions

* Start by installing Express
    * `npm install express`
* Next, build the skeleton of your server
    * This should include:
        * Requiring express
        * Declaring your `app` variable and setting it equal to express invoked
        * Declaring the port you want your server to `listen` on.
            * Remember, it should be different than the port our React Development Server will listen on, which is `3000`.
        * Invoking `app.listen`:
            * Provide it the port variable from above
            * And a callback with a console log so we know our server is running and ready to receive requests.
        * Finally, go to your terminal/git bash.
            * Type `pwd` to make sure you're in the root directory
                * It should look something like this `/Users/yourname/DevMountain/node/node-1`
                * If you're not, navigate back to the root of the project directory.
            * From there, run the following command `nodemon server/index.js`
            * You should see the `console.log` message from your `listen` method in the console.
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

## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.

## Copyright

© DevMountain LLC, 2019. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.

<p align="center">
<img src="https://s3.amazonaws.com/devmountain/readme-logo.png" width="250">
</p>
