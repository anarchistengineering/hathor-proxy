# Hathor Proxy

A wrapper around H2O2 for Hathor allowing simple proxy routes to be surface.

Route documentation can be found at https://github.com/hapijs/h2o2

## Install:

```
npm install hathor-proxy
```

## Example:

config/config.js

```js
module.exports = {
  server: {
    plugins: [
      require('hathor-proxy')
    ],
    ...Typical stuff here...
  }
}
```

routes/httpbin/index.js

```js
module.exports = {
  method: ['GET', 'POST', 'PUT', 'DELETE'],
  path: '/api/httpbin/{stuff*}',
  auth: true,
  config: {
    description: 'Basic proxy route to httpbin.',
    notes: 'Sends whatever it gets over to http://httpbin.org.',
    tags: ['api'],
  },
  handler: {
    proxy: {
      passThrough: true,
      mapUri(req, callback){
        return callback(null, `http://httpbin.org/${req.params.stuff}`)
      }
    }
  }
};
```

Start your app, hit /api/httpbin/get and see the glory!
