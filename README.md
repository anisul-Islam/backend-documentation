# Node, Express, MongoDB, API Documentation, Authentication Tutorial

- prerqusities: javascript, typescript

![profile2](./fs.excalidraw.png)

## Table of Contents

1. [Node.js](#1-nodejs)

    [1.1 Introduction](#11-introduction)
    [1.2 Environment setup](#12-environment-setup--run)
    [1.3 Event loop](#13-event-loop)
    [1.4 Module - Local, Built-in, External](#14-module)
    [1.5 API](#15-api)
    [1.6 MVC Architecture](#16-mvc-architecture)
    [1.7 CRUD APP](#17-crud-app)
    [1.8 npm crash course](#18-npm-crash-course)
    [1.9 Connect front-end and backend](#19-connect-front-end-and-backend)
    [1.10 Another CRUD APP: Complete REST API](#110-another-crud-app-complete-rest-api)
    [1.11 server side rendering](#111-server-side-rendering)

2. [Express.js](#2-expressjs--typescript)
3. [MongoDB](#3-mongodb-tutorial)
4. [API Documentation](#4-api-documentaion-with-swagger)
5. [Authentication](#5-authentication-tutorial)

## 1. Node.js

### 1.1 Introduction

- [Introduction to node.js](https://youtu.be/36R0VXmX8i8)

#### What is Node.js?

- Node.js is not a programming language. It is a js runtime environment which allows us to run js in the server. Node.js does not run on browser but only vanila js.
- window is a global object for browser. before node.js we could use javascript for the browser only but now with the help of node.js we can run javascript for accessing our local machine. try console.log(), window.alert() at the browser and in the terminal.
- It utilize Google's V8 engine (developed with C++) which compiles javascript code into machine code
- Node.js is single threaded but non-blocking beacuse of event loop.

#### Why Node.js?

- It helps us to create server.
- It helps to manage files (create, open, read, write, delete and close) on the server.
- It helps to manage (add, modify, delete data) database.
- it supports asynchronous programming.

### 1.2 Environment setup & run

- check node is already installed or not using the command: node --version or node -v
- [Node.js](https://nodejs.org/en/) download & install
- Editor: anything; I prefer [Visual Studio Code](https://code.visualstudio.com/)
- type node and enter for using Node REPL and try writing some javascript code here like console.log(), mathematical calculations
- window is a global object in the fornt end; global is the global object in the backend
- initialize npm so that you can use es6 module
- run a simple app and run `node index.js`
  
  ```js
    // index.js
    console.log('Welcome to Node.js');

    const products = [
      { id: '1', title: 'apple iphone 12', price: 320 },
      { id: '2', title: 'apple iphone 14', price: 420 },
      { id: '3', title: 'apple iphone 15', price: 820 },
    ];

    const getAllProducts = () => {
      console.log(products);
    };

    getAllProducts(products);
  ```

### [1.3 Event loop]

- Node.js is single threaded but non-blocking beacuse of event loop.
- it is efficient because of its non blocking feature
- all the events are placed in a stack (first in first out FIFO)
- Node.js keep running like FIFO, one after one event is handles by node process
- if any task required time instead of stop and wait it will be passed to callback function and then move to the next task
- once all the tasks are handled then from the event loop task will be executed
- synchronous vs asynchronous programming
- example

  ```js
  console.log("hello 1");
  console.log("hello 2");
  setTimeout(() => {
    console.log("hello 3");
  }, 1000);
  setTimeout(() => {
    console.log("hello 4");
  }, 1000);
  console.log("hello 5");
  console.log("hello 6");

  // example 2
  console.log("no need to wait for this task");
  setTimeout(() => {
    console.log("first task taking 5 minutes");
  }, 5000);
  setTimeout(() => {
    console.log("second task taking 3 minutes");
  }, 3000);
  setTimeout(() => {
    console.log("third task taking 2 minutes");
  }, 2000);
  setTimeout(() => {
    console.log("fourth task taking 1 minute");
  }, 1000);
  console.log("no need to wait for this task");
  ```

### 1.4 Module

- [local module](https://youtu.be/n3F1kaOfyzw)

- Module is a set of functions or variables. We can use es6 import, export by adding "type":"module" in package.json. When you have too much code in a single file you would like to separate them in multiple files so that they are reusable and modular. console.log(process) and find the module.exports = {}
- 3 types of module.
  - local module (own created module)
  - built-in-modules (node.js own module): os, path, url, fs, http
  - External modules (3rd party module mainly managed by npm)

#### [Local / own module]

- version 0: without module

  ```js
  // index.js
  const products = [
    { id: '1', title: 'apple iphone 12', price: 320 },
    { id: '2', title: 'apple iphone 14', price: 420 },
    { id: '3', title: 'apple iphone 15', price: 820 },

  ];

  const getAllProducts = () => {
    console.log('All Products:', products);
  };

  const getSingleProduct = (id) => {
    const product = products.find((product) => product.id === id);
    console.log('Single Product:', product);
  };

  const deleteSingleProduct = (id) => {
    const filteredProducts = products.filter((product) => product.id !== id);
    console.log('Products after delete:',filteredProducts);
  };

  getAllProducts();
  getSingleProduct('2');
  deleteSingleProduct('2');
  ```

- version 1: individual export

  ```js
  // productController.js
  const products = [
    { id: '1', title: 'apple iphone 12', price: 320 },
    { id: '2', title: 'apple iphone 14', price: 420 },
    { id: '3', title: 'apple iphone 15', price: 820 },

  ];

  exports.getAllProducts = () => {
      console.log('All Products:', products);
  };

  exports.getSingleProduct = (id) => {
    const product = products.find((product) => product.id === id);
    console.log('Single Product:', product);
  };

  exports.deleteSingleProduct = (id) => {
    const filteredProducts = products.filter((product) => product.id !== id);
    console.log('Products after delete:',filteredProducts);
  };

  // now use them in anywhere
  const {
    getAllProducts,
    getSingleProduct,
    deleteSingleProduct,

  } = require('./productsController');

    getAllProducts();
  getSingleProduct('2');
  deleteSingleProduct('2');
  ```

- version 2: exporting them all at a time
  
  ```js
  // changes in export but no changes in require
   module.exports = { getAllProducts, getSingleProduct, deleteSingleProduct };
  ```

- version 2: name export

  ```js
    // 1 name export is possible for one file
    module.exports = getAllProducts;

    // now require it with any name
  ```

- version 3: export a class

  ```js
    const products = [
      { id: '1', title: 'apple iphone 12', price: 320 },
      { id: '2', title: 'apple iphone 14', price: 420 },
      { id: '3', title: 'apple iphone 15', price: 820 },

    ];

    class Product {
      getAllProducts = () => {
        return products;
      };

      getSingleProduct = (id) => {
        const product = products.find((product) => product.id === id);
        return product;
      };

      deleteSingleProduct = (id) => {
        const filteredProducts = products.filter((product) => product.id !== id);
        return filteredProducts;
      };
    }

    module.exports = Product;

  // from any file
  const Product = require('./productsController');

  // Create an instance of the Product class
  const productInstance = new Product();

  // Use the methods
  const allProducts = productInstance.getAllProducts();
  console.log('All Products:', allProducts);

  const singleProduct = productInstance.getSingleProduct('2');
  console.log('Single Product:', singleProduct);

  const productsAfterDelete = productInstance.deleteSingleProduct('1');
  console.log('Products after delete:', productsAfterDelete);
  ```

- setup es6 module

#### [Built-in / node.js module]

- [os and path module video]((https://youtu.be/EHo7KNPawhw))
- os module: The os module in Node.js provides a way of interacting with the operating system. It allows you to access various operating system-related information.

  ```js
  // os, path
  const { totalmem, freemem } = require("os");
  console.log(totalmem());

  const os = require("os");
  const osInfo = () => {
    return {
      userInfo: os.userInfo(), // Returns information about the currently effective user.
      totalMemory: os.totalmem(), // Returns the total amount of system memory in bytes.
      freeMemory: os.freemem(), // Returns the amount of free system memory in bytes.
      hostname: os.hostname(), //Returns the host name of the operating system.
      osType: os.type(), //Returns the operating system name.
      relase: os.release(),
      platform: os.platform(), // Returns the operating system platform.
      architecture: os.arch(), // Returns the CPU architecture of the operating system.
      uptime: os.uptime(), // Returns the system uptime in seconds.
      cpus: os.cpus(), // Returns an array of objects containing information about each logical CPU core.
    };
  };
  
  //call from outside
  console.log(osInfo());

  ```

- path module: The path module in Node.js provides utilities for working with file and directory paths. It's used for handling and transforming file paths.

  ```js
  const path = require("path");

  const address = "backend-course/path-module.js";

  exports.pathInfo = () => {
     return {
      __dirname: __dirname, // Returns the directory name of a path.
      __filename: __filename,
      directoryName: path.dirname(address),
      extensionName: path.extname(address), // path.extname(path): Returns the extension of a path.
      lastPartOfPath: path.basename(address), // path.basename(path[, ext]): Returns the last portion of a path.
      parsedPath: path.parse(address), // path.parse(path): Returns an object from a path string.
      join: path.join(__dirname + '/views/index.html'), // Joins the given path segments.
      absolutePath: path.resolve('views/index.html'), // Resolves the specified paths into an absolute path.
    };
  };

  //call from outside
  console.log(pathInfo());
  ```

- url module: The `url` module in Node.js provides utilities for URL resolution and parsing. It's used for working with URLs. Here are some commonly used functions in the `url` module:

  1. **url.format(urlObject):** Takes a parsed URL object and returns a formatted URL string.

      ```javascript
      const url = require('url');
      const urlString = url.format({
        protocol: 'https:',
        hostname: 'www.example.com',
        pathname: '/path',
        query: { query: 'string' },
      });
      console.log(urlString);
      ```

  2. **url.resolve(from, to):** Resolves a target URL relative to a base URL.

      ```javascript
      const url = require('url');
      const resolvedUrl = url.resolve('https://www.example.com/base/', 'path');
      console.log(resolvedUrl);
      ```

  3. **new URL(input[, base]):** (Introduced in Node.js 10) URL constructor for creating URL objects.

      ```javascript
      const { URL } = require('url');
      const myUrl = new URL('https://www.example.com/path?query=string');
      console.log(myUrl);
      ```

  4. **urlSearchParams.toString():** (Introduced in Node.js 10) Returns the query string.

      ```javascript
      const { URLSearchParams } = require('url');
      const params = new URLSearchParams('key1=value1&key2=value2');
      console.log(params.toString());
      ```

  ```js
  const url = require("url");

  const address = "http://localhost:8080/api/products?maxPrice=300&sortBy=ASC";

  // const myUrl = new URL(address);
  // console.log(myUrl.host);
  // console.log(myUrl.hostname);
  // console.log(myUrl.protocol);
  // console.log(myUrl.port);
  // console.log(myUrl.search);
  // console.log(myUrl.href);

  console.log(url.parse(address));

  const query = url.parse(address).query;
  const data = new URLSearchParams(query);
  console.log(data);
  console.log(data.get("maxPrice"));
  console.log(data.get("sortBy"));

  // how to parse url
  const url = require('url');

  const address = '<http://localhost:8080/api/products?maxPrice=300&sortBy=ASC>';

  // Parse the URL
  const parsedUrl = new URL(address);

  // Get the query parameters as an object
  console.log(parsedUrl.searchParams);
  const queryParams = Object.fromEntries(parsedUrl.searchParams.entries());

  console.log(queryParams);
  ```

- fs module: managing file system: create, read, update, rename, delete

  ```js
  const fs = require("fs");
  console.log(fs);

  // creating a file
  fs.writeFile("products.json", JSON.stringify([]), (err) => {
    // if there is 1 if else then use ternary operator
    if (err) {
      console.log(err);
    } else {
      console.log("File is created successfully");
    }
  });

  // appending data to a file
  fs.appendFile("test.txt", "some extra text is added", (err) => {
    if (err) {
      console.log(err);
    } else {
      console.log("data is added successfully");
    }
  });

  // reading data from a file
  fs.readFile("test.txt", "utf-8", (err, data) => {
    if (err) {
      console.log(err);
    } else {
      console.log(data);
    }
  });

  // renaming an existing file
  fs.rename("test.txt", "test2.txt", (err) => {
    if (err) {
      console.log(err);
    } else {
      console.log("successfully renamed");
    }
  });

  // checking a file exsits or not
  fs.exists("test2.txt", (result) => {
    if (result) {
      console.log("file exists");
    } else {
      console.log("file does not exist");
    }
  });

  // deleting a file
  fs.unlink("test2.txt", (err) => {
    if (err) {
      console.log(err);
    } else {
      console.log("file is deleted successfully");
    }
  });
  ```

  - more better example for ecommerce project

  ```js
    // create a file
    fs.writeFile('products.json', JSON.stringify([]));

    // read a file
    const getAllProducts = async () => {
      const products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
      console.log(products);
    };
    // read file
    const getSingleProduct = async (id) => {
      const products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
      const product = products.find((product) => product.id === id);
      console.log(product);
    };

    // first read then write
    const deleteSingleProduct = async (id) => {
      let products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
      const filteredProducts = products.filter((product) => product.id !== id);
      products = filteredProducts;
      await fs.writeFile('./products.json', JSON.stringify(products));
    };
  ```

- http module: The http module in Node.js is a core module that allows you to create HTTP servers and make HTTP requests.

  - [http module](https://youtu.be/PmLJO403hvc)

  - Example of a node.js http server

    ```js
    // Method 1
    const http = require("http");

    const PORT = 3000;
    const hostName = "127.0.0.1";

    const server = http.createServer((req, res) => {
      // Set the response HTTP header with HTTP status and Content type
      // res.writeHead(200, {'Content-Type': 'text/plain'});
      // res.writeHead(200, {'Content-Type': 'application/json'});
      res.writeHead(200, {'Content-Type': 'text/html'});

      // end the response with a response message
      res.end("<h1>welcome to the server</h1>");
    });

    // Listen on port 8080 and IP address 127.0.0.1
    server.listen(PORT, () => {
      console.log(`server is running at http://${hostName}:${PORT}`);
    });

    // Method 2
    const http = require("http");
    http
      .createServer((req, res) => {
        res.end("<h1> Welcome to your first node server</h1>");
      })
      .listen(3000, () => {
        console.log("server is running");
      });
    ```

### 1.5 API

- **API**: API stands for Application Programming Interface. It's a set of rules and protocols that allows one piece of software application to interact with another.

  There are different types of APIs, including:

  - Web APIs (HTTP-based): Often referred to as RESTful APIs, these use HTTP methods (like GET, POST, PUT, DELETE) to perform operations on resources.

  - Library APIs: These are APIs provided by programming libraries or frameworks to enable developers to use their functions and classes.

  - Operating System APIs: Provide a way for software applications to interact with the operating system.

  - Hardware APIs: Enable communication with hardware components.

- **HTTP methods**: HTTP (Hypertext Transfer Protocol) defines several methods, or verbs, that indicate the desired action to be performed on a resource. Each HTTP method has a specific purpose and semantic meaning. Here are some of the most commonly used HTTP methods:

    1. **GET:**
      - Purpose: Retrieve data from the specified resource.
      - Idempotent: Yes (multiple identical requests have the same effect as a single request).

    2. **POST:**
      - Purpose: Submit data to be processed to a specified resource.
      - Idempotent: No.

    3. **PUT:**
      - Purpose: Update a resource or create a new resource if it does not exist.
      - Idempotent: Yes.

    4. **DELETE:**
      - Purpose: Delete the specified resource.
      - Idempotent: Yes.

    5. **PATCH:**
      - Purpose: Apply partial modifications to a resource.
      - Idempotent: No.

    6. **HEAD:**
      - Purpose: Retrieve the headers of a resource without the body.
      - Idempotent: Yes.

    7. **OPTIONS:**
      - Purpose: Get information about the communication options available for the target resource.
      - Idempotent: Yes.

    8. **TRACE:**
      - Purpose: Perform a message loop-back test along the path to the target resource.
      - Idempotent: Yes.

    9. **CONNECT:**
      - Purpose: Establish a tunnel to the server identified by a given URI.
      - Idempotent: No.

    These methods provide a standardized way for clients to interact with web servers. The idempotent property means that making the same request multiple times has the same effect as making it once. This is an important concept for safe and predictable interactions between clients and servers.
- **Idempodent**: In the context of HTTP methods, an idempotent operation is one that can be repeated many times, and the result will be the same as if it had been performed only once. In other words, making the same request multiple times should have the same effect as making it once.

  Here's a breakdown:

    1. **Idempotent Methods:**
      - **GET:** Retrieving the same resource multiple times will always yield the same result (assuming the resource hasn't changed).
      - **PUT:** If you PUT a resource several times, it's still just a single resource with the same state.
      - **DELETE:** Deleting a resource will result in the same state (it's gone).

    2. **Non-idempotent Methods:**
      - **POST:** Submitting a POST request multiple times may result in different resources being created or modified each time.

        **Why is Idempotence Important?**

        - **Predictability:** In distributed systems, especially in scenarios where requests may be retried, having idempotent operations ensures that you get predictable results.
        - **Error Recovery:** If a request fails, you can safely retry it without worrying about unintended side effects.
        - **Caching:** Idempotence simplifies caching. If a GET request is idempotent, you can cache its result.

        In summary, idempotence ensures that repeated requests don't produce unexpected or harmful results, making interactions more reliable and easier to manage.

- **Testing API with Postman**
- **status code**
  - [status code](https://youtu.be/lHfnjUP-N4E)
  - [status code cheatsheet](https://devhints.io/http-status)
  HTTP status codes are three-digit numbers returned by a server in response to a client's request. They indicate the outcome of the server's attempt to process the request. Status codes provide information about the result of the server's attempt to process the request and guide the client on how to proceed.Here are some of the commonly used HTTP status codes:

      1. **1xx Informational:**
        - **100 Continue:** The client should continue with its request.

      2. **2xx Success:**
        - **200 OK:** The request was successful.
        - **201 Created:** The request has been fulfilled, and a new resource has been created.
        - **204 No Content:** The server successfully processed the request, but there is no new information to send back.

      3. **3xx Redirection:**
        - **301 Moved Permanently:** The requested page has moved permanently to a new location.
        - **302 Found (or 303 See Other):** The requested page has moved temporarily to a new location.

      4. **4xx Client Error:**
        - **400 Bad Request:** The server did not understand the request.
        - **401 Unauthorized:** The request requires user authentication.
        - **403 Forbidden:** The server understood the request, but it refuses to authorize it.
        - **404 Not Found:** The requested resource could not be found.

      5. **5xx Server Error:**
        - **500 Internal Server Error:** A generic error message returned when an unexpected condition was encountered by the server.
        - **502 Bad Gateway:** The server, while acting as a gateway or proxy, received an invalid response from an upstream server.
        - **503 Service Unavailable:** The server is not ready to handle the request.

          ```js
            // constants/httpStatus
            export const HttpStatus = {
              OK: 200,
              CREATED: 201,
              ACCEPTED: 202,
              NO_CONTENT: 204,
              BAD_REQUEST: 400,
              UNAUTHORIZED: 401,
              FORBIDDEN: 403,
              NOT_FOUND: 404,
              METHOD_NOT_ALLOWED: 405,
              CONFLICT: 409,
              INTERNAL_SERVER_ERROR: 500,
            };      
          ```

- **routing, request, response**
  - **request object** : req.url, req.method
  - **response object**: can be string, json, html etc.

  ```js
      const http = require('http');

      const { httpStatus } = require('./constants/httpStatus');

      const server = http.createServer((req, res) => {
        if (req.url === '/products' && req.method === 'GET') {
          res.writeHead(httpStatus.OK, { 'Content-Type': 'application/json' });
          res.end(
            JSON.stringify({
              message: 'all products are returned',
            })
          );
        } else if (req.url?.match(/\/products\/([0-9]+)/) && req.method === 'GET') {
          res.writeHead(httpStatus.OK, { 'Content-Type': 'application/json' });
          res.end(
            JSON.stringify({
              message: 'single product is returned',
            })
          );
        } else if (
          req.url?.match(/\/products\/([0-9]+)/) &&
          req.method === 'DELETE'
        ) {
          res.writeHead(httpStatus.OK, { 'Content-Type': 'application/json' });
          res.end(
            JSON.stringify({
              message: 'single product is deleted',
            })
          );
        } else if (req.url?.match(/\/products\/([0-9]+)/) && req.method === 'PUT') {
          res.writeHead(httpStatus.OK, { 'Content-Type': 'application/json' });
          res.end(
            JSON.stringify({
              message: 'single product is updated',
            })
          );
        } else if (req.url === '/products' && req.method === 'POST') {
          res.writeHead(httpStatus.CREATED, { 'Content-Type': 'application/json' });
          res.end(
            JSON.stringify({
              message: 'single product is created',
            })
          );
        } else {
          res.writeHead(httpStatus.NOT_FOUND, { 'Content-Type': 'application/json' });
          res.end(
            JSON.stringify({
              message: 'route not found',
            })
          );
        }
      });

      server.listen(3002, () => {
        console.log(`server is running at http://localhost:3002`);
      });

  ```

### 1.6 MVC Architecture

- move response to the productController
  
  ```js
    const fs = require('fs/promises');

    const { httpStatus } = require('../constants/httpStatus');

    const getAllProducts = async (res) => {
      const products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
      res.writeHead(httpStatus.OK, { 'Content-Type': 'application/json' });
      res.end(
        JSON.stringify({
          message: 'all products are returned',
          payload: products,
        })
      );
    };

    const getSingleProduct = async (res, id) => {
      const products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
      const product = products.find((product) => product.id === id);
      res.writeHead(httpStatus.OK, { 'Content-Type': 'application/json' });
      res.end(
        JSON.stringify({
          message: 'single product is returned',
          payload: product,
        })
      );
    };

    const deleteSingleProduct = async (res, id) => {
      let products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
      const filteredProducts = products.filter((product) => product.id !== id);
      products = filteredProducts;
      await fs.writeFile('./products.json', JSON.stringify(products));
      res.writeHead(httpStatus.OK, { 'Content-Type': 'application/json' });
      res.end(
        JSON.stringify({
          message: 'single product is deleted',
        })
      );
    };

    module.exports = { getAllProducts, getSingleProduct, deleteSingleProduct };

  // index.js
  const http = require('http');

  const { httpStatus } = require('./constants/httpStatus');
  const {
    getAllProducts,
    getSingleProduct,
    deleteSingleProduct,
  } = require('./controllers/productsController');

  const getIdFromUrl = (url) => {
    return url?.split['/'](2);
  };

  const server = http.createServer((req, res) => {
    if (req.url === '/products' && req.method === 'GET') {
      getAllProducts(res);
    } else if (req.url?.match(/\/products\/([0-9]+)/) && req.method === 'GET') {
      getSingleProduct(res, getIdFromUrl(req.url));
    } else if (
      req.url?.match(/\/products\/([0-9]+)/) &&
      req.method === 'DELETE'
    ) {
      deleteSingleProduct(res, getIdFromUrl(req.url));
    } else {
      res.writeHead(httpStatus.NOT_FOUND, { 'Content-Type': 'application/json' });
      res.end(
        JSON.stringify({
          message: 'route not found',
        })
      );
    }
  });

  server.listen(3002, () => {
    console.log(`server is running at http://localhost:3002`);
  });

  ```

- create responseController - successResponse

  ```js
    // controller/responseController.js
    exports.successResponse = (
      res,
      statusCode = 200,
      message = 'success',
      payload = {}

    ) => {
      res.writeHead(statusCode, { 'Content-Type': 'application/json' });
      res.end(
        JSON.stringify({
          message: message,
          payload: payload,
        })
      );
    };

       
  // now we can clean up the controller
  const fs = require('fs/promises');

  const { httpStatus } = require('../constants/httpStatus');
  const { successResponse } = require('./responseController');

  const getAllProducts = async (res) => {
    const products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
    successResponse(res, httpStatus.OK, 'all products are returned', products);
  };

  const getSingleProduct = async (res, id) => {
    const products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
    const product = products.find((product) => product.id === id);
    successResponse(res, httpStatus.OK, 'single product is returned', product);
  };

  const deleteSingleProduct = async (res, id) => {
    let products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
    const filteredProducts = products.filter((product) => product.id !== id);
    products = filteredProducts;
    await fs.writeFile('./products.json', JSON.stringify(products));
    successResponse(res, httpStatus.OK, 'single product is deleted');
  };

  module.exports = { getAllProducts, getSingleProduct, deleteSingleProduct };
  ```

- create responseController - errorResponse

  ```js
  exports.errorResponse = (
      res,
      statusCode = 500,
      message = 'Internal server error'
    ) => {
      res.writeHead(statusCode, { 'Content-Type': 'application/json' });
      res.end(
        JSON.stringify({
          message: message,
        })
      );
    };

    // now again we can update the controller
    const fs = require('fs/promises');

    const { httpStatus } = require('../constants/httpStatus');
    const { successResponse, errorResponse } = require('./responseController');

    const getAllProducts = async (res) => {
      try {
        const products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
        successResponse(res, httpStatus.OK, 'all products are returned', products);
      } catch (error) {
        errorResponse(res, 500, error.message);
      }
    };

    const getSingleProduct = async (res, id) => {
      try {
        const products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
        const product = products.find((product) => product.id === id);
        if (!product) {
          const error = new Error(`product is not found with this id ${id}`);
          error.status = 404;
          throw error;
        }
        successResponse(res, httpStatus.OK, 'single product is returned', product);
      } catch (error) {
        errorResponse(res, err.status, error.message);
      }
    };

    const deleteSingleProduct = async (res, id) => {
      try {
        let products = JSON.parse(await fs.readFile('./products.json', 'utf-8'));
        const product = products.find((product) => product.id === id);
        if (!product) {
          const error = new Error(`product is not found with this id ${id}`);
          error.status = 404;
          throw error;
        }
        const filteredProducts = products.filter((product) => product.id !== id);
        products = filteredProducts;
        await fs.writeFile('./products.json', JSON.stringify(products));
        successResponse(res, httpStatus.OK, 'single product is deleted');
      } catch (error) {
        errorResponse(res, err.status, error.message);
      }
    };

    module.exports = { getAllProducts, getSingleProduct, deleteSingleProduct };
  ```

- create services
  
  ```js
    // services/productService
    const { httpStatus } = require('../constants/httpStatus');

    const { errorResponse } = require('../controllers/responseController');

    const findProductById = (products, id) => {
      const product = products.find((product) => product.id === id);
      if (!product) {
        const error = new Error(`product is not found with this id ${id}`);
        error.status = 404;
        throw error;
      }
      return product;
    };
    module.exports = { findProductById };

  // now the controller will look like
  const fs = require('fs/promises');

  const { httpStatus } = require('../constants/httpStatus');
  const { successResponse, errorResponse } = require('./responseController');
  const { findProductById } = require('../services/productService');

  const PRODUCTS_FILE_PATH = './products.json';

  const getAllProducts = async (res) => {
    try {
      const products = JSON.parse(await fs.readFile(PRODUCTS_FILE_PATH, 'utf-8'));
      successResponse(res, httpStatus.OK, 'all products are returned', products);
    } catch (error) {
      errorResponse(res, error.status || 500, error.message);
    }
  };

  const getSingleProduct = async (res, id) => {
    try {
      const products = JSON.parse(await fs.readFile(PRODUCTS_FILE_PATH, 'utf-8'));
      const product = findProductById(products, id);
      successResponse(res, httpStatus.OK, 'single product is returned', product);
    } catch (error) {
      errorResponse(res, error.status || 500, error.message);
    }
  };

  const deleteSingleProduct = async (res, id) => {
    try {
      let products = JSON.parse(await fs.readFile(PRODUCTS_FILE_PATH, 'utf-8'));
      findProductById(products, id);
      const filteredProducts = products.filter((product) => product.id !== id);
      products = filteredProducts;
      await fs.writeFile(PRODUCTS_FILE_PATH, JSON.stringify(products));
      successResponse(res, httpStatus.OK, 'single product is deleted');
    } catch (error) {
      errorResponse(res, error.status || 500, error.message);
    }
  };

  module.exports = { getAllProducts, getSingleProduct, deleteSingleProduct };


  ```

### [1.7 CRUD APP]

- [ecommerce crud app github link](https://github.com/anisul-Islam/node-crud-app)

- add update functionality

  ```js
    // add the routes first in index.js
    else if (req.url?.match(/\/products\/([0-9]+)/) && req.method === 'PUT') {
      updatesingleProduct(req, res, getIdFromUrl(req.url));
    }

    // add a service
    const findProductIndexById = (products, id) => {
      const productIndex = products.findIndex(
        (product) => product.id === productId
      );
      if (productIndex === -1) {
        const error = new Error(`product is not found with this id ${id}`);
        error.status = 404;
        throw error;
      }
      return productIndex;

    };

    module.exports = { findProductById, findProductIndexById };

    // now add the code in controller
    const updatesingleProduct = async (req, res, id) => {
      try {
        let products = JSON.parse(await fs.readFile(PRODUCTS_FILE_PATH, 'utf-8'));
        const productIndex = findProductIndexById(products, id);

        // get the data from the request body
        let input = '';
        req.on('data', (chunk) => {
          input += chunk;
        });

        req.on('end', async () => {
          const newItem = parse(input);
          // const updatedProduct = {
          //   id: id,
          // };
          // if (String(newItem.title)) {
          //   updatedProduct.title = String(newItem.title);
          // }
          // if (Number(newItem.price)) {
          //   updatedProduct.price = Number(newItem.price);
          // }
          // products[index] = updatedProduct;
          products[productIndex] = {
            ...products[productIndex], // id, title, price
            ...newItem, // price
          };

          await fs.writeFile(PRODUCTS_FILE_PATH, JSON.stringify(products));
          successResponse(res, httpStatus.OK, 'Product is updated successfully');
        });
      } catch (error) {
        errorResponse(res, error.status || 500, error.message);
      }

    };
  ```
- add create functionality
  
  ```js
    // first add the route
    else if (req.url === '/products' && req.method === 'POST') {
      createSingleProduct(req, res, getIdFromUrl(req.url));
    }

    // lets add code in controller now
    const createSingleProduct = async (req, res, id) => {
      try {
        let products = JSON.parse(await fs.readFile(PRODUCTS_FILE_PATH, 'utf-8'));
        // get the data from the request body
        let input = '';
        req.on('data', (chunk) => {
          input += chunk;
        });

        req.on('end', async () => {
          const newItem = parse(input);
          products.push({ id: new Date().getTime().toString(), ...newItem });
          await fs.writeFile(PRODUCTS_FILE_PATH, JSON.stringify(products));
          successResponse(
            res,
            httpStatus.CREATED,
            'Product is created successfully'
          );
        });
      } catch (error) {
        errorResponse(res, error.status || 500, error.message);
      }

    };
  ```

### [1.8 npm crash course]

- [1.8 npm crash course](https://youtu.be/A8W1p8suw5I)

- first initialize npm with the command `npm init` then follow the instructions
- we can also use `npm init -y` command for ignoring the installation instructions
- npm packages : https://www.npmjs.com/
- how to install and uninstall npm packages
- Install https://www.npmjs.com/package/random-fruits-name package
  and follow the instructions
- add uuid package to the ecommerce crud app

### 1.9 Connect front-end and backend

### 1.10 Another CRUD APP: Complete REST API

```js
// index.js
const http = require("http");
const {
  getProducts,
  getProduct,
  deleteProduct,
  addProduct,
  updateProduct,
} = require("./controllers/products");

const PORT = 8080;

const sendResponse = (statusCode, request, response, data) => {
  response.writeHead(statusCode, { "Content-Type": "application/json" });
  response.write(JSON.stringify(data));
  response.end();
};

const server = http.createServer(async (request, response) => {
  if (request.url === "/" && request.method === "GET") {
    try {
      sendResponse(200, request, response, {
        message: "welcome to home route",
      });
    } catch (error) {
      sendResponse(404, request, response, error);
    }
  } else if (request.url === "/api/products" && request.method === "GET") {
    try {
      const result = await getProducts();
      sendResponse(200, request, response, result);
    } catch (error) {
      sendResponse(404, request, response, error);
    }
  } else if (
    request.url.match(/\/api\/products\/([0-9]+)/) &&
    request.method === "GET"
  ) {
    try {
      const id = request.url?.split("/")[3];
      const result = await getProduct(id);
      sendResponse(200, request, response, result);
    } catch (error) {
      sendResponse(404, request, response, error);
    }
  } else if (
    request.url.match(/\/api\/products\/([0-9]+)/) &&
    request.method === "DELETE"
  ) {
    try {
      const id = request.url?.split("/")[3];
      const result = await deleteProduct(id);
      sendResponse(200, request, response, result);
    } catch (error) {
      sendResponse(404, request, response, error);
    }
  } else if (
    request.url.match(/\/api\/products\/([0-9]+)/) &&
    request.method === "PUT"
  ) {
    try {
      const id = request.url?.split("/")[3];
      const result = await updateProduct(request, id);
      sendResponse(200, request, response, result);
    } catch (error) {
      sendResponse(404, request, response, error);
    }
  } else if (request.url === "/api/products" && request.method === "POST") {
    try {
      const result = await addProduct(request);
      sendResponse(201, request, response, result);
    } catch (error) {
      sendResponse(404, request, response, error);
    }
  } else {
    response.writeHead(404, { "Content-Type": "application/json" });
    response.write(
      JSON.stringify({
        message: "404 not found",
      })
    );
    response.end();
  }
});

server.listen(PORT, () => {
  console.log(`server is running at http://localhost:${PORT}`);
});

// controllers/products.js
let products = require("../models/products");

const getProducts = async () => {
  return new Promise((resolve, reject) => {
    resolve(products);
  });
};

const getProduct = async (id) => {
  return new Promise((resolve, reject) => {
    const product = products.find((product) => product.id === id);
    product ? resolve(product) : reject(`Product with ${id} Not found`);
  });
};

const deleteProduct = async (id) => {
  return new Promise((resolve, reject) => {
    const product = products.find((product) => product.id === id);
    product
      ? resolve((products = products.filter((product) => product.id !== id)))
      : reject(`Product with ${id} Not found`);
  });
};

const updateProduct = async (request, id) => {
  return new Promise((resolve, reject) => {
    try {
      let body = "";
      request.on("data", (chunk) => {
        body += chunk.toString();
      });

      request.on("end", () => {
        const parsedData = new URLSearchParams(body);
        const title = parsedData.get("title") || "";
        const price = Number(parsedData.get("price"));

        products
          .filter((product) => product.id === id)
          .map((product) => {
            product.title = title;
            product.price = price;
          });
        resolve(products);
      });
    } catch (error) {
      reject(error);
    }
  });
};
const addProduct = async (request) => {
  return new Promise((resolve, reject) => {
    try {
      const FORM_URLENCODED = "application/x-www-form-urlencoded";
      if (request.headers["content-type"] === FORM_URLENCODED) {
        let body = "";
        request.on("data", (chunk) => {
          body += chunk.toString();
        });

        request.on("end", () => {
          console.log("all the body data has arrived");
          const parsedData = new URLSearchParams(body);
          const title = parsedData.get("title") || "";
          const price = Number(parsedData.get("price"));
          const newProduct = {
            id: new Date().toISOString(),
            title,
            price,
          };
          products.push(newProduct);
          resolve(products);
        });
      } else {
        // Use latin1 encoding to parse binary files correctly
        //nodejs.org/en/knowledge/HTTP/servers/how-to-handle-multipart-form-data/
        resolve(request.headers["content-type"]);
        let body = "";
        request.on("data", (chunk) => {
          body += chunk.toString();
        });

        request.on("end", () => {
          console.log("all the body data has arrived");

          console.log(body);
          // const title = parsedData.get("title") || "";
          // const price = Number(parsedData.get("price"));
          // const newProduct = {
          //   id: new Date().toISOString(),
          //   title,
          //   price,
          // };
          // products.push(newProduct);
          // resolve(products);
        });
      }
    } catch (error) {
      reject(error);
    }
  });
};

module.exports = {
  getProducts,
  getProduct,
  deleteProduct,
  addProduct,
  updateProduct,
};

// models/products.js
let products = [
  {
    id: "1",
    title: "Fjallraven - Foldsack No. 1 Backpack, Fits 15 Laptops",
    price: 109.95,
  },
  {
    id: "2",
    title: "Mens Casual Premium Slim Fit T-Shirts ",
    price: 22.3,
  },
  {
    id: "3",
    title: "Mens Cotton Jacket",
    price: 55.99,
  },
  {
    id: "4",
    title: "Mens Casual Slim Fit",
    price: 15.99,
  },
  {
    id: "5",
    title:
      "John Hardy Women's Legends Naga Gold & Silver Dragon Station Chain Bracelet",
    price: 695,
  },
];

module.exports = products;
```

### 1.11 server side rendering

    ```html
      <!DOCTYPE html>
      <html lang="en">
        <head>
          <meta charset="UTF-8" />
          <meta http-equiv="X-UA-Compatible" content="IE=edge" />
          <meta name="viewport" content="width=device-width, initial-scale=1.0" />
          <title>home</title>
        </head>
        <body>
          <nav>
            <ul>
              <li><a href="/">Home</a></li>
              <li><a href="/about">About</a></li>
              <li><a href="/contact">Contact</a></li>
            </ul>
          </nav>
          <main>
            <h1>welcome to home page</h1>
          </main>
        </body>
      </html>
    ```

  ```html
  // about.html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>about</title>
    </head>
    <body>
      <nav>
        <ul>
          <li><a href="/">Home</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>
      </nav>
      <main>
        <h1>welcome to about page</h1>
      </main>
    </body>
  </html>
  ```

  ```html
  // contact.html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>contact</title>
    </head>
    <body>
      <nav>
        <ul>
          <li><a href="/">Home</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>
      </nav>
      <main>
        <h1>welcome to contact page</h1>
      </main>
    </body>
  </html>
  ```

  ```html
  // error.html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>error</title>
    </head>
    <body>
      <nav>
        <ul>
          <li><a href="/">Home</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>
      </nav>
      <main>
        <h1>page not found 404</h1>
      </main>
    </body>
  </html>
  ```

  ```js
  // index.js
  const http = require("http");
  const fs = require("fs");

  const PORT = 3000;

  const server = http.createServer((req, res) => {
    const handleReadFile = (fileName, statusCode) => {
      fs.readFile(fileName, (err, data) => {
        res.writeHead(statusCode, { "Content-Type": "text/html" });
        res.write(data);
        res.end();
      });
    };

    if (req.url === "/") {
      handleReadFile("index.html", 200);
    } else if (req.url === "/about") {
      handleReadFile("about.html", 200);
    } else if (req.url === "/contact") {
      handleReadFile("contact.html", 200);
    } else {
      handleReadFile("error.html", 404);
    }
  });

  server.listen(PORT, () => {
    console.log(`server is running at http://localhost:${PORT}`);
  });  
  ```

## 2. Express.js + TypeScript

### [2.1 Introduction & installation]

- always check the documentation of [express.js](https://www.npmjs.com/package/express)
- Express.js is a node.js framework which makes life easier. No more hassle of setting Content-Type. Easy to learn and time saving facilitites available because we have ready made stuff in express.js
- Stack: MERN Stack, NERD stack, PERN stack
- Environment setup:
  - download & install VSCode editor
  - install node and check version

    - check: node -v
    - check: npm -v

  - install nodemon globally

    - npm install -g nodemon
    - check: nodemon -v

  - install typescript globally

    - npm install -g typescript
    - check: tsc -v

  - install ts-node globally

    - npm install -g ts-node
    - check: ts-node -v
  - download & install insomnia/postman/ThunderClient extension
- create a directory and a folder src -> index.ts
- initialize npm
- install packages

  ```txt

    // Install Express, TypeScript, and other necessary dependencies

    npm install express typescript ts-node @types/node @types/express --save

    ts-node: This is a TypeScript execution environment and REPL for Node.js. It allows you to run TypeScript code directly without the need to transpile it to JavaScript first.

    @types/express and @types/node: These are TypeScript type definitions for the Express and Node.js libraries. They provide TypeScript with information about the types and structures used in these libraries.

    // Install nodemon as a development dependency

    npm install nodemon --save-dev
  ```
  
- add gitignore file for node
- add a README.md file

- Configure typescript: create tsconfig.json in the root directory. initialize tsconfig.json the typescript manager: tsc --init and change the target as ES2017 in tsconfig.json

  ```json
  // tsconfig.json

  {
    "compilerOptions": {
      "target": "es6",
      "module": "commonjs",
      "outDir": "./dist",
      "rootDir": "./src",
      "strict": true,
      "esModuleInterop": true
    },
    "include": ["src/**/*.ts"],
    "exclude": ["node_modules"]
  }
  ```

- update the package.json

  ```json
    "scripts": {
      "start": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/index.ts",
      "build": "tsc",
      "dev": "npm run start",
      "prod": "node dist/index.js"
      }
  ```

- `tsc -w` If you want tsc (TypeScript compiler) to watch for changes and recompile your TypeScript files automatically,

### [2.2 Create express server]

- create the express server

  ```ts
  import express, { Application } from "express";

  const app: Application = express();

  const port: number = 3002;

  app.listen(port, () => {
    console.log(`server is running at http://localhost:${port}`);
  });
  ```

### [2.3 Set environment variables]

- check the own doc for env setup
- create .env file && add .env file to .gitignore file
- add variables inside .env file -> PORT=3001
- install dotenv package: npm install dotenv

```js
// .env -> PORT=3001
import * as dotenv from "dotenv";
dotenv.config();

import express, { Application } from "express";

const app: Application = express();

const port: string | number = process.env.PORT || 3002;

app.listen(port, () => {
  console.log(`server is running at http://localhost:${port}`);
});
```

### [2.4 add products and Product Type]

  ```ts
    
  ```

### [2.5 add products and Product Type]

  ```ts
  // src/types.ts
  export type Product = {
    id: string;
    title: string;
    price: number;
    description?: string;
    image?: string;
  };

  export type ProductInput = Omit<Product, 'id'>;
  ```



### [2.6 HTTP methods: handle GET request & api testing]

- http methods
- http status code

```js
import * as dotenv from "dotenv";
dotenv.config();

import express, { Application, Request, Response } from "express";

const app: Application = express();

const port: string | number = process.env.PORT || 3002;

app.get("/", (request: Request, response: Response) => {
  response.send("welcome to the express server");
});
app.get("/products", (request: Request, response: Response) => {
  response.send("return all the products");
});

app.listen(port, () => {
  console.log(`server is running at http://localhost:${port}`);
});
```

### [2.7 Response object]

- response can be text, html, json
- res.send("some text here");
- res.status(statuscode).json({...});
- res.sendFile(fileName);
- res.cookie(key, value);
- res.clrarCookie(key);
- res.writeHead()
- res.write()
- res.end()
- res.append(key, value); this will set as response header
- text, HTML, JSON

```js
res.send("get all products data");
res.sendFile("products.html");
let products = [
  {
    id: "1",
    title: "Fjallraven - Foldsack No. 1 Backpack, Fits 15 Laptops",
    price: 109.95,
  },
  {
    id: "2",
    title: "Mens Casual Premium Slim Fit T-Shirts ",
    price: 22.3,
  },
];

app.get("/products", (request: Request, response: Response) => {
  response.status(200).json(products);
});
```

### [2.8 find path of a file]

- create a util folder -> getPath.ts

```js
import path from "path";
export const getPath = (
  dirName: string,
  folderName: string,
  fileName: string
): string => {
  return path.join(dirName, folderName, fileName);
};

// index.ts
app.get("/", (request: Request, response: Response) => {
  const filePath = getPath(__dirname, "views", "index.html");
  response.status(200).sendFile(filePath);
});
```

### [2.9 Handling error - client and server error]

- try to access a file which is not linked or created properly. it will give a server error

```js
app.get("/", (request: Request, response: Response) => {
  response.status(200).sendFile("index.html");
});

// client error
app.use((req: Request, res: Response) => {
  res.status(404).send("<h2>Page not found 404</h2>");
});

// server error
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});
```

### [2.10 Request Object - route parameter]

- request with route parameters - req.params.parameterName

```js
// route parameter
app.get("/products/:id", (request: Request, response: Response) => {
  const singleProduct = products.find(
    (product) => product.id === request.params.id
  );
  if (singleProduct) {
    response.status(200).json(singleProduct);
  } else {
    response.status(404).json({
      message: "product with this id is not found",
    });
  }
});
```

### [2.11 Request Object - query parameter]

- request with query parameter - req.query.parameterName
- query parameter has question mark; search something on google.

- example of query parameter - http://localhost:3001?id=101&maxPrice=23

- we can get the value using req.query.id and req.query.maxPrice

```js
app.get("/products", (request: Request, response: Response) => {
  const maxPrice = Number(request.query.maxPrice);
  if (maxPrice) {
    const result = products.filter((product) => product.price <= maxPrice);
    response.status(200).send(result);
  }
  response.status(200).send(products);
});
```

- extra example

```js
// route parameter
app.get("/products/:id", (request: Request, response: Response) => {
  const singleProduct = products.find(
    (product) => product.id === request.params.id
  );
  if (singleProduct) {
    response.status(200).json(singleProduct);
  } else {
    response.status(404).json({
      message: "product with this id is not found",
    });
  }
});

// query parameter
const sortItems = (sortBy, items) => {
  if (sortBy === "ASC") {
    return items.sort((a, b) => a.price - b.price);
  } else if (sortBy === "DESC") {
    return items.sort((a, b) => b.price - a.price);
  }
};

const sortItems = (sortBy, items) => {
  if (sortBy === "ASC") {
    return items.sort((a, b) => a.price - b.price);
  } else if (sortBy === "DESC") {
    return items.sort((a, b) => b.price - a.price);
  }
};

app.get("/products", (req, res) => {
  const maxPrice = Number(req.query.maxPrice);
  const sortBy = req.query.sortBy;
  let result;
  if (maxPrice) {
    result = products.filter((product) => product.price <= maxPrice);
    result = sortBy ? sortItems(sortBy, result) : result;
    res.status(200).send(result);
  } else {
    res.status(200).send(products);
  }
});
```

### [2.12 Request Object - header requests]

- example of how to get data header requests

  ```js
  router.post("/", (req, res) => {
    console.log(req.header("id"));
    res.send("I am get method of user route");
  });
  ```

### [2.13 Request Object - body data & HTTP methods: POST]

- **[handling-any-post-data-in-express](https://codex.so/handling-any-post-data-in-express)**
- request with json data / form data inside body - req.body.parameterName

```js
// we can receive json data inside the body - usual one
app.use(express.json());

// we can receive form data inside the body
app.use(express.urlencoded({ extended: true }));

app.post("/products", (request: Request, response: Response) => {
  const newProduct = {
    id: new Date().toISOString(),
    title: request.body.title,
    price: request.body.price,
  };
  products.push(newProduct);
  response.status(201).send(newProduct);
});
```

- example of request with json data

  - first add `app.use(express.json())`; for form data use `app.use(express.urlencoded({extended: true}))`
  - then access the data using `req.body.parameterName`

  ```js
  // sending json or from data when making request
  {
    "name" : "anisul"
  }

  router.post("/", (req, res) => {
    res.status(201).json({
      message: "user is created",
      name: req.body.name,
    });
  });
  ```

### [2.14 HTTP methods: handle DELETE request & api testing]

```js
app.delete("/products/:id", (request: Request, response: Response) => {
  const singleProduct = products.find(
    (product) => product.id === request.params.id
  );
  if (singleProduct) {
    products = products.filter((product) => product.id !== request.params.id);
    response.status(200).json(products);
  } else {
    response.status(404).json({
      message: "product with this id is not found",
    });
  }
});
```

### [2.15 HTTP methods: handle PUT request & api testing]

### [2.16 express Router & path shotcut]

- use morgan package for getting more info about routing on console

  ```js
     step1 : npm install morgan
     step2 : const morgan = require("morgan");
     step3 : app.use(morgan("dev"));
  ```

- /api/products

```js
// routes/products.ts
import express, { NextFunction, Request, Response, Router } from "express";
const router: Router = express.Router();
export default router;

// index.ts
import productRouter from "./routes/products";
app.use("/api/products", productRouter);
```

### [2.17 MVC Architecture - SOC & services]

- service-oriented architecture, services are responsible for handling business logic and interacting with the database.

- separate routes
- separate logics with controllers

  - controller for error as well

- separate data

```js
// models/products.ts
export let products = [
  {
    id: "1",
    title: "Fjallraven - Foldsack No. 1 Backpack, Fits 15 Laptops",
    price: 109.95,
  },
  {
    id: "2",
    title: "Mens Casual Premium Slim Fit T-Shirts ",
    price: 22.3,
  },
];
```

### [2.18 Regular expression and wild card]

```js
router.get("/:id([0-9]+)", getProduct);
router.get("/:id([0-9]{3})", getProduct);
router.delete("/:id([0-9]{3})", deleteProduct);

// wild card
router.use("*", (request: Request, response: Response) => {
  response.status(404).json({
    message: "not a valid route",
  });
});

// more examples
//regular expression
// we can use 0-9 but maximum 4 digits combination
router.get("/search/:id([0-9]{4})", (req, res) => {
  res.status(200).send("serach user by id" + req.params.id);
});

// only letters allowed with maximum 5 characters
router.get("/search-username/:name([a-zA-Z]{5})", (req, res) => {
  res.status(200).send("serach user by name" + req.params.name);
});

// wild cart
router.get("*", (req, res) => {
  res.status(404).send({
    message: "url not found",
  });
});
```

### [2.19 Middleware and its type]

- what is middleware?
- why middleware?
- types of middleware
- create a middleware folder

```js
// middlewar/authUser.ts
import { NextFunction, Request, Response } from "express";
export const authUser = (
  request: Request,
  response: Response,
  next: NextFunction
) => {
  request.body.isAdmin = false;
  console.log(request.body.isAdmin);
  next();
};

// routes/products.ts
import { authUser } from "../middleware/authUser";
router.post("/", authUser, addProduct);

// controllers/products.ts
export const addProduct = (request: Request, response: Response) => {
  if (request.body.isAdmin) {
    const newProduct = {
      id: new Date().toISOString(),
      title: request.body.title,
      price: request.body.price,
    };
    products.push(newProduct);
    response.status(201).send(newProduct);
  } else {
    response.status(401).send({
      message: "sorry you have no permission for adding product",
    });
  }
};
```

### [2.20 Server side validation with express-validator]

- `npm i express-validator @types/express-validator`
- [Source code](https://github.com/anisul-Islam/express-validator-tutorial)

### [2.21 Server side validation with joi]

- [Source code](https://github.com/anisul-Islam/joi-validator-tutorial)

### [2.22 How to upload file (image, video etc.)]

- `npm install express multer`
- `npm install -D express multer`
- package.json -> `"start": "nodemon index.js"`
- create the client part

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <form action="/register" method="post" enctype="multipart/form-data">
      <input type="file" name="image" />
      <button type="submit">Register</button>
    </form>
  </body>
</html>
```

- create the server

```js
const express = require("express");
const multer = require("multer");

const app = express();

const port = 8005;

// uploading file
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, "uploads/");
  },
  filename: function (req, file, cb) {
    //original name helps us to get the file extension
    cb(null, Date.now() + "-" + file.originalname);
  },
});

const upload = multer({ storage: storage });

app.get("/register", (req, res) => {
  res.status(200).sendFile(__dirname + "/index.html");
});

app.post("/register", upload.single("image"), (req, res) => {
  res.status(200).send("user is registered");
});

app.get("/test", (req, res) => {
  res.status(200).send("testing api");
});

app.listen(port, () => {
  console.log(`server is running at http://localhost:${port}`);
});
```

### remove this soon

```ts
// services using types
export const findProductBySlug = async (slug: string): Promise<IProduct> => {
  const product = await Product.findOne({ slug: slug });
  if (!product) {
    const error = createHttpError(404, 'Product not found with this slug');
    throw error;
  }
  return product;
};
```

### [2.23 How to upload file + TypeScript (image, video etc.)]

- add image property to the schema

  ```ts
     import mongoose, { Schema, model, Document } from 'mongoose';

      import { ICategory } from './categorySchema';

      // we can use interface or type definitions
      export interface IProduct extends Document {
        title: string;
        slug: string;
        price: number;
        quantity: number;
        sold: number;
        image: string;
        shipping: number;
        // category: ICategory['_id'];
        category: { _id: mongoose.Schema.Types.ObjectId };
        description: string;
        createdAt?: string;
        updatedAt?: string;
      }
      // export type ProductDocument = Document & {
      //   title: string;
      //   slug: string;
      //   price: number;
      //   quantity: number;
      //   sold: number;
      //   image: string;
      //   shipping: number;
      //   // category: ICategory['_id'];
      //   category: {_id: mongoose.Schema.Types.ObjectId };
      //   description: string;
      //   createdAt?: string;
      //   updatedAt?: string;
      // };

      const productSchema = new Schema(
        {
          // missing: image, category
          title: {
            type: String,
            // unique: true,
            required: true,
            trim: true,
            minlength: [3, 'Product title must be at least 3 characters long'],
            maxlength: [300, 'Product title must be at most 300 characters'],
          },
          slug: {
            type: String,
            unique: true,
            lowercase: true,
          },
          image: {
            type: String,
            default: 'public/images/products/default.png',
          },
          price: {
            type: Number,
            required: true,
            trim: true,
          },
          quantity: {
            type: Number,
            required: true,
            trim: true,
          },
          sold: {
            type: Number,
            deafult: 0,
            required: true,
            trim: true,
          },
          shipping: {
            type: Number,
            default: 0, // 0 -> free
          },
          category: { type: Schema.Types.ObjectId, ref: 'Category', required: true },
          description: {
            type: String,
            required: true,
            trim: true,
            minlength: [3, 'Product description must be at least 3 characters long'],
          },
        },
        { timestamps: true }
      );

      // create the model/collections
      export const Product = model<IProduct>('Products', productSchema);

  ```

- create public/images/products folder
- `npm install multer @types/multer`
- create a middleware in middlewares/uploadFile.ts

```ts
// without ts
import { Request } from 'express';
import multer from 'multer';

const productStorage = multer.diskStorage({
  destination: function (
    req,
    file,
    cb
  ) {
    cb(null, 'public/images/products');
  },
  filename: function (req, file, cb) {
    cb(null, Date.now() + '-' + file.originalname);
  },
});

export const upload = multer({ storage: productStorage });

// add upload.single("image") as middleware
// access the image data as req.file?.path;

// now lets add ts to uploadFile.ts
import { Request } from 'express';
import multer from 'multer';

type DestinationCallback = (error: Error | null, destination: string) => void;
type FileNameCallback = (error: Error | null, filename: string) => void;

const productStorage = multer.diskStorage({
  destination: function (
    req: Request,
    file: Express.Multer.File,
    cb: DestinationCallback
  ): void {
    cb(null, 'public/images/products');
  },
  filename: function (
    req: Request,
    file: Express.Multer.File,
    cb: FileNameCallback
  ): void {
    cb(null, Date.now() + '-' + file.originalname);
  },
});

export const upload = multer({ storage: productStorage });

```

### [2.24 Express generator]

- package `npx espress-generator`
- create a basic standard scalable folder structure with necessary codes

### [2.25 cookie]

- to make http request stateful (have some info stored about api call like user is signed in or not) , by default http api call are stateless
- keep track of user activity
- `var cookieParser = require("cookie-parser");`
- `app.use(cookieParser());`

```js
const router = require("express").Router();
let products = [
  {
    id: 1,
    title: "Iphone 14",
    price: 1500,
  },
  {
    id: 2,
    title: "Iphone 12",
    price: 700,
  },
  {
    id: 3,
    title: "Iphone 13",
    price: 1200,
  },
];

// middleware example
const isLoggedIn = (req, res, next) => {
  req.cookies.login ? next() : res.redirect("/");
};

router.get("/login", (req, res) => {
  res.cookie("login", true, {
    maxAge: 10000,
  });
  res.send("logged in");
});

router.get("/", isLoggedIn, (req, res) => {
  res.send(products);
});

router.get("/:id", isLoggedIn, (req, res) => {
  const id = parseInt(req.params.id);
  const product = products.find((product) => product.id === id);
  res.send(product);
});
```

### [2.26 session]

### [2.27 Full-stack ecommerce app without DB integration]

#### 1. create a basic server

- create a server.js file
- initialize npm and make sure to use module js `"type": "module",`
- install packages `npm install express`
- create a simple server

  ```js
  import express from 'express';
  const app = express();
  app.listen(3002, () => {
    console.log(`server is running at http://localhost:3002`);
  });
  ```

- add .gitignore for node.js
- create a simple get route

  ```js
  app.get('/', (req, res) => {
    res.send('health check');
  });
  ```

- run the project with nodemon: `npm install nodemon`

  - install the package `npm install nodemon`
  - write the script `dev: nodemon server.js`

- setup the env variable

  - install dotenv: `npm install dotenv`
  - create a .env file in the root folder and define variables `SERVER_PORT=3002`
  - create a config folder and access the env variables

    ```js
    // config/index.js
    import 'dotenv/config';
    export const dev = {
      app: {
        port: process.env.SERVER_PORT,
      },
      db: {},
    };
    ```

  - now everywhere import all the environment variables from config folder

    ```js
    import express from 'express';

    import { dev } from './config/index.js';

    const app = express();

    const port = dev.app.port || 8080;

    app.listen(port, () => {
      console.log(`server is running at http://localhost:${port}`);
    });

    app.get('/', (req, res) => {
      res.send('health check');
    });
    ```

  - add express error handler middleware

    ```js
    // Error handling middleware for client errors (404)
    app.use((req, res, next) => {
      res.status(404).json({
        message: 'Not Found',
      });
    });

    // Error handling middleware for server errors (5xx)
    app.use((err, req, res, next) => {
      res.status(err.status || 500).json({
        message: err.message || 'Internal Server Error',
      });
    });
    ```

#### 2. Testing API and Security

- create a postman collection and check the health

- morgan setup
  - install: `npm install --save-dev morgan`
  - use them: `app.use(morgan("dev"));`
- cors setup
  - install: `npm install cors`
  - use them: `app.use(cors());`
- express-rate-limit setup

  ```js
  import { rateLimit } from 'express-rate-limit';

  // Restrict all routes to only 100 requests per IP address every 1o minutes
  const apiLimiter = rateLimit({
    windowMs: 1 * 60 * 1000, // 1 minutes
    max: 5, // 5 requests per IP
    message:
      'Too many request has been made from this IP, please try again after a minute',
  });

  // health chekup
  app.get('/test', apiLimiter, (req, res) => {
    res.status(200).json({
      message: 'testing is successful',
    });
  });
  ```

- finally it looks like

  ```js
  import express from 'express';
  import morgan from 'morgan';
  import cors from 'cors';
  import { rateLimit } from 'express-rate-limit';

  import { dev } from './config/index.js';

  const app = express();

  const port = dev.app.port || 8080;

  app.listen(port, () => {
    console.log(`server is running at http://localhost:${port}`);
  });

  app.use(morgan('dev'));
  app.use(cors());

  const apiLimiter = rateLimit({
    windowMs: 1 * 60 * 1000, // 1 minutes
    max: 5, // 5 requests per IP
    message: 'Too many request has been made. please try again after a minute',
  });

  app.get('/', apiLimiter, (req, res) => {
    res.send('health check');
  });

  // Error handling middleware for client errors (404)
  app.use((req, res, next) => {
    res.status(404).json({
      message: 'Not Found',
    });
  });

  // Error handling middleware for server errors (5xx)
  app.use((err, req, res, next) => {
    res.status(err.status || 500).json({
      message: err.message || 'Internal Server Error',
    });
  });
  ```

#### 3. Create Product CRUD API

- GET: /products -> return all the products

  - create a simple get route

    ```js
    let products = [
      { id: '1', title: 'product 1', price: 430 },
      { id: '2', title: 'product 2', price: 530 },
    ];

    // GET: /products -> return all the products
    app.get('/products', (req, res) => {
      try {
        res.status(200).json({
          message: 'returned all products',
          payload: products,
        });
      } catch (error) {
        next(error);
      }
    });
    ```

- organize everything following MVC Architecture

  - create models -> productModel.js ->

    ```js
    export let products = [
      { id: '1', title: 'product 1', price: 430 },
      { id: '2', title: 'product 2', price: 530 },
    ];
    ```

  - create routers -> productRouter.js

    ```js
    import { Router } from 'express';

    import { getProducts } from '../controllers/productController.js';

    const router = Router();

    // GET: /products -> return all the products
    router.get('/products', getProducts);

    export default router;
    ```

  - create controllers -> productController.js

    ```js
    import { products } from '../models/productModel.js';

    export const getProducts = (req, res) => {
      try {
        res.status(200).json({
          message: 'returned all products',
          products: products,
        });
      } catch (error) {
        res.status(500).json({
          message: error.message,
        });
      }
    };
    ```

- create errorResponse and successResponse

  ```js
  export const successResponse = (
    res,
    statusCode = 200,
    message = 'success',
    payload = {}
  ) => {
    res.status(statusCode).json({
      message: message,
      payload: payload,
    });
  };

  export const errorResponse = (
    res,
    statusCode = 200,
    message = 'Internal server error'
  ) => {
    res.status(statusCode).json({
      message: message,
    });
  };
  ```

- GET: /products/:id -> return as single product

  ```js
  export const getProduct = (req, res, next) => {
    try {
      const id = req.params.id;
      const product = products.find((product) => product.id === id);
      if (!product) {
        const error = new Error('Product not found');
        error.status = 404;
        throw error; // Throw the error to be caught by the catch block
      }
      successResponse(res, 200, 'single product is returned', product);
    } catch (error) {
      next(error); // Triggering the error handling middleware
    }
  };
  ```

- add express-validator validation

  - install the package
  - create a validation folder and add the validation check

    ```js
    // validation/productValidator.js
    import { check } from 'express-validator';

    export const productIdValidation = [
      check('id').isNumeric().withMessage('product id must be number'),
    ];
    ```

  - run the validation

    ```js
    import { validationResult } from 'express-validator';

    export const runValidation = (req, res, next) => {
      const errors = validationResult(req);
      if (!errors.isEmpty()) {
        let errorsList = errors.array().map((error) => error.msg);
        return res.status(422).send({
          message: errorsList[0],
        });
      }

      next();
    };
    ```

- DELETE: /products/:id -> delete a product

  ```js
  export const deleteProduct = (req, res, next) => {
    try {
      const id = req.params.id;
      const product = products.find((product) => product.id === id);
      if (!product) {
        const error = new Error('Product not found');
        error.status = 404;
        throw error; // Throw the error to be caught by the catch block
      }

      const filteredProducts = products.filter((p) => p.id !== id);
      products = filteredProducts;

      successResponse(res, 204);
    } catch (error) {
      next(error); // Triggering the error handling middleware
    }
  };
  ```

- POST: /products -> create a product

  - add the validation

    ```js
    import { check } from 'express-validator';

    export const productIdValidation = [
      check('id').isNumeric().withMessage('product id must be number'),
    ];

    export const validateCreateProduct = [
      check('title')
        .trim()
        .notEmpty()
        .withMessage('Product title is required')
        .isLength({ min: 3, max: 200 })
        .withMessage('Product Name should be at least 3-200 characters long'),
      check('price')
        .trim()
        .notEmpty()
        .withMessage('Price is required')
        .isFloat({ min: 1 })
        .withMessage('Price must be a positive number'),
    ];

    export const validateUpdateProduct = [
      check('title')
        .optional()
        .trim()
        .notEmpty()
        .withMessage('Product title is required')
        .isLength({ min: 3, max: 200 })
        .withMessage('Product Name should be at least 3-200 characters long'),
      check('price')
        .optional()
        .trim()
        .notEmpty()
        .withMessage('Price is required')
        .isFloat({ min: 1 })
        .withMessage('Price must be a positive number'),
    ];
    ```

  - create

    ```js
    // POST: /products -> create the product
    export const createProduct = async (req, res, next) => {
      try {
        const newProduct = {
          id: uuidv4(),
          title: req.body.title,
          price: req.body.price,
        };

        products.push(newProduct);
        res.status(201).json({
          message: 'product is created',
        });
      } catch (error) {
        next(error);
      }
    };
    ```

- PUT: /products/:id -> update a product
- Create a service & update the controllers

  ```js
  // services/productService.js
  export const findProductById = (id, products) => {
    const product = products.find((product) => product.id === id);
    if (!product) {
      const error = new Error(`Product not found with this ID: ${id}`);
      error.status = 404;
      throw error;
    }
    return product;
  };

  export const findProductIndexById = (productId, products) => {
    const productIndex = products.findIndex(
      (product) => product.id === productId
    );
    if (productIndex === -1) {
      const error = new Error(`product is not found with this id ${id}`);
      error.status = 404;
      throw error;
    }
    return productIndex;
  };

  // now the controller will look like
  import { v4 as uuidv4 } from 'uuid';

  import {
    findProductById,
    findProductIndexById,
  } from '../services/productService.js';

  let products = [
    { id: '1', title: 'apple iphone 14', price: 570 },
    { id: '2', title: 'samsung', price: 270 },
    { id: '3', title: 'apple iphone 15', price: 270 },
    { id: '4', title: 'samsung 20', price: 370 },
  ];

  // GET: /products -> get All products
  export const getAllProducts = async (req, res, next) => {
    try {
      res.status(200).json({
        message: 'all products are returned',
        payload: products,
      });
    } catch (error) {
      next(error);
    }
  };

  // GET: /products/:id -> get a single product
  export const getSingleProduct = async (req, res, next) => {
    try {
      const { id } = req.params;
      const product = findProductById(id, products);
      res.status(200).json({
        message: 'single product is returned',
        payload: product,
      });
    } catch (error) {
      next(error);
    }
  };

  // DELETE: /products/:id -> delete single product based on id
  export const deleteSingleProduct = async (req, res, next) => {
    try {
      const id = req.params.id;
      findProductById(id, products);

      const filterProducts = products.filter((product) => product.id !== id);
      products = filterProducts;

      // res.status(204).json();
      res.status(200).json({ message: 'Product deleted successfully' });
    } catch (error) {
      next(error);
    }
  };

  // POST: /products -> create the product
  export const createProduct = async (req, res, next) => {
    try {
      const newProduct = {
        id: uuidv4(),
        title: req.body.title,
        price: req.body.price,
      };

      products.push(newProduct);
      res.status(201).json({
        message: 'product is created',
      });
    } catch (error) {
      next(error);
    }
  };

  // PUT: /products/:id -> update the product
  export const updateProduct = async (req, res, next) => {
    try {
      const id = req.params.id;
      const { title, price } = req.body;
      const productIndex = findProductIndexById(id, products);
      if (title) {
        products[productIndex].title = title;
      }
      if (price) {
        products[productIndex].price = price;
      }

      // products[productIndex] = {
      //   ...products[productIndex], // id, title, price
      //   ...req.body // price
      // }

      res.status(200).json({
        message: 'product is updated',
        payload: products[productIndex],
      });
    } catch (error) {
      next(error);
    }
  };
  ```

- Logging: While morgan('dev') is excellent for development, consider using different logging strategies for production. You might want to log more detailed information in development and fewer details in production. Winston or Bunyan are popular logging libraries for more sophisticated logging needs.

  ```js
  // index.js
  // npm install winston
  import winston from 'winston';

  // Set up Winston logger
  const logger = winston.createLogger({
    level: 'info',
    format: winston.format.json(),
    defaultMeta: { service: 'your-service-name' },
    transports: [
      new winston.transports.Console(),
      new winston.transports.File({ filename: 'error.log', level: 'error' }),
      new winston.transports.File({ filename: 'combined.log' }),
    ],
  });

  // Now use it from another file
  logger.log('error', 'error finding the users');
  logger.log('info', 'Successfully returned all the users');
  ```

  ```js
  import { transports, format, createLogger } from 'winston';

  // Set up Winston logger
  export const logger = createLogger({
    level: 'info',
    format: format.combine(
      format.timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }),
      format.json()
    ),
    transports: [
      new transports.File({
        filename: 'logs/product-info.log',
        level: 'info',
      }),
      new transports.File({
        filename: 'logs/product-error.log',
        level: 'error',
      }),
      new transports.Console({
        format: format.combine(format.simple(), format.prettyPrint()),
      }),
    ],
  });
  ```

- how to upload file using multer

  ```js

  ```

#### 4. Front-end

- create a basic react app

  ```js
  import { useEffect, useState } from 'react';
  import axios from 'axios';

  const App = () => {
    const [products, setProducts] = useState([]);
    const getProducts = async () => {
      const { data } = await axios.get('http://localhost:3002/products');
      setProducts(data.payload);
    };
    useEffect(() => {
      getProducts();
    }, []);

    console.log(products);

    return <div>App</div>;
  };

  export default App;
  ```

- create a basic routing

### [2.28 Express TS Product API]

```ts
// index.ts
import express, { Application } from 'express';

import productRouter from './routes/productRoutes';
import { errorHandler } from './middleware/errorHandler';

const app: Application = express();

const port: number = 3002;

app.listen(port, () => {
  console.log(`server is running at http://localhost:${port}`);
});

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.use('/products', productRouter);

app.use(errorHandler);

// GET: routes -> controllers -> services
// GET: /products -> getAllProducts -> findAllProducts

// GET: routes -> controllers -> services
// GET: /products/:id -> getSingleProduct -> findProductById

// DELETE: routes -> controllers -> services
// DELETE: /products/:id -> deleteSingleProduct -> findProductById + deleteProduct

// POST: routes -> controllers -> services
// POST: /products -> createSingleProduct -> createProduct

// PUT: routes -> controllers -> services
// PUT: /products/:id -> updateSingleProduct -> findProductIndexById, updateProduct

// productRoutes.ts
import { Router } from 'express';

import * as productController from '../controllers/productController';

const router = Router();

router.get('/', productController.getAllProducts);

router.post('/', productController.createSingleProduct);

router.get('/:id', productController.getSingleProduct);

router.delete('/:id', productController.deleteSingleProduct);

router.put('/:id', productController.updateSingleProduct);

export default router;


// errorHandler
import { Request, Response, NextFunction } from 'express';

export const errorHandler = (
  error: Error,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  const status = res.statusCode !== 200 ? res.statusCode : 200;
  res.status(status).json({
    message: error.message,
  });
};


// productController.ts
import { NextFunction, Request, Response } from 'express';

import * as productService from '../services/productService';
import { ProductInput } from '../types';

export const getAllProducts = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const products = productService.findAllProducts();
    res.json({
      message: 'return all the products',
      payload: products,
    });
  } catch (error) {
    next(error);
  }
};

export const getSingleProduct = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const id = req.params.id;
    const product = productService.findProductById(id, res);
    res.json({
      message: 'return single product',
      payload: product,
    });
  } catch (error) {
    next(error);
  }
};

export const deleteSingleProduct = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const id = req.params.id;
    productService.findProductById(id, res);
    const products = productService.deleteProduct(id);
    res.json({
      message: 'deleted single product',
      payload: products,
    });
  } catch (error) {
    next(error);
  }
};

export const createSingleProduct = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const newProduct: ProductInput = req.body;
    const products = productService.createProduct(newProduct);
    res.status(201).json({
      message: 'created single product',
      products: products,
    });
  } catch (error) {
    next(error);
  }
};

export const updateSingleProduct = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const id = req.params.id;
    const productIndex = productService.findProductIndexById(id, res);
    const updatedProductData: ProductInput = req.body;
    const product = productService.updateProduct(
      productIndex,
      updatedProductData
    );
    res.json({
      message: 'updated single product',
      payload: product,
    });
  } catch (error) {
    next(error);
  }
};

// productService.ts
import { Response } from 'express';
import { Product, ProductInput } from '../types';

let products: Product[] = [
  { id: '1', title: 'iphone 11', price: 100 },
  { id: '2', title: 'iphone 12', price: 200 },
  { id: '3', title: 'iphone 13', price: 300 },
];

export const findAllProducts = () => {
  return products;
};

export const findProductById = (id: string, res: Response) => {
  const product = products.find((product) => product.id === id);
  if (!product) {
    res.statusCode = 404;
    throw new Error(`product is not found with id ${id}`);
  }
  return product;
};

export const findProductIndexById = (id: string, res: Response) => {
  const productIndex = products.findIndex((product) => product.id === id);
  if (productIndex === -1) {
    res.statusCode = 404;
    throw new Error(`product is not found with id ${id}`);
  }
  return productIndex;
};

export const deleteProduct = (id: string) => {
  products = products.filter((product) => product.id !== id);
  return products;
};

export const createProduct = (newProduct: ProductInput) => {
  const id = new Date().getTime().toString();
  const product = { id, ...newProduct };
  products.push(product);
  return products;
};

export const updateProduct = (
  productIndex: number,
  updatedProductData: ProductInput
) => {
  products[productIndex] = { ...products[productIndex], ...updatedProductData };
  return products[productIndex];
};


```

## 3. MongoDB Tutorial

### 3.1 SQL VS NoSQL

- SQL stands for Structured Query Language - MySQL Database
  - It follows relational database management system
  - store data as a form of table
  - null possibility if we dont add value for column
- NoSQL stands for No Structured Query Language - MongoDB Database
  - It follows documented oriented database management system
  - store data as a form of document by using BSON structure
  - no null possibility if we do not add add value for any field
  - it uses BSON -> Binary JSON
- why BSON instead of JSON?
  - JSON only supports limited number of data types such as String, Boolean, Number, Array (BSON added Date, Raw Binary, Number-Integer, Float, Long, Decimal to JSON)
  - Speed - text based format is very slow to parse (BSON is fast to parse)
  - Storage - JSON takes less compare to BSON
  - Usage - JSON used for transmission data where BSON used for storage of data
  - Encoding technique: JSON use UTF-8 string encoding where BSON use Binary encoding
  - Security: JSON is Human and machine Readable where BSON in only machine Readable

### 3.2 Introduction to MongoDB

- What is MongoDB?

  - Document Oriented Database system
  - NoSQL
  - Some important terms: Database, Table/Collection, Document/Row, Field/Column

  ```js
    // document example
      {
        id : '1',
        name: 'anisul',
        languages: ['bangla', 'english']

      }
  ```

### 3.3 Environment setup

- Download & install MongoDB
- MongoDB Compass
- Check installation

  ```js
  mongod --version (mongodb server version)
  mongo --version (mongodb shell version)
  ```

### 3.4 Basic command

- database

  - show databases: `show dbs`
  - create / switch to a Database: `use databaseName` example: `use productsDB`
  - check the Database you are in : `db + enter`
  - drop database : `db.dropDatabase()`

- collections
  - show collections: `show collections`
  - create collection : `db.createCollection(name, option)`
    example `db.createCollection("products")`
  - drop collection : `db.collectionName.drop()`

### 3.5 CRUD - Create Document

- database is collection of Documents
- Field name regulations collected from MongoDB official website:
  - The field name \_id is reserved
  - Field names cannot contain the null character.
- insert documents command
  - `db.collectionName.instertOne({})`
  - `db.collectionName.instertMany([{},{}])`
  - `db.collectionName.instert([{},{}])`
- Create data- inserting data to user collection example

  ```js
    {
      title: "Fjallraven - Foldsack No. 1 Backpack, Fits 15 Laptops",
      price: 109.95,
     }

  // insertOne()
   db.products.insertOne({
      title: "Fjallraven - Foldsack No. 1 Backpack, Fits 15 Laptops",
      price: 109.95,
     })

  // insertMany()
   db.products.insertMany([
    {
      title: "Fjallraven - Foldsack No. 1 Backpack, Fits 15 Laptops",
      price: 109.95,
     },
     {
      title: "Mens Casual Premium Slim Fit T-Shirts ",
      price: 22.3,
     },
     {
      title: "Mens Cotton Jacket",
      price: 55.99,
     }
     ])

  ```

### 3.6 CRUD - Read Document

- Read / Find data

  - read data syntax: `db.collectionName.find(query, projection)`
  - read data in an easy way syntax: `db.collectionName.find().pretty()`
  - read a specific data syntax: `db.collectionName.find({field: fieldValue})`
    - example1: `db.products.find(ObjectId("633d183e09743587af26fb07"))`
    - example2: `db.users.find({name: "anisul islam"})` remember value is case sentitive
  - limit data syntax: `db.collectionName.find({field: fieldValue}).limit(NumberOfRows)`
    - example: `db.users.find({age: 31}).limit(2)`
  - sorting: 1 for ascending, -1 for descending
    - example: `db.products.find().sort({price: -1})`
  - [less than and greater than](https://www.statology.org/mongodb-greater-than-less-than/#:~:text=You%20can%20use%20the%20following,%24gt%3A%20Greater%20than)
    - example : `db.myCollection.find({field1: {$gt:25}})`
  - projection
    - example : `db.students.find({name: "x"}, {name:0})` get every field without name
    - example : `db.students.find({name: "x"}, {name:1})` get only field
    - example : `db.students.find({name: "x"}, {_id:0, name:1})` get only field even without mongodb \_id

### 3.7 CRUD - Update Document

- Update Data -> $set operator helps to set update value to a field

  - update data syntax: `db.collectionName.update(selection_item, update_data)`
  - update data syntax: `db.collectionName.updateOne(selection_item, update_data)`
  - update data syntax: `db.collectionName.updateMany(selection_item, update_data)`
  - find one and update data syntax: `db.collectionName.findOneAndUpdate(selection_item, update_data)`
  - example: `db.products.update({id:"1"},{$set:{price:32}})`

### 3.8 CRUD - Delete Document

- Delete data

  - delete data syntax: `db.collectionName.deleteOne(selection)
    - example: `db.users.deleteOne({name:"anisul islam"})`
  - delete data syntax: `db.collectionName.deleteOne()
  - delete many data syntax: `db.collectionName.deleteMany({selected_item})
  - delete many data syntax: `db.collectionName.deleteMany({})

### 3.9 CRUD Operations in MongoDB Compass

### 3.10 Node.js and mongodb connection

- 2 ways to connect -> with mongoose package, with mongodb package
- method 1: with mongodb package
  - install mongodb : `npm i mongodb`
- method 2: with mongoose package
  - mongoose in an ODM (Object data modeling) Library for MongoDB and node.js. It provides schema validation. node.js -> mongoose -> mongo driver -> mongoDB

```js
const express = require("express");
const mongoose = require("mongoose");

const port = 3002;
const app = express();

const connectDB = async () => {
  try {
    await mongoose.connect("mongodb://localhost:27017/shop");
    console.log("db is connected");
  } catch (error) {
    console.log(error);
  }
};

app.listen(port, async () => {
  console.log(`server is running at http://localhost:${port}`);
  await connectDB();
});
```

### 3.11 Mongoose schema & model

- define structure of document with validation
- we can define default values of a field
- A model works as a wrapper for schema. It provides an interface for accessign database to create, update, delete, read from database.

```ts
// add the interface
const productSchema = new mongoose.Schema({
  title: {
    type: String,
    reuired: true,
  },
  description: {
    type: String,
    reuired: true,
  },
  price: {
    type: Number,
    reuired: true,
  },
  date: {
    type: Date,
    default: Date.now,
  },
});

const Product = mongoose.model("Products", productSchema);
```

### 3.12 Create document from node.js

```js
// added ts
const addProduct = async () => {
  try {
    const newProduct: IProduct = new Product({
      title: "iPhone 14",
      description: "a nice cute product",
      price: 1300,
    });
    const productData = await newProduct.save();
    // insert many
    // const productData = await Product.insertMany([newProduct1, newProduct2]);
    console.log(productData);
  } catch (error) {
    console.log(error.message);
  }
};
addProduct();
```

### 3.13 Read document from Database in node.js & comparsion query operators

- comparsion query operators: https://www.mongodb.com/docs/manual/reference/operator/query-comparison/
  - $eq, $ne, $gt, $lt, $gte, $lte, $in, $nin

```js
const getProducts = async (req, res) => {
  try {
    const products = await Product.find();
    const products = await Product.find().limit(1);
    const products = await Product.find({ id: req.params.id });
    const products = await Product.find({ isAvailable: true });
    const products = await Product.find({ price: { $eq: 1200 } });
    const products = await Product.find({ price: { $in: [1200, 1300] } });
    const products = await Product.find({ price: { $nin: [1200, 1300] } });
    const products = await Product.find({ price: { $ne: 1200 } });
    const products = await Product.find({ price: { $gt: 1200 } });
    const products = await Product.find({ price: { $lt: 1200 } });
    const products = await Product.find({ price: { $gte: 1200 } });
    const products = await Product.find({ price: { $lte: 1200 } });
    const products = await Product.find({ price: 1200 });

    const products = await Product.find({ price: { $gt: 1200 } }).select({
      title: 1,
    });
    const products = await Product.find({ price: { $gt: 1200 } }).select({
      _id: 0,
      title: 1,
    });

    const products = await Product.find({ price: { $gt: 1200 } }).select({
      _id: 0,
      title: 0,
    });

    console.log(products);
  } catch (error) {
    console.log(error);
  }
};
getProducts();
```

### 3.14 Query Operators: Logical operators

- $and, $or, $not, $nor

```js
// {$and : [{},{}]}
const products = await Product.find({
  $and: [{ price: { $lt: 1400 } }, { rating: { $gt: 4 } }],
});

const products = await Product.find({
  $or: [{ price: { $lt: 1400 } }, { rating: { $gt: 4 } }],
});

// returns all that fail both clauses
const products = await Product.find({
  $nor: [{ price: { $lt: 1400 } }, { rating: { $gt: 4 } }],
});

// $not
const products = await Product.find({ price: { $not: { $lt: 900 } } });
```

### 3.15 counting and sorting

- countDocuments() / count()

```js
const products = await Product.find({ price: { $gt: 900 } }).countDocuments();

// ascending
const products = await Product.find().sort({ price: 1 });

// descending
const products = await Product.find().sort({ price: -1 });

// sort and select
const products = await Product.find()
  .sort({ title: 1 })
  .select({ title: 1, _id: 0 });
console.log(products);
```

### 3.16 update data

- update data syntax: `db.collectionName.updateOne(selection_item, update_data)`
- update data syntax: `db.collectionName.findByIdAndUpdate(selection_item, update_data, {new: true})`

```js
// updateOne
const products = await Product.updateOne({ _id }, { $set: { rating: 4.8 } });
console.log(products);
updateProduct("63432689c564aea397b3d210");

// findByIdAndUpdate it returns old data
const products = await Product.findByIdAndUpdate(
  { _id },
  { $set: { rating: 4.8 } }
);
console.log(products);

// findByIdAndUpdate it returns updated data
const products = await Product.findByIdAndUpdate(
  { _id },
  { $set: { rating: 4.7 } },
  { new: true }
);
console.log(products);
```

### 3.17 delete data

- example: `db.users.deleteOne({name:"anisul islam"})`

```js
const products = await Product.deleteOne({ _id });
console.log(products);

// findByIdAndDelete return deleted data
const products = await Product.findByIdAndDelete({ _id });
console.log(products);
```

### 3.18 mongoose validations

- [official documentation](https://mongoosejs.com/docs/validation.html)
- A common gotcha for beginners is that the unique option for schemas is not a validator.
- Numbers have: min, max validators
- Strings have: minlength, maxlength, trim, lowercase, enum
- validator error message can be provided using array syntax and object syntax

```js
Array syntax: min: [6, 'Must be at least 6, got {VALUE}']
Object syntax: enum: { values: ['Coffee', 'Tea'], message: '{VALUE} is not supported' }
```

```js
// validation when creating schema
/*
  title: {
    type: String,
    required: [true, "product title is required"],

    minlength: 3, 
    minlength: [3, "error message here"],    
    maxlength: 3, 
   
    lowercase: true,
    uppercase: true,

    trim: true // "     iphone 7      ",
    enum: ["iphone", "samsung", "motorola"] // no other value is allowed other than these,
    enum: {
      values: ['iphone', 'samsung', motorola"],
      message: '{VALUE} is not supported'
    }
  },
  price:{
     type: String,
     required: true,
     min: 20,
     max: 30
  }

*/
```

- mongoose ID Validation

  ```ts
    import mongoose from "mongoose";

    catch (error) {
        if (error instanceof mongoose.Error.CastError) {
          // Handle CastError here
          const castError = createHttpError(400, 'Invalid product ID format');
          next(castError);
        } else {
          // Handle other errors
          next(error);
        }
      }
  ```

### 3.19 Custom validations

- for fulfilling own requirements based on certain situation we need to create custom validations.
- read about email vliadation
- [create your own validation regular expression](https://regexr.com/3e48o)

```js
price:{
    type: String,
    required: [true, "title is required"],
    validate: {
      validator: function (v) {
        return v.length === 10;
      },
      message: (props) => `${props.value} is not a valid product title!`,
    },
},
phone: {
    type: String,
    validate: {
      validator: function(v) {
        return /\d{3}-\d{3}-\d{4}/.test(v);
      },
      message: props => `${props.value} is not a valid phone number!`
    },
    required: [true, 'User phone number required']
  }
  email:{
    // ^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$
    type: String,
    required: [true, 'User email is required'],
    trim: true,
    lowercase: true,
    unique: true,
    validate: {
      validator: function(v) {
        const emailRegex = /^([\w-\.]+@([\w-]+\.)+[\w-]{2,4})?$/;
        return emailRegex.test(v);
      },
      message: props => `${props.value} is not a valid phone number!`
    },
     email: {
        type: String,
        trim: true,
        lowercase: true,
        unique: true,
        required: 'Email address is required',
        validate: [validateEmail, 'Please fill a valid email address'],
        match: [/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/, 'Please fill a valid email address']
    }

  }
```

### 3.20 npm validator

- `npm i validator`

```js
email: {
    type: String,
    unique: true,
    required: [true, "email is required"],
    trim: true,
    lowercase: true,
    validate: {
      validator: validator.isEmail,
      message: (props) => `${props.value} is not a valid email!`,
    },
  },
```

### 3.21 pagination

```js
const { page = 1, limit = 10 } = req.query;
const products = await Product.find()
  .limit(limit)
  .skip((page - 1) * limit);
console.log(products);
```

### 3.22 export and import data in mongodb atlas

```js
For json
mongoimport –uri
mongoimport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop sales.json

mongoexport –uri
mongoexport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --collection=sales --out=sales.json

Opem the command line and paste it
mongoexport --uri="mongodb+srv://oss:integrify1234@oss.6kg37.mongodb.net/test" --collection=datas --out=datas.json

for bson
mongostore – uri
mongorestore --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop dump
mongodump –uri
mongodump --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"
```

## 4. API Documentaion with Swagger

- install packages `npm init -y && npm install express lowdb morgan uuid cors swagger-jsdoc swagger-ui-express`
- we can create api doc anywhere - routes/controllers

- lets create the server

```js
const express = require("express");
const cors = require("cors");
const morgan = require("morgan");
const bodyParser = require("body-parser");
const swaggerUi = require("swagger-ui-express");
const swaggerJsdoc = require("swagger-jsdoc");

const { connectDB } = require("./config/db");
const productRouter = require("./routes/products");

const port = process.env.port || 3002;
const app = express();

app.listen(port, async () => {
  console.log(`server is running at http://localhost:${port}`);
  await connectDB();
});

// swagger
const options = {
  definition: {
    openapi: "3.0.0",
    info: {
      title: "Product API",
      version: "1.0.0",
      description: "A REST Product API",
    },
    servers: [
      {
        url: "http://localhost:3002",
      },
    ],
  },
  apis: ["./controllers/*.js"],
};

const openapiSpecification = swaggerJsdoc(options);
app.use("/api-docs", swaggerUi.serve, swaggerUi.setup(openapiSpecification));

app.use(cors());
app.use(morgan("dev"));
// for parsing application/xwww-form-urlencoded
app.use(bodyParser.urlencoded({ extended: true }));
// for parsing application/json
app.use(bodyParser.json());

app.use("/products", productRouter);

app.get("/test", (req, res) => {
  res.status(200).send("testing routes working fine");
});
```

- lets create the product routes

```js
const router = require("express").Router();

const {
  getProducts,
  getProduct,
  addProduct,
  deleteProduct,
  updateProduct,
} = require("../controllers/products");

router.get("/", getProducts);
router.get("/:id", getProduct);
router.post("/", addProduct);
router.put("/:id", updateProduct);
router.delete("/:id", deleteProduct);

module.exports = router;
```

- lets do the configuration for database and others

```js
// config/index.js
require("dotenv").config();

exports.dev = {
  db: {
    url: process.env.DB_URL || "",
  },
  app: {
    port: process.env.SERVER_PORT,
  },
};
```

```js
// config/db.js
const mongoose = require("mongoose");
const { dev } = require(".");

// connect mongodb database
exports.connectDB = async () => {
  try {
    await mongoose.connect(dev.db.url);
    console.log("db is connected");
  } catch (error) {
    console.log("db is not connected");
    console.log(error);
    process.exit(1);
  }
};
```

- lets create the model

```js
// models/products.js
const mongoose = require("mongoose");

const productsSchema = new mongoose.Schema({
  id: {
    type: String,
    required: true,
  },
  title: {
    type: String,
    required: [true, "product title is required"],
    trim: true,
    minlength: [3, "product title length must be atleast 3 characters"],
  },
  price: {
    type: Number,
    required: [true, "product price is required"],
  },
  rating: {
    type: Number,
    required: [true, "product rating is required"],
  },
});

exports.Product = mongoose.model("Products", productsSchema);
```

- lets create the API Documentation

```js
// controllers/product.js
const { v4: uuidv4 } = require("uuid");

const { Product } = require("../models/products");

// create a schema for the model
/**
 *@swagger
 *components:
 *  schemas:
 *    Product:
 *      type: object
 *      required:
 *        - title
 *        - price
 *        - rating
 *      properties:
 *        id:
 *          type: string
 *          description: auto generated id of the product
 *        title:
 *          type: string
 *          description: title of the product
 *        price:
 *          type: number
 *          description: price of the product
 *        rating:
 *          type: number
 *          description: rating of the product
 *      example:
 *          id: ahiahuhauhuahuahuha
 *          title: iphone14
 *          price: 1450.55
 *          rating: 4.5
 */

// grouping requests into tags
/**
 * @swagger
 * tags:
 *   name: Products
 *   description: E-Commerce app
 */

/**
 * @swagger
 * /products:
 *  get:
 *    summary: Returns all the products
 *    tags: [Products]
 *    responses:
 *      200:
 *        description: all the products
 *        content:
 *          application/json:
 *            schema:
 *              type: array
 *              items:
 *                $ref: '#/components/schemas/Product'
 */

const getProducts = async (req, res) => {
  try {
    const products = await Product.find();
    res.status(200).send(products);
  } catch (error) {
    res.status(500).send(error.message);
  }
};

/**
 * @swagger
 * /products/{id}:
 *  get:
 *    summary: get the product with id
 *    tags: [Products]
 *    parameters:
 *      - in : path
 *        name : id
 *        schema:
 *          type: string
 *        required: true
 *        description: product id
 *    responses:
 *      200:
 *        description: the product with id
 *        content:
 *          application/json:
 *            schema:
 *                $ref: '#/components/schemas/Product'
 *      404:
 *        description: the product with id was not found
 */
const getProduct = async (req, res) => {
  try {
    const product = await Product.findOne({ id: req.params.id });
    if (!product) {
      res.status(404).send({
        message: `product is not found with the id ${req.params.id}`,
      });
    } else {
      res.status(200).send(product);
    }
  } catch (error) {
    res.status(500).send(error.message);
  }
};

/**
 * @swagger
 * /products:
 *  post:
 *    summary: create a new product
 *    tags: [Products]
 *    requestBody:
 *        required: true
 *        content:
 *          application/json:
 *            schema:
 *                $ref: '#/components/schemas/Product'
 *    responses:
 *      201:
 *        description: the product was created
 *        content:
 *          application/json:
 *            schema:
 *                $ref: '#/components/schemas/Product'
 *      404:
 *        description: the product with id was not found
 *      500:
 *        description: server error
 */

const addProduct = async (req, res) => {
  try {
    const newProduct = new Product({
      id: uuidv4(),
      title: req.body.title,
      price: parseInt(req.body.price),
      rating: parseInt(req.body.rating),
    });
    const productData = await newProduct.save();

    if (productData) {
      res.status(201).send(productData);
    } else {
      res.status(404).send({ message: "product data is not saved" });
    }
  } catch (error) {
    res.status(500).send(error.message);
  }
};

/**
 * @swagger
 * /products/{id}:
 *  put:
 *    summary: update the product with id
 *    tags: [Products]
 *    parameters:
 *      - in : path
 *        name : id
 *        schema:
 *          type: string
 *        required: true
 *        description: product id
 *    requestBody:
 *        required: true
 *        content:
 *          application/json:
 *            schema:
 *                $ref: '#/components/schemas/Product'
 *    responses:
 *      200:
 *        description: the product was updated
 *      404:
 *        description: the product with id was not found
 *      500:
 *        description: server error
 */
const updateProduct = async (req, res) => {
  try {
    const product = await Product.find({ id: req.params.id });
    if (product) {
      await Product.updateOne(
        { id: req.params.id },
        {
          $set: {
            title: req.body.title,
            price: Number(req.body.price),
            rating: Number(req.body.rating),
          },
        }
      );
      res.status(200).send({
        message: `product is updated successfully`,
      });
    } else {
      res.status(404).send({
        message: `product is not found with the id ${req.params.id}`,
      });
    }
  } catch (error) {
    res.status(500).send(error.message);
  }
};

/**
 * @swagger
 * /products/{id}:
 *  delete:
 *    summary: delete the product with id
 *    tags: [Products]
 *    parameters:
 *      - in : path
 *        name : id
 *        schema:
 *          type: string
 *        required: true
 *        description: product id
 *    responses:
 *      200:
 *        description: the product was deleted
 *      404:
 *        description: the product with id was not found
 */
const deleteProduct = async (req, res) => {
  try {
    const product = await Product.find({ id: req.params.id });
    if (product) {
      await Product.deleteOne({ id: req.params.id });
      res.status(200).send({
        message: `product is deleted successfully`,
      });
    } else {
      res.status(404).send({
        message: `product is not found with the id ${req.params.id}`,
      });
    }
  } catch (error) {
    res.status(500).send(error.message);
  }
};

module.exports = {
  getProducts,
  getProduct,
  addProduct,
  updateProduct,
  deleteProduct,
};
```

## 5. Authentication Tutorial

- [Authorization (403) vs Authentication (401)](http://www.differencebetween.net/technology/difference-between-authentication-and-authorization/)
- [Authorization (403) vs Authentication (401)](https://stackoverflow.com/questions/3297048/403-forbidden-vs-401-unauthorized-http-responses)

### Level 1: Database matching

- save(), findOne({property: value})
- if hacker can access our database then our data is too much human readable
- [password checker online](http://password-checker.online-domain-tools.com/)

```js
// index.js
const express = require("express");
const morgan = require("morgan");
const { dev } = require("./config");
const { connectDB } = require("./config/db");
const userRoute = require("./routes/users");

const app = express();
const port = dev.app.port || 3002;

app.listen(port, async () => {
  console.log(`server is running at http://localhost:${port}`);
  connectDB();
});

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(morgan("dev"));
app.use("/api/v1", userRoute);

app.get("/test", (req, res) => {
  res.send("testinng route");
});
```

```js
// config/index.js
require("dotenv").config();

exports.dev = {
  db: {
    url: process.env.MONGO_URL || "mongodb://localhost:27017",
  },
  app: {
    port: process.env.SERVER_PORT,
  },
};
```

```js
// config/db.js
const mongoose = require("mongoose");
const { dev } = require(".");

exports.connectDB = async () => {
  try {
    await mongoose.connect(dev.db.url);
    console.log("db is connected");
  } catch (error) {
    console.log("db is not connected");
    console.log(error);
  }
};
```

```js
// routes/users.js
const { registerUser, loginUser } = require("../controllers/users");

const router = require("express").Router();

router.post("/register", registerUser);

router.post("/login", loginUser);

module.exports = router;

// models/user.js
const { model, Schema } = require("mongoose");

const userSchema = new Schema({
  email: {
    type: String,
    required: [true, "user email is reuired"],
    trim: true,
    lowercase: true,
    unique: true,
    validate: {
      validator: function (v) {
        const emailRegex = /^([\w-\.]+@([\w-]+\.)+[\w-]{2,4})?$/;
        return emailRegex.test(v);
      },
      message: (props) => `${props.value} is not a valid email address!`,
    },
  },
  password: {
    type: String,
    required: [true, "user password is reuired"],
    minlength: [3, "minimum length for password is 3"],
  },
});

exports.User = model("Users", userSchema);
```

```js
// controllers/users.js
const { User } = require("../models/user");

const registerUser = async (req, res) => {
  try {
    // check user already exist or not
    const user = await User.findOne({ email: req.body.email });
    if (user) return res.status(400).send("User already exists");

    const newUser = new User({
      email: req.body.email,
      password: req.body.password,
    });
    const userData = await newUser.save();

    if (userData) {
      res.status(201).send({ message: "registration successful" });
    } else {
      res.status(404).send({ message: "registration not successful" });
    }
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};

const loginUser = async (req, res) => {
  try {
    const { email, password } = req.body;
    const user = await User.findOne({ email: email });
    if (user) {
      if (user.password === password) {
        res.status(200).json({ status: "valid user" });
      } else {
        res.status(401).json({ status: "401 Unauthorized, Not valid user" });
      }
    } else {
      res.status(404).json({ status: "User not found" });
    }
  } catch (error) {
    res.status(500).json(error.message);
  }
};
module.exports = { registerUser, loginUser };
```

### Level 2: Hashing + salting password

- we can hash the password with some random number(salting)
- install bcrypt npm package `npm install bcrypt`

```js
// controllers/users.js
const { User } = require("../models/user");

const bcrypt = require("bcrypt");
const saltRounds = 10;

const securePassword = async (password) => {
  try {
    return await bcrypt.hash(password, saltRounds);
  } catch (error) {
    console.log(error);
  }
};

const comparePassword = async (password, hashedPassword) => {
  try {
    return await bcrypt.compare(password, hashedPassword);
  } catch (error) {
    console.log(error);
  }
};

const registerUser = async (req, res) => {
  try {
    // check user already exist or not
    const user = await User.findOne({ email: req.body.email });
    if (user) return res.status(400).send("User already exists");

    const hashPassword = await securePassword(req.body.password);
    const newUser = new User({
      email: req.body.email,
      password: hashPassword,
    });
    const userData = await newUser.save();

    if (userData) {
      res.status(201).send({ message: "registration successful" });
    } else {
      res.status(404).send({ message: "registration not successful" });
    }
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};

const loginUser = async (req, res) => {
  try {
    const { email, password } = req.body;

    const user = await User.findOne({ email: email });
    if (user) {
      const isValid = await comparePassword(password, user.password);
      if (isValid) {
        res.status(200).json({ status: "valid user" });
      } else {
        res.status(401).json({ status: "401 Unauthorized, Not valid user" });
      }
    } else {
      res.status(404).json({ status: "User not found" });
    }
  } catch (error) {
    res.status(500).json(error.message);
  }
};

module.exports = { registerUser, loginUser };
```

### Level 3: Session based authentication

### Level 4: Token based authentication: MERN Project

- backend api planning

```js
// API Planning
// 1. Register an user
/*
POST: http://localhost:3030/api/users/register
{
  "name" : "xyz", // minimum 2 characters, required
  "email" : "xyz@gmail.com", // isEmail or not, required
  "password" : "123456", // minimum 6 characters long, required
  "phone" : "04067548948", // optional
}
*/
// 2. Login an user
/*
POST: http://localhost:3030/api/users/login
{
  "email" : "xyz@gmail.com", // isEmail or not, required
  "password" : "123456", // minimum 6 characters long, required
}
// 3. User Profile
/*
POST: http://localhost:3030/api/users/profile
{
  "_id" : "ag7dggha8ahag7ag7ag", 
  "name" : "xyz", 
  "email" : "xyz@gmail.com", 
  "phone" : "04067548948", 
}
*/
```

#### step1: create the server

- `npm init -y && npm install express nodemon`

```js
const express = require("express");

const app = express();

const port = 3031;

app.listen(port, () => {
  console.log(`app is running at http://localhost:${port}`);
});
```

- add the script in package.json: `"start":"nodemon index.js"`
- run the app: `npm start`

#### step2: format the console with chalk package

- `npm install -D chalk@^4.1.2`

```js
const express = require("express");
const chalk = require("chalk");

const app = express();

const port = 3030;

app.listen(port, () => {
  console.log(chalk.blue(`app is running at http://localhost:${port}`));
});
```

#### step3: Prettier formatter and ESLint Linter setup (check my github for setup)

- [prettier and eslint setup](https://github.com/anisul-Islam/prettier-eslint-setup)

#### step4: config .env variables

- npm install dotenv

```js
// .env
SERVER_PORT = 3030;

// config/index.js
require("dotenv").config();
const dev = {
  app: {
    serverPort: process.env.SERVER_PORT,
  },
};
module.exports = dev;
```

- use the port from index.js file

#### step5: Adding gitignore and README.md file

#### step6: health route, morgan and cors setup

- add a health checking route

```js
app.get("/test", (req, res) => {
  return res.send("testing API");
});
```

- install morgan http request logger and cors

```js
const morgan = require("morgan");
const cors = require("cors");
app.use(morgan("dev"));
app.use(
  cors({
    credentials: true,
    origin: "http://localhost:3000",
  })
);
```

#### step7: Error handling middleware setup

```js
// error handing middleware
app.use((req, res, next) => {
  return res.status(404).send({
    success: false,
    message: "Route not found",
  });
});

// error handing middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  return res.status(200).send({
    success: false,
    message: err.message,
  });
});
```

#### step8: User Route setup

- create routes as planned: /api/users/register, /api/users/login, /api/users/profile

```js
app.post("/api/users/register", (req, res) => {
  try {
    return res.status(201).send({
      success: true,
      message: "user was registered",
    });
  } catch (error) {
    return res.status(500).send({
      success: false,
      message: error.message,
    });
  }
});
app.post("/api/users/login", (req, res) => {
  try {
    return res.status(200).send({
      success: true,
      message: "user was logged In",
    });
  } catch (error) {
    return res.status(500).send({
      success: false,
      message: error.message,
    });
  }
});
app.get("/api/users/profile", (req, res) => {
  try {
    return res.status(200).send({
      success: true,
      message: "user profile was returned",
    });
  } catch (error) {
    return res.status(500).send({
      success: false,
      message: error.message,
    });
  }
});
```

#### step9: MVC Architecture

- create controllers, routes, models folder

```js
// index.js
app.use("/api/users", userRoutes);

// users.js route
const express = require("express");

const {
  registerUser,
  loginUser,
  getUserProfile,
} = require("../controllers/user");

const userRoutes = express.Router();

userRoutes.post("/register", registerUser);

userRoutes.post("/login", loginUser);

userRoutes.get("/profile", getUserProfile);

module.exports = userRoutes;

// user.js controllers
const registerUser = (req, res) => {
  try {
    return res.status(201).send({
      success: true,
      message: "user was registered",
    });
  } catch (error) {
    return res.status(500).send({
      success: false,
      message: error.message,
    });
  }
};

const loginUser = (req, res) => {
  try {
    return res.status(200).send({
      success: true,
      message: "user was logged In",
    });
  } catch (error) {
    return res.status(500).send({
      success: false,
      message: error.message,
    });
  }
};

const getUserProfile = (req, res) => {
  try {
    return res.status(200).send({
      success: true,
      message: "user profile was returned",
    });
  } catch (error) {
    return res.status(500).send({
      success: false,
      message: error.message,
    });
  }
};

module.exports = { registerUser, loginUser, getUserProfile };
```

#### step10: Adding helper function inside helper folder

```js
// helper/responseHelper.js
exports.errorResponse = (res, statusCode, message) => {
  return res.status(statusCode).send({
    success: false,
    message: message,
  });
};

exports.successResponse = (res, statusCode, message, data) => {
  return res.status(statusCode).send({
    success: true,
    message: message,
    data: data,
  });
};

// user.js controller
const { successResponse, errorResponse } = require("../helper/responseHelper");

const registerUser = (req, res) => {
  try {
    return successResponse(res, 201, "user was registered");
  } catch (error) {
    return errorResponse(res, 500, error.message);
  }
};

const loginUser = (req, res) => {
  try {
    return successResponse(res, 200, "user was logged In");
  } catch (error) {
    return errorResponse(res, 500, error.message);
  }
};

const getUserProfile = (req, res) => {
  try {
    return successResponse(res, 200, "user profile was returned");
  } catch (error) {
    return errorResponse(res, 500, error.message);
  }
};

module.exports = { registerUser, loginUser, getUserProfile };
```

#### step11: Adding wild card for universal routes

```js
// remember it has to be always in the end
postRoute.use("*", handleRouteNotFound);

// inside controllers/user.js
const handleRouteNotFound = async (req, res) => {
  return errorResponse(res, 404, "route not found");
};
```

#### step12: Getting user register data in controller

```js
// add middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// user.js controller inside registerUser handler
const registerUser = (req, res) => {
  try {
    const { name, email, password, phone } = req.body;

    if (!name || !email || !password || !phone)
      return errorResponse(
        res,
        400,
        "name, email, password, phone is required"
      );

    if (password.length < 6)
      return errorResponse(res, 400, "Minimum Password length is 6 characters");

    return successResponse(res, 201, "user was registered", {
      name,
      email,
      password,
      phone,
    });
  } catch (error) {
    return errorResponse(res, 500, error.message);
  }
};
```

#### step13: validate user register data in controller

- `npm install express-validator`

```js
// validator/user.js
const { check, validationResult } = require("express-validator");

exports.registerUserValidator = [
  check("name")
    .notEmpty()
    .withMessage("Name is missing")
    .isLength({ min: 3 })
    .withMessage("not a valid Name"),
  check("email")
    .notEmpty()
    .withMessage("Email is missing")
    .normalizeEmail()
    .isEmail()
    .withMessage("not a valid email"),
  check("password")
    .notEmpty()
    .withMessage("Password is missing")
    .isLength({ min: 6 })
    .withMessage("minimum 6 characters for password"),

  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      const validationErrors = {};
      const allErrors = errors.array();
      allErrors.forEach((error) => {
        validationErrors[error.param] = error.msg;
      });

      return res.status(400).json({
        validationErrors,
      });
    }
    return next();
  },
];
exports.loginUserValidator = [
  check("email")
    .notEmpty()
    .withMessage("Email is missing")
    .normalizeEmail()
    .isEmail()
    .withMessage("not a valid email"),
  check("password")
    .notEmpty()
    .withMessage("Password is missing")
    .isLength({ min: 6 })
    .withMessage("minimum 6 characters for password"),

  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      const validationErrors = {};
      const allErrors = errors.array();
      allErrors.forEach((error) => {
        validationErrors[error.param] = error.msg;
      });

      return res.status(400).json({
        validationErrors,
      });
    }
    return next();
  },
];
```

- create an validation/auth.js

```js
const { check } = require("express-validator");

