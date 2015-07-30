# RESTfulError
When you implement a RESTful endpoint in Node.js you have to handle the error in response.
For example if you use expressjs to develop a webservice or a web application you follow the pattern reported below:
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

## Errors

### BAD_REQUEST
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
400 | Bad Request | BAD_REQUEST | [RFC7231, Section 6.5.1](http://tools.ietf.org/html/rfc7231)

##### Description:
The request could not be understood by the server due to malformed syntax. The client SHOULD NOT repeat the request without modifications.

### UNAUTHORIZED
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
401 | Unauthorized | UNAUTHORIZED | [RFC7235, Section 3.1](http://tools.ietf.org/html/rfc7235)

##### Description:
The request requires user authentication. The response MUST include a WWW-Authenticate header field containing a challenge applicable to the requested resource. The client MAY repeat the request with a suitable Authorization header field. If the request already included Authorization credentials, then the 401 response indicates that authorization has been refused for those credentials. If the 401 response contains the same challenge as the prior response, and the user agent has already attempted authentication at least once, then the user SHOULD be presented the entity that was given in the response, since that entity might include relevant diagnostic information.

