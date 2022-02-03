# Quick Review of Javascript fundamentals:
* **object**: a standalone entity with properties and type
* **object type**: is created using a constructor function and determines the object's name, properties, and methods.
* * **class**: a template for creating objects. It takes arguments, can contain methods and code to use those arguments, and returns an object (including primitive objects such as numbers, and arrays).
* **constructor**: a function that creates and initializes an object instance of a class. 
* **instance**: an object created using a particular constructor function.
* **higher-order functions**: functions that can receive a function as an argument and also return a function.
* * A [**promise**](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261) is an object that can be returned synchonously from an asychonous function, with one of three states: Fulfilled, Rejected, or Pending. Only the function that created the promise will have knowledge of its state.
    - A simple example of a promise is:
    ```
    const wait = time => new Promise((resolve) => setTimeout(resolve, time));

    wait(3000).then(() => console.log('Hello!')); // 'Hello!'
    ```
   - The function `wait` takes an argument `time`. `wait` generates a new promise, inside of which another function is passed. This function has the argument `resolve`, which upon execution is passed--along with `time`--to the `setTimeout()` method. 
   - The promise constructor takes two parameters, `resolve()` and `reject()`. The above example does not have a `reject()` parameter defined. Note that technically, the arguments could be called anything. 
   - `resolve()` is called when the `setTimeout` function is finished.
   - A promise must always pass and return a result variable
* * Bracket notation must be used when property names are to be dynamically determined (when the property name is not determined until runtime). 

# Project Initiation

The basic project structure for a MERN app looks like this:
```
project-name
|
|--server
    |--images (if saving images to MongoDB)
    |--routes
        |--**various route files**
    |--models
        |--**various model files**
    |--controllers
        |--auth.js
    package.json
    .env
    server.js
|--client
    |--src
    |--public
        |--images (if saving to a local server)
    package.json
```

## Setting up dependencies
Make sure `node` and `nodemon` are installed globally.

From inside the project folder `git init` and connect to remote Github repo. Then run `npm init -y`.

Set up seperate node dependency packages or the front and back end.

For the backend:
`npm i mongodb express cors dotenv mongoose multer uuid`

**mongoose** is a MongoDB object modeling tool designed to work in an asynchronous environment.

These backend dependencies are used if you plan on uploading images to your MongoDB database:
**multer** is a node.js middleware for handling multipart/form-data.
**uuid** is a package that generates random and unique ids.

For the frontend:
```
npx create-react-app <frontend-directory-name (e.g. client)>
npm i uuid axios react-router-dom
```
Note that `create-react-app` automatically initiates `git` inside the directory. Since git is already initiated in the parent directory, it can be removed from `client`

## .gitignore
Move the `.gitignore` file from `client` into the parent directory. Add the following lines:
```
/node_modules
node_modules/
```

# Setting up the backend
## General Terminolgy [(Ref)](https://www.freecodecamp.org/news/introduction-to-mongoose-for-mongodb-d2a7aa593c57/)
* **Collections** in Mongo are equivalent to tables in relational databases. They can hold multiple JSON documents.
* **Documents** are equivalent to records or rows of data in SQL. While a SQL row can reference data in other tables, Mongo documents usually combine that in a document.
* **Fields** or attributes are similar to columns in a SQL table.
* **Schema** is a document data structure (or shape of the document) that is enforced via the application layer.
* **Models** are higher-order constructors that take a schema and create an instance of a document. 
* **Record** is an instance of a model saved to the database
* **Controllers** can group related request handling logic into a single class

## Server.js (app.js)
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

// Connect to database (see below)
```

Some notes about the above example:
* `const app = express();` creates the server.
* The above example uses `mongoose` to connect with the backend. However, you can manually set up your backend connection as well, by creating and requiring a `./db/conn` module (See the wayou-kitchen project as an example of manual setup).
* `app.use(express.json());` allows server to accept JSON in the body of the request (this replaces the used of bodyParser).
* `app.use(require('./routes/<route-name>'));` is all that is needed to define routes if your are using React Router. If not, you need to specify the URL, followed by the file name: `app.use('/<route-name>', <route-name>Router);`

## Importing and exporting modules
Express uses the `require` method to import modules, i.e. it creates a single instance (singleton object/singleton pattern) and returns it. If you want to use `import` instead, you need to add `"type": "module"` to the `package.json` file.

Any .js file being imported needs to have in it a [`module.exports`](https://nodejs.org/api/modules.html#moduleexports) object to be exported. Example:

## Connecting to MongoDB Manually
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

## Connecting to MongoDB via mongoose
Mongoose manages relationships between data, provides schema validation, and is used to translate between objects in code and the representation of those objects in MongoDB.

Here is an alternative to the above for connecting to your database:
```
const uri = process.env.ATLAS_URI;
mongoose.connect(uri, { useNewUrlParser: true, useUnifiedTopology: true});

