# Application Load Balancer

Elastic Load Balancing invokes your Lambda function synchronously with an event that contains the request body and metadata.

## Limits

- The Lambda function and target group must be in the same account and in the same Region.
- The maximum size of the request body that you can send to a Lambda function is 1 MB. For related size limits, see HTTP header limits.
- The maximum size of the response JSON that the Lambda function can send is 1 MB.
- WebSockets are not supported. Upgrade requests are rejected with an HTTP 400 code.
- Local Zones are not supported.

## Input

### Input Fields

`targetGroupArn` (String)
: Target group arn for your Lambda function

`httpMethod` (String)
: The HTTP method used. Valid values include: DELETE, GET, HEAD, OPTIONS, PATCH, POST, and PUT.

`path` (String)
: Http request path

`multiValueQueryStringParameters` (Optional, map of string to list of strings)
: If you enable multi-value headers, the load balancer uses both key values sent by the client and sends you an event that includes query string parameters using multiValueQueryStringParameters

`queryStringParameters` (Optional, map of string to string)
: Query string parameters sent by the client.

`headers` (Optional, map of string to string)
: Http header sent by the client.

`isBase64Encoded` (Boolean)
: A Boolean flag to indicate if the request body is Base64-encoded.

`body` (Optional, string)
: The request body sent by the client.

### Getting the correlation id

JSON path to correlation id: `headers."x-amzn-trace-id"`

### Generating sample events via SAM CLI

```shell
# Gibhub PR (https://github.com/aws/aws-sam-cli/pull/3670)
```

### Example Event

```json title="Application Load Balance GET request"
{
  "requestContext": {
    "elb": {
      "targetGroupArn": "arn:aws:elasticloadbalancing:us-east-2:123456789012:targetgroup/lambda-279XGJDqGZ5rsrHC2Fjr/49e9d65c45c6791a"
    }
  },
  "httpMethod": "GET",
  "path": "/lambda",
  "queryStringParameters": {
    "query": "1234ABCD"
  },
  "multiValueQueryStringParameters": { "myKey": ["val1", "val2"] },
  "headers": {
    "accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8",
    "accept-encoding": "gzip",
    "accept-language": "en-US,en;q=0.9",
    "connection": "keep-alive",
    "host": "lambda-alb-123578498.us-east-2.elb.amazonaws.com",
    "upgrade-insecure-requests": "1",
    "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36",
    "x-amzn-trace-id": "Root=1-5c536348-3d683b8b04734faae651f476",
    "x-forwarded-for": "72.12.164.125",
    "x-forwarded-port": "80",
    "x-forwarded-proto": "http",
    "x-imforwards": "20"
  },
  "body": "Test",
  "isBase64Encoded": false
}
```

## Response

### Response Fields

`isBase64Encoded` (Boolean)
: A Boolean flag to indicate if the response body is Base64-encoded.

`statusCode` (Integer)
: The HTTP status code.

`statusDescription` (String)
: The HTTP status description.

`headers` (Optional, map of string to string)
: Http header to be sent in the response.

`body` (Optional, string)
: The response body sent by the server.

### Response Examples

```json title="Example 200 html response"
{
    "statusCode": 200,
    "statusDescription": "200 OK",
    "isBase64Encoded": False,
    "headers": {
        "Content-Type": "text/html"
    },
    "body": "<h1>Hello from Lambda!</h1>"
}
```

!!! NOTE
    If you enable multi-value headers, you must specify multiple cookies as follows

```json
{
  "multiValueHeaders":{
      "Set-cookie":[
        "cookie-name=cookie-value;Domain=myweb.com;Secure;HttpOnly",
        "cookie-name=cookie-value;Expires=May 8, 2019"
      ],
      "Content-Type":[
        "application/json"
      ]
  }
}  
```

## Libraries

Typed Lambda handlers by Language

- [Python - ALBEvent](https://awslabs.github.io/aws-lambda-powertools-python/latest/utilities/data_classes/#application-load-balancer){target="_blank"} - Pip `aws-lambda-powertools`
- [Typescript - ALBEvent](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/aws-lambda/trigger/alb.d.ts){target="_blank"} - NPM `@types/aws-lambda`
- [Rust - alb/mod.rs](https://github.com/LegNeato/aws-lambda-events/blob/master/aws_lambda_events/src/alb/mod.rs){target="_blank"} - Cargo `aws_lambda_events`
- [Go - ApplicationLoadBalancerRequest](https://github.com/aws/aws-lambda-go/blob/main/events/README_ALBTargetGroupEvents.md){target="_blank"} - Crate `github.com/aws/aws-lambda-go/events`
- [Java - ApplicationLoadBalancerRequestEvent](https://github.com/aws/aws-lambda-java-libs/blob/master/aws-lambda-java-events/src/main/java/com/amazonaws/services/lambda/runtime/events/ApplicationLoadBalancerRequestEvent.java){target="_blank"} - Maven `aws-lambda-java-events`
- [DoNet - ApplicationLoadBalancerEvents](https://github.com/aws/aws-lambda-dotnet/tree/master/Libraries/src/Amazon.Lambda.ApplicationLoadBalancerEvents){target="_blank"} - NuGet `Amazon.Lambda.ApplicationLoadBalancerEvents`

Event Handlers by Language

- [AWS Lambda Powertools Python - ALBResolver](https://awslabs.github.io/aws-lambda-powertools-python/latest/core/event_handler/api_gateway/){target="_blank"}
- [Serverless Java container](https://github.com/awslabs/aws-serverless-java-container){target="_blank"}

### Code Examples

- [application-load-balancer-serverless-app](https://github.com/aws/elastic-load-balancing-tools/tree/master/application-load-balancer-serverless-app){target="_blank"}

## Reference Docs

- [Using AWS Lambda with an Application Load Balancer](https://docs.aws.amazon.com/lambda/latest/dg/services-alb.html){target="_blank"}
- [ALB - Lambda functions as targets](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/lambda-functions.html){target="_blank"}
- [Lambda functions as targets for Application Load Balancers](https://aws.amazon.com/blogs/networking-and-content-delivery/lambda-functions-as-targets-for-application-load-balancers/){target="_blank"}