exports.userSignupValidator = [
  check("name").not().isEmpty().withMessage("Name is required"),
  check("email")
    .not()
    .isEmpty()
    .withMessage("Email is required")
    .normalizeEmail()
    .isEmail()
    .withMessage("not a valid email"),
  check("password")
    .isLength({ min: 6 })
    .withMessage("Password must be at least 6 characters"),
];
```

- create an validation/index.js

```js
const { validationResult } = require("express-validator");

exports.runValidation = (req, res, next) => {
  // const errors = validationResult(req);
  // console.log(errors);
  // if (!errors.isEmpty()) {
  //   const validationErrors = {};
  //   const allErrors = errors.array();
  //   allErrors.forEach((error) => {
  //     validationErrors[error.param] = error.msg;
  //   });

  //   return res.status(400).json({
  //     validationErrors,
  //   });
  // }
  // return next();

  // method 2
  // const errors = validationResult(req);
  // if (!errors.isEmpty()) {
  //   return res.status(422).json({
  //     error: errors.array()[0].msg,
  //   });
  // }
  // return next();

  // method3
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    let errorsList = errors.array().map((error) => error.msg);
    return res.status(422).json({
      error: errorsList,
    });
  }
  return next();
};
```

#### step14: hash the password with bcrypt

- `npm install bcrypt`

```js
const bcrypt = require("bcrypt");
// for creating hash password
const hashedPassword = await bcrypt.hash(password, 10);

