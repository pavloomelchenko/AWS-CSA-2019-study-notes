## Application Services:

## [SQS](https://aws.amazon.com/sqs/)

Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables you 
to decouple and scale microservices, distributed systems, and serverless applications.

* It's basically a message queue service, like Kafka for example. You add items in the queue, and then someone will ask for these objects from the queue.
* Retention can go up to 14 days!(vs SWF 1 year)
* It's a pull based system (not gonna push msgs to app, app have to pull ).
* At-least-once delivery
* Type of queues:
  * Standard: All queue are standard by default. 
    High performance, at-least-once delivery, but can be out of order and msg can be delivered twice.
  * FIFO: (first in first out) These queues guarantee ordering and no duplication. 
* You can have duplicates if you don't manage very well your visibility timeouts.
* Visibility timeout is the amount of time that the message is invisible in the SQS queue after a picks up
that message . Provided the job is processed before the visibility time out expires ,the message will then 
be deleted from the queue . If the job is not processed within that time ,the message will become visible
again and another reader will process it .This could result in the same msg delivered twice.
* Visibility timeout maximus is 12 hours
* Long polling(wait until msg arrives, way to save money) vs short polling

_Read the [SQS faqs](https://aws.amazon.com/sqs/faqs/) before taking the exam._

## [SWF](https://aws.amazon.com/swf/)

Amazon SWF (Simple Workflow Service) is an Amazon Web Services tool that helps developers coordinate, track and audit multi-step, 
multi-machine application jobs(tasks). 
It's tasks that can be assigned to machine or human (remember for exam!)

* It ensures that a task is assigned only once and is never duplicated.
* Retention can go up to 1 year
* SWF Workers: Workers are programs that interact with Amazon SWF to get tasks, process received task, and return the result.
* SWF Decider: The decider is a program that controls the coordination of tasks.
* SWF Domains: Domains isolate a set of types, executions, and task lists from others within the same account.

## [SNS](https://aws.amazon.com/sns/)

Amazon SNS enables message filtering and fan out to a large number of subscribers, including serverless functions, 
queues, and distributed systems. Additionally, Amazon SNS fans out notifications to end users via mobile push messages, SMS, and email.

* SNS stores copies of the messages on multiple AZ.
* Push-based! (vs SQL pull based)
* It can notify through:
  * AWS Lambda functions
  * HTTP/S webhooks
  * mobile push, SMS, email
  

## [Elastic Transcoder](https://aws.amazon.com/elastictranscoder/)

Amazon Elastic Transcoder a is media transcoding service.

## [API Gateway](https://aws.amazon.com/api-gateway/)

Amazon API Gateway is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale.

* You can enable cache (TTL)
* Scaled automatically and effortlessly
* You can throttle requests (to prevent attacks)
* Connect to CloudWatch to log all requests.
* Maintain multiple versions of API
* If using Javascript/Ajax that uses multiple domain names , ensure that you enabled CORS on API Gateway

## [Kinesis](https://aws.amazon.com/kinesis/)

Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information.

* Kinesis streams: It consists of shards, where each stream is saved and stored for 24h, up to 7 days. 
You can have multiple shards on each stream. A consumer then read from the shards and process the data.
* Kinesis Firehose: It doesn't have shards, so data traverses the Firehose. Data has to be processed immediately with lambda or stored in S3 for example.
  * It can stream data to Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, and Splunk.
* Kinesis Analytics: It allows you to analyze the data that exists in Kinesis Firehose of streams.


## [Web Identity Federation and Cognito]

You need to know what it is

* Federation auth (Facebook,Google,Amazon,...) bind to AWS IAM role
* Token management, transparent for application, user and identity pools





