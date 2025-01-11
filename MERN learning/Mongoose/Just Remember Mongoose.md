#### DataBase Connection 

```js

let URL = "mongodb+srv://abidAdmin:1234@cluster0.z72chbc.mongodb.net/TaskManager"


let URL = "mongodb://localhost:27017/TaskManager"  
let OPTION = {user:"", autoIndex: true}  
mongoose.connect(URL, OPTION).then((res)=>{  
    console.log("Connected")  
}).catch((err)=>{  
    console.log(err)  
})

```