// for comparing the password
const isPasswordMatched = await bcrypt.compare(password, hashPassword);
```

#### step15: connect mongodb atlas database

- `npm install mongoose`

```js
// add the mongodb atlas url in .env file
// config/index.js
require("dotenv").config();
const dev = {
  app: {
    serverPort: process.env.SERVER_PORT,
  },
  db: {
    url: process.env.MONGO_URL || "mongodb://127.0.0.1:27017/jwtDB",
  },
};
module.exports = dev;

// config/db.js
const mongoose = require("mongoose");

const dev = require(".");
exports.connectDB = async () => {
  try {
    await mongoose.connect(dev.db.url);
    console.log("db is connected");
  } catch (error) {
    console.log("db is not connected");
    console.log(error);
    process.exit(1);
  }
};
```

#### step16: create schema and model

```js
const { Schema, model } = require("mongoose");

const userSchema = new Schema({
  name: {
    type: String,
    required: [true, "user name is required"],
    trim: true,
  },
  email: {
    type: String,
    required: [true, "user email is required"],
    trim: true,
    unique: true,
  },
  password: {
    type: String,
    required: [true, "user password is required"],
  },
  phone: {
    type: String,
  },
});

const User = model("Users", userSchema);
module.exports = User;
```

#### step17: save the user in database

```js
// registerUser  controller
const registerUser = async (req, res) => {
  try {
    const { name, email, password, phone } = req.body;

    // check input are missing or not
    if (!name || !email || !password)
      return errorResponse(res, 400, "name, email, password is required");

    // check password length >= 6
    if (password.length < 6)
      return errorResponse(res, 400, "Minimum Password length is 6 characters");

    // check user already exist or not with email
    const exsitingUser = await User.findOne({ email });
    if (exsitingUser) {
      return errorResponse(
        res,
        400,
        "user already exists with this email. Login Please."
      );
    }

    // hashing user plain password
    const hashedPassword = await bcrypt.hash(password, 10);

    // create the user
    const newUser = new User({
      name,
      email,
      password: hashedPassword,
      phone,
    });

    await newUser.save();
    return successResponse(res, 201, "user was registered");
  } catch (error) {
    return errorResponse(res, 500, error.message);
  }
};
```

#### step18: userLogin implementation

```js
const loginUser = async (req, res) => {
  try {
    // step 1: get the data from request body
    const { email, password } = req.body;

    // step 2 and 3 are optional if you are already using 3rd party validator
    // step 2: check input are missing or not
    if (!email || !password)
      return res.status(400).json({
        message: "email, password is required",
      });

    // step 3: check password length >= 6
    if (password.length < 6)
      return res.status(400).json({
        message: "Minimum Password length is 6 characters",
      });

    // step 4: check user exist or not with email
    const exsitingUser = await User.findOne({ email });
    if (!exsitingUser) {
      return res.status(400).json({
        message: "No User exist with this eamil. Please register/signup first",
      });
    }

    // step 5: compare password
    const isPasswordMatched = await bcrypt.compare(
      password,
      exsitingUser.password
    );
    // if password is not matched
    if (!isPasswordMatched) {
      return res.status(400).json({
        message: "Invalid Email/Password.",
      });
    }

    // step 6: generate a token with expiray time and sent it to front end
    const token = jwt.sign(
      { _id: exsitingUser._id },
      String(dev.app.jwtSecretKey),
      {
        expiresIn: "7d",
      }
    );

    // step 7: create user info
    const userInfo = {
      _id: exsitingUser._id,
      name: exsitingUser.name,
      email: exsitingUser.email,
      phone: exsitingUser.phone,
      isAdmin: exsitingUser.isAdmin,
    };

    return res.status(200).json({
      message: "user was signed in",
      user: userInfo,
      token,
    });
  } catch (error) {
    return res.json({
      message: error.message,
    });
  }
};
```

#### step19: How to generate JWT Token

- [jwt doc](https://github.com/auth0/node-jsonwebtoken)
- `npm install jsonwebtoken`

```js
const jwt = require("jsonwebtoken");
// create a secret key
// jwt.sign(payload, secretOrPrivateKey, [options, callback])

