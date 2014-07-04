# Todo

## Done

- Catch event errors and return them to a callback
- Throw errors provided by a callback
- Prevent unexpected flow by ensuring completions only occur once
- Dealing with parallel execution and maintaing serial results


## Things being worked on

- Creating an unexpected state by throwing errors
- Creating an unexpected state by not calling a callback
- Debugging our application when a callback is not called


## Things still to cover

- Addressing callback hell for serial execution
- Addressing complexity of callback hell for parallel execution
- Handling uncaught exceptions, why throw is evil
- Using domains to isolate uncaught exceptions (more or less safely)
- The need for domains, why try...catch doesn't work all the time
- Why `err.stack` is trimmed when inside a new tick
