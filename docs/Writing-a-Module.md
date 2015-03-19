Say you want to write a module named foo.

## Getting started ##
- First, fork and check out the repository. Set it up on your machine using tools/install.sh
- Create a folder foo in the root
- If your module has server-side code, create a file foo/foo.js with the content `module.exports = function(core, config) { ... }`. This function will be called when Scrollback starts up, and is the point of entry to your module. In /index.js, add your plugin to the list.
- If your plugin has client-side code, create a file foo/foo.js with the content `module.exports = function(core, config) { ... }`. This function will be called when Scrollback loads, and is the point of entry to your module. In /client.js, require() your plugin.
- Write unit test for your module using mocha and and add the test file to test/test.js. see [testing](../Testing)

## Common tasks ##
Most of the work is done by attaching handlers to the core event dispatcher.

### Responding to text messages (server) ###
```javascript
core.on('text', function(textAction, next) {
  /* Do your work here */
  next();
}, prio);
```

### Sending text messages (server) ###
```javascript
core.emit('text', textAction, callback);
```

### Responding to text messages (client) ###
```javascript
core.on('text-dn', function(textAction, next) {
  /* Do your work here */
  next();
}, prio);
```

### Sending text messages (server) ###
```javascript
core.emit('text-up', textAction, callback);
```

### Responding to navigation ###
```javascript
core.on('navigate', function(navigationState, next) {
  /* Do your work here */
  next();
}, prio);
```
