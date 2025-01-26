
### Index.js

```js

const app = require("./app")  
const dotenv = require("dotenv")  
  
dotenv.config({ path: "./config.env" })  
  
app.listen(process.env.RUNNING_PORT, function (){  
    console.log("Runnig at "+process.env.RUNNING_PORT);  
});

```


### .env

``` js
RUNNING_PORT=8080  
  
JWT_SECRET="password4545"  
  
  
URL="mongodb+srv://abidAdmin:1234@cluster0.z72chbc.mongodb.net/blogs"

```


### App.js

```js
const express = require('express');  
const router = require("./src/routes/api")  
const app = express();  
const mongoose = require("mongoose");  
const dotenv = require("dotenv")  
const bodyParser = require("body-parser");  
  
dotenv.config({ path: "./config.env" })  
  
//Security Middleware  
const rateLimit = require("express-rate-limit");  
const helmet = require("helmet");  
const mongoSanitize = require("express-mongo-sanitize");  
const hpp = require("hpp");  
const xss =require("xss-clean")  
const cors = require("cors");  
  
  
app.use(cors());  
app.use(helmet());  
app.use(mongoSanitize());  
app.use(xss());  
app.use(hpp());  
  
  
const limiter = rateLimit({  
    windowMs: 60*15*1000,  
    max: 100  
})  
app.use(limiter);  
  
app.use(bodyParser.json())  
  
//Database connection  
  
let URL= process.env.URL;  
//let URL ="mongodb+srv://abidAdmin:1234@cluster0.z72chbc.mongodb.net/blogs"  
let OPTION = {user:"", autoIndex: true}  
mongoose.connect(URL, OPTION).then((res)=>{  
    console.log("Database Connected")  
}).catch((err)=>{  
    console.log(err)  
})  
  
app.use("/api/v1",router)  
  
//Undefined Route  
app.use("*",(req, res)  =>{  
    res.status(404).json({msg:"Wrong URL"})  
})  
  
  
module.exports = app;
```



### Api.js

```JS
const express = require('express');  
const productController = require("../controller/CURDcontroller");  
  
const router = express.Router();  
  
//Your router for Every controller and You can add auth Middleware 
router.post("/createProduct",/*in here*/ productController.createProduct);  
router.get("/readProduct", productController.readProduct);  
router.get("/readOneProduct/:id", productController.readOneProduct);  
router.get("/updateProduct/:id", productController.updateProduct);  
router.get("/deleteProduct/:id", productController.deleteProduct);  
  

//show all students with projection  
router.get("/all-students-projection",TokenVerifyMiddleware,StudentsController.ReadStudentsWithProjection) 

//show oen student
router.get("/student/:id",TokenVerifyMiddleware,StudentsController.ReadOneStudent) 

module.exports = router;

```

```
localhost:8080/api/v1/all-students-projection?fields=name roll class
```
### Model.js

```js
const mongoose = require('mongoose');  

//This the just DEMO, Create WHAT EVER YOU WANT
const DatabaseSchema = mongoose.Schema({  
    name: {type: String, required: true},  
    code: {type: String, required: true},  
    img: {type: String, required: true},  
    category: {type: String, required: true},  
    quantity: {type: Number, required: true},  
    price: {type: Number, required: true}  
},{timestamps: true, versionKey: false});  
  
const productModel = mongoose.model('Products', DatabaseSchema);  
  
module.exports = productModel;
```


### Controller.js

```js
const productModel = require("../models/productModel");  
  
  
exports.createProduct=async (req,res)=>{  
    try {  
        let reqBody = req.body;  
        await productModel.create(reqBody);  
        res.json({status:"success",message:"Product created"});  
    }catch(err){  
        res.json({status:"fail",message:err.toString()});  
    }  
}  
  
exports.readProduct=async (req,res)=>{  
    try {  
  
        let data = await productModel.find();  
        res.json({status:"success",message:data});  
    }catch(err){  
        res.json({status:"fail",message:err.toString()});  
    }  
}  
exports.readOneProduct=async (req,res)=>{  
    try {  
        let {id} = req.params  
        let data = await productModel.findOne({_id:id});  
        res.json({status:"success",message:data});  
    }catch(err){  
        res.json({status:"fail",message:err.toString()});  
    }  
}  
  
exports.updateProduct=async (req,res)=>{  
    try {  
        let { id } = req.params;  
        await productModel.updateOne({_id:id},req.body);  
        res.json({status:"success",message:"Product updated"});  
    }catch(err){  
        res.json({status:"fail",message:err.toString()});  
    }  
}  
  
exports.deleteProduct=async (req,res)=>{  
    try {  
        let { id } = req.params;  
        await productModel.deleteOne({_id:id});  
        res.json({status:"success",message:"Product deleted"});  
    }catch(err){  
        res.json({status:"fail",message:err.toString()});  
    }  
}
```

```js
//Read All Data with Projection  
exports.ReadStudentsWithProjection = async (req, res) => {  
    try {  
        // Get projection fields from query parameters, or default to some fields  
        const projection = req.query.fields ;  
  
        // Use projection to query the database  
        const data = await StudentsModel.find({}, projection.split(" ").join(" "));  
  
        res.json({ status: "success", message: data });  
    } catch (err) {  
        res.status(500).json({ status: "fail", message: err.message });  
    }  
};  
  
//Read One Student also with projection  
exports.ReadOneStudent=async (req,res)=>{  
    try {  
        let {id} = req.params  
        const projection = "name roll class";  
        let data = await StudentsModel.findOne({_id:id}, projection.split(" ").join(" "));  
        res.json({status:"success",message:data});  
    }catch(err){  
        res.json({status:"fail",message:err.toString()});  
    }  
}
```


### Middleware.js

```js
const jwt = require("jsonwebtoken");  
  
exports.TokenVerifyMiddleware=(req,res,next)=>{  
    let token = req.headers["token-key"];  
  
    jwt.verify(token, process.env.JWT_SECRET, (err,decoded) => {  
        if(err){  
            res.status(401).json({status:"Invalid Token",message:err.toString()});  
        }  
        else {  
            next();  
        }  
    })  
}
```



### Deploy Backend using Vercel

##### `vercel.js`
```json
{
    "version":2,
    "builds":[
        {
            "src":"./index.js",
            "use":"@vercel/node"
        }
    ],
    "routes":[
        {
            "src":"/(.*)",
           "dest":"/" 
        }
    ]
}
```
- We need to add `vercel.json` file in main directory



##### `package.json`
```json
{
  "name": "back-end",
  "version": "1.0.0",
  "main": "index.js",
  "engines": {
    "node": "20.x"
  },
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "dependencies": {
    "@vercel/speed-insights": "^1.0.12",
    "body-parser": "^1.20.2",
    "cors": "^2.8.5",
    "dotenv": "^16.4.5",
    "express": "^4.19.2",
    "express-mongo-sanitize": "^2.2.0",
    "express-rate-limit": "^7.3.1",
    "helmet": "^7.1.0",
    "hpp": "^0.2.3",
    "jsonwebtoken": "^9.0.2",
    "mongoose": "^8.4.5",
    "nodemailer": "^6.9.14",
    "xss-clean": "^0.1.4"
  },
  "devDependencies": {
    "nodemon": "^3.1.4"
  }
}
```
- Then edit `package.json` file
- Just add `"engines": {"node": "20.x"},` After the third line



