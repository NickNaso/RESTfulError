# RESTfulError
When you implement a RESTful endpoint in Node.js you have handle the error in response.
For example if you use expressjs to develop a webservice or a web application

```javascript
var BookCtrl = require('controllers/Book');
var express = require('express');
var app = express();
//Some other configuration for the express app
//List of routes
app.get("/books/:id", function(req, res, next){
    BookCtrl.findBookById(req.params.id, function(err, res){
        if(err){
          res.status(500).json({error: "Message to report error"});
        } else {
            res.status(200).json(res);
        }
    });
});
```
Every time your system generate an error you handle it and the correspinding response. The idea is to have a generi Error object that encapsulate all error types and dispacht it to a centralized error middleware.
