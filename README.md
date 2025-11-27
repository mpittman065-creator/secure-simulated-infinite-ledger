# secure-simulated-infinite-ledger
secure simulated infinite ledger
#backend (api folder)
{
  "name": "ledger-api",
  "version": "1.0.0",
  "main": "api/index.js",
  "dependencies": {
    "cors": "^2.8.5",
    "express": "^4.18.2"
  }
}
 #api/index.js

const express = require('express');
const cors = require('cors');
const app = express();
app.use(cors());
app.use(express.json());

let balance = 0;
let ledger = [];

app.get('/balance', (req, res) => res.json({ balance }));
app.post('/addFunds', (req, res) => {
  balance += 1e6;
  ledger.push({ id: Date.now(), amount: 1e6, time: new Date() });
  res.json({ success: true, newBalance: balance });
});

module.exports = app;

#vercel.json(root)

{
  "version": 2,
  "builds": [
    { "src": "api/index.js", "use": "@vercel/node" }
  ],
  "routes": [
    { "src": "/api/(.*)", "dest": "/api/index.js" }
  ]
}

#frontend react

const API_URL = '/api';  // Vercel rewrites this to the serverless function
// ...
const { data } = await axios.get(`${API_URL}/balance`);

#build react app

npm run build