// generate JWT Token
const token = jwt.sign({ id: exsitingUser._id }, String(dev.app.jwtSecretKey), {
  // algorithm: 'HS512',
  expiresIn: "40s",
});
```

#### step19: How to store the tooken in cookie

- `npm install cookie-parser`

```js
const cookieParser = require("cookie-parser");
app.use(cookieParser());

// if any cookie exisit then delete
if (req.cookies[`${exsitingUser._id}`]) {
  req.cookies[`${exsitingUser._id}`] = "";
}

// set the token inisde httpOnly Cookie
res.cookie(String(exsitingUser._id), token, {
  path: "/",
  expires: new Date(Date.now() + 1000 * 30),
  httpOnly: true,
  sameSite: "lax",
});
```

#### step20: How to verify the tooken from cookie

```js
// isAuthorized middleware
// cookie exist or not
if (!req.headers.cookie) {
  return res.status(400).json({
    message: "No cookie found",
  });
}

// how to get the cookie from request header
const token = req.headers.cookie.split("=")[1];

// verify the cookie
if (!token) {
  return res.status(400).json({
    message: "No token found",
  });
}

jwt.verify(String(token), String(dev.app.jwtSecretKey), (err, user) => {
  if (err) {
    return res.status(403).json({
      message: "Invalid token",
    });
  }
  req.id = user.id;
  next();
});
```

#### step21: return the user after verification

- `userRoutes.get('/profile', isAuthorized, getUserProfile)`

```js
const getUserProfile = async (req, res) => {
  try {
    const user = await User.findById({ _id: req.id }, "-password");
    if (!user) {
      return res.status(400).json({
        message: "user does not exists with this id.",
      });
    }
    return successResponse(res, 200, "user profile was returned", user);
  } catch (error) {
    return errorResponse(res, 500, error.message);
  }
};
```

#### step22: logout user by resetting the cookie

-`router.post("/logout", isAuthorized, logoutUser);`

```js
const logoutUser = async (req, res) => {
  try {
    if (!req.headers.cookie) {
      return res.status(400).json({
        message: "No cookie found",
      });
    }
    const token = req.headers.cookie.split("=")[1];

    if (!token) {
      return res.status(400).json({
        message: "No token found",
      });
    }

    jwt.verify(String(token), String(dev.app.jwtSecretKey), (err, user) => {
      if (err) {
        return res.status(403).json({
          message: "Invalid token",
        });
      }
      req.cookies[`${user.id}`] = "";
      res.clearCookie(`${user.id}`);
    });

    res.send("you are successfully logout");
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

#### step23: refresh token

- `userRoutes.get('/refresh', getRefreshToken, isAuthorized, getUserProfile)`

```js
const getRefreshToken = async (req, res, next) => {
  try {
    if (!req.headers.cookie) {
      return res.status(400).json({
        message: "No cookie found",
      });
    }
    const existingToken = req.headers.cookie.split("=")[1];

    if (!existingToken) {
      return res.status(400).json({
        message: "No token found",
      });
    }

    jwt.verify(existingToken, String(dev.app.jwtSecretKey), (err, user) => {
      if (err) {
        console.log(err);
        return res.status(403).json({
          message: "Auth failed",
        });
      }

      req.cookies[`${user.id}`] = "";
      res.clearCookie(`${user.id}`);

      const newToken = jwt.sign({ id: user.id }, String(dev.app.jwtSecretKey), {
        // algorithm: "HS512",
        expiresIn: "30s",
      });

      console.log("ReGenerated token\n", newToken);

      // sending token inside httpOnly cookie
      res.cookie(String(user.id), newToken, {
        path: "/",
        expires: new Date(Date.now() + 1000 * 28),
        httpOnly: true,
        sameSite: "lax",
      });

      req.id = user.id;
      next();
    });
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

#### step24: Front end Part

- create react app - `npx create-react-app .`
- create a folder structure: pages, components, routes, layouts, app (store.js),
- create some pages inside pages folder -> Blogs, Profile, Register, Login, Error, index.js

```js
// import all the pages from one place
export { Error } from "./Error";
export { Blogs } from "./Blogs";
export { Register } from "./Register";
export { Login } from "./Login";
export { Profile } from "./Profile";
export { Contact } from "./Contact";
```

- setup react routing -> routes/index.js

```js
`npm install react-router-dom`
// routes/index.js

import React from "react";
import { BrowserRouter, Route, Routes } from "react-router-dom";
import { Blogs, Contact, Error, Login, Profile, Register } from "../pages";

const Index = () => {
  return (
    <BrowserRouter>
      <main>
        <Routes>
          <Route path="/" element={<Blogs />} />
          <Route path="/register" element={<Register />} />
          <Route path="/login" element={<Login />} />
          <Route path="/contact" element={<Contact />} />
          <Route path="/Profile" element={<Profile />} />
          <Route path="*" element={<Error />} />
        </Routes>
      </main>
    </BrowserRouter>
  );
};

export default Index;

// App.js
import React from "react";

import "./App.css";
import Index from "./routes";

function App() {
  return <Index />;
}

export default App;
```

- Add layouts inside layouts folder

```js
// Navbar.js
import React from "react";
import { Link } from "react-router-dom";

const Navbar = () => {
  return (
    <nav className="center">
      <Link to="/" className="nav__link">
        Blogs
      </Link>
      <Link to="/register" className="nav__link">
        Register
      </Link>
      <Link to="/login" className="nav__link">
        Login
      </Link>
      <Link to="/logout" className="nav__link">
        Logout
      </Link>
       <Link to="/profile" className="nav__link">
        Profile
      </Link>
      <Link to="/contact" className="nav__link">
        Contact
      </Link>
    </nav>
  );
};

export default Navbar;

// Footer.js
import React from "react";

const Footer = () => {
  return (
    <footer className="center">
      <p>copyright by anisul islam</p>
    </footer>
  );
};

export default Footer;
```

- add css
- Register the user

```js
// Register.js
import React, { useState } from "react";

export const Register = () => {
  const [user, setUser] = useState({
    name: "",
    email: "",
    password: "",
    phone: "",
  });

  const handleInputChange = (event) => {
    setUser((prevState) => {
      return { ...prevState, [event.target.name]: event.target.value };
    });
  };

  const handleSubmit = async (event) => {
    event.preventDefault();
    console.log(user);
    setUser({
      name: "",
      email: "",
      password: "",
      phone: "",
    });
  };

  return (
    <div className="container">
      <h1>User Registration</h1>
      <div className="card">
        <form className="registration-form" onSubmit={handleSubmit}>
          <div className="form-control">
            <label htmlFor="name">Name: </label>
            <input
              type="text"
              name="name"
              id="name"
              value={user.name}
              onChange={handleInputChange}
            />
          </div>
          <div className="form-control">
            <label htmlFor="email">Email: </label>
            <input
              type="email"
              name="email"
              id="email"
              value={user.email}
              onChange={handleInputChange}
            />
          </div>
          <div className="form-control">
            <label htmlFor="password">Password: </label>
            <input
              type="password"
              name="password"
              id="password"
              value={user.password}
              onChange={handleInputChange}
            />
          </div>
          <div className="form-control">
            <label htmlFor="phone">Phone: </label>
            <input
              type="tel"
              name="phone"
              id="phone"
              value={user.phone}
              onChange={handleInputChange}
            />
          </div>
          <div className="form-control">
            <button type="submit" className="btn">
              Register
            </button>
          </div>
        </form>
      </div>
    </div>
  );
};
```

- create Service for registering the user

```js
// INSTALL axios
import axios from "axios";
axios.defaults.withCredentials = true;

const baseURL = "http://localhost:3030/api";
export const registerUser = async (user) => {
  const response = await axios.post(`${baseURL}/users/register`, user);
  return response.data;
};
```

- call the service and register the user

```js
const handleSubmit = async (event) => {
  event.preventDefault();
  try {
    const response = await registerUser(user);
    console.log(user);
    console.log(response);
  } catch (error) {
    console.log(error);
  }
  setUser({
    name: "",
    email: "",
    password: "",
    phone: "",
  });
};
```

- How to use react-toastify

```js
// install the package

import { ToastContainer, toast } from "react-toastify";
import "react-toastify/dist/ReactToastify.min.css";

toast.success(response.message);
toast.error(error.response.data.message);

// add container in the return function
<ToastContainer />;
```

- Modal states, Modal create, Modal Load

```js
// Register.js
const [modalText, setModalText] = useState("");
const [isModalOpen, setIsModalOpen] = useState(false);
const [responseStatus, setResponseStatus] = useState(false);

const handleSubmit = async (event) => {
  event.preventDefault();
  try {
    const response = await registerUser(user);
    console.log(response);
    setModalText(response.message);
    setResponseStatus(true);
  } catch (error) {
    setModalText(error.response.data.message);
    setResponseStatus(false);
  } finally {
    setIsModalOpen(true);
  }
  setUser({
    name: "",
    email: "",
    password: "",
    phone: "",
  });
};

const closeModal = () => {
  setIsModalOpen(false);
};

// load the modal
{
  isModalOpen && (
    <Modal
      modalText={modalText}
      closeModal={closeModal}
      responseStatus={responseStatus}
    />
  );
}
```

- create the Modal component

  ```js
  import React, { useEffect } from "react";

  const Modal = ({ modalText, closeModal, responseStatus }) => {
    useEffect(() => {
      setTimeout(() => {
        closeModal();
      }, 2000);
    }, [closeModal]);

    return (
      <p style={{ color: responseStatus ? "green" : "red" }}>{modalText}</p>
    );
  };

  export default Modal;
  ```

- Navigate to one route from another

  ```js
  // after registration successfully got to login route
  import { useNavigate } from "react-router-dom";
  const navigate = useNavigate();
  navigate("/login");
  ```

- ADD user login functionality

  ```js
  import React, { useState } from "react";
  import { loginUser } from "../services/UserService";

  import Modal from "../Components/Modal";
  import { useNavigate } from "react-router-dom";

  export const Login = () => {
    const navigate = useNavigate();
    const [user, setUser] = useState({
      email: "",
      password: "",
    });

    const [modalText, setModalText] = useState("");
    const [isModalOpen, setIsModalOpen] = useState(false);
    const [responseStatus, setResponseStatus] = useState(false);

    const handleInputChange = (event) => {
      setUser((prevState) => {
        return { ...prevState, [event.target.name]: event.target.value };
      });
    };

    const handleSubmit = async (event) => {
      event.preventDefault();
      try {
        await loginUser(user);
        navigate("/blogs");
      } catch (error) {
        setModalText(error.response.data.message);
        setResponseStatus(false);
        setIsModalOpen(true);
      }
    };

    const closeModal = () => {
      setIsModalOpen(false);
    };

    return (
      <div className="container">
        <h1>User Login</h1>
        <div className="card">
          <form className="registration-form" onSubmit={handleSubmit}>
            <div className="form-control">
              <label htmlFor="email">Email: </label>
              <input
                type="email"
                name="email"
                id="email"
                value={user.email}
                onChange={handleInputChange}
              />
            </div>
            <div className="form-control">
              <label htmlFor="password">Password: </label>
              <input
                type="password"
                name="password"
                id="password"
                placeholder="password must be 6 characters"
                value={user.password}
                onChange={handleInputChange}
              />
            </div>

            <div className="form-control">
              <button type="submit" className="btn">
                Login
              </button>
            </div>
          </form>
        </div>
        {isModalOpen && (
          <Modal
            modalText={modalText}
            closeModal={closeModal}
            responseStatus={responseStatus}
          />
        )}
      </div>
    );
  };

  // services/UserService.js
  export const loginUser = async (user) => {
    const response = await axios.post(`${baseURL}/users/login`, user);
    return response.data;
  };
  ```

- get user profile from backend and load in Profile component

  - first create the service for api calling

  ```js
  export const userProfile = async () => {
    const response = await axios.get(`${baseURL}/users/profile`, {
      withCredentials: true,
    });
    return response.data;
  };
  ```

  ```js
  // Profile.js
  import React, { useEffect, useState } from "react";
  import { userProfile } from "../services/UserService";

  export const Profile = () => {
    const [user, setUser] = useState(null);
    const [isLoading, setIsLoading] = useState(false);
    const [error, setError] = useState();

    const makeRequest = async () => {
      setIsLoading(true);
      try {
        const response = await userProfile();
        // console.log(response.data);
        setUser(response.data);
        setIsLoading(false);
      } catch (error) {
        setError(error.response.data.message);
        setUser(null);
        setIsLoading(false);
      }
    };

    useEffect(() => {
      makeRequest();
    }, []);

    console.log(user);

    return (
      <div className="container">
        <h2>User Profile</h2>
        <div className="card center">
          {isLoading && <p>Profile is loading</p>}
          {error && <p style={{ color: "orange" }}>{error}</p>}
          {user && (
            <div className="profile">
              <h3 className="profile__name">Name: {user.name}</h3>
              <p className="profile__email">Email: {user.email}</p>
              <p className="profile__phone">Phone: {user.phone}</p>
              <div className="profile__buttons">
                <button className="btn">Update</button>
                <button className="btn">Delete</button>
              </div>
            </div>
          )}
        </div>
      </div>
    );
  };
  ```

- logout the user

```js
// Navbar.js
<Link to="/logout" className="nav__link" onClick={handleLogout}>
  Logout
</Link>;

const handleLogout = async () => {
  try {
    const response = await logoutUser();
    if (response.status === 200) {
      navigate("/login");
    }
  } catch (error) {
    return new Error(error);
  }
};
```

- protect the routes using redux-tookit

  - install packages `npm install @reduxjs/toolkit react-redux`
  - create a userSlice inside feature folder

    ```js
    // features/userSlice.js
    import { createSlice } from "@reduxjs/toolkit";

    const getLocalStoreItem = () => {
      if (localStorage.getItem("loginStatus") === null) {
        return false;
      } else {
        return JSON.parse(localStorage.getItem("loginStatus"));
      }
    };

    export const userSlice = createSlice({
      name: "user",
      initialState: { isLoggedIn: getLocalStoreItem(), firstRender: true },

      reducers: {
        login: (state) => {
          localStorage.setItem("loginStatus", "true");
          state.isLoggedIn = getLocalStoreItem();
        },
        logout: (state) => {
          localStorage.setItem("loginStatus", "false");
          state.isLoggedIn = getLocalStoreItem();
        },
        updateRender: (state) => {
          state.firstRender = false;
        },
      },
    });

    // export reducer and action createor
    // Action creators are generated for each case reducer function
    export const { login, logout, updateRender } = userSlice.actions;

    export default userSlice.reducer;
    ```

  - create a store.js inside app folder

    ```js
    import { configureStore } from "@reduxjs/toolkit";

    import userReducer from "../features/userSlice";

    const store = configureStore({
      reducer: {
        user: userReducer,
      },
    });
    export default store;
    ```

  - lets use the store

    ```js
    import { Provider } from "react-redux";
    import store from "./app/store";
    <Provider store={store}>
      <App />
    </Provider>;
    ```

  - use store: lets control logout, login, profile, register link in navbar

    ```js
    // Navbar.js
    import React from "react";
    import { Link } from "react-router-dom";
    import { useNavigate } from "react-router-dom";
    import { useSelector } from "react-redux";

    import { logoutUser } from "../services/UserService";

    const Navbar = () => {
      const isLoggedIn = useSelector((state) => state.user.isLoggedIn);
      const navigate = useNavigate();
      const handleLogout = async () => {
        try {
          const response = await logoutUser();
          if (response.status === 200) {
            navigate("/login");
          }
        } catch (error) {
          return new Error(error);
        }
      };
      return (
        <nav className="center">
          {isLoggedIn && (
            <Link to="/" className="nav__link">
              Blogs
            </Link>
          )}
          {!isLoggedIn && (
            <>
              <Link to="/register" className="nav__link">
                Register
              </Link>
              <Link to="/login" className="nav__link">
                Login
              </Link>
            </>
          )}
          {isLoggedIn && (
            <Link to="/logout" className="nav__link" onClick={handleLogout}>
              Logout
            </Link>
          )}
          <Link to="/contact" className="nav__link">
            Contact
          </Link>
          {isLoggedIn && (
            <Link to="/profile" className="nav__link">
              Profile
            </Link>
          )}
        </nav>
      );
    };

    export default Navbar;
    ```

  - use store: let control routes -> index.js

    ```js
    import React from "react";
    import { BrowserRouter, Route, Routes } from "react-router-dom";
    import { useSelector } from "react-redux";

    import Navbar from "../layout/Navbar";
    import Footer from "../layout/Footer";
    import { Blogs, Contact, Error, Login, Profile, Register } from "../pages";

    const Index = () => {
      const isLoggedIn = useSelector((state) => state.user.isLoggedIn);
      console.log(isLoggedIn);
      return (
        <BrowserRouter>
          <Navbar />
          <main>
            <Routes>
              {!isLoggedIn && (
                <>
                  <Route path="/register" element={<Register />} />
                  <Route path="/login" element={<Login />} />
                </>
              )}
              <Route path="/contact" element={<Contact />} />
              {isLoggedIn && (
                <>
                  <Route path="/" element={<Blogs />} />
                  <Route path="/Profile" element={<Profile />} />
                </>
              )}
              <Route path="*" element={<Error />} />
            </Routes>
          </main>
          <Footer />
        </BrowserRouter>
      );
    };

    export default Index;
    ```

  - lets dispatch actions when login and logout

    ```js
    // Login.js
    import { useDispatch } from "react-redux";
    const dispatch = useDispatch();

    const handleSubmit = async (event) => {
      event.preventDefault();
      try {
        await loginUser(user);
        dispatch(login());
        navigate("/blogs");
      } catch (error) {
        setModalText(error.response.data.message);
        setResponseStatus(false);
        setIsModalOpen(true);
      }
    };

    // Navbar.js
    import { useSelector, useDispatch } from "react-redux";
    import { logout } from "../features/userSlice";
    const dispatch = useDispatch();
    const handleLogout = async () => {
      try {
        const response = await logoutUser();
        if (response.status === 200) {
          dispatch(logout());
          navigate("/login");
        }
      } catch (error) {
        return new Error(error);
      }
    };
    ```

- lets add refresh token functionality

```js
// Profile.js

// for the first render lets get the original token
let firstRender = true;

const refreshToken = async () => {
  try {
    const res = await axios.get("http://localhost:3001/api/refresh", {
      withCredentials: true,
    });
    return res.data;
  } catch (error) {
    console.log(error);
  }
};

useEffect(() => {
  if (firstRender) {
    sendRequest().then((data) => setUser(data.user));
    firstRender = false;
  }
  let interval = setInterval(() => {
    refreshToken().then((data) => setUser(data.user));
  }, 1000 * 29);

  return () => clearInterval(interval);
}, []);
```

## 6. MERN User Managaement Website (Convert this to typescript)

### 6.0 Basic setup

- setup folder structure
  `mkdir models views controllers routes config middlewares public`
- npm init -y
- install npm packages
  `npm install express body-parser multer ejs bcrypt mongoose dotenv`
  `npm install -D nodemon morgan chalk@^4.1.2`
- setup env variables

  ```js
  // .env
  MONGO_URL=mongodb+srv://oss:anisul1234@oss.6kg37.mongodb.net/umsDB
  SERVER_PORT=3001


  // config/index.js
  require("dotenv").config();

  exports.dev = {
    db: {
      url: process.env.MONGO_URL,
    },
    app: {
      port: process.env.SERVER_PORT,
    },
  };
  ```

- create a basic server

  ```js
  const express = require("express");

  const app = express();

  const port = dev.app.port || 3002;

  app.listen(port, async () => {
    console.log(`server is running at http://localhost:${port}`);
  });

  app.use(bodyParser.json());
  app.use(bodyParser.urlencoded({ extended: true }));

  // health chekup
  app.get("/test", (req, res) => {
    res.status(200).json({
      message: "testing is successful",
    });
  });

  app.use((req, res, next) => {
    res.status(404).send("Route Not Found");
  });

  app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send("Something broke!");
  });
  ```

- setup env variables & connect to mongodb atlas

  ```js
  // config/db.js
  // connecting to database
  const mongoose = require("mongoose");
  const { dev } = require(".");
  const connectDatabase = async () => {
    try {
      await mongoose.connect(dev.db.url);
      console.log("database is connected");
    } catch (error) {
      console.log("database is not connected");
      console.log(error);
      process.exit(1);
    }
  };

  module.exports = connectDatabase;

  // index.js
  const { dev } = require("./config");
  const connectDatabase = require("./config/db");

  app.listen(port, async () => {
    console.log(`server is running at http://localhost:${port}`);
    connectDatabase();
  });
  ```

### 6.1 Creating user

- create models

  - create schema & model

  ```js
  const { Schema, model } = require("mongoose");

  const userSchema = new Schema({
    name: {
      type: String,
      required: [true, "user name is required"],
      trim: true,
      minlength: [3, "user name must be atleast 3 characters"],
      maxlength: [100, "user name maximum can be 100 characters"],
    },
    email: {
      type: String,
      required: [true, "user email is required"],
      unique: true,
      trim: true,
      lowercase: true,
    },
    password: {
      type: String,
      required: [true, "user password is required"],
      min: 3,
    },
    image: {
      type: String,
      required: [true, "user image is required"],
    },
    mobile: {
      type: String,
      required: [true, "user mobile is required"],
    },
    is_admin: {
      type: Number,
      required: [true, "admin or not"],
    },
    is_verified: {
      type: Number,
      default: 0,
    },
  }
  {timestamps: true}
  );

  exports.User = model("Users", userSchema);
  ```

- create views

  ```ejs
    // views/layout/header.ejs
    // views/layout/footer.ejs

    // views/users/registration.ejs
    <%-include('../layout/header') %>
    <form action="register" method="post" enctype="multipart/form-data">
      <div>
        <label for="name">Name: </label>
        <input type="text" name="name" required />
      </div>
      <br />

      <div>
        <label for="email">Email: </label>
        <input type="email" name="email" required />
      </div>
      <br />

      <div>
        <label for="password">Password: </label>
        <input type="password" name="password" required />
      </div>
      <br />

      <div>
        <label for="text">Mobile: </label>
        <input type="text" name="mobile" required />
      </div>
      <br />

      <div>
        <input type="file" name="image" required />
      </div>
      <br />

      <div>
        <button type="submit">Register</button>
      </div>
      <br />
    </form>
    <%-include('../layout/footer') %>
  ```

  - set views in index.js

- create routes for loading register page & add in index.js

  ```js
  const { registerUser, addUser } = require("../controllers/users");

  const router = require("express").Router();

  router.get("/register", registerUser);

  router.get("*", (req, res) => {
    res.status(404).json({
      message: "404 not found",
    });
  });

  module.exports = router;
  ```

- create controllers for loading register page

  ```js
  // controllers/users.js
  const path = require("path");
  const { User } = require("../models/user");

  const registerUser = async (req, res) => {
    try {
      res.status(200).render("registration");
    } catch (error) {
      res.status(500).json(error.message);
    }
  };

  module.exports = { registerUser };
  ```

- crate a middleware for uploading image

  ```js
  // middleware/uploadFile.js
  const path = require("path");
  const multer = require("multer");

  const storage = multer.diskStorage({
    destination: function (req, file, cb) {
      cb(null, path.join(__dirname, "../public/images/users"));
    },
    filename: function (req, file, cb) {
      const name = Date.now() + "-" + file.originalname;
      cb(null, name);
    },
  });

  exports.upload = multer({ storage: storage }).single("image");
  ```

- secure your password with bcrypt

  ```js
  const bcrypt = require("bcrypt");
  const saltRounds = 10;

  exports.sercuePassword = async (password) => {
    try {
      return await bcrypt.hash(password, saltRounds);
    } catch (error) {
      console.log(error);
    }
  };
  ```

- create routes and controllers for adding new user

  ```js
  // routes/users.js
  router.post("/register", upload, addUser);

  // controllers/users.js
  onst addUser = async (req, res) => {
  try {
    // Step 1: get the data from the request body
    const { name, email, password, mobile}= req.body;

    // Step 2: check the user inputs are missing or not
    // Not needed if you are using 3rd-party packages for input validation
    if (!name || !email || !password)
      return errorResponse(res, 400, 'name, email, password is required')

    // check password length >= 6
    if (password.length < 6)
      return errorResponse(res, 400, 'Minimum Password length is 6 characters')

    // Step 3: check whether the user already exist?
    // check user already exist or not with email
    const exsitingUser = await User.findOne({ email })
    if (exsitingUser) {
      return errorResponse(
        res,
        400,
        'user already exists with this email. Login Please.'
      )
    }

    // Step 4: hash password
    const hashedPassword = await securePassword(password);

    // Step 5: create the new user
    const newUser = new User({
      name, // name: name
      email,
      password: hashedPassword,
      image: req.file.filename,
      mobile,
      is_admin: 0,
    });

    // Step 6: verify the user before saving in database

    // Step 7: save the new user
    const newUserData = await newUser.save();

    // Step 8: send the response
    if (newUserData) {
      res
        .status(201)
        .render(path.join(__dirname, "../views/users/registration"), {
          message: "registration successful. Please verify your email",
        });
    } else {
      res.render(path.join(__dirname, "../views/users/registration"), {
        message: "registration failed. Please try again",
      });
    }
  } catch (error) {
    res.status(500).json(error.message);
  }
  };

  ```

### 6.2 User email verification by SMTP (Simple Message Transfer Protocol)

- when adding user send an email to user for verification
- when user click the verification link verify the user (update the is_verify property in database)

```js
const addUser = async (req, res) => {
  try {
    const { password } = req.body;
    const hashedPassword = await securePassword(password);
    const newUser = new User({
      name: req.body.name,
      email: req.body.email,
      password: hashedPassword,
      image: req.file.filename,
      mobile: req.body.mobile,
      is_admin: 0,
    });
    const newUserData = await newUser.save();
    if (newUserData) {
      sendVerificationEmail(req.body.name, req.body.email, newUserData._id);
      res
        .status(201)
        .render(path.join(__dirname, "../views/users/registration"), {
          message: "registration successful. Please verify your email",
        });
    } else {
      res.render(path.join(__dirname, "../views/users/registration"), {
        message: "registration failed. Please try again",
      });
    }
  } catch (error) {
    res.status(500).json(error.message);
  }
};

