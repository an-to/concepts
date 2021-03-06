Web APIs are interfaces for other applications instead of users. They don't have a visual interface - only data. We often refer to these blocks of data returned from web APIs as _resources_. A resource maps very closely to what you understand as a database entity/table, a type of thing you are storing.


## REST and JSON

REST stands for Representational State Transfer and was coined by Roy Fielding in his PhD dissertation titled [Architectural Styles and the design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm). Chapter 5 describes stateless entities (what we're storing in our database) as resources. It also describes how we can refer to these resources using a uniform indicator: the URL. This is why you will see URLs describe individual resources like this: `/widgets/213/characteristics`.

JSON, JavaScript Object Notation, is a string representation for JavaScript objects. We can use `JSON.stringify(jsObj)` to convert (serialise) a JavaScript object to a string, and `JSON.parse(jsonString)` to convert (deserialise) a JSON string back to a JavaScript object. Fortunately, Express.js can do this for us as we'll see below. This is the recommended (and most common) format for REST API requests and responses.


## Exposing web API routes

Routes are exposed for web APIs in the same way they are for routes that return other web assets. The main difference is they accept and respond with JSON. 

There are a few different things you should notice about the code samples below. The endpoints they define use an `:id` syntax and `req.params` rather than `req.query` we've seen before. Using route params is more explicit and much more common with REST web APIs.

### Returning JSON

We use `res.json` to return JSON to the caller:

  ```js
  // server.js
  var widgets = require('./routes/widgets')
  app.use('/widgets', widgets)

  // routes/widgets.js
  var express = require('express')
  var router = express.Router()

  router.get('/:id', function (req, res) {
    var id = req.params.id
    res.json({id: id, name: 'Best widget'})
  })
  
  module.exports = router
  ```

Notice how it just uses a normal JavaScript object. The `.json()` method will take care of serialising the object to JSON.

Don't forget Express.js is also capable of receiving JSON in the request body using the `body-parser` middleware module. This is common when using the POST and PUT verbs to insert and update resources.

```js
// server.js
var express = require('express')
var bodyParser = require('body-parser')
var app = express()
app.use(bodyParser.json())
```

