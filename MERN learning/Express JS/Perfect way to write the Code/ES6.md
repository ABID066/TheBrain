#### First thing to do

 - To move from ES5 to ES6 we need to change something
 - In the `package.json` file type `"type": "module",` after `"main": "index.js",` line

#### index.js

- This is the starting file. 
- It calls the `app.js` to start the Action

```js
import app from "./app.js";  
app.listen(8000,()=>{  
    console.log("Server is running")  
})
```

#### app.js

- This file call the express JS and connect the API for routing


```js
import express from "express";
import ExpressMongoSanitize from "express-mongo-sanitize";
import mongoose from "mongoose";
import rateLimit from "express-rate-limit";
import helmet from "helmet";
import hpp from "hpp";
import cookieParser from "cookie-parser";
import cors from 'cors'
import router from "./src/routes/api.js";

const app = express()

//Middleware Implementation
app.use(cors());
app.use(helmet());
app.use(hpp());

const limiter =rateLimit({windowMs: 15*60*1000, max:3000})
app.use(limiter);

//Disable Response Cache
app.set('etag',false);


//Request Size limit
app.use(express.json({limit:'20MB'}));
app.use(express.urlencoded({limit: false}));

//Database Connection
//mongoose.connect("",{autoIndex: true});

//API Route Connect
app.use("/api",router)

export default app;
```

#### api.js

- This is the routing operation file.
- It calls the `HelloController`


```js
import express from "express";  
import * as WelcomeController  from "../controllers/WelcomeController.js";  
  
const router = express.Router();  
  
router.get("/", WelcomeController.Welcome);  
router.get("/welcome1", WelcomeController.Welcome1);  
router.get("/welcome2", WelcomeController.Welcome2);  
  
export default router;
```

#### WelcomeController.js

- The WelcomeController` file

```js 
export const Welcome =async (req,res) =>{  
    return res.status(200).json({  
        data: "Welcome"  
    })  
}  
  
export const Welcome1 =async (req,res) =>{  
    return res.status(200).json({  
        data: "Welcome1"  
    })  
}  
  
export const Welcome2 =async (req,res) =>{  
    return res.status(200).json({  
        data: "Welcome2"  
    })  
}
```

##### package.json

```json
{  
  "name": "my_api_1_ as_ES6",  
  "version": "1.0.0",  
  "description": "",  
  "main": "index.js",  
  "type": "module",  
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
    "mongoose": "^8.3.3",  
    "nodemailer": "^6.9.13",  
    "nodemon": "^3.1.0",  
    "xss-clean": "^0.1.4"  
  }  
}
```



#### Perfect Project File Directory

![[Pasted image 20240504233542.png]]