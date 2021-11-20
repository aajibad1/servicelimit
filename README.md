# servicelimit

Investigate Monitoring Alerts for Service Limits.

Scope
Create a plan to Track service limits and implementation of alerts if exceeding limits. This plan will help to proactively track resource usage and send notifications when approaching Service Limits.
Prerequisite
•	Permission to create AWS Trusted Advisor, SNS , Service quota  and AWS Cloud Watch.

Solution Resources
•	AWS Trusted Advisor:  Trusted Advisor evaluates account by using checks and providing recommendation best on best practice.
•	AWS Cloud Watch:  Provides Monitoring and Observability in AWS environment.
•	AWS Simple Notification Service:  Notification purpose 
•	AWS Service Quota:  Used to check limits and request increase quota
•	AWS Lambda: Used to trigger events.
•	DynamoDB: store events for audit.



Solution Architecture Flow Diagram
 

Process Flow
1.	The Lambda function refreshes the AWS Trusted Advisor Service Limits checks to retrieve the most current utilization and quota data through API calls once every 24 hrs.
2.	CloudWatch scheduled event rule trigged second Lambda. 
a.	Lambda function will check if an increase request has been submitted and if not submit a new one.
3.	A message is sent to SNS topic to notify you
a.	When reaching limits
b.	 when a request has been raised by it.

4.	Amazon CloudWatch Events captures the status events.
a.	CloudWatch Events rules send the status events (OK, WARN, and ERROR status) to SQS.
b.	Amazon SQS receives all the OK, WARN, and ERROR status.
5.	Lambda function act as Limit Summarizer to ingests the messages from the queue and stores them on the Amazon DynamoDB table for historical view of all quota related.
a.	The dead-letter queue stores all messages that couldn't be read by the Limit Summarizer Lambda function.
6.	Stored Data in DynamoDB.
Automation Provisioning?

Infrastructure as a code (Terraform) will be used to create below resources.
-	AWS Cloud Watch 
-	AWS Lambda
-	AWS SQS
-	AWS DynamoDB
-	SNS Topic
-	Service Quota
Manual Provisioning?
Below AWS service will be created on the console because Terraform do not support it.
-	AWS Trusted Advisor
Alternative to create this resource automated can be done using below process/tool;
-	AWS CloudFormation or
-	Run AWS CLI command in Terraform 



CloudWatch Rules 
-	A CloudWatch Event Rule that triggers on changes in the status of AWS Trusted Advisor checks, and forwards the events to an SNS topic
-	A CloudWatch Event Rule that triggers on changes in the status of AWS Service Quota checks, request  and forwards the events to an SNS topic




Notes:
References:
-	Limit Monitor | Implementations | AWS Solutions (amazon.com)
-	HeleCloud:Custom automation built on serverless architecture
-	Considerations - AWS Limit Monitor (amazon.com)


