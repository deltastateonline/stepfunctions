## Step Function
```Json
{
  "Comment": "Step function is triggered when there are entites to be reprocessed",
  "StartAt": "SendEntities",
  "States": {
    "SendEntities": {
      "Type": "Map",
      "ItemsPath": "$.unsententities",
      "ResultPath": "$.entitiesprocessed",
      "Iterator": {
        "StartAt": "CallTriggerRabbitmqLambda",
        "States": {
          "CallTriggerRabbitmqLambda": {
            "Type": "Task",
            "Resource": "TriggerRabbitmq:Prod",
            "End": true
          }
        }
      },
      "Catch": [
        {
          "ErrorEquals": [
            "States.All"
          ],
          "ResultPath": "$.error-info",
          "Next": "SendFailure"
        }
      ],
      "Next": "AllDone"
    },
    "AllDone": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "arn:aws:sns:ap-",
        "Message.$": "$"
      },
      "End": true
    },
    "SendFailure": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "arn:aws:sns:ap-",
        "Message.$": "$",
        "Subject": "An Error Occured Processing Unsent Entities."
      },
      "End": true
    }
  }
}
```

[Home ](Readme.md)