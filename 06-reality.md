Build upon the last example by dealing with reality; move the completion checks into the caller code.

Do not use a flow control library at this stage.


----------------------------------------------------------------------
## HINTS

Unfortunately, not everyone is trained in writing beautiful correct code like you. It's often the case that third party code we use, may fail at random times, throw errors, or even call the completion callback multiple times.

And even if it isn't often the case, we can't risk having our application enter an invalid state in production. So we must be diligent, always.

In this exercise, we will use the fetch code from exercise `03-throw-error` (before we added the completion checks in `04-duplicate-completion`), and instead add the completion checks to our calling code in our `app.js` file.

Do not worry about uncaught exceptions at this stage. We will cover that later.


----------------------------------------------------------------------


<!-- SOLUTION/ -->

``` javascript
// 06-fetch.js
// same as 03-fetch.js
```

``` javascript
// 06-app.js
var fetch = require('./fetch.js')
var completed1 = false, completed2 = false, completed3 = false
fetch(process.argv[2], function(err,data){
	// check for completion
	if (completed1)  throw new Error('first fetch already completed')
	completed1 = true

	// throw err if we have it
	if (err)  throw err

	// output result
	console.log(data)

	// Fetch the second url
	fetch(process.argv[3], function(err,data){
		// check for completion
		if (completed2)  throw new Error('second fetch already completed')
		completed2 = true

		// throw err if we have it
		if (err)  throw err

		// output result
		console.log(data)

		// Fetch the third url
		fetch(process.argv[4], function(err,data){
			// check for completion
			if (completed3)  throw new Error('third fetch already completed')
			completed3 = true

			// throw err if we have it
			if (err)  throw err

			// output result
			console.log(data)
		})
	})
})
```

<!-- /SOLUTION -->