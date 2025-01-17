# DynamoDB

Amazon DynamoDB stream, Lambda reads records from the stream and invokes your function synchronously with an event that contains stream records. 
Lambda reads records in batches and invokes your function to process records from the batch.

## Input

### Generating sample events via SAM CLI

```shell
sam local generate-event dynamodb update
```

### Input Event structure

```json
{
  "Records": [
    {
      "eventID": "c4ca4238a0b923820dcc509a6f75849b",
      "eventName": "INSERT",
      "eventVersion": "1.1",
      "eventSource": "aws:dynamodb",
      "awsRegion": "eu-central-1",
      "dynamodb": {
        "Keys": {
          "Id": {
            "N": "101"
          }
        },
        "NewImage": {
          "Message": {
            "S": "New item!"
          },
          "Id": {
            "N": "101"
          }
        },
        "ApproximateCreationDateTime": 1428537600,
        "SequenceNumber": "4421584500000000017450439091",
        "SizeBytes": 26,
        "StreamViewType": "NEW_AND_OLD_IMAGES"
      },
      "eventSourceARN": "arn:aws:dynamodb:eu-central-1:123456789012:table/ExampleTableWithStream/stream/2015-06-27T00:48:05.899",
      "userIdentity": {
        "principalId": "dynamodb.amazonaws.com",
        "type": "Service"
      }
    },
    {
      "eventID": "c81e728d9d4c2f636f067f89cc14862c",
      "eventName": "MODIFY",
      "eventVersion": "1.1",
      "eventSource": "aws:dynamodb",
      "awsRegion": "eu-central-1",
      "dynamodb": {
        "Keys": {
          "Id": {
            "N": "101"
          }
        },
        "NewImage": {
          "Message": {
            "S": "This item has changed"
          },
          "Id": {
            "N": "101"
          }
        },
        "OldImage": {
          "Message": {
            "S": "New item!"
          },
          "Id": {
            "N": "101"
          }
        },
        "ApproximateCreationDateTime": 1428537600,
        "SequenceNumber": "4421584500000000017450439092",
        "SizeBytes": 59,
        "StreamViewType": "NEW_AND_OLD_IMAGES"
      },
      "eventSourceARN": "arn:aws:dynamodb:eu-central-1:123456789012:table/ExampleTableWithStream/stream/2015-06-27T00:48:05.899"
    },
    {
      "eventID": "eccbc87e4b5ce2fe28308fd9f2a7baf3",
      "eventName": "REMOVE",
      "eventVersion": "1.1",
      "eventSource": "aws:dynamodb",
      "awsRegion": "eu-central-1",
      "dynamodb": {
        "Keys": {
          "Id": {
            "N": "101"
          }
        },
        "OldImage": {
          "Message": {
            "S": "This item has changed"
          },
          "Id": {
            "N": "101"
          }
        },
        "ApproximateCreationDateTime": 1428537600,
        "SequenceNumber": "4421584500000000017450439093",
        "SizeBytes": 38,
        "StreamViewType": "NEW_AND_OLD_IMAGES"
      },
      "eventSourceARN": "arn:aws:dynamodb:eu-central-1:123456789012:table/ExampleTableWithStream/stream/2015-06-27T00:48:05.899"
    }
  ]
}
```

## Response

```json title="Response schema"
{ 
  "batchItemFailures": [ 
        {
            "itemIdentifier": "<id>"
        }
    ]
}
```

## Libraries

Typing and data classes

- [Python - DynamoDBStreamEvent](https://awslabs.github.io/aws-lambda-powertools-python/latest/utilities/data_classes/#dynamodb-streams){target="_blank"} - Pip `aws-lambda-powertools`
- [Go - DynamoDBEvent](https://github.com/aws/aws-lambda-go/blob/main/events/README_DynamoDB.md){target="_blank"} - Go `github.com/aws/aws-lambda-go/events`
- [Typescript - DynamoDBStreamEvent](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/aws-lambda/trigger/dynamodb-stream.d.ts){target="_blank"} - NPM `@types/aws-lambda`
- [Java - DynamodbEvent](https://github.com/aws/aws-lambda-java-libs/blob/master/aws-lambda-java-events/src/main/java/com/amazonaws/services/lambda/runtime/events/DynamodbEvent.java){target="_blank"} - Java `aws-lambda-java-events`
- [Rust - aws_lambda_events::dynamodb::Event](https://docs.rs/aws_lambda_events/latest/aws_lambda_events/dynamodb/index.html){target="_blank"} - Rust `aws_lambda_events`
- [Ruby - DynamodbEvent](https://rubyonjets.com/docs/events/dynamodb/) - Gem `jets`

Batch handler

- [Python - BatchProcessor](https://awslabs.github.io/aws-lambda-powertools-python/latest/utilities/batch/#processing-messages-from-dynamodb){target="_blank"} - pip `aws-lambda-powertools`
- [Python - on_dynamodb_record](https://aws.github.io/chalice/topics/events.html#dynamodb-events) - pip `chalice`

### Code Examples

```python title="Example using AWS Lambda Powertool (Python)"
import json

from aws_lambda_powertools import Logger, Tracer
from aws_lambda_powertools.utilities.batch import BatchProcessor, EventType, batch_processor
from aws_lambda_powertools.utilities.data_classes.dynamo_db_stream_event import DynamoDBRecord
from aws_lambda_powertools.utilities.typing import LambdaContext


processor = BatchProcessor(event_type=EventType.DynamoDBStreams)
tracer = Tracer()
logger = Logger()


@tracer.capture_method
def record_handler(record: DynamoDBRecord):
    logger.info(record.dynamodb.new_image)
    payload: dict = json.loads(record.dynamodb.new_image.get("Message").get_value)
    # alternatively:
    # changes: Dict[str, dynamo_db_stream_event.AttributeValue] = record.dynamodb.new_image
    # payload = change.get("Message").raw_event -> {"S": "<payload>"}
    ...

@logger.inject_lambda_context
@tracer.capture_lambda_handler
@batch_processor(record_handler=record_handler, processor=processor)
def lambda_handler(event, context: LambdaContext):
    return processor.response()
```

## Reference Docs

- [Using AWS Lambda with Amazon DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html)
