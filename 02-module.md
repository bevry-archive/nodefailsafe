Build upon the last lesson by abstracting out the fetch function into its own module.

Do not use a flow control library at this stage.

If an error occurs, ignore it. We will cover error handling in a later exercise.


----------------------------------------------------------------------
## HINTS

If you are new to modules, check out learnyouode/make_it_modular (lesson 06).

Remember, node conventions dictate that callbacks are to have the first argument to be the error (or `null` if no error), with subsequent arguments being the results. E.g. `next(null, data)`.


----------------------------------------------------------------------


<!-- SOLUTION/ -->

``` javascript
// 02-fetch.js
module.exports = function(url, next){
	var request = require('http').get(url, function(response) {
		var data = ''
		response.on('data', function(chunk){
			data += chunk.toString()
		})
		response.on('end', function(){
			return next(null, data)
		})
	})
}
```

``` javascript
// 02-app.js
var fetch = require('./fetch.js')
fetch(process.argv[2], function(err,data){
	// ignore error
	console.log(data)

	// Fetch the second url
	fetch(process.argv[3], function(err,data){
		// ignore error
		console.log(data)

		// Fetch the third url
		fetch(process.argv[4], function(err,data){
			// ignore error
			console.log(data)
		})
	})
})
```

<!-- /SOLUTION -->