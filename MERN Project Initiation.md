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
* Express uses the `require` method to import modules, however, if you want to use `import` instead, you need to add `"type": "module"` to the `package.json` file.
* `const app = express();` creates the server.
* The above example uses `mongoose` to connect with the backend. However, you can manually set up your backend connection as well, by creating and requiring a `./db/conn` module (See the wayou-kitchen project as an example of manual setup).
* `app.use(express.json());` allows server to accept JSON in the body of the request (this replaces the used of bodyParser).
* `app.use(require('./routes/<route-name>'));` is all that is needed to define routes if your are using React Router. If not, you need to specify the URL, followed by the file name: `app.use('/<route-name>', <route-name>Router);`

# Database commands
`db.<collection-name>.find()` to show all items in a collection
`db.<collection-name>.drop()` to delete all items in a collection

# References:
https://www.mongodb.com/languages/mern-stack-tutorial
