# express-useful-packages

A list of useful packages to develop an ExpressJS application

## [nodemon](https://nodemon.io/)

Monitor any changes in the source code and automatically restart the server. Useful for development.

### Installation

```bash
$ npm install -g nodemon
```

### Usage

```bash
# Instead of calling your entry script like:
#   $ node index.js
# Use nodemon
$ nodemon index.js
```

## [mongoose](https://mongoosejs.com/)

An elegant mongodb object modeling

### Installation

```bash
$ npm install mongoose
```

### Usage

```js
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/test', { useNewUrlParser: true });

const Cat = mongoose.model('Cat', { name: String });

const kitty = new Cat({ name: 'Zildjian' });
kitty.save().then(() => console.log('meow'));
```

## [joi](https://github.com/hapijs/joi)

Object schema description language and validator for JavaScript objects.

### Installation

```bash
$ npm install @hapi/joi
```

### Usage

```js
const Joi = require('@hapi/joi');

const schema = Joi.object()
    .keys({
        username: Joi.string()
            .alphanum()
            .min(3)
            .max(30)
            .required(),
        password: Joi.string().regex(/^[a-zA-Z0-9]{3,30}$/),
        access_token: [Joi.string(), Joi.number()],
        birthyear: Joi.number()
            .integer()
            .min(1900)
            .max(2013),
        email: Joi.string().email({ minDomainSegments: 2 }),
    })
    .with('username', 'birthyear')
    .without('password', 'access_token');

// Return result.
const result = Joi.validate({ username: 'abc', birthyear: 1994 }, schema);
// result.error === null -> valid

// You can also pass a callback which will be called synchronously with the validation result.
Joi.validate({ username: 'abc', birthyear: 1994 }, schema, function(err, value) {}); // err === null -> valid
```

## [config](https://github.com/lorenwest/node-config)

### Installation

```bash
$ npm install config
$ mkdir config
$ vi config/default.json
```

```js
{
  // Customer module configs
  "Customer": {
    "dbConfig": {
      "host": "localhost",
      "port": 5984,
      "dbName": "customers"
    },
    "credit": {
      "initialLimit": 100,
      // Set low for development
      "initialDays": 1
    }
  }
}
```

##### Edit config overrides for production deployment

```bash
$ vi config/production.json
```

```js
{
  "Customer": {
    "dbConfig": {
      "host": "prod-db-server"
    },
    "credit": {
      "initialDays": 30
    }
  }
}
```

### Usage

```js
const config = require('config');
const dbConfig = config.get('Customer.dbConfig');
db.connect(dbConfig);
```

```bash
$ export NODE_ENV=production
$ node index.js
```

## [jsonwebtoken](https://jwt.io/)

JSON Web Token (JWT) is a compact, URL-safe means of representing claims to be transferred between two parties.

The claims in a JWT are encoded as a JSON object that is used as the payload of a JSON Web Signature (JWS) structure or as the plaintext of a JSON Web Encryption (JWE) structure, enabling the claims to be digitally signed or integrity protected with a Message Authentication Code (MAC) and/or encrypted

### Installation

```bash
$ npm install jsonwebtoken
```

### Usage

```js
const jwt = require('jsonwebtoken');

// Create a token async
jwt.sign({ foo: 'bar' }, privateKey, { algorithm: 'RS256' }, function(err, token) {
    console.log(token);
});

// Verify a token async, where getKey is a function which returns the private key
jwt.verify(token, getKey, options, function(err, decoded) {
    console.log(decoded.foo); // bar
});
```
