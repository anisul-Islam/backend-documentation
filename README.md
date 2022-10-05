# Complete Backend Tutorial

- prerqusities: javascript, typescript

- 1. Node.js
- 2. Express.js
- 3. MongoDB
- 4. Authentication

## Express.js

### 0. Introduction & installation

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

## 1. Bootstraping the project

- initialize package.json the package manager: npm init -yes
- initialize tsconfig.json the typescript manager: tsc --init and change the target as ES2017 in tsconfig.json
- `npm install nodemon typescript ts-node @types/node @types/express`
- add gitignore file
- add a README.md file
- add ESLint setup

## 2. Create first express server

- create the server example 1:

```js
import express, { Application } from "express";

const app: Application = express();

const port: number = 3002;

app.listen(port, () => {
  console.log(`server is running at http://localhost:${port}`);
});
```

## 3. Run express server

- how to run the server:
  - tsc index.ts and then node index.js
  - ts-node index.ts
  - nodemon index.ts
  - npm start -> add script in package.json -> "start": "nodemon index.ts"

## 4. Set environment variables

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

## 5. API

- what is API? why API?
- HTTP methods

## 6. HTTP methods: handle GET request & api testing

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

## 7. Response object

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

## 8. find path of a file

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

## 9. Handling error - client and server error

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

## 10. Request Object - route parameter

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

## 11. Request Object - query parameter

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

## 12. Request Object - header requests

- example of how to get data header requests

  ```js
  router.post("/", (req, res) => {
    console.log(req.header("id"));
    res.send("I am get method of user route");
  });
  ```

## 13. Request Object - body data & HTTP methods: POST

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

## 14. HTTP methods: handle DELETE request & api testing

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

## 15. HTTP methods: handle PUT request & api testing

## 16. express Router & path shotcut

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

## 17. MVC Architecture - SOC

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

## 18. Regular expression and wild card

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

## 19. Middleware and its type

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

## 20. Server side validation with express-validator

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

## 21. Express generator

- package `npx espress-generator`
- create a basic standard scalable folder structure with necessary codes

## 22. Introduction to MongoDB

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

## 23. Database Environment setup

- Download & install MongoDB
- MongoDB Compass
- Check installation

  ```js
  mongod --version (mongodb server version)
  mongo --version (mongodb shell version)
  ```

## 24. Basic command

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

## 25. CRUD - Create Document

- database is collection of Documents
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

## 26. CRUD - Read Document

- Read / Find data

  - read data syntax: `db.collectionName.find()`
  - read data in an easy way syntax: `db.collectionName.find().pretty()`
  - read a specific data syntax: `db.collectionName.find({field: fieldValue})`
    - example1: `db.products.find(ObjectId("633d183e09743587af26fb07"))`
    - example2: `db.users.find({name: "anisul islam"})`
  - limit data syntax: `db.collectionName.find({field: fieldValue}).limit(NumberOfRows)`
    - example: `db.users.find({age: 31}).limit(2)`
  - sorting: 1 for ascending, -1 for descending
    - example: `db.products.find().sort({price: -1})`
  - [less than and greater than](https://www.statology.org/mongodb-greater-than-less-than/#:~:text=You%20can%20use%20the%20following,%24gt%3A%20Greater%20than)
    - example : `db.myCollection.find({field1: {$gt:25}})`

## 27. CRUD - Update Document

- Update Data

  - update data syntax: `db.collectionName.update(selection_item, update_data)`
  - update data syntax: `db.collectionName.updateOne(selection_item, update_data)`
  - update data syntax: `db.collectionName.updateMany(selection_item, update_data)`
  - find one and update data syntax: `db.collectionName.findOneAndUpdate(selection_item, update_data)`
  - example: `db.products.update({id:"1"},{$set:{price:32}})`

## 28. CRUD - Delete Document

- Delete data

  - delete data syntax: `db.collectionName.deleteOne(selection)
    - example: `db.users.deleteOne({name:"anisul islam"})`
  - delete data syntax: `db.collectionName.deleteOne()
  - delete many data syntax: `db.collectionName.deleteMany({selected_item})
  - delete many data syntax: `db.collectionName.deleteMany({})

## 29. REST API with mongodb for CRUD operations

```

```
