API stands for Application Programming Interface, and is how two applications communicate with each other. All API interactions involve a **request** and **return**

HTTP APIs are used by browsers and are made up of:
- A request method
- A URI and protocol version
- Various other information

# HTTP Request
An **HTTP request** start line will include the HTTP version and a method (GET, POST, PUT, DELETE).

The request will also include a body, which will have content based on the method called. For instance the body of a GET request contains nothing, while POST will include post information

# HTTP Response
An **HTTP response** start line will include:
- The HTTP version
- A status code
  - 100 series: informational response
  - 200 series: success
  - 300 series: redirect
  - 400 series: client error
  - 500 series: server error

Data is sent through an API via JSON or XML
**XML** stores and transports data. It doesn't change or display data. It is good for exchanging data. Has tags, but only for readability.
**JSON** sends data in form of key:value pairs. It is a JS object, and is the preferrable mode of data transfer. JSON objects are comprised of data (properties) and functionality (methods).

# REST
REpresentational State Transfer is an architectural pattern that allows transfer of state to a resource.

Rest is comprised of:
- And endpoint (URL), possibly with parameters
- A method
- Headers
- A body (data)

# Client Side APIs for JS
- Either browser or third party API's
- AJAX (Asynchronous Javascript and XML) uses an httpXMLRequest to communicate with the server
- The **Fetch API** is a browser API used for data between applications across the web
- Third party APIs

# Design Patterns for making API requests
One great and modern way to make API requests is to use `fetch` with `async...await`. You can also use the `axios` package to simplify the code. Additionally, you can use `try...catch` for error handling.
Here is the basic structure:
```
const getData = async () => {
try {
  const { data } = await axios.get('website-url/path/');
  // Do things with data
 } catch (err) {
  console.log(err);
 }
```





