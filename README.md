# JWT DEMO

## How To USe

- Clone the project.
- Change directory and npm install.

```
const express = require('express');                            //Import express
const jwt = require('jsonwebtoken');                           //Import jsonwebtoken
const app = express();
const Secreatkey = "SecreatKey";

app.get('/', (req, resp) => {                                  // Sample API
    resp.json({
        message: "A simple API"
    })
})

app.post('/Login', (req, resp) => {
    // Mock user
    const user = {
        id: 1,
        Username: "Rutuja",
        Email: "rutuja@edulab.in"
    }
    jwt.sign({ user }, Secreatkey, { expiresIn: '300s' }, (err, token) => {   // Set Token Expire Time.
        resp.json({
            token
        })
    })
})


app.post('/profile', verifytoken, (req, resp) => {
    jwt.verify(req.token, Secreatkey, (err, authdata) => { 
        if (err) {
            resp.send({ result: "Invalid Token" });
        }
        else {
            resp.send({ result: "Accessed", authdata });
        }
    })
})

// FORMAT OF TOKEN 
// Authorization: Bearer <access_token>

// Verify Token

function verifytoken(req, resp, next) {
    // Get auth header value
    const bearerHeader = req.headers['authorization'];
    // Check if bearer is undefined
    if (typeof bearerHeader !== 'undefined') {
        // Split at the space
        const bearer = bearerHeader.split(" ");
        // Get token from array
        const token = bearer[1];
        // Set the token
        req.token = token;
        // Next middleware
        next();

    }
    else {
        // Forbidden
        resp.send({ result: "Token Is Invalid" });
    }
}

app.listen(5000, () => {
    console.log(("Page is running"));
}
);
```
