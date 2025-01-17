# Amazon Lex V2

Event-driven, synchronous invocation. Amazon Lex V2 uses one Lambda function per bot alias per language instead of one Lambda function for each intent.
To use an individual function for each intent, the Lambda function router section provides a function that you can use.

## Input

### Lex V2 input schema

Event fields notes:

`invocationSource`
: Indicates the action that called the Lambda function. When the source is DialogCodeHook, the Lambda function was called after input from the user.When the source is FulfillmentCodeHook the Lambda function was called after all required slots have been filled and the intent is ready for fulfillment.

`inputTranscript`:
: The text that was used to process the input from the user. For text or DTMF input, this is the text that the user typed. For speech input, this is the text that was recognized from the speech.

`interpretations`
: One or more intents that Amazon Lex V2 considers possible matches to the user's utterance. For more information, see Interpretation.

`proposedNextState`
: The next state of the dialog between the user and the bot if the Lambda function doesn't change the flow. For more information, see proposedNextState structure.

`requestAttributes`
: Request-specific attributes that the client sends in the request. Use request attributes to pass information that doesn't need to persist for the entire session.

`sessionState`
: The current state of the conversation between the user and your Amazon Lex V2 bot. For more information about the session state, see SessionState.

`transcriptions`
: one or more transcriptions that Amazon Lex V2 considers possible matches to the user's audio utterance. For more information, see Using voice transcription confidence scores.

```json title="Lex V2 schema"
{
    "messageVersion": "1.0",
    "invocationSource": "DialogCodeHook | FulfillmentCodeHook",
    "inputMode": "DTMF | Speech | Text",
    "responseContentType": "CustomPayload | ImageResponseCard | PlainText | SSML",
    "sessionId": "string",
    "inputTranscript": "string",
    "bot": {
        "id": "string",

        "name": "string",
        "aliasId": "string",
        "localeId": "string",
        "version": "string"
    },
    "interpretations": [
        {
            "intent": {

                "confirmationState": "Confirmed | Denied | None",
                "name": "string",
                "slots": {
                    "string": {
                        "value": {
                            "interpretedValue": "string",
                            "originalValue": "string",
                            "resolvedValues": [
                                "string"
                            ]
                        }
                    },
                    "string": {
                        "shape": "List",
                        "value": {
                            "interpretedValue": "string",
                            "originalValue": "string",
                            "resolvedValues": [
                                "string"
                            ]
                        },
                        "values": [
                            {

                                "shape": "Scalar",
                                "value": {
                                    "originalValue": "string",
                                    "interpretedValue": "string",
                                    "resolvedValues": [
                                        "string"
                                    ]
                                }
                            },
                            {

                                "shape": "Scalar",
                                "value": {

                                    "originalValue": "string",
                                    "interpretedValue": "string",
                                    "resolvedValues": [
                                        "string"
                                    ]
                                }
                            }
                        ]
                    }
                },
                "state": "Failed | Fulfilled | FulfillmentInProgress | InProgress | ReadyForFulfillment | Waiting",
                "kendraResponse": {
                    // Only present when intent is KendraSearchIntent. For details, see 
                    // https://docs.aws.amazon.com/kendra/latest/dg/API_Query.html#API_Query_ResponseSyntax
                }
            },
            "nluConfidence": {
                "score": number
            },
            "sentimentResponse": {

                "sentiment": "string",
                "sentimentScore": {
                    "mixed": number,
                    "negative": number,
                    "neutral": number,
                    "positive": number
                }
            }
        }
    ],
    "proposedNextState": {
        "dialogAction": {
            "slotToElicit": "string",
            "type": "Close | ConfirmIntent | Delegate | ElicitIntent | ElicitSlot"
        },
        "intent": {
            "name": "string",
            "confirmationState": "Confirmed | Denied | None",
            "slots": {},
            "state": "Failed | Fulfilled | InProgress | ReadyForFulfillment | Waiting"
        }
    },
    "requestAttributes": {
        "string": "string"

    },
    "sessionState": {
        "activeContexts": [
            {
                "name": "string",
                "contextAttributes": {
                    "string": "string"
                },
                "timeToLive": {
                    "timeToLiveInSeconds": number,
                    "turnsToLive": number
                }
            }
        ],
        "sessionAttributes": {
            "string": "string"
        },
        "runtimeHints": {
            "slotHints": {
                "string": {
                    "string": {
                        "runtimeHintValues": [
                            {
                                "phrase": "string"
                            },
                            {
                                "phrase": "string"
                            }
                        ]
                    }
                }
            }
        },
        "dialogAction": {
            "slotToElicit": "string",
            "type": "Close | ConfirmIntent | Delegate | ElicitIntent | ElicitSlot"
        },
        "intent": {
            "confirmationState": "Confirmed | Denied | None",
            "name": "string",
            "slots": {
                "string": {
                    "value": {
                        "interpretedValue": "string",
                        "originalValue": "string",
                        "resolvedValues": [
                            "string"
                        ]
                    }
                },
                "string": {
                    "shape": "List",
                    "value": {
                        "interpretedValue": "string",
                        "originalValue": "string",

                        "resolvedValues": [
                            "string"

                        ]
                    },
                    "values": [
                        {
                            "shape": "Scalar",
                            "value": {
                                "originalValue": "string",
                                "interpretedValue": "string",
                                "resolvedValues": [
                                    "string"

                                ]
                            }
                        },
                        {
                            "shape": "Scalar",
                            "value": {
                                "originalValue": "string",
                                "interpretedValue": "string",
                                "resolvedValues": [
                                    "string"
                                ]
                            }
                        }
                    ]
                }
            },
            "state": "Failed | Fulfilled | FulfillmentInProgress | InProgress | ReadyForFulfillment | Waiting",
            "kendraResponse": {
                // Only present when intent is KendraSearchIntent. For details, see
                // https://docs.aws.amazon.com/kendra/latest/dg/API_Query.html#API_Query_ResponseSyntax                     }
            },
            "originatingRequestId": "string"
        }
    },
    "transcriptions": [
        {
            "transcription": "string",
            "transcriptionConfidence": {
                "score": "number"
            },
            "resolvedContext": {
                "intent": "string"
            },
            "resolvedSlots": {
                "string": {
                    "shape": "List",
                    "value": {
                        "originalValue": "string",
                        "resolvedValues": [
                            "string"
                        ]
                    },
                    "values": [
                        {
                            "shape": "Scalar",
                            "value": {
                                "originalValue": "string",
                                "resolvedValues": [
                                    "string"
                                ]
                            }
                        },
                        {
                            "shape": "Scalar",
                            "value": {
                                "originalValue": "string",
                                "resolvedValues": [
                                    "string"
                                ]
                            }
                        }
                    ]
                }
            }
        }
    ]
}
```

