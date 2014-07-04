Build upon the last example by abstracting out our stream reading logic into its own module.

Do not use a flow control library at this stage.

Remember to prevent duplicate completions in your updated fetch code.


----------------------------------------------------------------------
## HINTS

No hints this time. You got this.


----------------------------------------------------------------------


<!-- SOLUTION/ -->

``` javascript
// 05-readstream.js
module.exports = function(stream, next){
	var completed = false
	var data = ''
	response.on('data', function(chunk){
		data += chunk.toString()
	})
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
}
```

``` javascript
// 05-fetch.js
var readstream = require('./readstream.js')
module.exports = function(url, next){
	var completed = false
	var request = require('http').get(url, function(response) {
		readstream(request, function(err, data){
			if ( completed === false ) {
				completed = true
				return next(err, data)
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
// 05-app.js
// same as 03-app.js
```

<!-- /SOLUTION -->