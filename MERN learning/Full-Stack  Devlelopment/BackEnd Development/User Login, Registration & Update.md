
```js
const UserModel = require("../models/UserModel");
const jwt = require("jsonwebtoken");
const bcrypt = require("bcrypt");



// Registration
exports.UserRegistration = async (req, res) => {
    try {
        let { firstName, lastName, email, mobile, password, photo } = req.body;

        // Hash the password
        const saltRounds = 10;
        const hashedPassword = await bcrypt.hash(password, saltRounds);

        await UserModel.create({
            firstName,
            lastName,
            email,
            mobile,
            password: hashedPassword, // Store hashed password
            photo
        });

        res.status(200).json({ status: "success", message: "User registration successful" });
    } catch (err) {
        res.status(400).json({ status: "fail", message: err.toString() });
    }
};



// Login
exports.UserLogin = async (req, res) => {
    try {
        let { email, password } = req.body;

        let user = await UserModel.findOne({ email }).select("email firstName lastName mobile photo password");

        if (!user) {
            return res.status(401).json({ status: "unauthorized", message: "Invalid email" });
        }

        // Compare the provided password with the stored hashed password
        const isMatch = await bcrypt.compare(password, user.password);
        if (!isMatch) {
            return res.status(401).json({ status: "unauthorized", message: "Invalid password" });
        }

        // Generate JWT token
        let payload = {
            exp: Math.floor(Date.now() / 1000) + 24 * 60 * 60, // Expires in 1 day
            email: user.email
        };
        let token = jwt.sign(payload, process.env.JWT_SECRET);

        // Set token in cookies
        res.cookie("token", token, {
            httpOnly: true,
            secure: process.env.NODE_ENV === "production",
            sameSite: "Strict",
            maxAge: 24 * 60 * 60 * 1000 // Cookie expiry (1 day)
        });

        // Remove password before sending response
        const { password: _, ...userData } = user.toObject();

        res.status(200).json({ status: "success", token: token, data: userData });

    } catch (err) {
        res.status(400).json({ status: "fail", message: err.message });
    }
};



// Update Profile
exports.profileUpdate = async (req, res) => {
    try {
        let email = req.headers["email"];
        let updateData = { ...req.body };

        // If the user is updating their password, hash the new password
        if (updateData.password) {
            const saltRounds = 10;
            updateData.password = await bcrypt.hash(updateData.password, saltRounds);
        }

        let data = await UserModel.updateOne({ email: email }, updateData);
        res.json({ status: "success", message: "Profile updated" , data: data});
    } catch (err) {
        res.json({ status: "fail", message: err.toString() });
    }
};



// Get Profile Details
exports.profileDetails = async (req, res) => {
    let email = req.headers["email"];

    try {
        let data = await UserModel.findOne({ email: email });
        res.status(200).json({ status: "success", result: data });
    } catch (err) {
        res.status(400).json({ status: "fail", message: err.toString() });
    }
};

```



# AuthVerification.js

```js
const jwt = require("jsonwebtoken");  
  
exports.AuthVerify = (req, res, next) => {  

    let token= req.headers["token"]  
	if(!token){  
	    token = req.cookies["token"]  
	}
  
    if (!token) {  
        return res.status(401).json({ status: "unauthorized", message: "No token provided" });  
    }  
  
    jwt.verify(token, process.env.JWT_SECRET, (err, decoded) => {  
        if (err) {  
            return res.status(401).json({  
                status: "unauthorized",  
                message: err.name === "TokenExpiredError" ? "Token has expired" : "Invalid token"  
            });  
        }  
  
        req.headers.email = decoded.email;  
        next();  
    });  
};
```


# UserModel.js

```js
const mongoose = require('mongoose')  
const DataSchema= new mongoose.Schema({  
    email:{ type:String,unique:true,required:true },  
    firstName:{ type:String,required:true},  
    lastName:{ type:String,required:true},  
    password:{ type:String,required:true},  
    mobile:{ type:String,required:true},  
    photo:{ type:String},  
    createdAt:{ type:Date,default:Date.now},  
  
},{versionKey:false});  
  
const UserModel = mongoose.model("users",DataSchema);  
module.exports = UserModel;
```