### Lex V2 example requests

```json title="Banking bot: CheckBalance"
{
    "sessionId": "254688924456798",
    "inputTranscript": "01/01/1990",
    "interpretations": [
        {
            "intent": {
                "slots": {
                    "dateofBirth": {
                        "shape": "Scalar",
                        "value": {
                            "originalValue": "01/01/1990",
                            "resolvedValues": [
                                "1990-01-01"
                            ],
                            "interpretedValue": "1990-01-01"
                        }
                    },
                    "accountType": {
                        "shape": "Scalar",
                        "value": {
                            "originalValue": "savings",
                            "resolvedValues": [
                                "Savings"
                            ],
                            "interpretedValue": "Savings"
                        }
                    }
                },
                "confirmationState": "None",
                "name": "CheckBalance",
                "state": "ReadyForFulfillment"
            },
            "nluConfidence": 1
        },
        {
            "intent": {
                "slots": {},
                "confirmationState": "None",
                "name": "FallbackIntent",
                "state": "ReadyForFulfillment"
            }
        },
        {
            "intent": {
                "slots": {},
                "confirmationState": "None",
                "name": "Welcome",
                "state": "ReadyForFulfillment"
            },
            "nluConfidence": 0.23
        }
    ],
    "responseContentType": "text/plain; charset=utf-8",
    "sessionState": {
        "sessionAttributes": {},
        "activeContexts": [],
        "intent": {
            "slots": {
                "dateofBirth": {
                    "shape": "Scalar",
                    "value": {
                        "originalValue": "01/01/1990",
                        "resolvedValues": [
                            "1990-01-01"
                        ],
                        "interpretedValue": "1990-01-01"
                    }
                },
                "accountType": {
                    "shape": "Scalar",
                    "value": {
                        "originalValue": "savings",
                        "resolvedValues": [
                            "Savings"
                        ],
                        "interpretedValue": "Savings"
                    }
                }
            },
            "confirmationState": "None",
            "name": "CheckBalance",
            "state": "ReadyForFulfillment"
        },
        "originatingRequestId": "f57dfc3f-44be-4df9-ae72-9681fc14e67f"
    },
    "messageVersion": "1.0",
    "invocationSource": "FulfillmentCodeHook",
    "transcriptions": [
        {
            "transcription": "01/01/1990",
            "transcriptionConfidence": 1,
            "resolvedSlots": {
                "dateofBirth": {
                    "shape": "Scalar",
                    "value": {
                        "originalValue": "01/01/1990",
                        "resolvedValues": [
                            "1990-01-01"
                        ]
                    }
                }
            },
            "resolvedContext": {
                "intent": "CheckBalance"
            }
        }
    ],
    "inputMode": "Text",
    "bot": {
        "aliasName": "TestBotAlias",
        "aliasId": "TSTALIASID",
        "name": "BankingBot",
        "version": "DRAFT",
        "localeId": "en_US",
        "id": "J866BA0UQC"
    }
}
```

