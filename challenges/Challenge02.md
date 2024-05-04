# Challenge 2: Build a Serverless Function

[&lt; Previous Challenge](./Challenge-01.md) - **[Home](../README.md)** - [Next Challenge &gt;](./Challenge-03.md)

## Introduction

In this challenge, you will build a serverless function using AWS Lambda to add interactivity to your web page for a later challenge. AWS Lambda is a compute service that lets you create serverless functions that can be invoked and scaled individually without a need to manage software and hardware. The serverless functions are triggered based on a specific event you will define in the code.

In this challenge, you will implement the following architectural diagram:

![image](https://github.com/ExxonMobil/AWSCodeFest-2024/assets/77283967/7f435fb4-0f3f-48c0-afae-43dbfa7a7768)

## Description

* In AWS console, you need to create a Lambda function from scratch named `codefest-aws-lambda-<your name>` in the same Region as other resources you created in the previous challenge.
* You will deploy and test a basic Python function to return a JSON response from an event input.
* You will then update the lambda code so that it is able to interact with DynamoDB.
  
## Key Concepts

**Compute service –** A service that provides computational processing power.

**Serverless function –** Piece of code that will be executed by a compute service, on demand.

**Lambda trigger –** The type of event that will make a Lambda (serverless) function run. This can be another AWS service or an external input.

## Implementation

### 1. CREATE AND CONFIGURE YOUR LAMBDA FUNCTION

1. In a new browser tab, log in to the AWS Lambda console.
2. Make sure you create your function in the same Region in which you created the web app in the previous module. You can see this at the very top of the page, next to your account name.
3. Choose the orange Create function button.
4. Under Function name, enter WriteToDynamoDB.
5. Select Python 3.8 from the runtime dropdown and leave the rest of the defaults unchanged.
6. Choose the orange Create function button.
7. You should see a green message box at the top of your screen with the following message "Successfully created the function WriteToDynamoDB."
8. Under Code source, replace the code in lambda_function.py with the following:

```Python
  # import the JSON utility package since we will be working with a JSON object
  import json
  # define the handler function that the Lambda service will use as an entry point
  def lambda_handler(event, context):
  # extract values from the event object we got from the Lambda service
      name = event['firstName'] +' '+ event['lastName']
  # return a properly formatted JSON object
      return {
      'statusCode': 200,
      'body': json.dumps('Hello from Lambda, ' + name)
      }
```
9. Save by going to the file menu and selecting Save to save the changes.
10. Choose Deploy to deploy the changes.
11. Let's test our new function. Choose the orange Test button to create a test event by selecting Configure test event.
12. Under Event name, enter WriteToDynamoDB.
13. Copy and paste the following JSON object to replace the default one:
```JSON
{
  "customername": "John Doe",
  "productname": "Ice Cream",
  "quantity": 5
}
```
15. Choose the Save button at the bottom of the page.

### 2. TEST YOUR BASIC LAMBDA FUNCTION

1. Under the WriteToDynamoDB section at the top of the page, select Test tab.
2. You should see a light green box at the top of the page with the following text: Execution result: succeeded. You can choose Details to see the event the function returned.
Well done! You now have a working Lambda function.

### 3. MODIFY THE LAMBDA FUNCTION TO WRITE TO DYNAMODB TABLE

1. Select the Code tab and select your function from the navigation pane on the left side of the code editor.
2. Replace the code for your function with the following:
`Full answer below. To remove some parts for participants to figure out themselves`

```Python
import json
import boto3
import time
import uuid

# Create a DynamoDB object using the AWS SDK
dynamodb = boto3.resource('dynamodb')
# Use the DynamoDB object to select our table
table = dynamodb.Table('HelloWorldDatabase')

# Define the handler function that the Lambda service will use as an entry point
def lambda_handler(event, context):
    try:
        # Get the current GMT time
        gmt_time = time.gmtime()

        # Store the current time in a human-readable format in a variable
        # Format the GMT time string
        now = time.strftime('%a, %d %b %Y %H:%M:%S +0000', gmt_time)

        # Generate UUID for DynamoDB ID
        generate_uuid = lambda: str(uuid.uuid4())

        # Extract values from the event object we got from the Lambda service and store in variables
        customer_name = event['customername']
        product_name = event['productname']
        quantity = event['quantity']

        # Generate UUID for DynamoDB ID
        order_id = generate_uuid()

        # Write product details to the DynamoDB table
        table.put_item(
            Item={
                'ID': order_id,
                'Customer Name': customer_name,
                'Product Name': product_name,
                'Quantity': quantity,
                'LatestGreetingTime': now
            })

        # Return order details in the response
        order_details = {
            'ID': order_id,
            'Customer Name': customer_name,
            'Product Name': product_name,
            'Quantity': quantity,
            'LatestGreetingTime': now
        }

        # Return a properly formatted JSON object with a 200 status code
        return {
            'statusCode': 200,
            'body': json.dumps(order_details)
        }

    except Exception as e:
        # If any error occurs, return an error response
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
``` 

## Success Criteria

1. You have successfully created a Lambda function from scratch using the AWS console (in Python, JavaScript, or Java).
2. You have successfully created events in the AWS console to test your Lambda function.
3. You have successfully defined the IAM policy for the Lambda function to access DynamoDB service.
   `Full answer below. To remove some parts for participants to figure out themselves`

```JSON
{
"Version": "2012-10-17",
"Statement": [
    {
        "Sid": "VisualEditor0",
        "Effect": "Allow",
        "Action": [
            "dynamodb:PutItem",
            "dynamodb:DeleteItem",
            "dynamodb:GetItem",
            "dynamodb:Scan",
            "dynamodb:Query",
            "dynamodb:UpdateItem"
        ],
        "Resource": "YOUR-TABLE-ARN"
    }
    ]
}
```

## Learning Resources

* [Overview of AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
* [How IAM works - AWS Identity and Access Management (amazon.com)](https://docs.aws.amazon.com/IAM/latest/UserGuide/intro-structure.html)
* [Techniques for writing least privilege IAM policies | AWS Security Blog (amazon.com)](https://aws.amazon.com/blogs/security/techniques-for-writing-least-privilege-iam-policies/)
