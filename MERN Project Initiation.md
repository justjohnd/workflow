# Project Initiation

The basic project structure for a MERN app looks like this:
```
project-name
|
|--server
    |--images (if saving images to MongoDB)
    |--routes
    |--models
    package.json
    .env
    server.js
|--client
    |--src
    |--public
    package.json
```

## Setting up dependencies
Make sure `node` and `nodemon` are installed globally.

From inside the project folder `git init` and connect to remote Github repo. Then run `npm init -y`.

Set up seperate node dependency packages or the front and back end.

For the backend:
`npm i mongodb express cors dotenv mongoose multer uuid`

These backend dependencies are used if you plan on uploading images to your MongoDB database
**mongoose** is a MongoDB object modeling tool designed to work in an asynchronous environment.
**multer** is a node.js middleware for handling multipart/form-data.
**uuid** is a package that generates random and unique ids.

For the frontend:
```
npx create-react-app <frontend-directory-name (e.g. client)>
npm i uuid axios react-router-dom
```

# Setting up the backend
`server.js` (or `app.js`) is central command for the backend.

Boilerplate for this file is as follows:
```
// Required dependencies
const express = require('express');
const app = express();
const cors = require('cors');
require('dotenv').config();
const port = process.env.PORT || 5000;
const mongoose = require('mongoose');

// Required routes (change <route-name> to the name of your route
app.use(require('./routes/<route-name>'));

// Required middleware
app.use(cors());
app.use(express.json());

// Connect to database
const uri = process.env.ATLAS_URI;
mongoose.connect(uri, { useNewUrlParser: true, useUnifiedTopology: true});

const connection = mongoose.connection;
connection.once('open', () => {
    console.log('mongo DB success');
});


app.listen(port, (err) => {
    if (err) console.error(err);
    console.log(`App listening at http://localhost:${port}`)});
```

Some notes about the above example:
* `const app = express();` creates the server.
* The above example uses `mongoose` to connect with the backend. However, you can manually set up your backend connection as well, by creating and requiring a `./db/conn` module (See the wayou-kitchen project as an example of manual setup).
* `app.use(express.json());` allows server to accept JSON in the body of the request (this replaces the used of bodyParser).
* `app.use(require('./routes/<route-name>'));` is all that is needed to define routes if your are using React Router. If not, you need to specify the URL, followed by the file name: `app.use('/<route-name>', <route-name>Router);`

## Importing and exporting modules
Express uses the `require` method to import modules, however, if you want to use `import` instead, you need to add `"type": "module"` to the `package.json` file.

Any .js file being imported needs to have in it a [`module.exports`](https://nodejs.org/api/modules.html#moduleexports) object to be exported. Example:

## Connecting to MongoDB
Here is one example of how to connect with your MongoDB database:
```
const { MongoClient } = require('mongodb');
const Db = process.env.ATLAS_URI;
const client = new MongoClient(Db, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

var _db;

module.exports = {
  connectToServer: function (callback) {
    client.connect(function (err, db) {
      // Verify we got a good "db" object
      if (db) {
        _db = db.db('myFirstDatabase');
        console.log('Successfully connected to MongoDB.');
      }
      return callback(err);
    });
  },

  getDb: function () {
    return _db;
  },
};
```
This would be accessed by adding the foloowing line to `server.js`: `const dbo = require('./db/conn');`. Then you would use the `connectToServer` method to connect to `dbo`.
Let's look at what the above code means:
* `mongodb` is a Node.js dependency, and the native driver that allows our javascript application to interact with the database
* `const { MongoClient } = require('mongodb');`: imports `MongoClient` from the driver. [`MongoClient()`](https://mongodb.github.io/node-mongodb-native/api-generated/mongoclient.html) is a constructor used to create a new MongoClient instance. You pass it a **serverConfig** object, and **options** object.
* The above `module.exports` object exports a `connectToServer` function and `getDb` function.
* The [`connect`](https://mongodb.github.io/node-mongodb-native/api-generated/mongoclient.html#connect) is passed the url, option, and a callback function. The callback runs after the method is executed and returns an `error` object, otherwise null.

Back in the `server.js` file, the object is imported, and `connectToServer` is passed a function to return an `error` object if it occurs:
```
app.listen(port, () => {
  // perform a database connection when server starts
  dbo.connectToServer(function (err) {
    if (err) console.error(err);
  });
  console.log(`Server is running on port: ${port}`);
});
```

# Database commands
`db.<collection-name>.find()` to show all items in a collection`
`db.<collection-name>.drop()` to delete all items in a collection`

# References:
https://www.mongodb.com/languages/mern-stack-tutorial
