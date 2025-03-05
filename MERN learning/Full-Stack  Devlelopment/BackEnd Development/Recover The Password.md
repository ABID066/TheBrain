
```js
const UserModel = require("../models/UserModel");
const OTPModel = require("../models/OTPModel");
const jwt = require("jsonwebtoken");
const bcrypt = require("bcrypt");
const SendEmailUtility = require("../utilites/SendEmailUtility");


// Recover Password 01: Send OTP
exports.VerifyUserEmail = async (req, res) => {
    let email = req.params.email;
    let OTPCode = Math.floor(100000 + Math.random() * 900000); // Generate 6-digit OTP

    try {
        let user = await UserModel.findOne({ email: email });

        if (user) {
            await OTPModel.create({ email: email, otp: OTPCode, status: 0 });

            let emailResult = await SendEmailUtility(email, OTPCode);
            res.status(200).json({ status: "success", result: emailResult });
        } else {
            res.status(404).json({ status: "fail", message: "No User Found" });
        }
    } catch (e) {
        res.status(500).json({ status: "fail", message: e.toString() });
    }
};



// Recover Password 02: Verify OTP
exports.VerifyOTP = async (req, res) => {
    let { email, otp } = req.body; // Use request body instead of params

    try {
        let otpRecord = await OTPModel.findOne({ email, otp, status: 0 });

        if (otpRecord) {
            await OTPModel.updateOne({ email, otp }, { status: 1 }); // Mark OTP as used
            res.status(200).json({ status: "success", message: "OTP Verified" });
        } else {
            res.status(400).json({ status: "fail", message: "Invalid OTP Code" });
        }
    } catch (e) {
        res.status(500).json({ status: "fail", message: e.toString() });
    }
};



// Recover Password 03: Reset Password
exports.ResetPassword = async (req, res) => {
    let { email, OTP, password } = req.body;

    try {
        let otpRecord = await OTPModel.findOne({ email, otp: OTP, status: 1 });

        if (!otpRecord) {
            return res.status(400).json({ status: "fail", message: "Invalid or expired OTP" });
        }

        // Hash new password
        const saltRounds = 10;
        const hashedPassword = await bcrypt.hash(password, saltRounds);

        // Update user password
        await UserModel.updateOne({ email }, { password: hashedPassword });

        // Delete OTP after successful password reset
        await OTPModel.deleteOne({ email });

        res.status(200).json({ status: "success", message: "Password changed successfully" });
    } catch (err) {
        res.status(500).json({ status: "fail", message: err.toString() });
    }
};

```


# SendEmailUtility.js

```js
const nodemailer = require("nodemailer");

const SendEmailUtility = async (email, OTPCode) => {
    try {
        let transporter = nodemailer.createTransport({
            service: 'gmail',
            auth: {
                user: process.env.EMAIL_USER, // Use environment variable
                pass: process.env.EMAIL_PASS  // Use App Password or SMTP password
            },
        });

        let mailHTML = `
            <div style="font-family: Arial, sans-serif; text-align: center; padding: 20px;">
                <h2 style="color: #333;">Task Manager OTP Verification</h2>
                <p>Hello,</p>
                <p>Use the following OTP to verify your email address:</p>
                <p style="font-size: 22px; font-weight: bold; color: #d32f2f; background: #f8d7da; padding: 10px; display: inline-block; border-radius: 5px;">
                    ${OTPCode}
                </p>
                <p>This OTP is valid for 10 minutes. Do not share it with anyone.</p>
                <p>If you did not request this, please ignore this email.</p>
                <p style="margin-top: 20px; font-size: 12px; color: #777;">© 2025 Task Manager. All Rights Reserved.</p>
            </div>
        `;

        let mailOptions = {
            from: `Task Manager <${process.env.EMAIL_USER}>`, 
            to: email,
            subject: "Task Manager OTP Verification",
            html: mailHTML
        };

        let info = await transporter.sendMail(mailOptions);
        return { success: true, message: `Email sent: ${info.response}` };

    } catch (error) {
        console.error("Email Sending Error:", error);
        return { success: false, message: "Email could not be sent." };
    }
};

module.exports = SendEmailUtility;

```