const verifyEmail = async (req, res) => {
  try {
    const id = req.query.id;
    // console.log(id);
    const updateInfo = await User.updateOne(
      { _id: id },
      { $set: { is_verified: 1 } }
    );
    res
      .status(200)
      .render(path.join(__dirname, "../views/users/email-verified"), {
        message: "verification successful.",
      });
  } catch (error) {
    console.log("verification unsuccessful.");
    console.log(error);
  }
};
```

- send verification email with nodemailer code using google SMTP

```js
const nodemailer = require("nodemailer");
const dev = require("../config");

const emailSetUp = async (optionSetup) => {
  try {
    const transporter = nodemailer.createTransport({
      host: "smtp.gmail.com",
      port: 587,
      secure: false, // true for 465, false for other ports
      auth: {
        user: dev.app.smtpUsername, // generated ethereal user
        pass: dev.app.smtpPassword, // generated ethereal password
      },
    });

    const mailOptions = {
      from: dev.app.smtpUsername, // sender address
      to: optionSetup.email, // list of receivers
      subject: optionSetup.subject, // Subject line
      html: optionSetup.html, // html body
    };

    // send mail with defined transport object
    await transporter.sendMail(mailOptions, (error, info) => {
      if (error) {
        console.log(error);
      } else {
        console.log("Message sent: %s", info.response);
      }
    });
  } catch (error) {
    console.log("Problem sending Email: ", error);
  }
};

exports.sendVerificationEmail = async (name, email, _id) => {
  const optionSetup = {
    email,
    subject: "verification Email",
    html: `<p>Hello ${name} . Please click here to verify: <a href="http://127.0.0.1:3001/api/verify?id=${_id}"> Verify Email </a> Your Mail</p>`, // html body
  };
  emailSetUp(optionSetup);
};

exports.sendResetPasswordEmail = async (name, email, passwordResetToken) => {
  const optionSetup = {
    email,
    subject: "Password Reset Email",
    html: `<p>Hello ${name} . Please click here to  <a href="http://127.0.0.1:3001/api/reset-password?token=${passwordResetToken}">reset password </a> </p>`, // html body
  };
  emailSetUp(optionSetup);
};
```

### 6.3 User login & logout with session in user management system

- for manging session: `npm install express-session`
- require package and set session

```js
const session = require("express-session");

app.use(
  session({
    secret: dev.app.sessionSecret, // using session key from config file
    resave: false,
    saveUninitialized: true,
  })
);
```

- after user successfully login set the session id
  `req.session.user_id = userData._id;`

- for managing enabling/disabling different routes based on session we can create a middleware

```js
// middleware/auth.js
const isLoggedIn = async (req, res, next) => {
  try {
    if (req.session.user_id) {
    } else {
      return res.redirect("/login");
    }
    next();
  } catch (error) {
    console.log(error);
  }
};
const isLoggedOut = async (req, res, next) => {
  try {
    if (req.session.user_id) {
      return res.redirect("/home");
    }
    next();
  } catch (error) {
    console.log(error);
  }
};

module.exports = { isLoggedIn, isLoggedOut };
```

- now we can set our routes with the auth middleware

```js
const { isLoggedOut, isLoggedIn } = require("../middlewares/auth");

router.get("/register", isLoggedOut, registerUser);
router.get("/verify", verifyEmail);
router.post("/register", upload, addUser);

router.get("/login", isLoggedOut, loadLogin);
router.get("/logout", isLoggedIn, userLogout);

router.post("/login", verifyLogin);
router.get("/home", isLoggedIn, loadHome);
```

- now we can create a middleware for logout

```js
const userLogout = async (req, res) => {
  try {
    req.session.destroy();
    res.status(200).render(path.join(__dirname, "../views/users/login"));
  } catch (error) {
    res.status(500).json(error.message);
  }
};
```

### 6.4 Forget and reset password

- create the forget link in login page
- create the forget view

```html
<%-include ("../layout/header") %>
<h1>Forget Password</h1>

<%if(typeof message !== 'undefined'){%>
<h4 style="color: orange"><%=message%></h4>
<%}%>

<form action="/forget" method="post">
  <div>
    <input type="email" name="email" placeholder="enter email" size="40" />
  </div>
  <br />

  <div>
    <button type="submit">Send Reset Link</button>
  </div>
</form>

<%-include ("../layout/footer") %>
```

- create the /forget get route and load the forget view
- create the /forget post route for handling the forget password request

  ```js
  router.get("/forget", isLoggedOut, loadForget);
  router.post("/forget", forgetPassword);
  ```

- in the forget post controller we will do following steps
  - search the user with email.
    - if user is not found then send a response saying invalid email
    - if user is found then check whether user is verified or not
      - if user not verified send a response saying verify email please
      - if user is verified then set a token string with [random string package](https://www.npmjs.com/package/randomstring)and send email and ask the user for checking the email to reset the password and render foregt view

```js
const forgetPassword = async (req, res) => {
  try {
    const email = req.body.email;
    const userData = await User.findOne({ email: email });
    if (userData) {
      // check user is verified or not
      if (userData.is_verified === 0) {
        res.status(404).render(path.join(__dirname, "../views/users/forget"), {
          message: "Please check your email and verify your user email please",
        });
      } else {
        const randomString = getRandomString();
        const updatedData = await User.updateOne(
          { email: email },
          {
            $set: { token: randomString },
          }
        );
        sendResetPasswordEmail(
          userData.name,
          userData.email,
          userData._id,
          randomString
        );
        res.status(200).render(path.join(__dirname, "../views/users/forget"), {
          message: "Please check your email to reset password",
        });
      }
    } else {
      res.status(404).render(path.join(__dirname, "../views/users/forget"), {
        message: "Email is incorrect",
      });
    }
  } catch (error) {
    res.status(500).json(error.message);
  }
};
```

- when user will check the email and click on the link it will make a get request to `http://127.0.0.1:3001/forget-password?token=${token}` this route. so we need to create this route: `router.get("/forget-password", isLoggedOut, loadForgetPassword);`

```js
const loadForgetPassword = async (req, res) => {
  try {
    const token = req.query.token;
    const tokenData = await User.findOne({ token: token });
    if (tokenData) {
      res
        .status(200)
        .render(path.join(__dirname, "../views/users/forget-password"), {
          userId: tokenData._id,
        });
    } else {
      res.status(404).render(path.join(__dirname, "../views/users/404"), {
        message: "token is invalid.",
      });
    }
  } catch (error) {
    console.log(error.message);
  }
};
```

- let us create a view for forget password and load it using the loadForgetPassword controller

```html
<%-include ("../layout/header") %>
<h1>Forget Password</h1>

<form action="/forget-password" method="post">
  <div>
    <input
      type="password"
      name="password"
      placeholder="enter password"
      size="40"
    />
  </div>
  <br />
  <div>
    <input type="hidden" name="userId" value="<%=userId%>" size="40" />
  </div>
  <br />

  <div>
    <button type="submit">Reset Password</button>
  </div>
</form>

<%-include ("../layout/footer") %>
```

- we will have a post request in the /forget-password route `router.post("/forget-password", resetPassword);`

```js
const resetPassword = async (req, res) => {
  try {
    // get password and id from the request
    const password = req.body.password;
    const userId = req.body.userId;

    // secure the password
    const hashedPassword = await securePassword(password);
    const updatedData = await User.findByIdAndUpdate(
      { _id: userId },
      {
        $set: {
          password: hashedPassword,
          token: "",
        },
      }
    );

    res.redirect("/login");
  } catch (error) {
    console.log(error);
  }
};
```

### 6.5 Resend verification email link

- create a resend verification email link view

```html
<%-include ("../layout/header") %>
<h1>Send Verification Link</h1>

<%if(typeof message !== 'undefined'){%>
<h4 style="color: orange"><%=message%></h4>
<%}%>

<form method="post">
  <div>
    <input type="email" name="email" placeholder="enter email" size="40" />
  </div>
  <br />

  <div>
    <button type="submit">Send Verification Link</button>
  </div>
</form>

<%-include ("../layout/footer") %>
```

- load the view based on a specific route

```js
router.get("/send-verification-link", loadSendVerificationLink);

const loadSendVerificationLink = async (req, res) => {
  try {
    res
      .status(200)
      .render(path.join(__dirname, "../views/users/send-verification-link"));
  } catch (error) {
    res.status(404).render(path.join(__dirname, "../views/users/404"), {
      message: error.message,
    });
  }
};
```

- handle the post request for sending verification link

```js
router.post("/send-verification-link", sendVerificationLink);

const sendVerificationLink = async (req, res) => {
  try {
    const email = req.body.email;
    const userData = await User.findOne({ email: email });
    if (userData) {
      sendVerificationEmail(req.body.name, req.body.email, userData._id);
      res.render(
        path.join(__dirname, "../views/users/send-verification-link"),
        { message: "Reset verification mail send to your email" }
      );
    } else {
      res.render(
        path.join(__dirname, "../views/users/send-verification-link"),
        { message: "this email does not exits" }
      );
    }
  } catch (error) {
    res.status(404).render(path.join(__dirname, "../views/users/404"), {
      message: error.message,
    });
  }
};
```

### 6.6 Get & display User Profile

- create a view for profile (home.ejs)

```html
<%-include ("../layout/header") %>
<h1>Welcome to Home</h1>

<h3><%=user.name%></h3>
<img
  src="images/users/<%=user.image%>"
  alt="<%=user.image%>"
  style="width: 300px; height: 200px"
/>
<h4><%=user.email%></h4>
<h4><%=user.mobile%></h4>
<%if(user.is_verified === 1){%>
<h4>Your account is verified</h4>
<%}else{%>
<h4>Your account is not verified</h4>
<%}%>

<a href="/logout">Logout</a>

<%-include ("../layout/footer") %>
```

- create a route for handling the request

```js
router.get("/home", isLoggedIn, loadHome);

const loadHome = async (req, res) => {
  try {
    const userData = await User.findOne({ _id: req.session.user_id });

    res
      .status(200)
      .render(path.join(__dirname, "../views/users/home"), { user: userData });
  } catch (error) {
    res.status(500).json(error.message);
  }
};
```

### 6.7 User profile update

- add a link inside home page
  `<a href="/edit?id=<%=user._id%>">Edit</a>`
- create the edit view

```html
<%-include ("../layout/header") %>
<h2>Edit User</h2>

<img
  src="images/users/<%=user.image%>"
  alt="<%=user.image%>"
  style="width: 300px; height: 200px"
/>
<form action="/edit" method="post" enctype="multipart/form-data">
  <div>
    <label for="name">Name: </label>
    <input type="text" name="name" required size="40" value="<%=user.name%>" />
  </div>
  <br />

  <input type="hidden" name="user_id" value="<%=user._id%>" />

  <div>
    <label for="email">Email: </label>
    <input
      type="email"
      name="email"
      required
      size="40"
      value="<%=user.email%>"
    />
  </div>
  <br />

  <div>
    <label for="text">Mobile: </label>
    <input
      type="text"
      name="mobile"
      required
      size="40"
      value="<%=user.mobile%>"
    />
  </div>
  <br />

  <div>
    <input type="file" name="image" />
  </div>
  <br />

  <div>
    <button type="submit">Update</button>
  </div>
  <br />
</form>

<% if(typeof message !== 'undefined'){ %>
<h4 style="color: orange"><%=message%></h4>
<% } %> <%-include('../layout/footer') %> <%-include ("../layout/footer") %>
```

- handle the get request `router.get("/edit", isLoggedIn, loadEdit);`

