# Lambda-Automation

# S3 to SNS Notification Lambda Function

# Overview:
This AWS Lambda function is designed to be triggered by S3 events, specifically when a new file is uploaded to a specified S3 bucket. Upon trigger, the function counts the number of rows in the uploaded CSV file and sends an email notification via Amazon SNS (Simple Notification Service) with the count of rows.

# Setup Instructions
1. Create an S3 Bucket: If you haven't already, create an S3 bucket where you want to monitor file uploads.
2. Create an SNS Topic: Create an SNS topic that will be used to send email notifications. Make sure to note down the ARN (Amazon Resource Name) of the SNS topic.
3. Subscribe to the SNS Topic: Subscribe the email addresses you want to receive notifications to the SNS topic.
4. Create the Lambda Function:
     Navigate to the AWS Lambda console.
     Click on "Create function" and choose "Author from scratch".
     Provide a name for your function, select the runtime as Python, and choose an existing execution role that has permissions to interact with S3 and SNS. If you don't have an appropriate role, create one with necessary permissions.
     Click on "Create function".
5. Configure Trigger:
     Once the function is created, click on "Add trigger".
     Select "S3" as the trigger type.
     Choose the S3 bucket where you want to monitor file uploads.
     Select the event type (e.g., 'All object create events').
     Click on "Add".
6. Deploy the Lambda Function Code:
     Copy the provided Python code and paste it into the Lambda function code editor.
     Modify the sns_topic_arn variable to include the ARN of the SNS topic you created earlier.
7. Save and Test:
     Save the Lambda function.
     You can test the function by manually uploading a CSV file to the configured S3 bucket. Check the SNS topic for the email notification.

# Creation of s3-topic:
Go to the Simple Notification Service (SNS)

Click on create topic

![alt text](<Screenshot 2024-05-04 224322.png>)

Select the type as : standard
Enter the name of the notification : email-notification
Click on create

![alt text](<Screenshot 2024-05-04 224353.png>)

The topic is created.

![alt text](<Screenshot 2024-05-04 224439.png>)

Click on create subscription

![alt text](<Screenshot 2024-05-04 224505.png>)

Select the arn of the topic
Select the protocol has : email
Enter the endpoint : sathishgurka@024
click on create

![alt text](<Screenshot 2024-05-04 224614.png>)

The subscription for the email-notification is created successfully

![alt text](<Screenshot 2024-05-04 224652.png>)

Click on the confirm-subscription

![alt text](<Screenshot 2024-05-04 224749.png>)

The Subscription is confirmed successfully

![alt text](<Screenshot 2024-05-04 224803.png>)
![alt text](<Screenshot 2024-05-04 224852.png>)

# Creation of lambda-function:
Click on the create function button

![alt text](<Screenshot 2024-05-04 215128.png>)

Select the Author from Scratch option
Enter the name of the function : Lambda-project
Select the Runtime : Python 3.12
Architecture as : x86_64

![alt text](<Screenshot 2024-05-04 215332.png>)

Click on create function

![alt text](<Screenshot 2024-05-04 215400.png>)

The Lambda-project function is created:

![alt text](<Screenshot 2024-05-04 215458.png>)
![alt text](<Screenshot 2024-05-04 215644.png>)

# Adding the s3-trigger
Click on add trigger

![alt text](<Screenshot 2024-05-04 215550.png>)

Select the trigger as : S3
Select the bucket     : s3-data-storage20
Select the event types as : all object create events

![alt text](<Screenshot 2024-05-04 223242.png>)

Click on add to add the trigger:

![alt text](<Screenshot 2024-05-04 223454.png>)

The s3 trigger is added to the lambda function

![alt text](<Screenshot 2024-05-04 223541.png>)

# Adding the SNS-trigger
Click on add trigger
Select the trigger as : SNS
Select the sns topic that has created earlier
Than click on add

![alt text](<Screenshot 2024-05-04 224931.png>)

The SNS trigger is added to the lambda-function

![alt text](<Screenshot 2024-05-04 224955.png>)

 # Adding the destination
 Adding this destination can get notification for every successfull or failure events based on your notification

![alt text](<Screenshot 2024-05-04 225058.png>)

The destination is added successfully

![alt text](<Screenshot 2024-05-04 225207.png>)

# Lambda Function : CODE
import json
import boto3

def lambda_handler(event, context):
    s3 = boto3.client('s3')  # Initialize the S3 client
    sns = boto3.client('sns')  # Initialize the SNS client

    # Get the S3 bucket and object key from the event
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']

    # Download the CSV file from S3
    response = s3.get_object(Bucket=bucket, Key=key)
    csv_content = response['Body'].read().decode('utf-8').splitlines()

    # Count the number of rows in the CSV file
    row_count = len(csv_content)

    # Print the result (you can modify this to store the count or send it elsewhere)
    print(f"Number of rows in {key}: {row_count}")

    # Publish a message to SNS
    sns_topic_arn = 'arn:aws:sns:ap-south-1:136543311415:email-notification'
    sns_message = f"Number of rows in {key}: {row_count}"

    sns.publish(
        TopicArn=sns_topic_arn,
        Message=sns_message,
        Subject=f"CSV File Upload: {key}"
    )

    return {
        'statusCode': 200,
        'body': json.dumps(f"Number of rows in {key}: {row_count}")
    }

# Uploading the CSV file to the s3-bucket
Click on add files and upload the file

![alt text](<Screenshot 2024-05-04 225830.png>)

The lambda function has ran successfully
# CLOUDWATCH LOGS:

![alt text](<Screenshot 2024-05-04 231358.png>)
![alt text](<Screenshot 2024-05-04 231502.png>)

# NOTIFICATION
I got notification to the email when the file is uploaded to the s3-bucket
Hence the lambda is successfully triggered for event and automated the process

![alt text](<Screenshot 2024-05-04 231616.png>)

# The Project IS Completed 
 

