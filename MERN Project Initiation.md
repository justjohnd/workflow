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


# References:
https://www.mongodb.com/languages/mern-stack-tutorial
