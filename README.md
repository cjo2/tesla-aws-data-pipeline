# tesla-aws-data-pipeline
A real-time Tesla vehicle monitoring data pipeline built on AWS.

## General workflow
* AWS Lambda function, "query-vehicle", will activate at a set interval by a CloudWatch event rule
* "query-vehicle" will publish an event to Amazon SNS FIFO Queue
* AWS Lambda function, "dump-data" will be triggered by Amazon SQS. A buffer may be needed so that one log won't be processed for each function invocation. Potential that this may "break" real-time features and instead make the application near-real-time.
* "dump-data" will write data to the a DynamoDB instance
