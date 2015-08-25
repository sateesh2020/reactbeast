## Isomorphic Javascript.

Isomorphic. Javascript. It means writing one codebase that can run on both the server side and the client side.

This is the concept behind frameworks like Rendr, Meteor & Derby. You can also accomplish this using React, and today we are going to learn how.

We are going to build an app that shows tweets about this article, and loads new ones in real time. Here are the requirements:

* It should listen to the Twitter streaming API and save new tweets as they come in.
* On save, an event should be emitted to the client side that will update the views.
* The page should render server side initially, and the client side should take it from there.
* We should use infinity scroll pagination to load blocks of 10 tweets at a time.
* New unread tweets should have a notification bar that will prompt the user to view them.

Let’s take a look at some of the tools we are going to use besides React:

* Express – A node.js web application framework
* Handlebars – A templating language we are going to write our layout templates in
* Browserify – A dependency bundler that will allow us to use CommonJS syntax
* Mongoose – A mongoDB object modeling library
* Socket.io – Real time bidirectional event based communication
* nTwitter – Node.js Twitter API library

### Server Side
#### DIRECTORY STRUCTURE

```sh
components/ // React Components Directory
---- Loader.react.js            // Loader Component
---- NotificationBar.react.js   // Notification Bar Component
---- Tweet.react.js             // Single Tweet Component
---- Tweets.react.js            // Tweets Component
---- TweetsApp.react.js         // Main App Component
models/ // Mongoose Models Directory
---- Tweet.js // Our Mongoose Tweet Model
public/ // Static Files Directory
---- css
---- js
---- svg
utils/
----streamHandler.js // Utility method for handling Twitter stream callbacks
views/      // Server Side Handlebars Views
----layouts
-------- main.handlebars
---- home.handlebars
app.js      // Client side main
config.js   // App configuration
package.json
routes.js // Route definitions
server.js   // Server side main
```
#### PACKAGE.JSON
```sh
{
  "name": "react-beast",
  "version": "0.0.0",
  "description": "Isomorphic React Example",
  "main": "app.js",
  "scripts": {
    "watch": "watchify app.js -o public/js/bundle.js -v",
    "browserify": "browserify app.js | uglifyjs > public/js/bundle.js",
    "build": "npm run browserify ",
    "start": "npm run watch & nodemon server.js"
  },
  "author": "Satheesh Smart",
  "license": "MIT",
  "dependencies": {
    "express": "~4.9.7",
    "express-handlebars": "~1.1.0",
    "mongoose": "^3.8.17",
    "node-jsx": "~0.11.0",
    "ntwitter": "^0.5.0",
    "react": "~0.11.2",
    "socket.io": "^1.1.0"
  },
  "devDependencies": {
    "browserify": "~6.0.3",
    "nodemon": "^1.2.1",
    "reactify": "~0.14.0",
    "uglify-js": "~2.4.15",
    "watchify": "~2.0.0"
  },
  "browserify": {
    "transform": [
      "reactify"
    ]
  }
}
```
