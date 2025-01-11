

#### index.js

- This is the starting file. 
- It calls the `app.js` to start the Action

```js
const app=require('./app')  
  
app.listen(5000,function (){  
    console.log("APP Running in 5000 port")  
})
```

#### app.js

- This file call the express JS and connect the API for routing

```js
const express=require('express')
const router =require('./src/routes/api')
const app =new express()

app.use('/',router)

module.exports=app;
```

#### api.js

- This is the routing operation file.
- It calls the `StudentController` and `middleware`


```js
const express = require('express')  
//const {ReadStudent, CreateStudent, UpdateStudent, DeleteStudent} = require("../controllers/StudentController");  
const  router =express.Router();  
const  StudentController=require('../controllers/StudentController')  
const AuthMiddleware=require('../middleware/auth')  
  
router.get('/read',StudentController.ReadStudent)  
  
router.post("/create",AuthMiddleware, StudentController.CreateStudent)  
  
router.put("/update", StudentController.UpdateStudent)  
  
router.delete("/delete", StudentController.DeleteStudent)  
  
  
  
module.exports=router
```

#### auth.js

- The `middleware` file

```js
module.exports=(req,res,next)=>{  
  
    console.log("auth middleware");  
    return res.end("STOP");  
  
    //next();  
}
```

#### StudentController.js

- The `StudentController` file

```js 
exports.CreateStudent=(req,res)=>{  
    res.end("I am create function")  
}  

exports.DeleteStudent=(req,res)=>{  
    res.end("I am delete function")  
}

exports.UpdateStudent=(req,res)=>{  
    res.end("I am update function")  
}

exports.ReadStudent=(req,res)=>{  
    res.end("I am read function")  
}
```



#### Perfect Project File Directory

![[Pasted image 20240501141243.png]]