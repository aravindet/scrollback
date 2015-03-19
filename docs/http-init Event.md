##"http/init" Event
If an app want to register some URL handler then it can listen for "http/init" event
###Event
```javascript
	core.on("http/init", function(payload, callback) {
		payload.push({
			get: {
				"/r/twitter/*": function(req, res, next) {
					//handle response here
				},
				......
			},
			post: {
				"/r/twitter/post/*": function(req, res, next) {
					//code.....
				}
			}
		});
        callback(null, payload);
	}, 1000/*priority*/);

```

###Object structure
```javascript
{
 
	get: {
		"pattern1": {function(req, res, next)},
		"pattern2": {function(req, res, next)}
	},
	post: {
		"pattern1": {function(req, res, next)},
		"pattern2": {function(req, res, next)}
	}	
	
}
```
Each pattern should be unique. Intersection between regular expressions will be resolved by priority of event.
See [[Events]]


