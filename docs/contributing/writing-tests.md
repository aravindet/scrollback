#Testing
##Unit
While writing new module ```foo``` make a file ```foo-test.js``` inside that module and write unit test using mocha for the module.

Add ``` require("../foo/foo-test.js") ``` to ```test/test.js```

Example: ```recommendation-test.js```
``` javascript

/* jshint mocha: true */
var core = new (require('ebus'))();
var assert = require('assert');
var config = require('../server-config-defaults.js');
require('./recommendation.js')(core, config.recommendation);
describe('Recommendation tests', function() {

	it("getRooms API: (get featured rooms)", function(done) {
		core.emit("getRooms", {featured: true}, function(err, results) {
			assert.equal(results.ref instanceof Array, true, "Should update query( ref is not an array)");
			assert.equal(results.featured, true, "Should have featured property");
			done();
		});
	});

	it("getRooms API: (don't get featured rooms)", function(done) {
		core.emit("getRooms", {ref: "scrollback"}, function(err, results) {
			assert.equal(results.ref instanceof Array, false, "Should not update query( ref is an array)");
			done();
		});
	});
});
```

##Selenium
Create a new file inside ```test/selenium```
Example: ``` embed-test.js ```
``` javascript
var assert = require('assert'),
timeout = 25000,
webdriver = require('browserstack-webdriver'),
q = require('q'),
testUtils = require('./testUtils.js');

module.exports = function(capabilities, options) {
	describe('Navigating to embed page url: ' + options.id, function() {
		this.timeout(4 * timeout);
		var driver, server = options.server;

		it("embed script testing", function(done) {
			console.log("embed page testing");
			driver = testUtils.openUrl(capabilities, server,"s/test-embed.html?room=testroom1&minimize=false");
			q.delay(timeout).then(function() {
				return driver.switchTo().frame(0); //if there is only one frame
			}).then(function() {
				return driver.findElement(webdriver.By.css('.user-area')).isDisplayed();
			}).then(function(t) {
				assert.equal(t, true, "page is not loaded");
				driver.quit();
				done();
			});
		});

		it("embed script testing(minimized)", function(done) {
			console.log("minimized embed page testing");
			driver = testUtils.openUrl(capabilities, server,"s/test-embed.html?room=testroom1&minimize=true");
			q.delay(timeout).then(function() {
				return driver.switchTo().frame(0);
			}).then(function() {
				return driver.findElement(webdriver.By.css('.user-area')).isDisplayed();
			}).then(function(t) {
				assert.equal(t, false, "page is not loaded");
				driver.quit();
				done();
			});
		});
	});
};

```
Add this new file to ```test/config.sample.js``` and ```test/config.js``` and Then run ``` mocha test/selenium/test.js``` from scrollback directory. 
```test/selenium/test-utils.js``` can be used common operations like sign-in, sign-out etc.