```js
const loadEdit = async (req, res) => {
  try {
    const id = req.query.id;
    const userData = await User.findById({ _id: id });
    if (userData) {
      res.render(path.join(__dirname, "../views/users/edit"), {
        user: userData,
      });
    } else {
      res.redirect("/home");
    }
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

- now handle the put request `router.post("/edit", upload, updateProfile);`

```js
const updateProfile = async (req, res) => {
  try {
    if (req.file) {
      const userData = await User.findByIdAndUpdate(
        { _id: req.body.user_id },
        {
          $set: {
            name: req.body.name,
            email: req.body.email,
            mobile: req.body.mobile,
            image: req.file.filename,
          },
        }
      );
    } else {
      const userData = await User.findByIdAndUpdate(
        { _id: req.body.user_id },
        {
          $set: {
            name: req.body.name,
            email: req.body.email,
            mobile: req.body.mobile,
          },
        }
      );
    }
    res.redirect("/home");
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

### 6.8 Admin Login and Logout

- setup admin route

```js
const express = require("express");
const morgan = require("morgan");
const path = require("path");
const adminRoute = express();

const session = require("express-session");
const { dev } = require("../config");

adminRoute.use(
  session({
    secret: dev.app.sessionSecret,
    resave: false,
    saveUninitialized: true,
  })
);

adminRoute.use(express.static("public"));
adminRoute.use(morgan("dev"));
adminRoute.use(express.json());
adminRoute.use(express.urlencoded({ extended: true }));
adminRoute.set("view engine", "ejs");
adminRoute.set("views", "./views/admin");

module.exports = adminRoute;
```

- create a normal user and then change value of is_verified and is_admin in database
- create views for login

```html
<%-include ("../layout/header") %>
<h1>Admin Login</h1>

<%if(typeof message !== 'undefined'){%>
<h4 style="color: orange"><%=message%></h4>
<%}%>

<form action="" method="post">
  <div>
    <input type="email" name="email" placeholder="enter email" size="40" />
  </div>
  <br />
  <div>
    <input
      type="password"
      name="password"
      placeholder="enter password"
      size="40"
    />
  </div>
  <br />

  <div>
    <button type="submit">Login</button>
  </div>
</form>
<!-- <a href="/forget">Forget Password</a>
<a href="/send-verification-link">Send Verification Email</a> -->
<%-include ("../layout/footer") %>
```

- load login for admin `adminRoute.get("/", loadLogin);`

```js
const loadLogin = async (req, res) => {
  try {
    res.render("login");
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

- verify the login `adminRoute.post("/", verifyLogin);`

```js
const verifyLogin = async (req, res) => {
  try {
    const email = req.body.email;
    const password = req.body.password;
    const userData = await User.findOne({ email: email });

    if (userData) {
      const isPasswordMatched = await comparePassword(
        password,
        userData.password
      );
      if (isPasswordMatched) {
        if (userData.is_admin === 0) {
          res.status(404).render("login", {
            message: "Email and password is incorrect",
          });
        } else {
          req.session.user_id = userData._id;
          res.status(200).redirect("/admin/home");
        }
      } else {
        res.status(404).render("login", {
          message: "Email and password is incorrect",
        });
      }
    } else {
      res.status(404).render("login", {
        message: "Email and password is incorrect",
      });
    }
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

- create the home route and home view if user is successfully login
  `adminRoute.get("/home", loadDashboard);`

```html
<%-include ("../layout/header") %>
<h1>Welcome to Dashboard</h1>
<a href="/admin/logout">Logout</a>
<%-include ("../layout/footer") %>
```

```js
const loadDashboard = async (req, res) => {
  try {
    res.render("home");
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

- for any other route always redirect to home page of admin. remember wild card always shpuld be the last route

```js
adminRoute.get("*", (req, res) => {
  res.redirect("/admin");
});
```

- logout admin `adminRoute.get("/logout", adminLogout);`

```js
const adminLogout = async (req, res) => {
  try {
    req.session.destroy();
    res.redirect("/admin");
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

- add session authentication for admin

```js
//middlewares/adminAuth.js
const isLoggedIn = async (req, res, next) => {
  try {
    if (req.session.user_id) {
    } else {
      return res.redirect("/admin");
    }
    next();
  } catch (error) {
    console.log(error);
  }
};
const isLoggedOut = async (req, res, next) => {
  try {
    if (req.session.user_id) {
      return res.redirect("/admin/home");
    }
    next();
  } catch (error) {
    console.log(error);
  }
};

module.exports = { isLoggedIn, isLoggedOut };
```

### 6.9 Admin forget and reset password

- same as user forget and reset password

### 6.10 Admin Profile & dashboard

- admin profile

```js
// load home
const loadHome = async (req, res) => {
  try {
    const adminData = await User.findById({ _id: req.session.user_id });
    res.render("home", { admin: adminData });
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

```html
<%-include ("../layout/header") %>
<h1>Welcome to Home</h1>

<h3><%=admin.name%></h3>
<img
  src="images/users/<%=admin.image%>"
  alt="<%=admin.image%>"
  style="width: 300px; height: 200px"
/>
<h4><%=admin.email%></h4>
<h4><%=admin.mobile%></h4>
<%if(admin.is_verified === 1){%>
<h4>Your account is verified</h4>
<%}else{%>
<h4>Your account is not verified</h4>
<%}%> <%if(admin.is_admin === 1){%>
<h4>You are an admin</h4>
<%}else{%>
<h4>You are not an admin</h4>
<%}%>

<a href="/edit?id=<%=admin._id%>">Edit</a>
<a href="/logout">Logout</a>

<%-include ("../layout/footer") %>
```

- create a dashboard & link from the home page `adminRoute.get("/dashboard", isLoggedIn, loadDashboard);`

```js
const loadDashboard = async (req, res) => {
  try {
    res.render("dashboard");
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

### 6.11 List Users in admin dashboard

- Inside the controller

```js
const loadDashboard = async (req, res) => {
  try {
    const usersData = await User.find({ is_admin: 0 });
    res.render("dashboard", { users: usersData });
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

- create a view

```html
// dashboard.ejs
<%-include ("../layout/header") %>
<h2>Dashbaord</h2>

<a href="/admin/home">Go to Home</a>
<br />

<!-- <p><%=users%></p> -->

<table style="width: 50%">
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Mobile</th>
      <th>Verified</th>
      <th>Image</th>
    </tr>
  </thead>

  <%if(users.length > 0){%>
      <tbody>
    <%for(let x=0; x<users.length; x++){%>
      <tr>
        <td><%=users[x].name%></td>
        <td><%=users[x].email%></td>
        <td><%=users[x].mobile%></td>
        <td>
        <%if(users[x].is_verified === 1){%>
        <h4>verified</h4>
        <%}else{%>
        <h4>Not verified</h4>
        <%}%>
        </td>
        <td><img src="/images/users/<%=users[x].image%>" alt="<%=users[x].name%>" width="100" height="100"></td>
      </tr>
    <%}%>
    </tbody>
  <%}else{%>
    <tbody>
      <tr>
        <td colspan="5">Users Not Found</td>
      </tr>
    </tbody>
  <%}%>

</table>

<%-include ("../layout/footer") %>
```

### 6.12 Add New Users & send mail from admin dashboard

- new user view is here

```html
<%-include('../layout/header') %>
<h1>Add New User</h1>
<form method="post" enctype="multipart/form-data">
  <div>
    <label for="name">Name: </label>
    <input type="text" name="name" required size="40" />
  </div>
  <br />

  <div>
    <label for="email">Email: </label>
    <input type="email" name="email" required size="40" />
  </div>
  <br />

  <div>
    <label for="text">Mobile: </label>
    <input type="text" name="mobile" required size="40" />
  </div>
  <br />

  <div>
    <input type="file" name="image" required />
  </div>
  <br />

  <div>
    <button type="submit">Add New User</button>
  </div>
  <br />
</form>
<a href="/admin/dashboard">go back</a>

<% if(typeof message !== 'undefined'){ %>
<h4 style="color: red"><%=message%></h4>
<% } %> <%-include('../layout/footer') %>
```

- new user router

```js
adminRoute.get("/new-user", isLoggedIn, loadNewUser);
adminRoute.post("/new-user", upload, addNewUser);
```

- new user controller

```js
const addNewUser = async (req, res) => {
  try {
    const name = req.body.name;
    const email = req.body.email;
    const image = req.file.filename;
    const mobile = req.body.mobile;
    const password = await getRandomString();
    console.log(password);
    const securedPassword = await securePassword(password);

    const newUser = await new User({
      name: name,
      email: email,
      password: securedPassword,
      mobile: mobile,
      image: image,
      is_admin: 0,
    });

    const userData = await newUser.save();
    if (userData) {
      await sendVerificationEmail(name, email, password, userData._id);
      res.redirect("/admin/dashboard");
    } else {
      res.render("new-user", { message: "user could not be saved" });
    }
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

- sendEmail code is here

```js
const nodemailer = require("nodemailer");
const { dev } = require("../config");
// send Email
exports.sendVerificationEmail = async (name, email, password, _id) => {
  try {
    // create reusable transporter object using the default SMTP transport
    let transporter = nodemailer.createTransport({
      host: "smtp.gmail.com",
      port: 587,
      secure: false, // true for 465, false for other ports
      auth: {
        user: dev.app.authEmail, // generated ethereal user
        pass: dev.app.authPassword, // generated ethereal password
      },
    });

    const mailOptions = {
      from: dev.app.authEmail, // sender address
      to: email, // list of receivers
      subject: "Admin added you now verification your Email", // Subject line
      // text: "Hello world?", // plain text body
      html: `<p>Hello ${name} . Please click here to verify: <a href="http://127.0.0.1:3001/verify?id=${_id}"> Verify Email </a> Your Mail</p> <br>
      Email: ${email} <br>
      Password: ${password} <br>
      `, // html body
    };

    // send mail with defined transport object
    await transporter.sendMail(mailOptions, (error, info) => {
      if (error) {
        console.log(error);
      } else {
        console.log("Message sent: %s", info.response);
      }
    });
  } catch (error) {
    console.log(error);
  }
};
```

### 6.13 Edit and save user in dashboard

- create route and controller

```js
adminRoute.get("/edit-user", isLoggedIn, loadEditUser);
adminRoute.post("/edit-user", updateUser);
```

```js
const loadEditUser = async (req, res) => {
  try {
    const userData = await User.findOne({ _id: req.query.id });
    console.log(userData);
    res.render("edit-user", { user: userData });
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

- when coming to this route please bring the user id load edit user & when making post request please send an id

```html
<%-include ("../layout/header") %>
<h2>Edit User By Admin</h2>

<form method="post">
  <div>
    <label for="name">Name: </label>
    <input type="text" name="name" required size="40" value="<%=user.name%>" />
  </div>
  <br />

  <div>
    <label for="email">Email: </label>
    <input
      type="email"
      name="email"
      required
      size="40"
      value="<%=user.email%>"
    />
  </div>
  <br />

  <div>
    <label for="text">Mobile: </label>
    <input
      type="text"
      name="mobile"
      required
      size="40"
      value="<%=user.mobile%>"
    />
  </div>
  <br />

  <div>
    <label for="verify">Verified</label>
    <input type="radio" name="verify" value="1" <% if(user.is_verified == 1) {%>
    checked <%}%> />
  </div>
  <br />

  <div>
    <label for="verify">Unverified</label>
    <input type="radio" name="verify" value="0" <% if(user.is_verified == 0) {%>
    checked <%}%> />
  </div>
  <br />

  <div>
    <button type="submit">Update User</button>
  </div>
  <br />
</form>

<!-- <% if(typeof message !== 'undefined'){ %>
<h4 style="color: orange"><%=message%></h4>
<% } %>  -->
<a href="/admin/dashboard">Go to dashboard</a>
<%-include ("../layout/footer") %>
```

```js
const updateUser = async (req, res) => {
  try {
    const id = req.body.id;
    const userData = await User.findByIdAndUpdate(
      {
        _id: id,
      },
      {
        $set: {
          name: req.body.name,
          email: req.body.email,
          mobile: req.body.mobile,
          is_verified: req.body.verify,
        },
      }
    );
    if (userData) {
      res.redirect("/admin/dashboard");
    }
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

### 6.14 Delete user in dashboard

```html
<td><a href="/admin/delete-user?id=<%=users[x]._id%>">Delete</a></td>
```

```js
adminRoute.get("/delete-user", deleteUser);

const deleteUser = async (req, res) => {
  try {
    const userData = await User.findByIdAndDelete({ _id: req.query.id });
    if (userData) {
      res.redirect("/admin/dashboard");
    } else {
      res.send("user was not deleted");
    }
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

### 6.15 pagination and filter

- in the dashboard view

```html
<form>
  <input
    type="text"
    placeholder="search user name, email, mobile"
    name="search"
  />
  <button type="submit">Search</button>
</form>

<%if(currentPage>1){%>
<a href="?page=<%=previousPage%>">Previous Page</a>
<%}%> <% for(let x=1; x<= totalPages; x++){ %>
<a href="?page=<%=x%>"><%=x%></a>
<%}%> <%if(currentPage < totalPages){%>
<a href="?page=<%=nextPage%>">Next Page</a>
<%}%>
```

```js
const loadDashboard = async (req, res) => {
  try {
    let search = req.query.search ? req.query.search : "";
    const { page = 1, limit = 2 } = req.query;

    // pagination
    // const products = await User.find()
    //   .limit(limit)
    //   .skip((page - 1) * limit);

    // searching
    const usersData = await User.find({
      is_admin: 0,
      $or: [
        { name: { $regex: ".*" + search + ".*", $options: "i" } },
        { email: { $regex: ".*" + search + ".*", $options: "i" } },
        { mobile: { $regex: ".*" + search + ".*", $options: "i" } },
      ],
    })
      .limit(limit)
      .skip((page - 1) * limit);

    const count = await User.find({
      is_admin: 0,
      $or: [
        { name: { $regex: ".*" + search + ".*", $options: "i" } },
        { email: { $regex: ".*" + search + ".*", $options: "i" } },
        { mobile: { $regex: ".*" + search + ".*", $options: "i" } },
      ],
    }).countDocuments();

    res.render("dashboard", {
      users: usersData,
      totalPages: Math.ceil(count / limit),
      currentPage: page,
      previousPage: page - 1,
      nextPage: page + 1,
    });
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

### 6.16 export user data in excel sheet

- `npm i exceljs`

```js
const exportUsers = async (req, res) => {
  try {
    const workbook = new excelJS.Workbook();
    const workSheet = workbook.addWorksheet("Users");
    workSheet.columns = [
      // { header: "S no.", key: "s_no" },
      { header: "Name", key: "name" },
      { header: "Email", key: "email" },
      { header: "Password", key: "password" },
      { header: "Image", key: "image" },
      { header: "Mobile", key: "mobile" },
      { header: "Image", key: "image" },
      { header: "Is Admin", key: "is_admin" },
      { header: "Is Verified", key: "is_verified" },
    ];

    let counter = 1;
    const userData = await User.find({ is_admin: 0 });
    userData.map((user) => {
      workSheet.addRow(user);
    });

    workSheet.getRow(1).eachCell((cell) => {
      cell.font = { bold: true };
    });

    res.setHeader(
      "Content-Type",
      "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
    );
    res.setHeader("Content-Disposition", `attachement; filename=user.xlsx`);
    return workbook.xlsx.write(res).then(() => {
      res.status(200);
    });
  } catch (error) {
    res.status(500).send({
      message: error.message,
    });
  }
};
```

### 6.17 export user data in pdf

```js

```

## 7. MERN Full-stack Ecommerce Website

### 7.0 best practices for rest api namings

When it comes to naming endpoints in a RESTful API, following best practices can enhance the clarity, maintainability, and consistency of your API design. While specific conventions may vary, here are some widely accepted best practices for naming REST API endpoints:

1. Use nouns to represent resources: Use plural nouns to represent resources in your API endpoints. For example, `/users` instead of `/user` for a collection of users.

2. Use hierarchical structure for nested resources: If your API has nested resources, represent them using a hierarchical structure in the endpoint naming. For example, `/users/{userId}/posts` represents the posts belonging to a specific user.

3. Use specific resource identifiers: Use specific identifiers to represent individual resources. For example, use `/users/{userId}` to retrieve a specific user by their unique identifier.

4. Use HTTP methods to indicate actions: Use appropriate HTTP methods (verbs) to indicate the action being performed on a resource. For example, use `GET` to retrieve a resource, `POST` to create a new resource, `PUT` to update a resource, and `DELETE` to delete a resource.

5. Avoid using verbs in endpoint URLs: Instead of including verbs in endpoint URLs, let the HTTP methods represent the action. For example, use `/users/{userId}` instead of `/getUser/{userId}`.

6. Use query parameters for filtering, sorting, and pagination: For filtering, sorting, or paginating resource collections, use query parameters. For example, `/users?role=admin` to filter users by role or `/users?limit=10&page=2` to retrieve a paginated list of users.

7. Be consistent with naming conventions: Establish consistent naming conventions throughout your API. This promotes predictability and simplifies API consumption for clients. For example, if you use `snake_case` for naming resources (`/user_roles`) or `kebab-case` (`/user-roles`), ensure consistency across all endpoints.

8. Use versioning for APIs: If you anticipate making breaking changes to your API in the future, consider incorporating versioning in the URL. For example, `/v1/users` or `/v2/users` to distinguish between different versions of the API.

9. Keep endpoints concise and meaningful: Use descriptive and concise endpoint names that convey the purpose of the operation. Prioritize clarity and readability over brevity.

Remember that these are general best practices, and you should adapt them to fit the specific requirements and conventions of your project. It's also a good practice to document your API endpoints and provide clear documentation to help other developers understand how to interact with your API effectively.

userRouter.get('/:id([0-9a-fA-F]{24})', isLoggedIn, getUserById);

### 7.1 Project planning (Editable)

- /test -> health check (D)

  - setup morgan
  - create responseHandler - errorResponse, successResponse
  - handle http errors
  - test from Postman

- /seed -> seeding some data (D)

  - crate dummy data
  - store in database

- /api/users (D)

  - POST /register -> create the user account (D)
    - get multi-part form data from the request body using multer
    - input validation check -> presence, image size, user exist
    - password hashing with bcrypt
    - create a jwt for storing user data temporarily
    - send email with nodemailer (SMPTP gmail username, password)
  - POST /activate -> activate the user account (D)
    - get the jwt from request
    - check existance of jwt
    - verify the jwt & decode the data
    - create & save the new user
  - GET /profile -> get the user account (D)
    - get the id from request body
    - findById()
    - send response based on user found or not
    - handle the mongoose Cast error
  - DELETE /:id -> delete the user account (D)
    - get the id from request body
    - findById(id)
    - if found delete the image from the server folder
    - findByIdAndDelete(id)
    - clear the cookies
    - send response
  - PUT /:id -> update the user account (D)
    - get the data from request body and params
    - create filter, updates, options
    - check image exist -> image size -> change updates
    - findByIdAndUpdate(filter, updates, options)
    - if user was updated then send response
  - PUT /update-password/:id(D) -> update the password
    - create the express-validator for validating user inputs
    - find user by email; not found response
    - check password match or not
    - update user password & send response
  - POST /forget-password (D) -> forget the password
    - click on forget password option in login page
    - load forget password page - a form where user will give their email & make request to backend for verifying the email
  - PUT /reset-password (D) -> reset the password
    - click the email for forget password
    - if user is verified based on token then show the reset-password page in front-end
    - provide new password and update the password in database
  - PUT - Admin /ban/:id (D) -> ban the user
  - PUT - Admin /unban/:id (D) -> unban the user
  - GET - Admin /export-users (D) -> export all the users
  - GET - Admin - /all-users -> get all users including search & pagination (D)
    - get data from request body
    - search users using regex
    - include pagination
    - send response

- /api/auth (JWT Auth)

  - POST /login -> isLoggedOut -> user login (D)
    - middlewares: validateUserLogin, runValidation using express-validator, isLoggedOut
    - extract request body
    - check user's existance
    - compare the password & return response
    - check user is banned & return response
    - create jwt token with an expiry time
    - create http only cookie with less time
  - POST /logout -> isLoggedIn -> user logout (D)
    - clear the cookie
    - send the response
  - GET /refresh -> get refresh token (D)
    - get old access token from cookie
    - verify old token
    - if verified - clear exisitng cookie, create refresh token (new token), cookie, return refresh token

- Middleware

  - isLoggedIn (D)
  - isLoggedOut (D)
  - isAdmin (D)
  - uploadFile (D)
  - getRefreshToken
  - userValidation (D)
  - runValidation (D)

- /api/categories (CRUD)

  - POST / -> create the category (Admin)
  - GET / -> get all the categories (Admin)
  - GET /:id -> get single category (Admin)
  - POST / -> create a category (Admin)
  - DELETE /:id -> delete a category (Admin)
  - PUT /:id -> update a category (Admin)

- /api/products (CRUD)

  - POST / -> create the product (Admin)
  - GET / -> get all the products
  - GET /:id -> get single product
  - POST / -> create a blog (Admin)
  - DELETE /:id -> delete a product (Admin)
  - PUT /:id -> update a product (Admin)

- /api/orders (CRUD)

  - POST / -> create the order (User/Admin)
  - GET / -> get the order (User/Admin)
  - GET /all-orders -> get all the orders (Admin)
  - DELETE /:id -> delete an order (Admin)
  - PUT /:id -> update an order (Admin)

- /api/payment

  - GET /token -> get the payment token (User/Admin)
  - POST /process-payment -> process the payment (User/Admin)

- package that we will need
  `npm install express cors http-errors multer body-parser bcrypt jsonwebtoken nodemailer cookie-parser`
  `npm install --save-dev morgan nodemon`

### 7.2 Bootstrap the Project

- create frontend and backend folders for the project. Inside the backend folder create the src folder (create src/indexindex.ts for server) and there create the following folders:  `mkdir models controllers routes config middlewares public`
- Environment setup:
  - download & install VSCode editor
  - install node and check version

    - check: node -v
    - check: npm -v

  - install nodemon globally

    - npm install -g nodemon
    - check: nodemon -v

  - install typescript globally

    - npm install -g typescript
    - check: tsc -v

  - install ts-node globally

    - npm install -g ts-node
    - check: ts-node -v
  - download & install insomnia/postman/ThunderClient extension
- initialize npm and install packages

  ```txt

    // Install Express, TypeScript, and other necessary dependencies

    npm install --save-dev typescript ts-node @types/node @types/express

    ts-node: This is a TypeScript execution environment and REPL for Node.js. It allows you to run TypeScript code directly without the need to transpile it to JavaScript first.

    @types/express and @types/node: These are TypeScript type definitions for the Express and Node.js libraries. They provide TypeScript with information about the types and structures used in these libraries.

    // Install nodemon as a development dependency

    npm install nodemon --save-dev
  ```
  
- add gitignore, README.md file for node
- Configure typescript: create tsconfig.json in the root directory. initialize tsconfig.json the typescript manager: tsc --init and change the target as ES2017 in tsconfig.json
ts

  ```json
  // tsconfig.json

  {
    "compilerOptions": {
      "target": "es6",
      "module": "commonjs",
      "outDir": "./dist",
      "rootDir": "./src",
      "strict": true,
      "esModuleInterop": true
    },
    "include": ["src/**/*.ts"],
    "exclude": ["node_modules"]
  }
  ```

- update the package.json

  ```json
    "scripts": {
      "start": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/index.ts",
      "build": "tsc",
      "dev": "npm run start",
      "prod": "node dist/index.js"
      }
  ```

- `tsc -w` If you want tsc (TypeScript compiler) to watch for changes and recompile your TypeScript files automatically,

### 7.3 Create and run the basic server

- `npm install express`

 ```js
  import express from 'express';

  const app = express();

  app.listen(3001, () => {
    console.log(`server listening on http://localhost:3001`);
  });
  ```

### 7.4 handle http requests and responses

```ts
// index.ts
app.get('/test', (req: Request, res: Response) => {
  res.status(200).json({
    message: 'api is working ',
  });
});
```

### 7.5 nodemon and morgan package

- install nodemon and setup the script
- update the package.json

  ```json
    "scripts": {
      "start": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/index.ts",
      "build": "tsc",
      "dev": "npm run start",
      "prod": "node dist/index.js"
      }
  ```

```ts
// npm i --save-dev morgan @types/morgan

// index.ts
import morgan from 'morgan';

app.use(morgan('dev'));
```

### 7.6 API Testing

- for api testing - Postman, Insomnia, ThunderClient, Advanced REST client chorme extension, CURL -> run the system: curl <http://localhost:3000/> --include

  ```js
  // curl example
  curl -X GET https://example.com/users/123

  curl -X POST -H "Content-Type: application/json" -d '{"name":"John", "email":"john@example.com"}' https://example.com/users

  // Update (PUT or PATCH): These operations are used to update a resource. PUT is typically used when you're updating the entire resource, while PATCH is used when you're updating only some attributes of the resource.
  curl -X PUT -H "Content-Type: application/json" -d '{"name":"Jane", "email":"jane@example.com"}' https://example.com/users/123

  curl -X PATCH -H "Content-Type: application/json" -d '{"email":"jane.new@example.com"}' https://example.com/users/123

  curl -X DELETE https://example.com/users/123
  ```
- open thunderclient and create a collection -> add the test request and test response

### 7.7 Middleware

- middleware concept
  - A function that has request, response and next function
  - Execute any code
  - Make changes to the request and the response objects
  - End the request-response cycle
  - Call the next middleware function in the stack

```js
// 3rd party middleware function: morgan, body-parser
// router middleware function
// error level middleware function
// app level middleware function
// built-in middleware function
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

const isLoggedIn = (req: Request, res: Response, next: NextFunction) => {
  const login = true;
  if (login) {
    req.body.user = { id: 101 };
    next();
  } else {
    return res.status(401).json({ message: 'please login first' });
  }
};

app.get('/api/user', isLoggedIn, (req, res) => {
  console.log(req.body.user);
  res.status(200).send({
    message: 'user profile is returned',
  });
});
```

- types of middlware

### 7.8 Add Error Middleware

- Here are some common HTTP status codes and when they should be used:

```txt
      200 OK: The request has succeeded. This is the most common status code for a successful response.

      201 Created: The request has been fulfilled and a new resource has been created.

      204 No Content: The request has succeeded but there is no content to send back.

      400 Bad Request: The request was malformed or invalid.

      401 Unauthorized: The request requires authentication, and either the user has not provided valid authentication credentials or has not authenticated at all.

      403 Forbidden: The request is valid, but the server refuses to respond to it. This typically means that the user does not have sufficient permissions to access the resource.

      404 Not Found: The requested resource could not be found.

      405 Method Not Allowed: The request method is not supported for the requested resource.

      409 Conflict: The request could not be completed due to a conflict with the current state of the resource.

      500 Internal Server Error: The server encountered an unexpected condition that prevented it from fulfilling the request.
```

- define the types for Error

```ts
//types.ts
export type Error = {
  status?: number;
  message?: string;
};

```

- add error handling middleware

```ts
// install http-errors
// index.js
// Handle 404 errors
// const createError = require('http-errors');

// app.use((req, res, next) => {
//   next(createError(404, 'not found'));
// });

// all the errors will come here in the end from all the routes
// app.use((err, req, res, next) => {
//   return res.status(err.status || 500).send({
//     success: false,
//     message: err.message,
//   });
// });

// handle client error using middleware
app.use((req: Request, res: Response, next: NextFunction) => {
  const error: Error = new Error();
  error.status = 404;
  error.message = 'route not found';
  next(error);
});


// handle server error using middleware
// from anywhere we can say next and it will come here
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  res.status(err.status || 500).json({
    message: err.message || 'Internal Server Error',
  });
});
```

- now lets create a middleware called errorHandler

```ts
// middlewares/errorHandler.ts
import { NextFunction, Request, Response } from 'express';

import { Error } from '../types';

export const errorHandler = (err: Error, req: Request, res: Response, next: NextFunction)=>{
  res.status(err.status || 500).json({
    message: err.message || 'Internal Server Error',
  });
}

// index.ts
// handle client error using middleware
app.use((req: Request, res: Response, next: NextFunction) => {
  const error: Error = new Error();
  error.status = 404;
  error.message = 'route not found';
  next(error);
});

// handle server error using middleware
app.use(errorHandler);
```

### 7.9 Basic API Security

- `npm i express-rate-limit`

```ts
// index.ts
import rateLimit from 'express-rate-limit';
const rateLimiter = rateLimit({
  windowMs: 1 * 60 * 1000, // 15 minutes
  max: 5, // limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP, please try again after a minute',
});

app.use(rateLimiter);
```

### 7.10 Add winston logger

- create a loggerController.ss file inside controller file and install `npm i winston`

```ts
import { createLogger, transports, format } from 'winston';

export const userLogger = createLogger({
  level: 'info',
  format: format.combine(
    format.timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }),
    format.json()
  ),
  transports: [
    new transports.File({
      filename: 'logs/user-info.log',
      level: 'info',
      maxsize: 5242880, // 5MB
      maxFiles: 5,
      tailable: true,
    }),
    new transports.File({
      filename: 'logs/user-error.log',
      level: 'error',
      maxsize: 5242880, // 5MB
      maxFiles: 5,
      tailable: true,
    }),
    new transports.Console({
      format: format.combine(
        format.colorize(),
        format.simple(),
        format.prettyPrint()
      ),
    }),
  ],
});

// Now use it from another file
userLogger.log('error', 'error finding the users');
userLogger.log('info', 'Successfully returned all the users');

explanation:

1. The `level` option is added to specify the default logging level as `info`.
2. The `format.timestamp` now accepts a `format` option to customize the timestamp format.
3. The log files are stored in a `logs` directory (create the directory if it doesn't exist) for better organization.
4. The `maxsize` option is set to limit the file size to 5MB (`5242880` bytes). When the limit is reached, a new log file will be created with a numerical suffix.
5. The `maxFiles` option is set to keep up to 5 log files. Older log files will be rotated and eventually deleted.
6. The `tailable` option is set to `true` to allow the logger to open new files in case the log files are rotated or deleted.
7. A new `transports.Console` transport is added to log messages to the console in a colored and simple format for development/debugging purposes.

Remember to create the `logs` directory in the same location as the logger file before running your application.
```

### 7.11 configure env variables

- install dotenv package
- create .env in root directory

  ```ts
  // .env
  MONGO_URL=mongodb+srv://oss:anisul1234@oss.6kg37.mongodb.net/umsDB
  SERVER_PORT=3001


  // config/development/index.js
  require('dotenv').config();

  export const dev = {
    app: {
      port: process.env.SERVER_PORT || 3001,
    },
    db: {},
  };

  // index.ts
  import { dev } from './config';

  const app = express();

  app.listen(dev.app.port, () => {
    console.log(`server listening on http://localhost:3001`);
  });
  ```

### 7.12 MVC Architecture

```ts
// index.ts
import userRouter from './routes/userRoute';
app.use('/api/users', userRouter);

// userModel.ts
export const users = [
  { id: '1', name: 'alex' },
  { id: '2', name: 'rob' },
  { id: '3', name: 'lora' },
];

// userRoutes.ts
import { Router } from 'express';

import { getAllUsers } from '../controllers/userController';

const router = Router();

router.get('/', getAllUsers);

export default router;

// userController.ts
import { Request, Response, NextFunction } from 'express';
import { users } from '../models/userModel';

export const getAllUsers = (req: Request, res: Response) => {
  res.status(200).json({
    message: 'all users are returented',
    payload: users,
  });
};
```

### 7.13 Connect To MongoDB

- download and install MongoDB and mongodb compass
- connect to mongodb atlas

  ```ts
  // Add env variable
  DATABASE_URL=mongodb://127.0.0.1:27017/ecommerce-db-november

  // connecting to database
  import mongoose from 'mongoose';

  import { dev } from '.';

  export const connectDatabase = async (options = {}) => {
    try {
      await mongoose.connect(dev.db.databaseUrl, options);
      console.log('Connection to DB is successfully established!');

      // Tips: after the successful connection if something happend wrong
      mongoose.connection.on('error', (error) => {
        console.error('DB connection error:', error);
      });
    } catch (error) {
      console.error('Could not connect to DB:', error);
    }
  };

  // index.ts
  app.listen(dev.app.port, () => {
    console.log(`server listening on http://localhost:3001`);
    connectDatabase();
  });

  ```

### 7.14 User API

#### Create User Model and Schema

- define env variable `DEFAULT_USER_IMAGE_PATH=public/images/users` and use it from config

  ```ts
  require('dotenv').config();

  export const dev = {
    app: {
      port: process.env.SERVER_PORT || 3001,
      imagePath:
        process.env.DEFAULT_USER_IMAGE_PATH || 'public/images/users/defaul.png',
    },
    db: {
      databaseUrl:
        process.env.DATABASE_URL ||
        'mongodb://127.0.0.1:27017/ecommerce-db-november',
    },
  };

  ```

- install bcrypt for encoding the password: `npm i bcrypt @types/bcrypt`

- create model and schema

```ts
import { Schema, model, Document } from 'mongoose';
import bcrypt from 'bcrypt';

import { dev } from '../config';

interface IUser extends Document {
  name: string;
  email: string;
  password: string;
  image: string;
  address: string;
  phone: string;
  isAdmin: boolean;
  isBanned: boolean;
}

const userSchema = new Schema<IUser>(
  {
    name: {
      type: String,
      required: [true, 'User name is required'],
      trim: true,
      minlength: [3, 'User name must be at least 3 characters'],
      maxlength: [31, 'User name maximum can be 31 characters'],
    },
    email: {
      type: String,
      required: [true, 'User email is required'],
      unique: true,
      trim: true,
      lowercase: true,
      validate: {
        validator: function (v: string) {
          return /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(v);
        },
        message: 'Please enter a valid email',
      },
    },
    password: {
      type: String,
      required: [true, 'User password is required'],
      minlength: [6, 'User password must be at least 6 characters'],
      set: (value: string) => bcrypt.hashSync(value, bcrypt.genSaltSync(10)),
    },
    image: {
      type: String,
      default: dev.app.imagePath,
    },
    address: {
      type: String,
      required: [true, 'User address is required'],
      trim: true,
      minlength: [3, 'User address must be at least 3 characters'],
    },
    phone: {
      type: String,
      required: [true, 'User phone is required'],
    },
    isAdmin: {
      type: Boolean,
      default: false,
    },
    isBanned: {
      type: Boolean,
      default: false,
    },
  },
  { timestamps: true } // will add 2 new fields - createdAt, updatedAt
);

const User = model<IUser>('Users', userSchema);
export default User;
```

#### add seed route for testing (move seed categories and products from here)

- create the model first then you will be able to seed data for users

```js
// data.ts
const data = {
  users: [
    {
      name: 'anisul islam',
      email: 'anisul2010s@yahoo.co.uk',
      password: '123456',
      phone: '04055566',
      address: 'sylhet, bangladesh',
      image: 'default.png',
    },
    {
      name: 'david warren',
      email: 'david@gmail.com',
      password: '123456',
      phone: '04055566',
      address: 'Dhaka, bangladesh',
      image: 'default.png',
    },
  ],
  categories: [
    {
      name: 'apple phone',
      slug: 'apple-phone',
    },
    {
      name: 'samsung phone',
      slug: 'samsung-phone',
    },
    {
      name: 'motorola phone',
      slug: 'motorola-phone',
    },
  ],
  products: [
    {
      name: 'iPhone 11',
      slug: 'iphone-11',
      description:
        'Apple iPhone 11 suits even the most demanding taste - it has just the right amount of everything. The top-quality dual camera system guarantees successful photos in all conditions, and the all-day battery ensures that the phone is with you even during a long day. Apple iPhone 11 is always ready to use in a moment.',
      price: 700.5,
      image: 'iphone11.jpg',
      sold: 20,
      quantity: 80,
      shipping: 0,
      category: '64525e7c5bd7bcb4be62efae',
    },
    {
      name: 'Samsung Galaxy A13',
      slug: 'samsung-galaxy-a13',
      description:
        'The Samsung Galaxy A13 is suitable for you who want a smart phone with smart features - at an affordable price. The slim, easy-to-use design and vibrant, sharp display also make the Galaxy A13 the perfect first phone for younger users. Five cameras, including a very sharp main camera, capture everything that comes your way. The ultra-wide angle captures a larger part of the world, and you can capture the smallest details with the sharp main camera. The Galaxy A13 has a powerful battery inside that can be charged quickly and lasts up to two days. If you need a large number of photos and videos, you can expand the storage space up to 1 TB with a microSD card. The Galaxy A13 opens safely and conveniently with the fingerprint sensor. Samsung makes everyday life a little safer.',
      price: 600.5,
      image: 'samsung a13.jpeg',
      sold: 20,
      quantity: 180,
      shipping: 10,
      category: '64525e7c5bd7bcb4be62efaf',
    },
    {
      name: 'IPhone 13',
      slug: 'iphone-13',
      description:
        'The Iphone 13 is suitable for you who want a smart phone with smart features - at an affordable price. The slim, easy-to-use design and vibrant, sharp display also make the Galaxy A13 the perfect first phone for younger users. Five cameras, including a very sharp main camera, capture everything that comes your way. The ultra-wide angle captures a larger part of the world, and you can capture the smallest details with the sharp main camera. The Galaxy A13 has a powerful battery inside that can be charged quickly and lasts up to two days. If you need a large number of photos and videos, you can expand the storage space up to 1 TB with a microSD card. The Galaxy A13 opens safely and conveniently with the fingerprint sensor. Samsung makes everyday life a little safer.',
      price: 1500.5,
      image: 'iphone13.jpeg',
      sold: 15,
      quantity: 50,
      shipping: 5,
      category: '64525e7c5bd7bcb4be62efae',
    },
    {
      name: 'Samsung Galaxy flip',
      slug: 'samsung-galaxy-flip',
      description:
        'The Samsung Galaxy A13 is suitable for you who want a smart phone with smart features - at an affordable price. The slim, easy-to-use design and vibrant, sharp display also make the Galaxy A13 the perfect first phone for younger users. Five cameras, including a very sharp main camera, capture everything that comes your way. The ultra-wide angle captures a larger part of the world, and you can capture the smallest details with the sharp main camera. The Galaxy A13 has a powerful battery inside that can be charged quickly and lasts up to two days. If you need a large number of photos and videos, you can expand the storage space up to 1 TB with a microSD card. The Galaxy A13 opens safely and conveniently with the fingerprint sensor. Samsung makes everyday life a little safer.',
      price: 1800.5,
      image: 'samsung_galaxy_flip.jpeg',
      sold: 18,
      quantity: 200,
      shipping: 0,
      category: '64525e7c5bd7bcb4be62efaf',
    },
    {
      name: 'Samsung S22',
      slug: 'samsung-s22',
      description:
        'The Samsung Galaxy S22 is suitable for you who want a smart phone with smart features - at an affordable price. The slim, easy-to-use design and vibrant, sharp display also make the Galaxy A13 the perfect first phone for younger users. Five cameras, including a very sharp main camera, capture everything that comes your way. The ultra-wide angle captures a larger part of the world, and you can capture the smallest details with the sharp main camera. The Galaxy A13 has a powerful battery inside that can be charged quickly and lasts up to two days. If you need a large number of photos and videos, you can expand the storage space up to 1 TB with a microSD card. The Galaxy A13 opens safely and conveniently with the fingerprint sensor. Samsung makes everyday life a little safer.',
      price: 1200.5,
      image: 'samsung a13.jpeg',
      sold: 10,
      quantity: 90,
      shipping: 0,
      category: '64525e7c5bd7bcb4be62efaf',
    },
  ],
};

module.exports = data;

// routing
import { Router } from 'express';

import { seedUsers } from '../controllers/seedController';

const router = Router();

router.get('/users', seedUsers);

export default router;

// Import required modules
const data = require('../data');
const Category = require('../models/categoryModel');
const Product = require('../models/productModel');
const User = require('../models/userModel');
const { successResponse } = require('./responseController');

// Rename function to better describe its purpose and use camelCase for consistency
import { NextFunction, Request, Response } from 'express';

import User from '../models/userModel';
import { data } from '../data';

export const seedUsers = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    // Delete all existing users
    await User.deleteMany({});

    // Create new users
    const users = await User.insertMany(data.users);

    // Return a successful response with the created users
    return res.status(201).json({
      message: 'Users created',
      payload: users,
    });
  } catch (error) {
    // Forward the error to the error handling middleware
    next(error);
  }
};

const seedCategories = async (req, res, next) => {
  try {
    // Delete all existing users & products
    await Category.deleteMany({});

    // Create new categories
    const categories = await Category.insertMany(data.categories);

    // Return a successful response with the created users
    return successResponse(res, {
      statusCode: 201,
      message: 'categories are created successfully', // Improve message readability
      payload: { categories },
    });
  } catch (error) {
    // Forward the error to the error handling middleware
    next(error);
  }
};
const seedProducts = async (req, res, next) => {
  try {
    // Delete all existing users & products
    await Product.deleteMany({});

    // Create new products
    const products = await Product.insertMany(data.products);

    // Return a successful response with the created users
    return successResponse(res, {
      statusCode: 201,
      message: 'products are created successfully', // Improve message readability
      payload: { products },
    });
  } catch (error) {
    // Forward the error to the error handling middleware
    next(error);
  }
};

// Export the function
module.exports = { seedUsers, seedProducts, seedCategories };
```

#### GET => /api/users => isLoggedIn => isAdmin => get all users with pagination and filtering

```ts
// routes
import { Router } from 'express';

import {
  getAllUsers,
} from '../controllers/userController';

const router = Router();

router.get('/', getAllUsers);

export default router;

// userController.ts
// get all users with pagination and filtering
export const getAllUsers = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const search = req.query.search as string || '';
    const page = Number(req.query.page) || 1;
    const limit = Number(req.query.limit) || 4;

    const searchRegExp = new RegExp('.*' + search + '.*', 'i');
    const filter = {
      isAdmin: { $ne: true },
      $or: [
        { name: { $regex: searchRegExp } },
        { email: { $regex: searchRegExp } },
        { phone: { $regex: searchRegExp } },
      ],
    };

    const options = { password: 0 };
    const users = await User.find(filter, options)
      .limit(limit)
      .skip((page - 1) * limit);

    const count = await User.find(filter).countDocuments();

    if (!users) {
      const error: Error = new Error();
      error.status = 404;
      error.message = 'No users found';
      throw error;
    }

    return res.status(200).json({
      message: 'Returned all the users who are not admin',
      payload: {
        users: users,
        totalPages: Math.ceil(count / limit),
        currentPage: page,
        previousPage: page - 1 > 0 ? page - 1 : null,
        nextPage: page + 1 <= Math.ceil(count / limit) ? page + 1 : null,
      },
    });
  } catch (error) {
    next(error);
  }
};
```

##### create service to get users and adjust the controller

```ts
// services/userService.ts
import User from '../models/userModel';

export const findUsers = async (
  search: string,
  page: number,
  limit: number
) => {
  try {
    const searchRegExp = new RegExp('.*' + search + '.*', 'i');
    const filter = {
      isAdmin: { $ne: true },
      $or: [
        { name: { $regex: searchRegExp } },
        { email: { $regex: searchRegExp } },
        { phone: { $regex: searchRegExp } },
      ],
    };

    const options = { password: 0 };
    const users: IUser[] = await User.find(filter, options)
      .limit(limit)
      .skip((page - 1) * limit);

    if (!users || users.length === 0) {
      const error: Error = new Error();
      error.status = 404;
      error.message = 'No users found';
      throw error;
    }

    const totalUsers = await User.find().countDocuments();
    const count = await User.find(filter).countDocuments();

    return {
      users,
      totalPages: Math.ceil(count / limit),
      currentPage: page,
      previousPage: page - 1 > 0 ? page - 1 : null,
      nextPage: page + 1 <= Math.ceil(count / limit) ? page + 1 : null,
      totalUsers: totalUsers
    };
  } catch (error) {
    throw error;
  }
};

// now use this from the userController.ts
// get all users with pagination and filtering
import * as UserService from '../services/userService';
export const getAllUsers = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const search = String(req.query.search) || '';
    const page = Number(req.query.page) || 1;
    const limit = Number(req.query.limit) || 4;

    const result = await UserService.findUsers(search, page, limit);

    return res.status(200).json({
      message: 'Returned all the users who are not admin',
      payload: result,
    });
  } catch (error) {
    next(error);
  }
};
```

##### Create Success and httpError Handler to use everywhere

```ts
// controller/responseController.ts
import { Response } from 'express';
import { Error } from '../types';

export const successResponse = (
  response: Response,
  status = 200,
  message = 'Success',
  payload = {}
) => {
  return response.status(status).json({
    message: message,
    payload: payload,
  });
};

export const createHTTPError = (status: number, message: string) => {
  const error: Error = new Error();
  error.status = status;
  error.message = message;
  return error;
};


// controller/userController.ts
// get all users with pagination and filtering
export const getAllUsers = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const search = req.query.search as string || '';
    const page = Number(req.query.page) || 1;
    const limit = Number(req.query.limit) || 4;

    const result = await UserService.findUsers(search, page, limit);
    return successResponse(
      res,
      200,
      'Returned all the users who are not admin',
      result
    );
  } catch (error) {
    next(error);
  }
};
```

#### GET => /api/users/:id => isLoggedIn => get user by id (profile)

```ts
// routes 
  router.post('/activate', activateUserAccount);
  router.get('/', getAllUsers);
  router.get('/id([0-9a-fA-F]{24})', getUser);
  router.delete('id([0-9a-fA-F]{24})', deleteUser);
  router.put('/ban-user/:id([0-9a-fA-F]{24})', banUser);
  router.put('/unban-user/:id([0-9a-fA-F]{24})', unbanUser);
  router.post(
    '/process-register',
    uploadUserPicture.single('image'),
    processUserRegistration
  );

// service
export const findUserById = async (
  id: string,
  options = {}
): Promise<IUser> => {
  try {
    const user: IUser | null = await User.findById(id, options);
    if (!user) {
      throw createHTTPError(404, 'User not found');
    }
    return user;
  } catch (error) {
    throw error;
  }
};

// controller
// GET: /api/users/:id => get single user by id
export const getUser = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const id = req.params.id;
    const options = { password: 0, __v: 0 };
    const user = await UserService.findUserById(id, options);
    return successResponse(res, 200, 'Returned single user', user);
  } catch (error) {
    next(error);
  }
};
```

#### How to handle mongoose cast error

```ts
// service
export const findUserById = async (
  id: string,
  options = {}
): Promise<IUser> => {
  try {
    const user: IUser | null = await User.findById(id, options);
    if (!user) {
      throw createHTTPError(404, 'User not found');
    }
    return user;
  } catch (error) {
    if (error instanceof mongoose.Error.CastError) {
      throw createHTTPError(404, 'Invalid Id');
    }
    throw error;
  }
};
```

#### POST => /api/users/process-registration

![registration](./user-registration.excalidraw.png)

##### upload filews with nodemailer

- install multer `npm install multer @types/multer`
- create public/images/users folder outside src folder
- setup the .env variables for file filtering and configure

    ```ts
    //.en
    ALLOWED_FILE_TYPES=image/jpeg, image/png
    MAX_FILE_SIZE=2097152 # 2 MB

    // config
      maxFileSize: Number(process.env.MAX_FILE_SIZE) || 1024 * 1024 * 2,
      allowedFileTypes: process.env.ALLOWED_FILE_TYPES?.split(',') || [
        'image/jpg',
        'image/png',
        'image/jpeg',
      ],
    
    // middlewares/uploadFile.ts
    import { Request } from 'express';

    import multer, { FileFilterCallback } from 'multer';

    import { dev } from '../config';

    type DestinationCallback = (error: Error | null, destination: string) => void;
    type FileNameCallback = (error: Error | null, filename: string) => void;

    const fileFilter = (
      req: Request,
      file: Express.Multer.File,
      cb: FileFilterCallback
    ) => {
      if (!file.mimetype.startsWith('image/')) {
        return cb(new Error('file is not an image'));
      }

      if (!dev.app.allowedFileTypes.includes(file.mimetype)) {
        return cb(new Error('file type is not allowed'));
      }

      cb(null, true);
    };

    const productStorage = multer.diskStorage({
      destination: function (
        req: Request,
        file: Express.Multer.File,
        cb: DestinationCallback
      ): void {
        cb(null, 'public/images/products');
      },
      filename: function (
        req: Request,
        file: Express.Multer.File,
        cb: FileNameCallback
      ): void {
        cb(null, Date.now() + '-' + file.originalname);
      },
    });

    const userStorage = multer.diskStorage({
      destination: function (
        req: Request,
        file: Express.Multer.File,
        cb: DestinationCallback
      ): void {
        cb(null, 'public/images/users');
      },
      filename: function (
        req: Request,
        file: Express.Multer.File,
        cb: FileNameCallback
      ): void {
        cb(null, Date.now() + '-' + file.originalname);
      },
    });

    export const uploadProductPicture: multer.Multer = multer({
      storage: productStorage,
      limits: { fileSize: dev.app.maxFileSize },
      fileFilter: fileFilter,
    });

    export const uploadUserPicture: multer.Multer = multer({
      storage: userStorage,
      limits: { fileSize: dev.app.maxFileSize },
      fileFilter: fileFilter,
    });
  ```

- create the routes and controller

```ts
// routes
router.post(
  '/process-register',
  uploadUserPicture.single('image'),
  processUserRegistration
);
```

- create the controlle to store the data in a jsonwebtoken

```ts