const connection = mongoose.connection;
connection.once('open', () => {
    console.log('mongo DB success');
});

app.listen(port, (err) => {
    if (err) console.error(err);
    console.log(`Example app listening at http://localhost:${port}`)});
```
Notes:
* `mongoose.connect` is passed the uri, and can also be passed options (as in the example above) and a callback function to fire when the initial connection is complete
* [`mongoose.connection`](https://mongoosejs.com/docs/api.html#mongoose_Mongoose-ConnectionStates) is the default connection used for the Mongoose module.  It is used by default for every model created using mongoose.model
* `mongoose.connection` is an instance of an Node.js [EventEmitter](https://nodejs.org/api/events.html#events_class_events_eventemitter) class, and can access the methods associated with that class, including `once`. This adds a one-time listener function for the event named eventName.

## Controllers
Here you will define request handling logic, and export that logic as a function. For instance, the `auth.js` constoller will handle user authorization:
```
exports.register = (req, res, next) => {
res.send("Register Route");
};
```

`exports` will save the `register` function as a named export. Note that `next` wil be very important for error handling. 

## Schema
Schema define the structure of the documents and fields in the database. They also include validations. They are defined in files in the `models` directory. They will be accessed in the appropriate file under **routes**

**mongoose** allows you to easily create new models using:
```
mongoose.model(modelName, schema)
```

SchemaTypes control they property type and handle validation among many other things. Here is a example of a new shema:
```
const UserSchema = new mongoose.Schema({
  password: {
    type: String,
    required: [true, "Please add a password"],
    minlength: 6,
    select: false
  },
  resetPasswordToken: String,
  resetPasswordExpire: Date
});
```
In the above example `required`, `minlength`, and `select` are built in validators for the type `String`. 

Notes on built-in validators:
* **match**: RegExp, creates a validator that checks if the value matches the given regular expression
* **select**: Set to true if this path should always be included in the results, false if it should be excluded by default. In the above, if a client queries for a user, they will not be able to access the password.

## Routes and Routing
**Routing** refers to determining how an application responds to a client request to a particular endoint (URI) and a specific HTTP request method.

**Routes** take on the following structure:
```
app.METHOD(PATH, HANDLER)
```
* `app` is the instance of the `express` application
* `METHOD` can be any HTTP request method (lowercase)
* `PATH` is the path on the server
* `HANDLER` is a callback function executed when the route is matched.

From ExpressJS 4.0 onward `Route()` can also be used. `Route()` is like a mini-Express application that allows access to routing APIs like `.get` and `route`. It is an isolated instance of middleware and routes. You can think of it as a “mini-application,” capable only of performing middleware and routing functions. 

Individual route files are kept in a `routes` directory. The top of the route will include something like:
```
const express = require('express');

// recordRoutes is an instance of the express router.
// We use it to define our routes.
// The router will be added as a middleware and will take control of requests starting with path /record.
const recordRoutes = express.Router();

// This will help us connect to the database
const dbo = require('../db/conn');

// This help convert the id from string to ObjectId for the _id.
const ObjectId = require('mongodb').ObjectId;
```

* [`ObjectId`](https://docs.mongodb.com/manual/reference/method/ObjectId/) returns a new ObjectId value.

### Get all documents in a collection
If you want to get all items in a collection:
```
// This section will help you get a list of all the records.
recordRoutes.route('/record').get(function (req, res) {
  let db_connect = dbo.getDb('recipes');
  db_connect
    .collection('records')
    .find({})
    .toArray(function (err, result) {
      if (err) throw err;
      res.json(result);
    });
});
```
Notes:
* [`collection()`](https://mongoosejs.com/docs/api/connection.html#connection_Connection-collection) retrieves a collection, creating it if not cached.
* [`toArray()`](https://docs.mongodb.com/manual/reference/method/cursor.toArray/) method returns an array that contains all the documents from a cursor.

### Get a document by id
```
// This section will help you get a single record by id
recordRoutes.route('/record/:id').get(function (req, res) {
  let db_connect = dbo.getDb();
  let myquery = { _id: ObjectId(req.params.id) };
  db_connect.collection('records').findOne(myquery, function (err, result) {
    if (err) throw err;
    res.json(result);
  });
});
```
Notes:
* This route accepts a URL parameter id (`:id`) which can be accessed via `req.params.id`
* In MongoDb the [`_id`](https://www.guru99.com/mongodb-objectid.html) field is the primary key for the collection so that each document can be uniquely identified in the collection. The `_id field` contains a unique `ObjectID` value.
 
 ### Post a new document
 ```
 // This section will help you create a new record.
