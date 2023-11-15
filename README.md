# Node, Express, MongoDB, API Documentation, Authentication Tutorial

- prerqusities: javascript, typescript

![profile2](./fs.excalidraw.png)

## Table of Contents

1. [1. Node.js](#1-nodejs)
2. [2. Express.js](#2-expressjs)
3. [3. MongoDB](#3-mongodb-tutorial)
4. [4. API Documentation](#4-api-documentaion-with-swagger)
5. [5. Authentication](#5-authentication-tutorial)

## 1. Node.js

### [1.1 Introduction to Node.js & setup](https://youtu.be/36R0VXmX8i8)

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

### [1.3 Node js event loop]

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

### [1.4 Module](https://youtu.be/n3F1kaOfyzw)

#### What is module? Types of module?

- Module is a set of functions or variables.
- we can use es6 import, export by adding "type":"module" in package.json
- when you have too much code in a single file you would like to separate them in multiple files so that they are reusable and modular.
- console.log(process) and find the module.exports = {}
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
      res.writeHead(200, {'Content-Type': 'text/plain'});

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

### [1.10 request, response and status code](https://youtu.be/lHfnjUP-N4E)

- req.url, req.method
- response can be string, json, html etc.
- [status code cheatsheet](https://devhints.io/http-status)
- Example

  ```js
  // index.js file
  const http = require("http");

  const PORT = 3000;

  const server = http.createServer((req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain" });
    res.write("welcome to the server");

    // if we want to return html as status
    // res.writeHead(200, { "Content-Type": "text/html" });
    // res.write("<h1>welcome to the server</h1>");

    res.end();
  });

  server.listen(PORT, () => {
    console.log(`server is running at http://localhost:${PORT}`);
  });
  ```

### [1.11 External modules | npm crash course](https://youtu.be/A8W1p8suw5I)

- first initialize npm with the command `npm init` then follow the instructions
- we can also use `npm init -y` command for ignoring the installation instructions
- npm packages : https://www.npmjs.com/
- how to install and uninstall npm packages
- Install https://www.npmjs.com/package/random-fruits-name package
  and follow the instructions

### [1.12 create node server and deploy on heroku](https://youtu.be/2IFDMvfJJHc)

- [node-server-demo](https://node-server-2022.herokuapp.com/)

- Create 3 html pages inside views folder: index.html, about.html, contact.html

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Document</title>

      <style>
        body {
          background-color: bisque;
        }
      </style>
    </head>
    <body>
      <nav>
        <ul>
          <li><a href="/">Home</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>
      </nav>
      <h1>This is Home page</h1>
    </body>
  </html>
  ```

- create the server (server) and load the html files based on request url

  ```js
  const http = require("http");
  const fs = require("fs");
  const PORT = 3000;

  const handleReadFile = (fileName, statusCode, req, res) => {
    fs.readFile(fileName, "utf-8", (err, data) => {
      if (err) {
        console.log(err);
      } else {
        res.writeHead(statusCode, { "Contant-Type": "text/plian" });
        res.write(data);
        res.end();
      }
    });
  };
  const server = http.createServer((req, res) => {
    if (req.url === "/" && req.method === "GET") {
      handleReadFile("./views/index.html", 200, req, res);
    } else if (req.url === "/about" && req.method === "GET") {
      handleReadFile("./views/about.html", 200, req, res);
    } else if (req.url === "/contact" && req.method === "GET") {
      handleReadFile("./views/contact.html", 200, req, res);
    } else {
      handleReadFile("./views/error.html", 404, req, res);
    }
  });

  server.listen(PORT, () => {
    console.log(`Server is running at http://localhost:${PORT}`);
  });
  ```
- How to response json 
```js
const http = require("http");
const fs = require("fs");

const PORT = 3001;

const server = http.createServer((req, res) => {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Request-Method', '*');
  res.setHeader(
    'Access-Control-Allow-Methods',
    'OPTIONS, GET, POST, PUT, DELETE, GET'
  );
  res.setHeader(
    'Access-Control-Allow-Headers',
    'Origin, X-Requested-With, X-PINGOTHER,Content-Type, Accept'
  );

  if (req.url === "/users" && req.method === "GET") {
    res.writeHead(200,  { "Content-Type": "application/json" });
    res.end(
      JSON.stringify({
        users: [
          { id: 1, name: "anisul islam", email: "anisul@gmail.com" },
          { id: 2, name: "sufia begum", email: "sufia@gmail.com" },
        ],
      })
    );
  }
});

server.listen(PORT, () => {
  console.log(`server is running at http://localhost:${PORT}`);
});


```
- How to deploy on heroku

  - step1 : const PORT = process.env.PORT || 3000;
  - step2 : add Procfile -> `web: node index.js`
  - step3 : npm init -y && npm install nodemon
  - step4 : follow the steps in heroku
  
### [1.13 Complete REST API]

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

## 2. Express.js

### [2.0 Introduction & installation]

- always check the documentation of [express.js](https://www.npmjs.com/package/express)
- Express.js is a node.js framework which makes life easier
- no more hassle of setting Content-Type
- easy to learn and time saving facilitites available because we have ready made stuff in express.js
- MERN Stack, NERD stack, PERN stack

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

- download & install VSCode editor
- download & install insomnia/postman/ThunderClient extension

### [2.1 Bootstraping the project]

- initialize package.json the package manager: npm init -yes
- initialize tsconfig.json the typescript manager: tsc --init and change the target as ES2017 in tsconfig.json
- `npm install nodemon typescript ts-node @types/node @types/express`
- add gitignore file
- add a README.md file
- add ESLint setup

### [2.2 Create first express server]

- create the server example 1:

```js
import express, { Application } from "express";

const app: Application = express();

const port: number = 3002;

app.listen(port, () => {
  console.log(`server is running at http://localhost:${port}`);
});
```

### [2.3 Run express server]

- how to run the server:
  - tsc index.ts and then node index.js
  - ts-node index.ts
  - nodemon index.ts
  - npm start -> add script in package.json -> "start": "nodemon index.ts"

### [2.4 Set environment variables]

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

### [2.5 API]

- what is API? why API?
- HTTP methods

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

### [2.17 MVC Architecture - SOC]

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

### [2.23 Express generator]

- package `npx espress-generator`
- create a basic standard scalable folder structure with necessary codes

### [2.24 cookie]

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

### [2.25 session]

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

```js
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
const addProduct = async () => {
  try {
    const newProduct = new Product({
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
