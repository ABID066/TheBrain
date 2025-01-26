
```js
const jwt = require("jsonwebtoken");  


//Encoding  
exports.CreateToken=(req,res)=>{  
  
    let Payload ={  
        exp: Math.floor(Date.now() / 1000)+(60*60),  //1 hour
        data: {Name:"Sayad Bin Kamrul", City: "Khulna", admin: true}  
    }  
  
    let token = jwt.sign(Payload,process.env.JWT_SECRET);  
  
    res.status(200).json({status:"success",token:token});  
}  


//Decoding
exports.DecodeToken=(req,res)=>{  
    let token = req.headers["token-key"];  
  
    jwt.verify(token, process.env.JWT_SECRET, (err,decoded) => {  
        if(err){  
            res.status(401).json({status:"Invalid Token",message:err.toString()});  
        }  
        else {  
            res.status(200).json({status:"success",data:decoded});  
        }  
    })  
}
```