const express = require('express');
const app = express();
const port = 3000;

// Store OTPs in memory for the demo
const otpExpiry = {};

app.use(express.json());

// Endpoint to generate OTP
app.post('/generate-otp', (req, res) => {
    const phoneNumber = req.body.phoneNumber;
    const otp = Math.floor(Math.random() * (9999 - 1000 + 1)) + 1000;
    otpExpiry[phoneNumber] = { otp: otp, expiry: new Date(Date.now() + 5 * 60000) }; // 5 minutes expiry
    console.log(`Generated OTP ${otp} for ${phoneNumber}`);
    res.json({ otp: otp });
});

// Endpoint to verify OTP
app.post('/verify-otp', (req, res) => {
    const { phoneNumber, otp } = req.body;
    if (phoneNumber in otpExpiry) {
        if (new Date() < otpExpiry[phoneNumber].expiry) {
            if (parseInt(otp) === otpExpiry[phoneNumber].otp) {
                console.log("OTP verification successful");
                res.json({ verified: true });
            } else {
                console.log("Incorrect OTP");
                res.json({ verified: false });
            }
        } else {
            console.log("OTP expired");
            res.json({ verified: false, message: "OTP expired" });
        }
    } else {
        console.log("No OTP generated for this number");
        res.json({ verified: false, message: "No OTP generated for this number" });
    }
});

app.listen(port, () => {
    console.log(`OTP API listening at http://localhost:${port}`);
});
