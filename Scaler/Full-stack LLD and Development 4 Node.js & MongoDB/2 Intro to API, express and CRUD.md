## APIs
   In backend developement, an API of various endpoints or routes that define specific functionality or service that your server provide. 


## Express

  1. Simpler and cleaner syntax
  
  2. Code readability
  
  3. Routing: Express provides a straightforward way to define routes
     and handle different HTTP methods (GET, POST, PUT, DELETE,
     etc.). In vanilla Node.js, you'd typically have to handle URLs and
     methods manually, which can be cumbersome.
  
  4. Middleware: One of the core features of Express is its
     convenience in use of middleware, which are functions that have
     access to the request object (req), the response object (res), and
     the next middleware function in the application’s
     request-response cycle. .
   

Setting up an express application.
Step 1: create a file index.js
step 2: npm init -y
step 3: npm install express --save
step 4: sudo npm i nodemon -g   (it will install nodemon at global level)


## Seeing the request body.
    What Does `app.use(express.json())` Do?
    
    Purpose: It helps your Express application understand and work with JSON data sent in requests.
    
    Functionality: It automatically parses incoming JSON requests and
    makes the data available in req.body.


## `Creating a Express Server`

```
const express = require("express");

// create an express app.
const app = express();
app.use(express.json());   // it is a middleware for all post request which help to receive reqest body.
app.use(express.static("public"))
app.use(express.urlencoded({ extended: true }));


// define a route
app.get("/", (req, res) => {
    res.send("Hello express!!")
});

app.get("/about", (req, res) => {
    res.send("this is about page");
});

app.post("/data", (req, res) => {
    // console.log("request recieved", req);
    console.log("request recieved", req.body);
    res.send("this is post request");
});

const users = [
    { id: 1, name: "john" },
    { id: 2, name: "Doe" },
];

app.post("/users", (req, res) => {
    const newUser = req.body;

    // Operation
    const userId = users.length + 1;
    newUser.id = userId;

    // persist this information in your DB
    users.push(newUser);

    // send the status code.
    res.status(201).json({ message: "User created", user: newUser });

    console.log("Print all Users: ", users);
});


const usersDb = [];
const notAllowList = ["R", "r"];
app.post('/payment', (req, res) => {
    const paymentBody = req.body;
    if (paymentBody.user.startsWith('r') || paymentBody.user.startsWith('R')) {   // instead of this one create a notAllowList
        res.status(400).json({ message: 'Invalid User' });
    }else {
        usersDb.push(paymentBody);
        res.status(200).json({ message: 'Valid User,User saved'});
    }

    console.log("Print all Users: ", usersDb)
});


app.delete("/users/:id", (req,res)=>{
    const userId = parseInt(req.params.id);

    // find the user with id.
    const userIndex = users.findIndex((user)=> user.id === userId);
    if(userIndex === -1){
        return res.status(404).json({message:"user not found"});
    }

    users.splice(userIndex, 1);
    res.status(200).json({message: "user deleted"})

    console.log("Print all Users: ", users)
});

app.get("/special", loggerMiddleware, (req, res)=> {
    res.send("this is a special route");
});

// localhost:3000/search?name=rajneesh
app.get("/search", (req, res)=> {
    const queryParam = req.query;   
    console.log("queryParam: ", queryParam);
    res.send(`Your parameter are ${JSON.stringify(queryParam)}`)
});

app.use((req, res)=>{
    res.status(404).send("page not found");
});

// Start the server.
const port = 3000;
app.listen(port, () => {
    console.log(`server is running on port ${port}`);
})

```


Importing Express 
```
const express = require("express");
```

create an express app.
```
const app = express();
```

This is how we use middleware,
Here `express.json()` It parses incoming requests with JSON payloads and is based on body-parser
```
app.use(express.json());
```


`urlencoded` parses incoming requests with URL-encoded payloads and is based on a body parser. Parameter
```
app.use(express.urlencoded({ extended: true }));
```


Creating routes
```
app.get("/", (req, res) => {
    res.send("Hello express!!")
});
```
```
app.get("/about", (req, res) => {
    res.send("this is about page");
});
```



`listen` use to listen to the port 
```
app.listen(port, () => {
    console.log(`server is running on port ${port}`);
})
```


`req.body` is the paylood which come from client 
```
const newUser = req.body;
```
```
const {userName, password} = req.body;
```
Importing Express 
```
const express = require("express");
```
Importing Express 
```
const express = require("express");
```




