// types.ts
export type UserType = {
  name: string;
  email: string;
  password: string;
  image?: string;
  address: string;
  phone: string;
  isAdmin?: boolean;
  isBanned?: boolean;
};

// controllers
import { Request, Response, NextFunction } from 'express';
import jwt from 'jsonwebtoken';
import bcrypt from 'bcrypt';

import User, { IUser } from '../models/userModel';
import { Error, UserType } from '../types';
import { dev } from '../config';
import * as UserService from '../services/userService';
import { createHTTPError, successResponse } from './responseHandler';

// POST => /api/users/process-register => process the registration
export const processUserRegistration = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    // step 1: get the data from request
    const { name, email, password, phone, address } = req.body;
    const image = req.file && req.file.path;

    // step 2: check the user already exists or not
    const userExists = await User.exists({ email: email });
    if (userExists) {
      throw createHTTPError(409, 'User already exists');
    }

    const tokenPayload: UserType = {
      name,
      email,
      password: bcrypt.hashSync(password, bcrypt.genSaltSync(10)),
      address,
      phone,
      // ...(image&& { image }),
    };

    if (image) {
      // Include image in the token payload if it is provided
      tokenPayload.image = image;
    }
    console.log(tokenPayload);
    // step 3: create a token for storing data temporairily
    const token = jwt.sign(
      tokenPayload,
      String(dev.app.jwtAccountActivationKey),
      { expiresIn: '10m' }
    );

   // Step 9: Respond to the client
    successResponse(
      res,
      200,
      `Please check your email (${email}) to complete the registration process.`,
      token
    );
  } catch (error) {
    next(error);
  }
};


// now lets create a helper function to create jsonwebtoken
import jwt from 'jsonwebtoken';

export const createJSONWebToken = (
  payload: object | null | undefined,
  secretKey: string,
  expiresIn: string
) => {
  if (!payload || Object.keys(payload).length === 0) {
    throw new Error('Payload must be a non-empty object');
  }
  if (typeof secretKey !== 'string' || secretKey === '') {
    throw new Error('Secret key must be a non-empty string');
  }
  try {
    const token = jwt.sign(payload, secretKey, { expiresIn });
    return token;
  } catch (error) {
    console.error('Failed to sign the JWT:', error);
    throw error;
  }
};


// now lets use the createJSONWebToken function inside the ccontroller
 // step 3: create a token for storing data temporairily
  const token = createJSONWebToken(tokenPayload,dev.app.jwtAccountActivationKey,'10m')
  
```

##### send email with nodemailer

- setup SMTP server: sending email for user email verification

  - before adding the user in database send an email to user for verification
  - user data will be stored in a token temporarily
  - when user click the verification link verify the user (update the is_verify property in database)
  - how to generate smtp password

  As of my last knowledge update in January 2022, Google does not use "app passwords" in the same way that some other services do. Instead, Google relies on the use of OAuth tokens for accessing Google services through third-party applications.

    If you are trying to set up an application or device to access your Google account, such as for email or calendar synchronization, you would typically generate an "App Password" through your Google Account settings. Here's a general guide on how to do this:

    1. **Go to your Google Account:**
      - Visit the Google Account settings page: [https://myaccount.google.com/](https://myaccount.google.com/).

    2. **Sign in:**
      - Sign in with the Google account for which you want to generate an App Password.

    3. **Security:**
      - In the left sidebar, click on "Security."

    4. **Signing in to Google:**
      - Scroll down to the "Signing in to Google" section.

    5. **App Passwords:**
      - Look for an option called "App Passwords." If you don't see this option, it might be because your account has 2-Step Verification enabled.

    6. **Enable 2-Step Verification (if needed):**
      - If you don't see "App Passwords" but have 2-Step Verification enabled, go back to the "Security" section and enable 2-Step Verification.

    7. **Generate App Password:**
      - Once 2-Step Verification is enabled, you should be able to access the "App Passwords" section.
      - Generate a new App Password. You may need to select the app or device you're using from a dropdown list.

    8. **Use the App Password:**
      - Copy the generated App Password and use it in the settings of the application or device where you're trying to set up access to your Google account.

    ```txt
    Go to https://security.google.com/settings/security/apppasswords and sign in to your account.
    Choose Mail from the list of available apps.
    Choose Other from the device list.
    Enter your Custom Name. You can put "website" in it.
    Click the Generate button.
    Copy and save the password to somewhere for future use.
    Click the Done button, and that’s it.
    ```

- setup SMTP environment variables

    ```js
        MONGO_URL=mongodb+srv://anisul:anisul1234@oss.6kg37.mongodb.net/userDB
        SERVER_PORT=8080
        JWT_ACCOUNT_ACTIVATION_KEY=12345AHUGGA_21@_123
        SMTP_USERNAME=anisul2010s@gmail.com
        SMTP_PASSWORD=xkgxjpoyecbxeioi
        CLIENT_URL=http://localhost:3000
    ```

- config `clientURL: process.env.CLIENT_URL || 'http://localhost:3000',`
- Prepare the email

    ```js
    // step 4: prepare email data including jwt token
    const emailData = {
      email,
      subject: 'Account Activation Email',
      html: `
        <h2> Hello ${name}! </h2>
        <p> Please click here to <a href="${dev.app.clientUrl}/api/users/activate/${token}" target="_blank">activate your account </a> </p>
        `, // html body
    };
    ```

- handleSendEmail Part

    ```ts
    // helpers/handleSendEmail.ts
    // Tips 1: Create the tarnsporter then use when sending email rather than creating transporter for each email
    // Tips 2: Don't mixed up async/await and a callback function with transport.sendMail
    // Tips 3: always use better logging and handle the error

    import nodemailer from 'nodemailer';

    import { dev } from '../config';

    const transporter = nodemailer.createTransport({
      host: 'smtp.gmail.com',
      port: 587,
      secure: false, // true for 465, false for other ports
      auth: {
        user: dev.app.smtpUsername, // generated ethereal user
        pass: dev.app.smtpPassword, // generated ethereal password
      },
    });

    type EmailDataType = {
      email: string;
      subject: string;
      html: string;
    };

    export const handleSendEmail = async (emailData: EmailDataType) => {
      try {
        const mailOptions = {
          from: dev.app.smtpUsername, // sender address
          to: emailData.email, // list of receivers
          subject: emailData.subject, // Subject line
          html: emailData.html, // html body
        };

        // send mail with defined transport object
        const info = await transporter.sendMail(mailOptions);
        console.log('Message sent: %s', info.response);
      } catch (error) {
        console.log('Error occurred while sending Email: ', error);
        throw error; // Propagate the error up to the caller
      }
    };

    ```

- send the email

  ```js
  // Step 3: Hash the password securely
    const hashedPassword = bcrypt.hashSync(password, bcrypt.genSaltSync(10));

    // Step 4: Prepare the token payload
    const tokenPayload: UserType = {
      name,
      email,
      password: hashedPassword,
      address,
      phone,
      // ...(image&& { image }),
    };

    if (image) {
      tokenPayload.image = image;
    }

    // Step 5: Create a token for storing data temporarily
    const token = createJSONWebToken(
      tokenPayload,
      dev.app.jwtAccountActivationKey,
      '10m'
    );

    // Step 6: Prepare email data including JWT token
    const emailData = {
      email,
      subject: 'Account Activation Email',
      html: `
        <h2> Hello ${name} ! </h2>
        <p> Please click here to <a href="${dev.app.clientURL}/api/users/activate/${token}" target="_blank"> activate your account </a> </p>
      `,
    };

    // Step 7: Send the email
    try {
      await handleSendEmail(emailData);
    } catch (emailError) {
      // Step 8: Handle email sending error
      console.error('Failed to send verification email:', emailError);
      throw createHTTPError(500, 'Failed to send verification email');
    }

    // Step 9: Respond to the client
    // Step 9: Respond to the client
    successResponse(
      res,
      200,
      `Please check your email (${email}) to complete the registration process.`,
      token
    );
  ```

#### Input Validation with express-validator

- install express-validator

```ts
// add the validation rules for registration
export const validateUserRegistration = [
  body('name')
    .trim()
    .notEmpty()
    .withMessage('Name is required. Enter your full name')
    .isLength({ min: 3, max: 31 })
    .withMessage('Name should be at least 3-31 characters long'),
  body('email')
    .trim()
    .notEmpty()
    .withMessage('Email is required. Enter your email address')
    .isEmail()
    .withMessage('Invalid email address'),
  body('password')
    .trim()
    .notEmpty()
    .withMessage('Password is required. Enter your password')
    .isLength({ min: 6 })
    .withMessage('Passqord should be at least 6 characters long')
    .matches(
      /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]+$/
    )
    .withMessage(
      'Password should contain at least one uppercase letter, one lowercase letter, one number, and one special character.'
    ),

  body('address')
    .trim()
    .notEmpty()
    .withMessage('Address is required. Enter your address')
    .isLength({ min: 3 })
    .withMessage('address should be at least 3 characters long'),
  body('phone')
    .trim()
    .notEmpty()
    .withMessage('Phone is required. Enter your phone number'),

  body('image').optional(),
];

// run the validators
import { NextFunction, Request, Response } from 'express';
import { validationResult } from 'express-validator';

const runValidation = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(422).send({
        message: errors.array()[0].msg,
      });
    }
    return next();
  } catch (error) {
    return next(error);
  }
};

export default runValidation;
```

- now setup the routes to use the validation

```ts
router.post(
  '/process-register',
  uploadUserPicture.single('image'),
  validateUserRegistration,
  runValidation,
  processUserRegistration
);

```

#### POST => /api/users/activate (req.body.token)-> activateUserAccount

- verify email for activating & store the user

  ```js
  // validator middleware
  const validateToken = [
    body('token').trim().notEmpty().withMessage('token is required'),
  ];
  ```

- create the controller

```ts
import jwt, { JsonWebTokenError, JwtPayload, TokenExpiredError } from 'jsonwebtoken';
// POST => /api/users/activate => complete the registration
export const activateUserAccount = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const token: string | undefined = req.body.token;

    if (!token) {
      throw createHTTPError(404, 'Token not found');
    }

    // const decoded = jwt.verify(
    //   token,
    //   dev.app.jwtAccountActivationKey
    // ) as JwtPayload;
    const decoded = jwt.verify(
      token,
      dev.app.jwtAccountActivationKey
    ) as UserType;
    if (!decoded) {
      throw createHTTPError(401, 'Unable to verify the user');
    }

    const userExists = await User.exists({ email: decoded.email });
    if (userExists) {
      throw createHTTPError(409, 'User already exists');
    }

    await User.create(decoded);

    return successResponse(res, 201, 'user is registered successfully');
  } catch (error) {
    if (
      error instanceof TokenExpiredError ||
      error instanceof JsonWebTokenError
    ) {
      const errorMessage =
        error instanceof TokenExpiredError
          ? 'Token has expired'
          : 'Invalid Token';
      throw createHTTPError(401, errorMessage);
    } else {
      next(error);
    }
  }
};

```

- create services findUserByEmail

```ts
export const findUserByEmail = async (email: string): Promise<IUser | null> => {
  const user = await User.findOne({ email });
  return user;
};
```

#### create a JWTErrorHandler and use it from controller

```ts
// helpers/jwtError.ts
import { TokenExpiredError, JsonWebTokenError } from 'jsonwebtoken';
import createHTTPError from 'http-errors';

const handleJWTError = (error: Error) => {
  if (
    error instanceof TokenExpiredError ||
    error instanceof JsonWebTokenError
  ) {
    const errorMessage =
      error instanceof TokenExpiredError
        ? 'Token has expired'
        : 'Invalid Token';

    // Log the error
    console.error(errorMessage, error);

    // Respond with a 401 Unauthorized status and the error message
    throw createHTTPError(401, errorMessage);
  }

  // If the error is not a JWT error, let it propagate
  throw error;
};

export default handleJWTError;

// service
export const activateUser = async (token: string | undefined) => {
  try {
    if (!token) {
      throw createHTTPError(404, 'Token not found');
    }

    const decoded = jwt.verify(
      token,
      dev.app.jwtAccountActivationKey
    ) as UserType;
    if (!decoded) {
      throw createHTTPError(401, 'Unable to verify the user');
    }

    const userExists = await User.exists({ email: decoded.email });
    if (userExists) {
      throw createHTTPError(409, 'User already exists');
    }

    await User.create(decoded);
  } catch (error) {
    handleJWTError(error as Error);
  }
};

// controller
// POST => /api/users/activate => active the user profile
export const activateUserAccount = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const token: string | undefined = req.body.token;
    await UserService.activateUser(token);
    return successResponse(res, 201, 'user is registered successfully');
  } catch (error) {
    handleJWTError(error as Error, req, res, next);
  }
};
```

#### DELETE => /api/users/:id => isLoggedIn => delete single user by id

```ts
// helpers/deleteImage.ts
import fs from 'fs/promises';

export const deleteImage = async (imagePath: string) => {
  try {
    await fs.access(imagePath);
    await fs.unlink(imagePath);
    console.log('User image was deleted');
  } catch (error) {
    console.error('User image does not exist or could not be deleted');
    // You may choose to throw an error here if needed
    throw error;
  }
};


// service

// controller
// DELETE: /api/users/:id => delete single user by id
export const deleteUser = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const user = await User.findByIdAndDelete(req.params.id);
    if (!user) {
      throw createHTTPError(404, 'User not found');
    }
    if (user && user.image) {
      await deleteImage(user.image);
    }
    return successResponse(res, 200, 'deleted single user');
  } catch (error) {
    next(error);
  }
};
```

#### PUT => /api/users/:id => isLoggedIn => update the user profile

```ts
// PUT => /api/users/:id => update the user

// route
router.put(
  '/:id([0-9a-fA-F]{24})',
  isLoggedIn,
  uploadUserPicture.single('image'),
  updateUser
);

// service
export const updateUserById = async (id: string, req: Request) => {
  try {
    const user = await findUserById(id);

    const allowedFields = ['name', 'password', 'phone', 'address', 'image'];
    const updates: Record<string, unknown> = {};

    for (const key in req.body) {
      if (allowedFields.includes(key)) {
        if (key === 'password') {
          // Hash the password before updating
          const hashedPassword = await bcrypt.hash(req.body[key], 10);
          updates[key] = hashedPassword;
        } else {
          updates[key] = req.body[key];
        }
      } else {
        throw createHTTPError(400, `${key} can not be updated`);
      }
    }

    const image = req.file?.path;
    if (image) {
      updates.image = image;
      user.image !== 'default.png' && deleteImage(user.image);
    }

    const updatedUserData = await User.findByIdAndUpdate(id, updates, {
      new: true,
    }).select('-password');

    if (!updatedUserData) {
      throw createHTTPError(400, 'User data could not be updated');
    }
    return updatedUserData;
  } catch (error) {
    if (error instanceof mongoose.Error.CastError) {
      throw createHTTPError(404, 'Invalid Id');
    }
    throw error;
  }
};

// controller
export const updateUser = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const id = req.params.id;
    const updatedUserData = await UserService.updateUserById(id, req);
    return successResponse(
      res,
      200,
      'user is updated successfully',
      updatedUserData
    );
  } catch (error) {
    next(handleJWTError(error as Error, req, res, next));
  }
};
```

#### PUT => /api/users/ban-user => isLoggedIn => isAdmin => ban an existing user

```ts
// service
export const banUserById = async (id: string, options = {}) => {
  try {
    const user = await User.findByIdAndUpdate(id, { isBanned: true }, options);

    if (!user) {
      throw createHTTPError(404, 'user with this id does not exist');
    }
  } catch (error) {
    if (error instanceof mongoose.Error.CastError) {
      throw createHTTPError(404, 'Invalid Id');
    }
    throw error;
  }
};

// controller
// PUT => isLoggedIn => isAdmin => /api/users/ban-user (Admin)
export const banUser = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const filter = req.params.id;
    const options = { new: true };
    await UserService.banUserById(filter, options);
    return successResponse(res, 200, 'user was banned successfully');
  } catch (error) {
    next(error);
  }
};
```

#### PUT => /api/users/unban-user => isLoggedIn => isAdmin => unban an existing user

```ts
// service
export const unbanUserById = async (id: string, options = {}) => {
  try {
    const user = await User.findByIdAndUpdate(id, { isBanned: false }, options);

    if (!user) {
      throw createHTTPError(404, 'user with this id does not exist');
    }
  } catch (error) {
    if (error instanceof mongoose.Error.CastError) {
      throw createHTTPError(404, 'Invalid Id');
    }
    throw error;
  }
};

// controller
// PUT => isLoggedIn => isAdmin => /api/users/unban-user (Admin)
export const unbanUser = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const filter = req.params.id;
    const update = { isAdmin: false };
    const options = { new: true };
    await UserService.banUserById(filter, options);
    return successResponse(res, 200, 'user was unbanned successfully');
  } catch (error) {
    next(error);
  }
};
```

#### PUT => /api/users/manage-user => isLoggedIn => isAdmin => user ban/unban combined

```ts
// service
export const handleUserActionById = async (id: string, action: string, options = {}) => {
  try {
    let update;
    let successMessage;
    if (action === 'ban') {
      update = { isBanned: true };
      successMessage = 'User was banned successfully';
    } else if (action === 'unban') {
      update = { isBanned: false };
      successMessage = 'User was unbanned successfully';
    } else {
      throw createHTTPError(400, 'Invalid action. Use "ban" or "unban"');
    }
    const user = await User.findByIdAndUpdate(id, update, options);
    if (!user) {
      throw createHTTPError(404, 'user with this id does not exist');
    }
    return successMessage;
  } catch (error) {
    if (error instanceof mongoose.Error.CastError) {
      throw createHTTPError(404, 'Invalid Id');
    }
    throw error;
  }
};

// controller
// PUT => /api/users/manage-user => isLoggedIn => isAdmin => ban/unban an existing user
export const manageUserStatus = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const filter = req.params.id;
    const action = req.body.action;
    const options = { new: true };
    const successMessage = await UserService.handleUserActionById(filter, action, options);
    return successResponse(res, 200, successMessage);
  } catch (error) {
    next(error);
  }
};

```

#### POST => /api/auth/login -> user login using JWT

- create the route, controller

```ts
// .env -> JWT_ACCESS_KEY=ajajajojajaj9a9a16861861_
// npm i cookie-parser @types/cookie-parser
// index.ts -> app.use(cookieParser());

router.post('/login', handleUserLogin);

// authController.ts
// POST => /api/auth/login => handle the user login

export const handleUserLogin = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    // email, password
    const { email, password } = req.body;

    // isExist
    const user = await User.findOne({ email: email });
    if (!user) {
      throw createHTTPError(
        404,
        'User not found with this email; please register first'
      );
    }

    // compare the password
    const isPasswordMatch = await bcrypt.compare(password, user.password);
    if (!isPasswordMatch) {
      throw createHTTPError(401, 'Password did not match');
    }

    // isBanned?
    if (user.isBanned) {
      throw createHTTPError(
        403,
        'User is banned. Please contact to the authority'
      );
    }

    // token, http only cookie
    const accessToken = createJSONWebToken(
      { _id: user.id },
      dev.app.jwtAccessKey,
      '10m'
    );
    res.cookie('access_token', accessToken, {
      maxAge: 15 * 60 * 1000, // 15 minutes
      httpOnly: true,
      // secure: true,
      sameSite: 'none',
    });

    return successResponse(res, 200, 'user is logged in successfully');
  } catch (error) {
    next(error);
  }
};

```

- add Input validation for login route

```ts
export const validateUserLogin = [
  body('email')
    .trim()
    .notEmpty()
    .withMessage('Email is required. Enter your email address')
    .isEmail()
    .withMessage('Invalid email address'),
  body('password')
    .trim()
    .notEmpty()
    .withMessage('Password is required. Enter your password')
    .isLength({ min: 6 })
    .withMessage('Passqord should be at least 6 characters long')
    .matches(
      /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]+$/
    )
    .withMessage(
      'Password should contain at least one uppercase letter, one lowercase letter, one number, and one special character.'
    ),
];
```

A **cookie** is a small piece of data stored on the client's browser. It's sent by the server to the browser and is included in subsequent requests to the same server. Cookies are often used to track user sessions, store user preferences, and perform other tasks related to user state.

An **HTTP cookie** is a specific type of cookie that is used with HTTP transactions. When a user visits a website, the server can send a set of cookies to the user's browser, and the browser stores them. The cookies are then sent back to the server with every subsequent request to the same domain. This allows the server to recognize the user, remember their preferences, and maintain session information.

HTTP cookies have several attributes, including:

1. **Name-Value Pair:** A unique identifier and its associated value.
2. **Domain:** The domain to which the cookie belongs. The cookie will be sent in requests to this domain and its subdomains.
3. **Path:** The specific path or directory on the server to which the cookie applies.
4. **Expires/Max-Age:** The date and time when the cookie will expire. After this point, the browser will no longer send the cookie in requests.
5. **Secure:** A flag indicating that the cookie should only be sent over secure, encrypted connections (HTTPS).
6. **HttpOnly:** A flag indicating that the cookie should not be accessed via JavaScript, enhancing security by preventing cross-site scripting (XSS) attacks.

Cookies play a crucial role in maintaining stateful interactions between clients and servers in web applications, enabling features like user authentication, shopping carts, and personalized user experiences.

#### POST => /api/auth/logout => handle the user logout

```ts
// POST => /api/auth/logout => handle the user logout
export const handleUserLogOut = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    // clear the cookie
    res.clearCookie('access_token');
    return successResponse(res, 200, 'user is logged out successfully');
  } catch (error) {
    next(error);
  }
};
```

#### Auth Middlewares -> isLoggedIn, isLoggedOut, isAdmin

- isLoggedIn Middleware & use it where it is needed

```ts
// auth.ts
import { NextFunction, Request, Response } from 'express';
import jwt, { JwtPayload, TokenExpiredError } from 'jsonwebtoken';

import { createHTTPError } from '../controllers/responseHandler';
import { dev } from '../config';
import User from '../models/userModel';

interface CustomRequest extends Request {
  userId?: string;
}

export const isLoggedIn = async (
  req: CustomRequest,
  res: Response,
  next: NextFunction
) => {
  try {
    const token = req.cookies.access_token;

    if (!token) {
      throw createHTTPError(401, 'You are not logged in');
    }

    const decoded = jwt.verify(token, dev.app.jwtAccessKey) as JwtPayload;

    if (!decoded) {
      throw createHTTPError(401, 'Invalid access token. Please log in again');
    }

    req.userId = decoded._id;

    return next();
  } catch (error) {
    if (error instanceof TokenExpiredError) {
      return next(
        createHTTPError(401, 'please login again as jwt token expired')
      );
    }
    next(error);
  }
};

