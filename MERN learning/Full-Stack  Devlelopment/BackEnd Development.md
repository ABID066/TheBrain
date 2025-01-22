### Introduction


- `npm init --y`

-  `npm i express`

-  `npm i `

-  `npm run build` 

- `create index.js and app.js`

- `npm i body-parser`

- `npm i jsonwebtoken`

- `npm i mongoose`

-  `npm i cors`

- `npm i express-mongo-sanitize`

-  `npm i express-rate-limit`

- `npm i helmet`

-  `npm i hpp`

- `npm i xss-clean`

- `npm i dotenv`

- `npm i nodemon`

- `npm i nodemailer`

- create `.env` file and `src` folder and SO ON 

- then in `src/routes` create api.js 

- `CONTINUE `


### package.json 

```json
{  
  "name": "back-end",  
  "version": "1.0.0",  
  "main": "index.js",  
  "scripts": {  
    "test": "echo \"Error: no test specified\" && exit 1"  },  
  "keywords": [],  
  "author": "",  
  "license": "ISC",  
  "description": "",  
  "dependencies": {  
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

```js
RUNNING_PORT=8080

```


### App.js

```js
const express =require('express');  
const router = require('./src/routes/api');  
const app = new express();  
const rateLimit = require('express-rate-limit');  
const helmet = require('helmet');  
const hpp = require('hpp');  
const cors= require('cors');  
const mongoose = require('mongoose');  
  
//Cors  
app.use(cors());  
  
//Security  
app.use(helmet())  
app.use(hpp())  
app.use(express.json({limit:'20mb'}))  
app.use(express.urlencoded({extended: true}))  
  
let limiter = rateLimit({windowMs: 15*60*1000,max: 3000})  
app.use(limiter);  
  
//Mongoose  
let URL = "mongodb+srv://abidAdmin:1234@cluster0.z72chbc.mongodb.net/CRUD_Application"  
let OPTION = {user:"", autoIndex: true}  
mongoose.connect(URL, OPTION).then((res)=>{  
    console.log("Connected")  
}).catch((err)=>{  
    console.log(err)  
})  
  
  
//Route  
app.use("/api/v1", router);  
  
//404 not found  
app.use("*",(req,res)=>{  
    res.status(404).json({data: "Not & Nor Found"})  
})  
  
module.exports=app;
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
  
  
module.exports = router;

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


### Middleware.js

```js

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



