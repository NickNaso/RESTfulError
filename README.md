# RESTfulError
When you implement a RESTful endpoint in Node.js you have to handle the error in response.
For example if you use expressjs to develop a webservice or a web application you use the pattern reported below:
```javascript
//Import some required modules
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
Every time your system generate an error you handle it and the corresponding response. The idea is to have a generic Error object that encapsulate all types of error and dispatch it to a centralized error middleware that will provide to generate the response.

