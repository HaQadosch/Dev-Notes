Lambdas are functions run inside an execution environment/runtime. https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html 

---

Creating a Lambda
  1. Choose a function name
  2. Create your code
  3. Specify the configuration of the runtime
     1. Maximum memory size
     2. Timeout
     3. Role, using IAM

Calling a Lambda
  1. You provide an event and a context
     1. Event: the way to send the input parameters for your functions, JSON format
     2. Context: describe the execution environment and how the event is received and processed
  2. Called synchronously via the `requestResponse` invocation type.
  3. Called asynchronously via the `Event` invocation type.
     1. The invocation returns immediately while the function continues its work.
     2. Can listen for a specific event that will trigger the function call.
  4. Stateless, no session information is retained by the lambda service.



For example: a simple synchronous function computing the sum of two numbers.
```js
/* requestResponse, synchronous call.
const event = {
  "value1": 10,
  "value2": 20
}
*/
exports.handler = (event, context, callback) => {
  const result = event.value1 + event.value2
  callback(null, result)
}

exports.handler = ({value1, value2}, _, callback) => callback(null, value1 + value2)
```

```js
/* Event, asynchronous call.
const event = {
  message: 'This message is being logged.'
}
*/
exports.handler = function (event, context) {
  console.log(event.message)
  context.done()
}

exports.handler = ({message}, context) => {
  console.log(message)
  context.done()
}
```

Functions are executed without a default output device, _headless environment_, and a default logging capability in given via **CloudWatch**.

---

