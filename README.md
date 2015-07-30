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

### FORBIDDEN
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
403 | Forbidden | FORBIDDEN | [RFC7231, Section 6.5.3](http://tools.ietf.org/html/rfc7231)

##### Description:
The server understood the request, but is refusing to fulfill it. Authorization will not help and the request SHOULD NOT be repeated. If the request method was not HEAD and the server wishes to make public why the request has not been fulfilled, it SHOULD describe the reason for the refusal in the entity. If the server does not wish to make this information available to the client, the status code 404 (Not Found) can be used instead.

### NOT_FOUND
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
404 | Not Found | NOT_FOUND | [RFC7231, Section 6.5.4](http://tools.ietf.org/html/rfc7231)

##### Description:
The server has not found anything matching the Request-URI. No indication is given of whether the condition is temporary or permanent. The 410 (Gone) status code SHOULD be used if the server knows, through some internally configurable mechanism, that an old resource is permanently unavailable and has no forwarding address. This status code is commonly used when the server does not wish to reveal exactly why the request has been refused, or when no other response is applicable.

### METHOD_NOT_ALLOWED
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
405 | Method Not Allowed | METHOD_NOT_ALLOWED | [RFC7231, Section 6.5.5](http://tools.ietf.org/html/rfc7231)

##### Description:
The method specified in the Request-Line is not allowed for the resource identified by the Request-URI. The response MUST include an Allow header containing a list of valid methods for the requested resource.

### NOT_ACCEPTABLE
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
406 | Not Acceptable | NOT_ACCEPTABLE | [RFC7231, Section 6.5.6](http://tools.ietf.org/html/rfc7231)

##### Description:
The resource identified by the request is only capable of generating response entities which have content characteristics not acceptable according to the accept headers sent in the request.

### PROXY_AUTHENTICATION_REQUIRED
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
407 | Proxy Authentication Required | PROXY_AUTHENTICATION_REQUIRED | [RFC7235, Section 3.2](http://tools.ietf.org/html/rfc7235)

##### Description:
This code is similar to 401 (Unauthorized), but indicates that the client must first authenticate itself with the proxy. The proxy MUST return a Proxy-Authenticate header field containing a challenge applicable to the proxy for the requested resource. The client MAY repeat the request with a suitable Proxy-Authorization header field.

### REQUEST_TIMEOUT
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
408 | Request Timeout | REQUEST_TIMEOUT | [RFC7231, Section 6.5.7](http://tools.ietf.org/html/rfc7231)

##### Description:
The client did not produce a request within the time that the server was prepared to wait. The client MAY repeat the request without modifications at any later time.

### CONFLICT
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
409 | Conflict | CONFLICT | [RFC7231, Section 6.5.8](http://tools.ietf.org/html/rfc7231)

##### Description:
The request could not be completed due to a conflict with the current state of the resource. This code is only allowed in situations where it is expected that the user might be able to resolve the conflict and resubmit the request. The response body SHOULD include enough information for the user to recognize the source of the conflict. Ideally, the response entity would include enough information for the user or user agent to fix the problem; however, that might not be possible and is not required.

### GONE
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
410 | Gone | GONE | [RFC7231, Section 6.5.9](http://tools.ietf.org/html/rfc7231)

##### Description:
The requested resource is no longer available at the server and no forwarding address is known. This condition is expected to be considered permanent. Clients with link editing capabilities SHOULD delete references to the Request-URI after user approval. If the server does not know, or has no facility to determine, whether or not the condition is permanent, the status code 404 (Not Found) SHOULD be used instead. This response is cacheable unless indicated otherwise.

### LENGTH_REQUIRED
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
411 | Length Required | LENGTH_REQUIRED | [RFC7231, Section 6.5.10](http://tools.ietf.org/html/rfc7231)

##### Description:
The server refuses to accept the request without a defined Content- Length. The client MAY repeat the request if it adds a valid Content-Length header field containing the length of the message-body in the request message.

### PRECONDITION_FAILED
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
412 | Precondition Failed | PRECONDITION_FAILED | [RFC7232, Section 4.2](http://tools.ietf.org/html/rfc7232)

##### Description:
The precondition given in one or more of the request-header fields evaluated to false when it was tested on the server. This response code allows the client to place preconditions on the current resource metainformation (header field data) and thus prevent the requested method from being applied to a resource other than the one intended.

### REQUEST_ENTITY_TOO_LARGE
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
413 | Request Entity Too Large | REQUEST_ENTITY_TOO_LARGE | [RFC7231, Section 6.5.11](http://tools.ietf.org/html/rfc7231)

##### Description:
The server is refusing to process a request because the request entity is larger than the server is willing or able to process. The server MAY close the connection to prevent the client from continuing the request.

### REQUEST_URI_TOO_LONG
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
414 | Request-URI Too Long | REQUEST_URI_TOO_LONG | [RFC7231, Section 6.5.12](http://tools.ietf.org/html/rfc7231)

##### Description:
The server is refusing to service the request because the Request-URI is longer than the server is willing to interpret. This rare condition is only likely to occur when a client has improperly converted a POST request to a GET request with long query information, when the client has descended into a URI black hole of redirection (e.g., a redirected URI prefix that points to a suffix of itself), or when the server is under attack by a client attempting to exploit security holes present in some servers using fixed-length buffers for reading or manipulating the Request-URI.

### UNSUPPORTED_MEDIA_TYPE
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
415 | Unsupported Media Type | UNSUPPORTED_MEDIA_TYPE | [RFC7231, Section 6.5.13](http://tools.ietf.org/html/rfc7231)

##### Description:
The server is refusing to service the request because the entity of the request is in a format not supported by the requested resource for the requested method.

### REQUESTED_RANGE_NOT_SATISFIABLE
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
416 | Requested Range Not Satisfiable | REQUESTED_RANGE_NOT_SATISFIABLE | [RFC7233, Section 4.4](http://tools.ietf.org/html/rfc7233)

##### Description:
A server SHOULD return a response with this status code if a request included a Range request-header field, and none of the range-specifier values in this field overlap the current extent of the selected resource, and the request did not include an If-Range request-header field. (For byte-ranges, this means that the first- byte-pos of all of the byte-range-spec values were greater than the current length of the selected resource.) When this status code is returned for a byte-range request, the response SHOULD include a Content-Range entity-header field specifying the current length of the selected resource. This response MUST NOT use the multipart/byteranges content-type.

### EXPECTATION_FAILED
Status code | Error name | Error type | Reference
----- | ------------------------------------------------ | ------------------------------------------------ | -----------------------------
417 | Expectation Failed | EXPECTATION_FAILED | [RFC7231, Section 6.5.14](http://tools.ietf.org/html/rfc7231)

##### Description:
The expectation given in an Expect request-header field could not be met by this server, or, if the server is a proxy, the server has unambiguous evidence that the request could not be met by the next-hop server.

