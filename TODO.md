# Todo

## Things still to cover

- Addressing callback hell for serial execution
- Addressing complexity of callback hell for parallel execution
- Handling uncaught exceptions, why throw is evil
- Using domains to isolate uncaught exceptions (more or less safely)
- The need for domains, why try...catch doesn't work all the time
- Why `err.stack` is trimmed when inside a new tick
