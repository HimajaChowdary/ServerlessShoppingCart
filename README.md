# ServerlessShoppingCart
Creating a Serverless Application with Amazon SNS, SQS, and Lambda
In this project, we build an event-driven serverless application using Amazon Simple Notification Service (SNS), Amazon Simple Queue Service (SQS), and AWS Lambda. This architecture is useful when you want to decouple services and process messages asynchronously with high reliability and scalability.

🧩 What Is an Event-Driven Architecture?
An event-driven architecture allows systems to respond to changes (events) asynchronously. For example, an application that processes user sign-ups could use this architecture to trigger welcome emails or audit logging without blocking the main workflow.

🏗️ Architecture Overview
Here’s how the components connect:

Application → SNS Topic → SQS Queue → Lambda Consumer → Processed Output

1. SNS Topic receives messages from the application.

2. SQS Queue is subscribed to the SNS topic to reliably queue messages.

3. ambda Function polls messages from the queue and processes them asynchronously.

🛠️ Step-by-Step Implementation
1. Create an SNS Topic
Go to the AWS SNS console and create a new topic:

Type: Standard

Name: user-notifications

Once created, note down the ARN — it will be used for publishing messages.

2. Create an SQS Queue
Next, navigate to the SQS console and create a new standard queue:

Name: user-notification-queue

Enable raw message delivery if subscribing directly from SNS.

3. Subscribe the SQS Queue to the SNS Topic
Go back to your SNS topic

Under Subscriptions, click Create subscription

Protocol: SQS

Endpoint: ARN of the SQS queue

Make sure both SNS and SQS are in the same region

Grant SNS permission to send messages to the SQS queue by modifying the queue's access policy

{
  "Effect": "Allow",
  "Principal": "*",
  "Action": "SQS:SendMessage",
  "Resource": "arn:aws:sqs:us-east-1:123456789012:user-notification-queue",
  "Condition": {
    "ArnEquals": {
      "aws:SourceArn": "arn:aws:sns:us-east-1:123456789012:user-notifications"
    }
  }
}

4. Create the Lambda Function
Create a Lambda function in Python or Node.js.

🧪 Testing the Application
To test, publish a message to the SNS topic:
{
  "eventType": "UserSignup",
  "userEmail": "hello@example.com",
  "timestamp": "2025-05-03T10:00:00Z"
}

✅  We should see:

SNS publishes the message

SQS queues it

Lambda picks it up and logs the content

✅ Benefits of This Architecture
Scalability: Lambda scales automatically

Decoupling: Services are isolated and independent

Reliability: SQS ensures no messages are lost

Cost-effective: You only pay for what you use (serverless)

