
#### index.js

- This is the starting file. 
- It calls the `app.js` to start the Action
- And it has the connection of `.env` file

```js
const app = require("./app");
const dotenv =require('dotenv'); 

dotenv.config({path: './config.env'})

app.listen(process.env.RUNNING_PORT, function (){
    console.log('server is running from env '+process.env.RUNNING_PORT)
})
```

#### config.env

```env
RUNNING_PORT = 5000
```
#### app.js

- This file call the express JS and connect the API for routing


```js
const express = require('express');
const router=require("./src/routes/api")
const app = new express();


//Security Middleware
const rateLimit = require('express-rate-limit');
const helmet =require('helmet');
const mongoSanitize = require('express-mongo-sanitize');
const xss = require('xss-clean');
const hpp = require('hpp');
const cors = require('cors');


//Security Middleware Implemen
app.use(cors());
app.use(helmet());
app.use(mongoSanitize());
app.use(xss());
app.use(hpp());


//Request rate limit
const limiter = rateLimit({
    windowMs: 15*60*1000,
    max: 100
});
app.use(limiter);


app.use("/api/v1",router)
//Undefined Route
app.use("*", (req,res)=>{
    res.status(404).json({status: "fail", data: "Not Found"})
})

module.exports=app;
```

#### api.js

- This is the routing operation file.
- It calls the `HelloController`


```js
const express = require('express');  
const router = express.Router();  
const HelloController =require("../controllers/HelloController")  
  
router.get("/hello-get", HelloController.HelloGet)  
router.post("/hello-post", HelloController.HelloPost)  
  
  
module.exports=router;
```

#### HelloController.js

- The HelloController` file

```js 
exports.HelloGet=(req,res)=>{  
    res.status(200).json({status: "Success", data: "tHE First express REST API for GET method"})  
}  
exports.HelloPost=(req,res)=>{  
    res.status(201).json({status: "Success", data: "tHE First express REST API for POST method"})  
}
```

##### package.json

```json
{  
  "name": "my_api_1_ as_ES5",  
  "version": "1.0.0",  
  "description": "",  
  "main": "index.js",  
  "scripts": {  
    "test": "echo \"Error: no test specified\" && exit 1"  },  
  "keywords": [],  
  "author": "",  
  "license": "ISC",  
  "dependencies": {  
    "axios": "^1.6.8",  
    "cookie-parser": "^1.4.6",  
    "cors": "^2.8.5",  
    "dotenv": "^16.4.5",  
    "express": "^4.19.2",  
    "express-mongo-sanitize": "^2.2.0",  
    "express-rate-limit": "^7.2.0",  
    "express-validator": "^7.0.1",  
    "helmet": "^7.1.0",  
    "hpp": "^0.2.3",  
    "nodemailer": "^6.9.13",  
    "nodemon": "^3.1.0",  
    "xss-clean": "^0.1.4"  
  }  
}
```


#### Perfect Project File Directory

![[Pasted image 20240504232512.png]]