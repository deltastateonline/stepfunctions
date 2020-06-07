# Using Step Functions

In our application, emails and other forms for communications are sent asynchronously.

## For Example.
- Emails are composed on the UI and sent from the UI, a sent flag is set to 0, to indicate that the email has not been sent.
- SMS are also composed on the UI and an entry is written into the database with the sent flag been set to 0.

For each of these interactions, The primary keys of the entities are written into a queue as well as the operations that the worker has to perform.
From the examples above
1. For emails `send.email`
2. For sms `send.sms`

These can also be generalised as `send.communications`. An example of the payload might be
```json
{
	"primaryId": "123455",
	"operation": "send.communication",
	"createdBy": {identifer of the user},
	"created": {utc time stamp}
}
```
After these entries are enqueued they get processed by workers but sometimes they fail.

Failed entries get written to a dead letter queue and get requeued after a ttl. 

The process of queuing and dequeuing gets repeated for x number of times until it finally fails and the entry has to be processed manually.

In the application there are endpoints which can be interogated which will return a list or entities which failed. These endpoints take parameters which are used
to filter records been returned.




[I'm an inline-style link](https://www.google.com)
