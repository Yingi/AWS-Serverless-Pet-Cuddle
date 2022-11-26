# Step 2

In this stage, we will be creating an `IAM Role` for the lambda function, we will also be creating the lambda function that will be used by the serverless application. We will also be configuring the created lambda function to use the email we specified as our sender’s email in `SES`. So lets start.


## CREATE THE Lambda Execution Role for Lambda

We can create the role on our `AWS Management Console` directly, but it will save us time if we can use `CloudFormation`. We will create an `IAM role` which the `email_reminder_lambda` will use to interact with `SES`. So lets use `Cloudformation`

- Click https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?stackName=LAMBDAROLE&templateURL=https://kem-labs-stack.s3.amazonaws.com/lambdarolecfn.yaml

- Check the `I acknowledge that AWS CloudFormation might create IAM resources`. box and then click `Create Stack`

- Wait for the Stack to move into the `CREATE_COMPLETE` state before moving into the next

- Move to the `IAM Console` https://console.aws.amazon.com/iam/home?#/roles and review the `execution role`

Notice that it provides SES, SNS and Logging permissions to whatever assumes this role. This is what gives lambda the permissions to interact with those services



## CREATE the email_reminder_lambda function

Next You're going to create the lambda function which will will be used by the serverless application to create an email and then send it using `SES`

- Move to the `lambda` console https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions

- Click on `Create Function`

- Select `Author from scratch`

- For `Function name` enter "email_reminder_lambda"

- For runtime click the dropdown and pick Python 3.9

- Expand `Change default execution role`

- Pick to `Use an existing Role`

- Click the `Existing Role` dropdown and pick `LambdaRole` (there will be randomness and thats ok)

- Click `Create Function`



## CONFIGURE the email_reminder_lambda function to use our Sender’s Email Address

Scroll down, to `Function code` in the `lambda_function` code box, select all the code and delete it

`Paste` in this code

```
import boto3, os, json

FROM_EMAIL_ADDRESS = 'REPLACE_ME'

ses = boto3.client('ses')

def lambda_handler(event, context):
    # Print event data to logs .. 
    print("Received event: " + json.dumps(event))
    # Publish message directly to email, provided by EmailOnly or EmailPar TASK
    ses.send_email( Source=FROM_EMAIL_ADDRESS,
        Destination={ 'ToAddresses': [ event['Input']['email'] ] }, 
        Message={ 'Subject': {'Data': 'Whiskers Commands You to attend!'},
            'Body': {'Text': {'Data': event['Input']['message']}}
        }
    )
    return 'Success!'
  ```


This function will send an email to an address it's supplied with (by step functions) and it will be FROM the email address we specify.


- Select `REPLACE_ME` and replace with the PetCuddleOTron `Sending Address` which you noted down in `STEP 1`

- Click `Deploy` to configure the lambda function

Scroll all the way to the top, and click the `copy` icon next to the lambda function `ARN`.

Note this `ARN` down somewhere same as the `email_reminder_lambda` ARN



## DONE WITH STEP 2

You have configured the lambda function which will be used to send emails on behalf of the serverless application.



