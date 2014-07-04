Build upon the last lesson by providing our callback with the error that occurs, and throw it in your application code.

Sometimes things go wrong. One of the three requests may fail. Crash the application by throwing the error.

Do not use a flow control library at this stage.

----------------------------------------------------------------------
## HINTS

Calling `http.get` (like `http.request)`) returns a `http.ClientRequest` instance. `http.ClientRequest` is a class that inherits from the `events.EventEmitter` class.

Event Emitters in Node.js are objects (or rather class instances) that can have listeners bound and emitted on the instance. This means we can do `person.on('thirsty', drinkWaterFunction)` to listen to the `thirsty` event on our person instance, and drink some water with a function we defined elsewhere.

Event Emitters are important, as they are a common paradigm for accomplishing background tasks in Node.js.

So far, the code we've written has been very procedural. A very "do this, then this, then this" style. However, events emtiters, allow us to do a "listen for this, and when it eventually happens, then do this". In fact, we've already used an event emitter when listening to the `response` argument provided by the `http.get` callback, which is an instance of the `http.ClientResponse` class. It's an event emitter that exposes the `data` and `end` events, because it is a stream.

However, event emitters also play another important role. They are great for emitting background errors. That is, errors that occur outside our normal procedural synchronous flow. In the case here, if the request fails, that could happen at any point in our application, therefore there is no clear place to emit it. Instead, we should emit an `error` event on the `http.ClientRequest` instance.

Node does something great for us, where if an event emitter emits an `error` event, that is not listened to, the application will throw the error. This ensures that we are alerted of uncaught exceptions and they do not slip through the cracks. This forces us to deal with errors correctly.

At this point, we just want our fetch module to return any emitted error to our completion callback if it occurs. Then have our app module, throw the error if it was given to us. It's not the best way of doing things, as it still causes our application to crash, but we will build on this concept later.


----------------------------------------------------------------------


<!-- SOLUTION/ -->

``` javascript
// 03-fetch.js
module.exports = function(url, next){
	var request = require('http').get(url, function(response) {
		var data = ''
		response.on('data', function(chunk){
			data += chunk.toString()
		});
		response.on('end', function(){
			return next(null, data)
		})
		response.on('error', function(err){
			return next(err)
		})
	})
	request.on('error', function(err){
		return next(err)
	})
}
```

``` javascript
// 03-app.js
var fetch = require('./fetch.js')
fetch(process.argv[2], function(err,data){
	if (err)  throw err
	console.log(data)

	// Fetch the second url
	fetch(process.argv[3], function(err,data){
		if (err)  throw err
		console.log(data)

		// Fetch the third url
		fetch(process.argv[4], function(err,data){
			if (err)  throw err
			console.log(data)
		})
	})
})
```

<!-- /SOLUTION -->