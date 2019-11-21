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

Back end serverless application, each lambda is associated with a resource management role, via **AIM**.
Each interaction is described by the relationship between the resources involved.
The design is simplified from a top down orchestration to a bottom-up choreography.

The serverless backend is an **event driven application** that takes advantage of all the best practices from distributed systems:
  * Avoid synchronous transactions across multiple resources
  * Design each function to work independently
    * Via Event Subscriptions
    * With eventual consistency of Data
      * Data is not in sync across all resources used by the back end
      * The data will eventually converge over time to the last updated stage.


`Invoke API` is a lambda API handling the synchronous or asynchronous call of lambdas.
  1. Run the security check,
  2. Check permission to invoke the lambda are met
  3. Amazon Cognito creates temporary AWS credentials that allow you to execute

A lambda can also be called directly by a call to its full URL.

The `Amazon API Gateway` add: 
  * caching results capabilities
  * throttlintg
  * managing developer keys
  * generate SDKs

It allows a layer between the app and the lambdas.


# First Lambda Function

1. [Login](https://myapps.microsoft.com/signin/AWS-Console/a6712947-1792-4555-8ee9-04fec4988123?tenantId=58dea911-047d-43fb-9342-19ca17665d67)
2. Select `AAD-CX-SolAdmin` from `Account: ecs-cx (088417944319)`
3. In AWS Services, Select Lambdas
4. Click `Create function`
5. No template, Choose `Author from scratch`
   1. Name: xb_greetingsOnDemand
   2. Runtime: nodejs 10.x
   3. Click create function
6. Choose a trigger. Since we'll be calling the lambda directly, no need of a trigger. Skip.
7. Description: Returns greetings when you ask for them.
8. Handler, Which function inside your lambda should be called. `filename.function` in our case `index.handler`
9. IAM Role: It makes sense to create a basic role that you can reuse instead of a new role for each new lambda
   1.  Choose Create new role
   2.  Give it a fancy name: xb_basicExecutionRole
   3.  Leave the policy template empty
10. Basic Setting
    1.  Memory: 128MB is more than enough
    2.  Timeout: 3 sec. Just to prevent out of control bills.
11. VPC: no VPC
12. Click Save.
13. Test your function:
    1.  One event with {"name": "John", ...}
    2.  Another event without "name" param.
    3.  check you have the result expected in both cases.
    4.  Applause.

```js
// xb_AWSLambdaInAction_Chapter2_greetingsOnDemand.js
console.log('Loading function')

exports.handler = ({ name = 'World', ...event }, context, callback) => {
  console.log('Received event', JSON.stringify(event, null, 2))
  console.log('Received context', JSON.stringify(context, null, 2))
  console.log({ event, context })
  const greetings = `Hello ${name}`
  console.log({ greetings })
  callback(null, greetings)
}
```

You can have multiple functions in the code and multiple files if you upload a zip archive, but only the function specified in the handler is called by AWS Lambda. The other functions can be used internally in the code.

## Skeleton of a Lambda
  * Initialisation step. Put code that can be executed only once, open a connection to a database, initialise cache, load configuration data.
  * Amazon Cloudwatch Logs. All the `console.log` go there.
  * `event` and `context` are created and passed to the function.
  * The logic of the function depends on the content of `event` and `context`.
  * End the fonction with a call to the `callback` following the Node standard:
    * First parameter is for errors, if no errors then `null`
    * Second parameter is the data.


## Executing through the Lambda API

Done via the `AWS Lambda Invoke API`. Install the AWS CLI and follow the configuration instruction https://foresttechnologies.atlassian.net/wiki/spaces/AWS/pages/1507688563/CLI+and+AWS+login .

```sh
> aada login -n --profile aad
> aws lambda list-functions --profile aad --region eu-central-1
```

```json
{
  "FunctionName": "xb_AWSLambdaInAction_Chapter2_greetingsOnDemand",
  "FunctionArn": "arn:aws:lambda:eu-central-1:088417944319:function:xb_AWSLambdaInAction_Chapter2_greetingsOnDemand",
  "Runtime": "nodejs12.x",
  "Role": "arn:aws:iam::088417944319:role/service-role/xb_BasicLambda",
  "Handler": "index.handler",
  "CodeSize": 311,
  "Description": "Display greetings",
  "Timeout": 3,
  "MemorySize": 128,
  "LastModified": "2019-11-19T10:45:06.307+0000",
  "CodeSha256": "W2MkKgUVDJB/2wOsV23D7uXgKfJwWtEHK9vA8VlFBIo=",
  "Version": "$LATEST",
  "VpcConfig": {
      "SubnetIds": [],
      "SecurityGroupIds": [],
      "VpcId": ""
  },
  "Environment": {
      "Variables": {
          "EnvKey1": "EnvValue1"
      }
  },
  "TracingConfig": {
      "Mode": "PassThrough"
  },
  "RevisionId": "1c8183e1-5881-4406-bbf8-fbda3cc3729b"
}
```

Call the function via the following command:
```sh
> aws lambda invoke --function-name xb_AWSLambdaInAction_Chapter2_greetingsOnDemand --payload '{ "name": "Gandalf" }' output.txt --profile aad --region eu-central-1

{
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
}
```

A `output.txt` file has been created:
```txt
"Hello Gandalf"
```

Same when there is no name in the payload:

```sh
aws lambda invoke --function-name xb_AWSLambdaInAction_Chapter2_greetingsOnDemand  --payload '{}' output_NoName.txt  --profile aad --region eu-central-1

{
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
}
```

A `output.txt` file has been created:
```txt
"Hello Gandalf"
```