### Generating sample events via SAM CLI

```shell
# Github PR (https://github.com/aws/aws-sam-cli/pull/3649)
```

## Response

### Lex V2 response schema

```json title="Lex V2 response schema"
{
    "sessionState": {
        "activeContexts": [
            {
                "name": "string",
                "contextAttributes": {
                    "key": "value"
                },
                "timeToLive": {
                    "timeToLiveInSeconds": number,
                    "turnsToLive": number
                }
            }
        ],
        "sessionAttributes": {
            "string": "string"
        },
        "runtimeHints": {
            "slotHints": {
                "string": { 
                    "string": { 
                        "runtimeHintValues": [
                            {
                                "phrase": "string"
                            },
                            {
                                "phrase": "string"
                            }
                        ]
                    }
                }
            } 
        },
        "dialogAction": {
            "slotElicitationStyle": "Default | SpellByLetter | SpellByWord",
            "slotToElicit": "string",
            "type": "Close | ConfirmIntent | Delegate | ElicitIntent | ElicitSlot"
        },
        "intent": {
            "confirmationState": "Confirmed | Denied | None",
            "name": "string",
            "slots": {
                "string": {
                    "value": {
                        "interpretedValue": "string",
                        "originalValue": "string",
                        "resolvedValues": [
                            "string"
                        ]
                    }
                },
                "string": {
                    "shape": "List",
                    "value": {
                        "originalValue": "string",
                        "interpretedValue": "string",
                        "resolvedValues": [
                            "string"
                        ]
                    },
                    "values": [
                        {
                            "shape": "Scalar",
                            "value": {
                                "originalValue": "string",
                                "interpretedValue": "string",
                                "resolvedValues": [
                                    "string"
                                ]
                            }
                        },
                        {
                            "shape": "Scalar",
                            "value": {
                                "originalValue": "string",
                                "interpretedValue": "string",
                                "resolvedValues": [
                                    "string"
                                ]
                            }
                        }
                    ]
                }
            },
            "state": "Failed | Fulfilled | FulfillmentInProgress | InProgress | ReadyForFulfillment | Waiting"
        }
    },
    "messages": [
        {
            "contentType": "CustomPayload | ImageResponseCard | PlainText | SSML",
            "content": "string",
            "imageResponseCard": {
                "title": "string",

                "subtitle": "string",
                "imageUrl": "string",
                "buttons": [

                    {
                        "text": "string",
                        "value": "string"
                    }
                ]
            }
        }
    ],
    "requestAttributes": {
        "string": "string"
    }
}
```

## Libraries

AWS Solution using Lex V2

- [A Question and Answer Bot Using Amazon Lex and Amazon Alexa](https://github.com/aws-solutions/aws-qnabot)

Typed Lambda handlers by Language

- [Typescript - LexV2Event](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/aws-lambda/trigger/lex-v2.d.ts) - NPM package `@types/aws-lambda`

## Reference Docs

- [Using AWS Lambda with Amazon Lex V2](https://docs.aws.amazon.com/lexv2/latest/dg/lambda.html)
- [Creating a BankingBot on Amazon Lex V2 Console with support for English and Spanish](https://aws.amazon.com/blogs/machine-learning/creating-a-bankingbot-on-amazon-lex-v2-console-with-support-for-english-and-spanish/)
- [Hands On: Create a BankingBot with support for English and Spanish on the Amazon Lex V2 console](https://aws.amazon.com/getting-started/hands-on/create-banking-bot-on-amazon-lex-v2-console/)
