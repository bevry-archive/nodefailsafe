Build upon the last example by preventing our completion callback from firing twice.

In the case that our url completes, then errors, we do not want our completion callback to execute twice, causing an unexpected state, and repeated logic flows.

Do not use a flow control library at this stage.


----------------------------------------------------------------------
## HINTS

As we are still not using a flow control library at this stage, using a boolean flag in our fetch logic of whether or not we have completed yet will be the easiest way of accomplishing this.


----------------------------------------------------------------------


<!-- SOLUTION/ -->

``` javascript
// 04-fetch.js
module.exports = function(url, next){
	var completed = false
	var request = require('http').get(url, function(response) {
		var data = ''
		response.on('data', function(chunk){
			data += chunk.toString()
		});
		response.on('end', function(){
			if ( completed === false ) {
				completed = true
				return next(null, data)
			}
		})
		response.on('error', function(err){
			if ( completed === false ) {
				completed = true
				return next(err)
			}
		})
	})
	request.on('error', function(err){
		if ( completed === false ) {
			completed = true
			return next(err)
		}
	})
}
```

``` javascript
// 04-app.js
// same as 03-app.js
```

<!-- /SOLUTION -->