recordRoutes.route('/record/add').post(function (req, response) {
  let db_connect = dbo.getDb();
  let myobj = {
    title: req.body.title,
    extendedIngredients: req.body.extendedIngredients
  };
  db_connect.collection('records').insertOne(myobj, function (err, res) {
    if (err) throw err;
    response.json(res);
  });
});
```
Notes:
* [`req.body`](https://www.geeksforgeeks.org/express-js-req-body-property/) property contains key-value pairs of data submitted in the request body. 

### Post an update to an existing document
```
// This section will help you update a record by id.
recordRoutes.route('/update/:id').post(function (req, response) {
  let db_connect = dbo.getDb();
  let myquery = { _id: ObjectId(req.params.id) };
  let newvalues = {
    $set: {
      title: req.body.title,
      extendedIngredients: req.body.extendedIngredients,
    },
  };
  db_connect
    .collection('records')
    .updateOne(myquery, newvalues, function (err, res) {
      if (err) throw err;
      console.log('1 document updated');
      response.json(res);
    });
});
```
Notes:
* [`$set`](https://docs.mongodb.com/manual/tutorial/update-documents/) is an update operator used to modify field values

## Add image upload capability
You will need the `multer` middleware to upload images. [Multer](http://expressjs.com/en/resources/middleware/multer.html) is a node.js middleware for handling multipart/form-data, which is primarily used for uploading files. You can also use `uuid` to generate random values for your image names.

At the top of the appropriate route, include:
```
const multer = require('multer');
const { v4: uuidv4 } = require('uuid');
```
Next, set your storage options using the `diskStorage` engine:
```
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, 'images');
  },
  filename: function (req, file, cb) {
    cb(null, uuidv4() + '-' + Date.now() + path.extname(file.originalname));
  },
});
```
Notes:
* `cb(null, 'images');` means that if the function doesn't error, the upload will be stored in the `images` directory.

Next, use a function to specify what kinds of images are acceptable for upload, set your `upload` object and set your route. Make sure to set your image data to `req.file.filename`. If no image is entered into the input on submit `req.file` will return undefined, and you will receive an error when trying to upload to the database. You can get around this by adding a tertiary operator to the `image` value:
```
const fileFilter = (req, file, cb) => {
  const allowedFileTypes = ['image/jpeg', 'image/jpg', 'image/png'];
  if (allowedFileTypes.includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(null, false);
  }
};

let upload = multer({ storage, fileFilter });

router.route('/add').post(upload.single('photo'), (req, res) => {
sourceUrl: req.body.sourceUrl,
image: req.file === undefined ? "placeholder.jpg" : req.file.filename,
analyzedInstructions: JSON.parse(req.body.analyzedInstructions),
}
```
Note here that `analyzedInstructions` contains an array, which must converted into a string on the frontend (see below), and therefore parsed, using `JSON.parse()` on the backend.

On the front end you will create a `FormData` object, and then post that object data to the database. `FormData` compiles key/value pairs to send using an `XMLHttpRequest`. It can be used to send form data or keyed data. For example:
```
  function reciepData(e) {
    // When post request is sent to the create url, axios will add a new record to the database.

    const formData = new FormData();
    formData.append('image', recipe.image);
    formData.append('extendedIngredients', JSON.stringify(recipe.extendedIngredients));

    axios
      .post('http://localhost:5000/record/add', formData)
      .then((res) => console.log(res.data));
  }
```
  
Any array data, such as in the example `extendedIngredients` above, must first use `JSON.stringify()` to stringify the data prior to sending to the database.

`multer` also uses `multipart/form-data` so this needs to be set in the form by adding `encType="multipart/form-data` to the `form` tag.

# Database commands
`db.<collection-name>.find()` to show all items in a collection`
`db.<collection-name>.drop()` to delete all items in a collection`

# References:
https://www.mongodb.com/languages/mern-stack-tutorial
