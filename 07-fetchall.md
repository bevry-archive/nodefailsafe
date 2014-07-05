Build upon the last example by abstracting out the fetching of the three URLs into a generic `fetchall` module.

The `fetchall` module should accept a signature of `(urls, next)`, where `urls` is an array of the urls we want to fetch the data for, and `next` is the completion callback accepting the signature `(err, results)`, where `err` is a possible error, and `results` is an array of the result data of each url, in the order the URLs were given (the order is important, be sure to remember this).

To make our lives easier, the following is recommended:

1. URLs should be fetched in parallel (all at once), rather than serially (one after the other)
2. Errors when fetching a URL should be thrown at this stage (we will build on this later)
3. Do not worry about the use case of receiving zero URLs at this stage (we will build on it later)

Do not use a flow control library at this stage.

Be sure to check for multiple completion events of our `fetchall` module when using it in our `app` module.


----------------------------------------------------------------------
## HINTS

A lot is asked of you in this step, however the changes actually simplify our code thus far, but may be a bit difficult to grasp. Here are a few hints.

You will need to keep track of which URLs have completed, and which have not. You should throw an error if fetching a URL has emitted the completion callback more than once.

You will need to keep track of the results of each URLs, and make sure you return the results as an array in the order they were received.

You will need to keep track of how many URLs have completed, and if the completion count is the same as the amount of URLs we were given, then it is time to call the completion callback.

The easiest way of accomplishing this is to maintain a count variable for the amount of completed URL fetches, as well as a hash object containing the fetched result of our URLs.

To get the result array in the same order as we were provided the URLs, we could create a new results array, then cycle through our received URLs, and add the result data (from our hash object), to the array; recreating the order we received them. We could even use `Array.prototype.map` to simplify this cycling.

If this is over your head, ask a mentor or try whatever way you think is best. It's okay to make mistakes, they provide a lesson of what not to do, which is equally important in the grand scheme of things.


----------------------------------------------------------------------
## BONUS POINTS

Once you've finished the exercise, there are a few bonus points you can get.

There are several interesting things going on in this exercise, can you answer the following?

1. Why would throwing an error in our `fetchall` module cause an unexpected state?

2. What would happen if our `fetchall` module received zero URLs?

3. Why do we still need to check for multiple completions of our `fetchall` module when calling it in our `app` module?

Once you've tried answered those questions, validate your assumptions by comparing them with the best practices provided by the [Error Handling in Node.js](http://joyent.com/developers/node/design/errors) guide issued by Joyent.


----------------------------------------------------------------------


<!-- SOLUTION/ -->

``` javascript
// 07-fetch.js
// same as 03-fetch.js
```

``` javascript
// 07-fetchall.js
var fetch = require('./fetch.js')
module.exports = function(urls, next){
	var completedCount = 0
	var completedHash = {}
	urls.forEach(function(url){
		completedHash[url] = false
		fetch(url, function(err,data){
			// check for completion
			if ( completedHash[url] )  throw new Error('fetch of '+url+' already completed')
			completedHash[url] = data
			++completedCount)

			// put our application in an unexpected state by throwing the error if we have it at this stage
			if (err)  throw err

			// fire the completion callback if all urls are done
			if ( completedCount === urls.length ) {
				var results = urls.map(function(url){
					return completedHash[url]
				})
				return next(null, results)
			}
		})
	})
}
```

``` javascript
// 07-app.js
var fetchall = require('./fetchall.js')
var urls = process.argv.slice(2)
var completed = false
fetchall(urls, function(err, results){
	// check for completion
	if (completed)  throw new Error('our application is in an unexpected state')
	completed = true

	// throw err if we have it
	if (err)  throw err

	// output each result on a newline
	// .join saves us doing a forEach loop
	console.log(results.join('\n'))
})
```

<!-- /SOLUTION -->