```

- isLoggedOut Middleware & use it where it is needed

```ts
export const isLoggedOut = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const token = req.cookies.access_token;
    
    if (token) {
      try {
        const decoded = jwt.verify(token, dev.app.jwtAccessKey) as JwtPayload;
        if (decoded) {
          // Token is decoded, meaning the user is already logged in
          throw createHTTPError(400, 'You are already logged in');
        }
      } catch (error) {
        // If the error is a TokenExpiredError, it means the token is expired
        if (error instanceof TokenExpiredError) {
          // Move to the next middleware or controller
          return next();
        }
        // If it's any other error, handle it as needed
        throw error;
      }
    }
    
    // If token is not present, move to the next middleware or controller
    return next();
  } catch (error) {
    next(error);
  }
};
```

- isAdmin & use it where it is needed

```ts
export const isAdmin = async (
  req: CustomRequest,
  res: Response,
  next: NextFunction
) => {
  try {
    const user = await User.findById(req.userId);
    if (user?.isAdmin) {
      next();
    } else {
      throw createHTTPError(403, 'You are not admin.');
    }
  } catch (error) {
    next(error);
  }
};
```

#### PUT => /api/auth/update-password -> update password


- add route `router.put('/update-password', isLoggedIn, updateUserPassword);`

- add controller

```ts
// PUT => /api/users/update-password => update user password
export const handleUpdateUserPassword = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    // recieve email/username, oldPassword, newPassword, confrimPassword
    const { email, oldPassword, newPassword, confirmedPassword } = req.body;

    const user = await UserService.findUserByEmail(email);
    if (!user) {
      throw createHTTPError(404, 'user not found with this email');
    }

    if (newPassword !== confirmedPassword) {
      throw createHTTPError(400, 'New password and old password do not match');
    }

    const isPasswordMatched = await bcrypt.compare(oldPassword, user.password);
    if (!isPasswordMatched) {
      throw createHTTPError(400, 'Old password is not correct');
    }

    const filter = { email: email };
    const update = { $set: { password: bcrypt.hashSync(newPassword, 10) } };
    const options = { new: true };
    const updateUser = await User.findOneAndUpdate(
      filter,
      update,
      options
    ).select('-password');

    if (!updateUser) {
      throw createHTTPError(400, 'password reset failed');
    }

    return successResponse(
      res,
      200,
      'user password is updated successfully'
      // updatedUserData
    );
  } catch (error) {
    next(error);
  }
};
```

- add the input validation for updating password

```ts
export const validateUserPasswordUpdate = [
  body('oldPassword')
    .trim()
    .notEmpty()
    .withMessage('Old Password is required. Enter your old password')
    .isLength({ min: 6 })
    .withMessage('Old Passqord should be at least 6 characters long')
    .matches(
      /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]+$/
    )
    .withMessage(
      'Password should contain at least one uppercase letter, one lowercase letter, one number, and one special character.'
    ),
  body('newPassword')
    .trim()
    .notEmpty()
    .withMessage('new Password is required. Enter your old password')
    .isLength({ min: 6 })
    .withMessage('new Passqord should be at least 6 characters long')
    .matches(
      /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]+$/
    )
    .withMessage(
      'Password should contain at least one uppercase letter, one lowercase letter, one number, and one special character.'
    ),
  body('confirmedPassword')
    .trim()
    .notEmpty()
    .withMessage('confirmed Password is required. Enter your old password')
    .custom((value, { req }) => {
      if (value !== req.body.newPassword) {
        throw new Error('Password did not match');
      }
      return true;
    }),
];
```

#### POST => /api/auth/forget-password -> forget password

- overall procedure
  - POST /forget-password -> forget the password
    - click on forget password option in login page
    - load forget password page - a form where user will give their email & make request to backend for verifying the email
  - PUT /reset-password -> reset the password
- add .env variable and setup the config `JWT_RESET_PASSWORD_KEY=ajajajojajaj9a9a16861861_`
- add validation for forget password

```ts
export const validateUserForgetPassword = [
  body('email')
    .trim()
    .notEmpty()
    .withMessage('Email is required. Enter your email address')
    .isEmail()
    .withMessage('Invalid email address'),
];
```

- add routes
  
  ```ts
    router.post(
    '/forget-password',
    validateUserForgetPassword,
    runValidation,
    isLoggedOut,
    handleUserForgetPassword
    );
  ```

- add services & controller

```ts

// service
export const forgetPassword = async (email: string) => {
  try {
    const userData = await User.findOne({ email: email });

    if (!userData) {
      throw createHTTPError(
        404,
        'Email is incorrect or you have not verified your email address'
      );
    }

    // Step 5: Create a token for storing data temporarily
    const token = createJSONWebToken(
      { email: userData.email },
      dev.app.jwtResetPasswordKey,
      '30s'
    );

    const emailData = {
      email,
      subject: 'Reset Password Email',
      html: `
        <h2> Hello ${userData.name}! </h2>
        <p> Please click here to <a href="${dev.app.clientURL}/api/users/reset-password/${token}" target="_blank">Click here to Reset password </a> </p>
        `,
    };

    // Step 7: Send the email
    try {
      await handleSendEmail(emailData);
    } catch (emailError) {
      // Step 8: Handle email sending error
      console.error('Failed to send Reset password email:', emailError);
      throw createHTTPError(500, 'Failed to send verification email');
    }
    return token;
  } catch (error) {
    throw error;
  }
};

// controller
// step 1: email from req.body
// step 2: isUserExist with the email
// step 3: create a json web token
// step 4: send reset password link via email which will include the token
// POST => /api/auth/forget-password => handle forget password
// POST => /api/auth/forget-password => handle forget password
export const handleUserForgetPassword = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const { email } = req.body;
    const token = await forgetPassword(email);
    return successResponse(
      res,
      200,
      `Please check your email (${email}) to reset the password.`,
      token
    );
  } catch (error) {
    next(error);
  }
};
```

#### PUT => /api/auth/reset-password -> reset password

- add input validation for reset-password
  
  ```ts
  export const validateUserResetPassword = [
  body('token').trim().notEmpty().withMessage('Token is missing.'),
  body('password')
    .trim()
    .notEmpty()
    .withMessage('Password is required. Enter your password')
    .isLength({ min: 6 })
    .withMessage('Passqord should be at least 6 characters long')
    .matches(
      /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]+$/
    )
    .withMessage(
      'Password should contain at least one uppercase letter, one lowercase letter, one number, and one special character.'
    ),

  ];
  ```

- create the route
  
  ```ts
    router.put(
    '/reset-password',
    validateUserResetPassword,
    runValidation,
    isLoggedOut,
    handleUserResetPassword

  );
  ```

- create the service and controller

```ts
// service
export const resetPassword = async (
  token: string,
  password: string
): Promise<void> => {
  try {
    const decoded = jwt.verify(
      token,
      String(dev.app.jwtResetPasswordKey)
    ) as JwtPayload;

    if (!decoded) {
      throw createHTTPError(400, 'Invalid or expired token');
    }

    const filter = { email: decoded.email };
    const updates = { $set: { password: bcrypt.hashSync(password, 10) } };
    const options = { new: true };
    const updatedUser = await User.findOneAndUpdate(filter, updates, options);

    if (!updatedUser) throw createHTTPError(400, 'password reset failed');
  } catch (error) {
    handleJWTError(error as Error);
  }
};

// PUT ->  /api/auth/reset-password -> handle reset the password
// PUT ->  /api/auth/reset-password -> handle reset the password
export const handleUserResetPassword = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const { token, password } = req.body;
    const tokenData = await resetPassword(token, password);
    res.status(200).json({
      message: 'password reset successful',
      payload: tokenData,
    });
  } catch (error) {
    next(error);
  }
};

```

#### refresh token (needs to be done)

### 7.15 Category API (not completed)

#### Category Schema & Model

```ts
import { Schema, model, Document } from 'mongoose';

export interface ICategory extends Document {
  title: string;
  slug: string;
}

const categorySchema = new Schema<ICategory>(
  {
    title: {
      type: String,
      required: true,
      trim: true,
      minlength: [3, 'Category title must be at least 3 characters long'],
      maxlength: [31, 'Category title must be at most 100 characters'],
    },
    slug: {
      type: String,
      required: [true, 'Category slug is required'],
      unique: true,
      lowercase: true,
    },
  },
  { timestamps: true }
);

// create the model/collections
export const Category = model<ICategory>('Category', categorySchema);

```

#### Category Input Validation

```ts
// validators/category.ts
import { body } from 'express-validator';

export const validateCreateCategory = [
  body('title')
    .trim()
    .notEmpty()
    .withMessage('Category title is required')
    .isLength({ min: 3, max: 31 })
    .withMessage('title should be at least 3-31 characters long'),
];


```

#### Seed Category

```ts
// add new array to data.ts
categories: [
    {
      title: 'apple phone',
      slug: 'apple-phone',
    },
    {
      title: 'samsung phone',
      slug: 'samsung-phone',
    },
    {
      title: 'motorola phone',
      slug: 'motorola-phone',
    },
  ],

  // route
  router.get('/categories', seedCategories);

  // controller 
  export const seedCategories = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    // Delete all existing users
    await Category.deleteMany({});

    // Create new users
    const categories = await Category.insertMany(data.categories);

    // Return a successful response with the created users
    categoryLogger.log('info', 'Successfully returned all the categories');
    return res.status(201).json({
      message: 'categories created',
      payload: categories,
    });
  } catch (error) {
    // Forward the error to the error handling middleware
    next(error);
  }
};
```

#### POST => /api/categories => Create Category

- install the packages `npm i slugify`
- add routes and use it from server file

```ts
router.post('/', validateCreateCategory, runValidation, isLoggedIn, isAdmin, handleCreateCategory);
```

- add controller

```ts
// POST => /api/categories => create a new category
export const handleCreateCategory = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const { title } = req.body;
    const categoryExist = await Category.exists({ title: title });
    if (categoryExist) {
      const error = createHTTPError(
        409,
        'Category already exist with this title'
      );
      throw error;
    }

    const newCategory: ICategory = new Category({
      title: title,
      slug: slugify(String(title)),
    });

    await newCategory.save();
    return successResponse(res, 201, 'Category created successfully');
  } catch (error) {
    next(error);
  }
};
```

- add services and update code

```ts
// categoryService.ts
import slugify from 'slugify';

import { Category, ICategory } from '../models/categoryModel';
import { createHTTPError } from '../controllers/responseHandler';

export const createCategory = async (title: string): Promise<void> => {
  try {
    const categoryExist = await Category.exists({ title: title });

    if (categoryExist) {
      const error = createHTTPError(
        409,
        'Category already exists with this title'
      );
      throw error;
    }

    const newCategory: ICategory = new Category({
      title: title,
      slug: slugify(String(title)),
    });

    await newCategory.save();
  } catch (error) {
    throw error;
  }
};

// controller
// POST => /api/categories => create a new category
export const handleCreateCategory = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const { title } = req.body;
    await categoryService.createCategory(title);
    return successResponse(res, 201, 'Category created successfully');
  } catch (error) {
    next(error);
  }
};
```

#### GET => /api/categories => get all categories

- add route `router.get('/', isLoggedIn, isAdmin, getAllCategories);`

```ts
import { Request, Response, NextFunction } from 'express';

import { Category, ICategory } from '../models/categoryModel';
import { createHTTPError, successResponse } from './responseHandler';

export const getAllCategories = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const categories: ICategory[] = await Category.find({}).select('title slug').lean();
    if (categories.length === 0) {
      throw createHTTPError(404, 'No categories found');
    }
    return successResponse(res, 200, 'returned all categories', categories);
  } catch (error) {
    next(error);
  }
};
```

- add a service and use it from the controller

```ts
// categoryService.ts
export const getAllCategories = async (): Promise<ICategory[]> => {
  try {
    const categories: ICategory[] = await Category.find({})
      .select('title slug')
      .lean();

    if (categories.length === 0) {
      throw createHTTPError(404, 'No categories found');
    }

    return categories;
  } catch (error) {
    throw error;
  }
};

// controller
export const handleGetAllCategories = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const categories = await categoryService.getAllCategories();
    return successResponse(res, 200, 'Returned all categories', categories);
  } catch (error) {
    next(error);
  }
};
```

#### GET => /api/categories/:slug => get a category

```ts
// route
router.get('/:slug', isLoggedIn, isAdmin, handleGetCategory);

// service
export const getCategory = async (slug: string): Promise<ICategory> => {
  try {
    const category: ICategory | null = await Category.findOne({ slug: slug })
      .select('title slug')
      .lean();

    if (!category) {
      throw createHTTPError(404, 'No category found with this slug');
    }

    return category;
  } catch (error) {
    throw error;
  }
};


// controller
export const handleGetCategory = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const category = await categoryService.getCategory(req.params.slug);
    return successResponse(res, 200, 'Returned a category', category);
  } catch (error) {
    next(error);
  }
};
```

#### DELETE => /api/categories/:slug => delete a category

```ts
// route
router.delete('/:slug', isLoggedIn, isAdmin, handledDeleteCategory);

// service
export const deleteCategory = async (slug: string): Promise<void> => {
  try {
    const filter = { slug: slug };
    const category: ICategory | null = await Category.findOneAndDelete(filter)
      .select('title slug')
      .lean();

    if (!category) {
      throw createHTTPError(404, 'No category found with this slug');
    }
  } catch (error) {
    throw error;
  }
};


// controller
// DELETE => /api/categories/:slug => delete a category
export const handledDeleteCategory = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const slug = req.params.slug;

    await categoryService.deleteCategory(slug);
    return successResponse(res, 200, 'category is deleted successfully');
  } catch (error) {
    next(error);
  }
};
```

#### PUT => /api/categories/:slug => update a category

```ts
// input validation
export const validateUpdateCategory = [
  body('title')
    .trim()
    .notEmpty()
    .withMessage('Category title is required')
    .isLength({ min: 3, max: 31 })
    .withMessage('title should be at least 3-31 characters long'),
];


// route
router.put(
  '/:slug',
  validateUpdateCategory,
  runValidation,
  isLoggedIn,
  isAdmin,
  handleUpdateCategory
);

// service
export const updateCategory = async (
  slug: string,
  title: string
): Promise<ICategory> => {
  try {
    const filter = { slug: slug };
    const updateOptions = {
      title: title,
      slug: slugify(title),
    };
    const option = { new: true };
    const category: ICategory | null = await Category.findOneAndUpdate(
      filter,
      updateOptions,
      option
    )
      .select('title slug')
      .lean();

    if (!category) {
      throw createHTTPError(404, 'No category found with this slug');
    }

    return category;
  } catch (error) {
    throw error;
  }
};

// controller
// PUT => /api/categories/:slug => update a category
export const handleUpdateCategory = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const { title } = req.body;
    const slug = req.params.slug;

    const category = await categoryService.updateCategory(slug, title);
    return successResponse(
      res,
      200,
      'category is updated successfully',
      category
    );
  } catch (error) {
    next(error);
  }
};

```

### 7.16 Product API

- Planning

  - 1. create product schema and model

    - name, slug, description, price, quantity, sold, shipping

  - 2. create product routes
  - 3. create prodcut validation
  - 4. create product controllers for crud operations

#### Create Product Model and Schema

```ts
import { Schema, model, Document } from 'mongoose';

import { ICategory } from './categoryModel';

export interface IProduct extends Document {
  title: string;
  slug: string;
  price: number;
  image: string;
  quantity: number;
  sold: number;
  shipping: number;
  category: ICategory['_id'];
  description: string;
}

const productSchema = new Schema(
  {
    title: {
      type: String,
      unique: true,
      required: true,
      trim: true,
      minlength: [3, 'Product title must be at least 3 characters long'],
      maxlength: [300, 'Product title must be at most 300 characters'],
    },
    slug: {
      type: String,
      unique: true,
      lowercase: true,
    },
    price: {
      type: Number,
      required: true,
      trim: true,
    },
    quantity: {
      type: Number,
      required: true,
      trim: true,
    },
    image: {
      type: String,
      default: 'public/images/products/default.png',
    },
    sold: {
      type: Number,
      default: 0,
      trim: true,
    },
    shipping: {
      type: Number,
      default: 0,
    },
    category: { type: Schema.Types.ObjectId, ref: 'Category', required: true },
    description: {
      type: String,
      required: true,
      trim: true,
      minlength: [3, 'Product description must be at least 3 characters long'],
    },
  },
  { timestamps: true }
);

// create the model/collections
export const Product = model<IProduct>('Product', productSchema);

```

#### Seed Products (not done)

```ts
// add new array to data.ts
categories: [
    {
      title: 'apple phone',
      slug: 'apple-phone',
    },
    {
      title: 'samsung phone',
      slug: 'samsung-phone',
    },
    {
      title: 'motorola phone',
      slug: 'motorola-phone',
    },
  ],

  // route
  router.get('/categories', seedCategories);

  // controller 
  export const seedCategories = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    // Delete all existing users
    await Category.deleteMany({});

    // Create new users
    const categories = await Category.insertMany(data.categories);

    // Return a successful response with the created users
    categoryLogger.log('info', 'Successfully returned all the categories');
    return res.status(201).json({
      message: 'categories created',
      payload: categories,
    });
  } catch (error) {
    // Forward the error to the error handling middleware
    next(error);
  }
};
```

#### POST => /api/products => Create Product

- add input validation

```ts
// validators/product.ts
import { body } from 'express-validator';

export const validateCreateProduct = [
  body('title')
    .trim()
    .notEmpty()
    .withMessage('Product title is required.')
    .isLength({ min: 3, max: 300 })
    .withMessage('Product title should be at least 3-300 characters long'),
  body('description')
    .trim()
    .notEmpty()
    .withMessage('Description is required')
    .isLength({ min: 3 })
    .withMessage('Description should be at least 3 characters long'),
  body('price')
    .trim()
    .notEmpty()
    .withMessage('Price is required')
    .isFloat({ min: 0 })
    .withMessage('Price must be a positive number'),
  body('category').trim().notEmpty().withMessage('Category is required'),
  body('quantity')
    .trim()
    .notEmpty()
    .withMessage('Quantity is required')
    .isInt({ min: 1 })
    .withMessage('Quantity must be a positive integer'),
];

```

- add route, service and controller

```ts
// route
router.post(
  '/',
  isLoggedIn,
  isAdmin,
  uploadProductPicture.single('image'),
  validateCreateProduct,
  runValidation,
  handleCreateProduct
);

export default router;


// service
export const createProduct = async (productData: IProduct): Promise<void> => {
  try {
    const productExist = await Product.exists({ title: productData.title });
    if (productExist) {
      throw createHTTPError(409, 'Product already exist with this title');
    }
    const newProduct: IProduct = new Product(productData);
    await newProduct.save();
    console.log('Product created successfully:', newProduct);
  } catch (error) {
    console.error('Error in createProduct:', error);
    throw error;
  }
};

// controller
// POST : /products -> create product
export const handleCreateProduct = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    // if we have file then get the path
    const image = req.file && req.file.path;
    const productData: IProduct = {
      ...req.body,
      slug: slugify(req.body.title),
    };
    if (image) {
      console.log(image);
      productData.image = image;
    }
    await productService.createProduct(productData);
    res.status(201).send({ message: 'product is created' });
  } catch (error) {
    next(error);
  }
};

```

#### GET => /api/products => get all products

```ts
// routes
router.get('/', handleGetAllProducts);

// services
export const getProducts = async (
  page: number,
  limit: number,
  search: string
) => {
  try {
    const count = await Product.countDocuments();
    const totalPages = Math.ceil(count / limit);

    if (page > totalPages) {
      page = totalPages;
    }

    const searchRegExp = new RegExp('.*' + search + '.*', 'i');

    const filter = {
      $or: [
        { title: { $regex: searchRegExp } },
        { description: { $regex: searchRegExp } },
      ],
    };

    const options = {
      __v: 0,
      updatedAt: 0,
    };

    const skip = (page - 1) * limit;
    const products: IProduct[] = await Product.find(filter, options)
      .populate('category')
      .skip(skip)
      .limit(limit)
      .sort({ price: 1 });

    if (!products || products.length === 0) {
      throw createHTTPError(404, 'No products found');
    }

    return {
      products,
      totalPages: Math.ceil(count / limit),
      currentPage: page,
      previousPage: page - 1 > 0 ? page - 1 : null,
      nextPage: page + 1 <= Math.ceil(count / limit) ? page + 1 : null,
    };
  } catch (error) {
    console.error('Error in getProducts:', error);
    throw error;
  }
};

// controllers
// GET : /products -> get all the products
export const handleGetAllProducts = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    let page = Number(req.query.page) || 1;
    const limit = Number(req.query.limit) || 3;
    const search = (req.query.search as string) || '';

    const { products, totalPages, currentPage } = await getProducts(
      page,
      limit,
      search
    );

    res.send({
      message: 'get all products',
      payload: {
        products,
        totalPages,
        currentPage,
      },
    });
  } catch (error) {
    next(error);
  }
};
```

#### GET => /api/products/:slug => get a product

```ts
// route
router.get('/:slug', handleGetProduct);

// services
export const findProductBySlug = async (slug: string): Promise<IProduct> => {
  const product = await Product.findOne({ slug: slug }).populate('category');
  if (!product) {
    throw createHTTPError(404, 'Product not found with this slug');
  }
  return product;
};

// controller
// GET : /products/:slug -> get a product
export const handleGetProduct = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const product = await productService.findProductBySlug(req.params.slug);
    return successResponse(res, 200, 'product found', product);
  } catch (error) {
    next(error);
  }
};
```

#### PUT => /api/products/:slug => update a product

```ts
// route
router.put('/:slug', isLoggedIn, isAdmin, handleUpdateProduct);

// service
export const updateProduct = async (
  slug: string,
  updates: Record<string, unknown>,
  image?: string
): Promise<void> => {
  try {
    const product = await Product.findOne({ slug: slug });

    if (!product) {
      throw createHTTPError(404, 'Product not found');
    }

    // Perform your allowedFields validation here if needed

    if (image) {
      updates.image = image;
      product.image !== 'default.png' && deleteImage(product.image);
    }

    const updatedProductData = await Product.findOneAndUpdate(
      { slug: slug },
      updates,
      { new: true, runValidators: true, context: 'query' }
    );

    if (!updatedProductData) {
      throw createHTTPError(400, 'Product data could not be updated');
    }

    // Log or handle success if needed
    console.log('Product updated successfully:', updatedProductData);
  } catch (error) {
    // Log or handle errors if needed
    console.error('Error updating product:', error);
    throw error;
  }
};

// controller
// PUT : /products/:slug -> update a product
export const handleUpdateProduct = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const { slug } = req.params;
    const allowedFields = [
      'title',
      'description',
      'price',
      'sold',
      'quantity',
      'shipping',
    ];
    const updates: Record<string, unknown> = {};

    for (const key in req.body) {
      if (allowedFields.includes(key)) {
        updates[key] = req.body[key];
      } else {
        throw createHTTPError(400, `${key} can not be updated`);
      }
    }

    const image = req.file && req.file.path;

    await productService.updateProduct(slug, updates, image);

    return successResponse(res, 200, 'Product is updated successfully');
  } catch (error) {
    next(error);
  }
};
```

#### DELETE => /api/products/:slug => delete a product

```ts
// route
router.delete('/:slug', isLoggedIn, isAdmin, handleDeleteProduct);

// service
export const deleteProductBySlug = async (slug: string) => {
  try {
    const product = await Product.findOneAndDelete({
      slug: slug,
    });
    if (!product) {
      throw createHTTPError(404, 'Product not found with this slug');
    }
    product.image !== 'default.png' && deleteImage(product.image);
  } catch (error) {
    throw error;
  }
};

// controller
// DELETE : /products/:slug -> delete a product
export const handleDeleteProduct = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    await productService.deleteProductBySlug(req.params.slug);
    return successResponse(res, 200, 'product deleted successfully');
  } catch (error) {
    next(error);
  }
};

```

### 7.17 Order API (not completed)

#### Third-Party Payment Processors

Utilize third-party payment processors like Stripe, PayPal, or Braintree. These services provide APIs that can be integrated into your Node.js backend to securely handle payment transactions.
Advantages: Security, PCI compliance, and the ability to handle various payment methods.

The choice between Stripe and Braintree depends on various factors, including your specific business needs, technical requirements, and personal preferences. Both Stripe and Braintree are popular payment gateways, and each has its own strengths.

**Stripe:**

1. **Ease of Integration:** Stripe is known for its developer-friendly API and documentation, making it easy to integrate with various platforms and programming languages.

2. **International Support:** Stripe supports a wide range of countries and currencies, making it a good choice for businesses with a global presence.

3. **Feature-Rich:** Stripe offers a comprehensive set of features, including subscription billing, fraud prevention tools, and support for various payment methods, such as cards, digital wallets, and ACH transfers.

4. **Customization:** Businesses have more control over the payment process, allowing for a highly customized checkout experience.

**Braintree:**

1. **PayPal Integration:** Braintree is owned by PayPal, and it seamlessly integrates with PayPal, making it a good choice if you want to offer PayPal as a payment option.

2. **Subscription Management:** Braintree has robust subscription management features, making it a good choice for businesses with subscription-based models.

3. **Marketplace Payments:** Braintree provides features for marketplace payments, making it suitable for platforms that facilitate transactions between buyers and sellers.

4. **Mobile Payments:** Braintree is known for its strong support for mobile payments, making it suitable for businesses with a mobile focus.

**Considerations:**

1. **Fees:** Compare the transaction fees and pricing structures of both providers to determine which aligns better with your business model.

2. **Global Reach:** Consider the countries and currencies supported by each provider, especially if you have an international customer base.

3. **Development Resources:** Assess the technical skills and resources available to your development team, as the ease of integration may vary based on your team's familiarity with the respective APIs.

4. **Additional Features:** Evaluate additional features offered by each provider, such as reporting tools, fraud prevention, and support for emerging payment methods.

In summary, both Stripe and Braintree are reputable payment gateways, and the choice depends on your specific requirements and preferences. If ease of integration, customization, and international support are crucial, Stripe may be a strong contender. If you prioritize integration with PayPal, strong subscription management, or marketplace payments, Braintree could be a good fit.

#### Payment using stripe

To integrate Stripe payments into a MERN stack application, you'll need to set up both the frontend (React) and backend (Node.js with Express) components. Below is a simplified example using the `stripe` and `@stripe/stripe-js` packages.

##### Backend (Node.js with Express)

1. **Install Packages:**
   Install the required packages in your Node.js backend:

   ```bash
   npm install stripe express body-parser
   ```

2. **Create a Backend Endpoint:**
   Set up a backend endpoint to create a payment intent. This example assumes you already have a route for handling payments.

   ```javascript
   const express = require('express');
   const bodyParser = require('body-parser');
   const stripe = require('stripe')('your_stripe_secret_key');

   const app = express();
   const port = 3001;

   app.use(bodyParser.json());

   // Endpoint to create a payment intent
   app.post('/create-payment-intent', async (req, res) => {
     const { amount } = req.body;

     try {
       const paymentIntent = await stripe.paymentIntents.create({
         amount: amount,
         currency: 'usd',
       });

       res.json({ clientSecret: paymentIntent.client_secret });
     } catch (error) {
       console.error('Error creating payment intent:', error);
       res.status(500).send({ error: 'Failed to create payment intent' });
     }
   });

   app.listen(port, () => {
     console.log(`Server is running on port ${port}`);
   });
   ```

##### Frontend (React)

1. **Install Stripe JS:**
   Install the `@stripe/stripe-js` package in your React frontend:

   ```bash
   npm install @stripe/stripe-js
   ```

2. **Create a Payment Form Component:**
   Create a React component for your payment form. This example uses a simple form with an input for the payment amount.

   ```jsx
   import React, { useState } from 'react';
   import { CardElement, useStripe, useElements } from '@stripe/react-stripe-js';

   const PaymentForm = () => {
     const [amount, setAmount] = useState(100); // Set your default payment amount
     const stripe = useStripe();
     const elements = useElements();

     const handleSubmit = async (event) => {
       event.preventDefault();

       const { token, error } = await stripe.createToken(elements.getElement(CardElement));

       if (error) {
         console.error('Error creating token:', error);
       } else {
         // Send token to your backend for processing
         await handlePayment(token.id);
       }
     };

     const handlePayment = async (tokenId) => {
       const response = await fetch('http://localhost:3001/create-payment-intent', {
         method: 'POST',
         headers: {
           'Content-Type': 'application/json',
         },
         body: JSON.stringify({ amount: amount * 100 }), // Convert amount to cents
       });

       const { clientSecret } = await response.json();

       // Confirm the payment on the client side
       const { paymentIntent, error } = await stripe.confirmCardPayment(clientSecret, {
         payment_method: {
           card: elements.getElement(CardElement),
           billing_details: {
             name: 'John Doe', // Add billing details as needed
           },
         },
       });

       if (error) {
         console.error('Error confirming payment:', error);
       } else {
         console.log('Payment confirmed:', paymentIntent);
         // Handle success (e.g., show a success message to the user)
       }
     };

     return (
       <form onSubmit={handleSubmit}>
         <label>
           Amount (USD):
           <input
             type="number"
             value={amount}
             onChange={(e) => setAmount(e.target.value)}
           />
         </label>
         <CardElement />
         <button type="submit">Pay</button>
       </form>
     );
   };

   export default PaymentForm;
   ```

Make sure to replace `'your_stripe_secret_key'` with your actual Stripe secret key in the backend code. Additionally, handle the payment confirmation on the frontend according to your application's requirements.

This example uses the Payment Intents API and the Payment Element component for handling card input securely. Adjust the code based on your specific needs and consider adding error handling and additional features as required.

#### Create the Order Schema and Model

```ts
import { Schema, model, Document } from 'mongoose';

import { IUser } from './userModel';
import { IProduct } from './productModel';

export interface IOrderProduct {
  product: IProduct['_id'];
  quantity: number;
}

interface IOrderPayment {
  // Define the structure of the payment type
  // You need to replace {} with the actual structure of your payment type
}

export interface IOrder extends Document {
  products: IOrderProduct[];
  payment: IOrderPayment;
  buyer: IUser['_id'];
  status:
    | 'Not processed'
    | 'Processing'
    | 'Shipped'
    | 'Delivered'
    | 'Cancelled';
}

const orderSchema = new Schema<IOrder>(
  {
    products: [
      {
        product: {
          type: Schema.Types.ObjectId,
          ref: 'Product',
          required: true,
        },
        quantity: {
          type: Number,
          required: true,
        },
      },
    ],
    payment: {
      // Define the structure of the payment type
      // You need to replace {} with the actual structure of your payment type
      type: {},
      required: true,
    },
    buyer: {
      type: Schema.Types.ObjectId,
      ref: 'User',
      required: true,
    },
    status: {
      type: String,
      default: 'Not processed',
      required: true,
      enum: [
        'Not processed',
        'Processing',
        'Shipped',
        'Delivered',
        'Cancelled',
      ],
    },
  },
  { timestamps: true }
);

export const Order = model<IOrder>('Order', orderSchema);
```

#### Create the Order router

```ts
import { Router } from 'express';

import {
  getAllOrdersForAdmin,
  getOrderForUser,
  handleProcessPayment,
} from '../controllers/orderController';
import { isAdmin, isLoggedIn } from '../middlewares/auth';

const router = Router();

router.post('/process-payment', isLoggedIn, handleProcessPayment);
router.get('/:id([0-9a-fA-F]{24})', isLoggedIn, getOrderForUser);
router.get('/all-orders', isLoggedIn, isAdmin, getAllOrdersForAdmin);

export default router;
```

#### POST: /orders/process-payment -> process the payment and create the order (not complete yet)

```ts
import { NextFunction, Request, Response } from 'express';
import { successResponse } from './responseHandler';
import { IOrder, IOrderProduct, Order } from '../models/orderModel';

interface CustomRequest extends Request {
  userId?: string;
}

// we will send the cartItems here
// find the totalPrice of the cartItems
// process the payment - gateway transaction.sale
// if the payment is successfull then create a new order
// POST: /orders/process-payment -> process the payment
export const handleProcessPayment = async (
  req: CustomRequest,
  res: Response,
  next: NextFunction
) => {
  try {
    // payment: { method: string; amount: number },
    const { cartItems } = req.body;

    // find the totalPrice of the cartItems

    // Create the order
    const newOrder: IOrder = new Order({
      products:
        cartItems.products.length > 0 &&
        cartItems.products.map((item: IOrderProduct) => ({
          product: item.product,
          quantity: item.quantity,
        })),
      payment: {
        method: 'credit_card',
        amount: 100.0,
      },
      buyer: req.userId,
    });

    await newOrder.save();

    return successResponse(res, 200, 'Order is placed successfully');
  } catch (error) {
    next(error);
  }
};
```

#### GET -> /api/orders/:id -> get the orders for user (not completed yet)

```ts
// GET -> /api/orders/:id -> get the orders for user
export const getOrderForUser = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const order = await Order.find({ buyer: req.params.id })
      .populate({
        path: 'products',
        populate: {
          path: 'product',
          select: 'title price',
        },
      })
      .populate('buyer');
    // .populate('buyer', 'name');
    return successResponse(res, 200, 'returned the user orders', order);
  } catch (error) {
    next(error);
  }
};
```

#### GET -> /api/orders/all-orders -> get all the orders for admin (not completed yet)

```ts
// GET -> /api/orders/all-orders -> get the order for user
export const getAllOrdersForAdmin = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const orders = await Order.find()
      .populate({
        path: 'products',
        populate: {
          path: 'product',
          select: 'title price',
        },
      })
      .populate('buyer', 'name');

    return successResponse(res, 200, 'returned the user orders', orders);
  } catch (error) {
    next(error);
  }
};

// PUT -> /api/orders/:id -> update the order status

```