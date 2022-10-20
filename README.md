# Complete Backend Tutorial

- prerqusities: javascript, typescript

- 1. Node.js
- 2. Express.js
- 3. MongoDB
- 4. Authentication

## 1. Node.js

## [1.1 Introduction to Node.js & setup](https://youtu.be/36R0VXmX8i8)

### What is Node.js & Why Node.js?

![profile2](./images/full-stack-blueprint-2.png)

- fullstack = front-end + back-end

- Node.js is server side environment; not a programming language.
- helps us to create real time applications
- It helps to manage files (create, open, read, write, delete and close) on the server.
- it supports asynchronous programming.
- it helps to collect form data.
- It helps to manage (add, modify, delete data) database.

- It utilize Google's V8 engine (developed with C++) which compiles javascript code into machine code
- a js runtime which allows us to run js in the server
- node.js does not run on browser but only vanila js
- window is a global object for browser. before node.js we could use javascript for the browser only but now with the help of node.js we can run javascript for accessing our local machine. try console.log(), window.alert() at the browser and in the terminal.

### 1.2 Environment setup

- check node is already installed or not using the command: node --version or node -v
- [Node.js](https://nodejs.org/en/) download & install
- Editor: anything; I prefer [Visual Studio Code](https://code.visualstudio.com/)
- type node and enter for using Node REPL and try writing some javascript code here like console.log(), mathematical calculations
- window is a global object in the fornt end; global is the global object in the backend

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

### [1.4 Local module](https://youtu.be/n3F1kaOfyzw)

#### What is module? Types of module?

- we can use es6 import, export by adding "type":"module" in package.json
- when you have too much code in a single file you would like to separate them in multiple files so that they are reusable and modular.
- Module is a set of functions or variables.
- console.log(process) and find the module.exports = {}
- 3 types of module.
  - local module (own created module)
  - built-in-modules (node.js own module)
  - External modules (3rd party module mainly managed by npm)
- example 1

  ```js
  // message.js
  const SUBJECT = "Node.js";

  const displayInfo = (name) => {
    console.log(`Welcome ${name}`);
  };

  module.exports = { SUBJECT, displayInfo };

  // index.js
  const message = require("./message");
  console.log(message.SUBJECT);
  message.displayInfo("Anisul Islam");
  ```

- example 2

  ```js
  // message.js

  const SUBJECT = "Node.js";
  // exports.SUBJECT = "Node.js";

  const displayInfo = (name) => {
    console.log(`Welcome ${name}`);
  };

  exports.SUBJECT = SUBJECT;
  exports.displayInfo = displayInfo;

  // index.js

  // following line will get everything from message module
  const message = require("./message");

  // following line will get separate items from message module
  const { SUBJECT, displayInfo } = require("./message");
  console.log(SUBJECT);
  displayInfo("anisul");

  // another example
  exports.add = (num1, num2) => {
    return num1 + num2;
  };
  exports.sub = (num1, num2) => {
    return num1 - num2;
  };
  exports.mul = (num1, num2) => {
    return num1 * num2;
  };
  ```

- example 3

  ```js
  // converter.js
  const convertIntoNumber = (text) => {
    return parseInt(text);
  };

  module.exports = { convertIntoNumber };

  // index.js
  const { convertIntoNumber } = require("./converter");

  const price = "27.5";

  console.log(typeof convertIntoNumber(price));
  ```

### [1.5 Built-in module - os and path module](https://youtu.be/EHo7KNPawhw)

- Example of os module

  ```js
  // os, path
  const { totalmem, freemem } = require("os");
  console.log(totalmem());

  const os = require("os");
  const osInfo = () => {
    return {
      userInfo: os.userInfo(),
      totalMemory: os.totalmem(),
      freeMemory: os.freemem(),
      hostname: os.hostname(),
      osType: os.type(),
      relase: os.release(),
      platform: os.platform(),
      architecture: os.arch(),
      uptime: os.uptime(),
      cpus: os.cpus(),
    };
  };
  ```

### [1.6 Built-in module - path module]

- Example of path module

```js
const path = require("path");

const address = "backend-course/path-module.js";

exports.pathInfo = () => {
  return {
    __dirname: __dirname,
    __filename: __filename,
    directoryName: path.dirname(address),
    extensionName: path.extname(address),
    lastPartOfPath: path.basename(address),
    join: path.join(__dirname + "/views/index.html"),
  };
};
```

### [1.7 Built-in module - url module]

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
```

### [1.8 Built-in module - fs module]

- managing file system: create, read, update, rename, delete

```js
const fs = require("fs");
console.log(fs);

// creating a file
fs.writeFile("test.txt", "this is some demo text", (err) => {
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

### [1.9 Built-in module - http module](https://youtu.be/PmLJO403hvc)

- Example of a node.js http server

  ```js
  // Method 1
  const http = require("http");

  const PORT = 3000;
  const hostName = "127.0.0.1";

  const server = http.createServer((req, res) => {
    // res.end("welcome to the server");

    // end the response with a response message
    res.end("<h1>welcome to the server</h1>");
  });

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

## Express.js

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

## [2.4 Set environment variables]

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

## [2.5 API]

- what is API? why API?
- HTTP methods

## [2.6 HTTP methods: handle GET request & api testing]

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

## [2.7 Response object]

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

## [2.8 find path of a file]

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

## [2.9 Handling error - client and server error]

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

## [2.10 Request Object - route parameter]

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

## [2.11 Request Object - query parameter]

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

## [2.12 Request Object - header requests]

- example of how to get data header requests

  ```js
  router.post("/", (req, res) => {
    console.log(req.header("id"));
    res.send("I am get method of user route");
  });
  ```

## [2.13 Request Object - body data & HTTP methods: POST]

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

## [2.14 HTTP methods: handle DELETE request & api testing]

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

## [2.15 HTTP methods: handle PUT request & api testing]

## [2.16 express Router & path shotcut]

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

## [2.17 MVC Architecture - SOC]

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

## [2.18 Regular expression and wild card]

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

## [2.19 Middleware and its type]

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

## [2.20 Server side validation with express-validator]

- `npm i express-validator @types/express-validator`

```js
//middleware/formValidation.ts
import { body, validationResult } from "express-validator";
import { NextFunction, Request, Response } from "express";
export const formValidation = [
  body("title").isLength({ min: 5 }),
  body("username").isEmail(),
  body("price").not().isEmpty().withMessage("Price is required"),
  (request: Request, response: Response, next: NextFunction) => {
    const errors = validationResult(request);
    if (!errors.isEmpty()) {
      return response.status(422).json({ errors: errors.array() });
    } else next();
  },
];

//routes/products.ts
router.post("/", authUser, formValidation, addProduct);
```

## [2.21 How to upload file (image, video etc.)]

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

## [2.22 Express generator]

- package `npx espress-generator`
- create a basic standard scalable folder structure with necessary codes

## [2.23 cookie]

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

## [2.24 session]

## MongoDB Tutorial

## 0. SQL VS NoSQL

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

## 1. Introduction to MongoDB

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

## 2. Environment setup

- Download & install MongoDB
- MongoDB Compass
- Check installation

  ```js
  mongod --version (mongodb server version)
  mongo --version (mongodb shell version)
  ```

## 3. Basic command

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

## 4. CRUD - Create Document

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

## 5. CRUD - Read Document

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

## 6. CRUD - Update Document

- Update Data -> $set operator helps to set update value to a field

  - update data syntax: `db.collectionName.update(selection_item, update_data)`
  - update data syntax: `db.collectionName.updateOne(selection_item, update_data)`
  - update data syntax: `db.collectionName.updateMany(selection_item, update_data)`
  - find one and update data syntax: `db.collectionName.findOneAndUpdate(selection_item, update_data)`
  - example: `db.products.update({id:"1"},{$set:{price:32}})`

## 7. CRUD - Delete Document

- Delete data

  - delete data syntax: `db.collectionName.deleteOne(selection)
    - example: `db.users.deleteOne({name:"anisul islam"})`
  - delete data syntax: `db.collectionName.deleteOne()
  - delete many data syntax: `db.collectionName.deleteMany({selected_item})
  - delete many data syntax: `db.collectionName.deleteMany({})

## 8. CRUD Operations in MongoDB Compass

## 9. Node.js and mongodb connection

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

## 10. Mongoose schema & model

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

## 11. Create document from node.js

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

## 12. Read document from Database in node.js & comparsion query operators

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

## 13. Query Operators: Logical operators

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

## 14. counting and sorting

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

## 15. update data

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

## 16. delete data

- example: `db.users.deleteOne({name:"anisul islam"})`

```js
const products = await Product.deleteOne({ _id });
console.log(products);

// findByIdAndDelete return deleted data
const products = await Product.findByIdAndDelete({ _id });
console.log(products);
```

## 17. mongoose validations

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

## 18. Custom validations

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

## 19. npm validator

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

## 20. pagination

```js
const { page = 1, limit = 10 } = req.query;
const products = await Product.find()
  .limit(limit)
  .skip((page - 1) * limit);
console.log(products);
```

## 21: export and import data in mongodb atlas

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

## API Documentaion with Swagger

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

## Authentication Tutorial

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
