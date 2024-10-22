# Code Snippets

## Getting started
```html
mkdir express-app
cd express-app
npm init
npm install -save express body-parser
npm start
```
Add the following to package.json
```html
"scripts": {   
       "start": "node ./index.js",
       "test": "echo \"Error: no test specified\" && exit 1"
}
```
Create a file called "index.js"

## Server setup
```html
const express = require("express");
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
```

## Route setup
```html
const route = express.Router();
const port = process.env.PORT || 5001;

app.use('/v1', route);
app.listen(port, () => {
  console.log(`Server listening on port ${port}`);
});
```

## Basic GET route
```html
route.get('/simple-get', (req, res) => {
  res.send("here");
});
```

## Dynamic GET route
```html
route.get('/dynamic-get', (req, res) => {
  res.send(req.body.inputString);
});
```

## Pokemon API route
```html
npm install express axios
```
```html
const axios = require("axios");

...

route.get('/pokemon/:name', async (req, res) => {
  const pokemonName = req.params.name.toLowerCase();

  try {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`);
    
    const pokemonData = {
      name: response.data.name,
      id: response.data.id,
      height: response.data.height,
      weight: response.data.weight,
    };

    res.json(pokemonData);
  } catch (error) {
    res.status(404).send({ error: "Pokémon not found!" });
  }
});
```

## Email setup
```html
npm install nodemailer
```
```html
const nodemailer = require('nodemailer');

...

const transporter = nodemailer.createTransport({
  port: 465,
  host: "smtp.gmail.com",
  auth: {
      user: 'ucladevxtest@gmail.com',
      pass: 'pwek lyup fzqt qtji'
  },
  secure: true,
});
```

## Email route
```html
route.post('/send-email', (req, res) => {
  const {to, subject, text} = req.body;
  console.log(to + " " + subject + " " + text)
  const mailData = {
      from: 'ucladevxtest@gmail.com',
      to: to,
      subject: subject,
      text: text,
      html: '<p>' + text + '<p>',
  };

  transporter.sendMail(mailData, (error, info) => {
      if (error) {
          return console.log(error);
      }
      res.status(200).send({ message: "Mail send", message_id: info.messageId });
  });
});
```
