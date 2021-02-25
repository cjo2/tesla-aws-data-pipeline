# tesla-aws-data-pipeline
A (near-ish) real-time Tesla vehicle monitoring data pipeline built on AWS.

## Wait but why?
Tesla does not provide a service out-of-the-box to allow you to compile and export your vehicle's _basic_ telemetry data. In addition, most tools available to the broader ownership community store your data in the vendor's private cloud. This data pipeline will allow you to simply compile your vehicle's data and keep it within your own AWS account.

## General workflow
* AWS Lambda function, "query-vehicle", will activate at a set interval by a CloudWatch event rule
* "query-vehicle" will publish an event on the topic "tdp-data-log-entry-retrieved" to Amazon SNS
* AWS Lambda function, "dump-data" will be triggered by Amazon SNS. A buffer may be needed so that one log won't be processed for each function invocation. Potential that this may "break" real-time features and instead make the application near-real-time.
* Other optional services may subscribe to "tdp-data-log-entry-retrieved"
* "dump-data" will write data to a DynamoDB instance

## Limitations
* This approach limits querying the vehicle once a minute. Another approach may be needed if you are relying on functions as a service.
