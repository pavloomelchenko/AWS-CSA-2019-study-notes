


###Lambda concepts
Traditional vs Serverless Architecture comparison(comes up a lot in exam)

Pricing:
* Nubmer of requests
* Duration

Exam tips:
* Lambda scales out (not up) automatically
* Lambda functions are independent (1event = 1 function)
* Lambda is serverless
* Know which services are serverless!

DynamoDB,S3,Lambda,API Gateway, Aurora RDS - all serverless
RDS, EC2 is not serverless

* Lambda f-ns can trigger other Lambda functions (1event = x functions)
* AWS X-ray allows to debug (complicated lambda architectures)
* Know your triggers (f.e. RDS cannot trigger lambda)



###Lets build a serverless webpage
Lambda triggers
ELB (Application LB), Cognito, Alexa, API Gateway, CloudFront (Lambda@Edge), Kinesis Data Firehose
S3, SNS, SES,  CloudFormation,  CloudWatch(logs,events), AWS CodeCommit, AWS Config


Cannot trigger: EC2,IAM...





