##Libs##
libs are small pieces of code those are used by multiple modules.
###email.js###
Send email from any module.
```javascript
var email = (new require('../lib/email.js')(email.auth)); 
```
####functions
  - email.send(from, to, subject, html, callback)

### SbError.js ###

SbError provides the developer the option to add a custom payload as part of an Error that is thrown. The standard Javascript Error object provides just an error message as a payload, however at times it becomes useful to have more information relevant to what caused the error to be included in the Error Object. 

SbError extends the standard Javascript Error object and is backward compatible with it. Thus, it can be used wherever the standard Javascript Error is used.

#### Usage:
```javascript
var SbError = require('../lib/SbError.js');

try {
    function divide(dividend, divisor) {
        if (divisor === 0) throw new SbError('ERR_DIVIDE_BY_0', {source: 'divide.js', foo: "baz"})
    }
} catch (e) {
      console.log("Caught error ", e.message, "Source of error is ", e.source, "e.foo =", e.foo);
}

```
###logger.js###
It provides a single logger for the whole application.
It depends on the ```process.node.env``` variable. ```process.node.env``` should be ```production``` for production and ```dev``` for development. 
```javascript
var log = require('../lib/logger.js');
```
All logs will be shown in development environment.
####functions
  - **log.i(s)** - Info log. Color: ```green```
  - **log(s)** - Analogous to log.i(s).
  - **log.w(w)** - Warn message. Color ```yellow```
  - **log.setEmailConfig(config)** - Sets the email configuration used to send emails on error. 
     
     ```javascript
        {
            to: "email@example.com",
            from: "example@example.com",
            auth: {
               user: "...",
               pass: "...."
            }
        }
      ```
  - **log.d(d)** - Debug message. It can be used to print large JSON objects and will not be logged in production environment. Color: ```blue```
  - **log.e(e)** - Error message. Email will be sent to ```config.to``` only if it is in production environment. Color: ```red```

Use log.e(e) for **critical errors**. Example: database errors, validation errors etc.
  
   

