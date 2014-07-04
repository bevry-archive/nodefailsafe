Output the result of 3 URLs (given to you as command line arguments) in the order they are specified.

Do not use a flow control library at this stage.

If an error occurs, ignore it. We will cover error handling in a later exercise.


----------------------------------------------------------------------
## HINTS

Servers may output data in chunks, meaning that there could be multiple data events emitted. For each request, you will need to concatenate the result of data event into a data variable that is then used for that request.

As you will be handling 3 requests, you will want to move your "fetching" logic into it's own function, that is then called for each request.

Remember from learnyounode:

- Command line arguments are provided within the `process.argv` array
- You can use `http.get` to get the data of a URL, more documentation is available on `http.request` if you get stuck

For now you can expect each request to work successfully, enabling you to safely ignore errors. Later, this will not be the case, but do not worry about it now, we will build on error handling later.

For now, do not use a flow control library, just call our created "fetch" function multiple times using "callback hell" style code. We'll build on this later.

----------------------------------------------------------------------


<!-- SOLUTION/ -->

``` javascript
// 01-app.js
var fetch = function(url, next){
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

// Fetch the first url
fetch(process.argv[2], function(err,data){
	// ignore error

	// output result
	console.log(data)

	// Fetch the second url
	fetch(process.argv[3], function(err,data){
		// ignore error

		// output result
		console.log(data)

		// Fetch the third url
		fetch(process.argv[4], function(err,data){
			// ignore error

			// output result
			console.log(data)
		})
	})
})
```

<!-- /SOLUTION -->