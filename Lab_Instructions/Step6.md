It is required that after every tutorial, you do a thorough clean-up to avoid unwarranted billing that may occur. Although this our tutorial is free from cost, its good practice to clean up after a hands-on.


Go to the S3 console https://s3.console.aws.amazon.com/s3/home?region=us-east-1 Select the bucket you created Click Empty, type or copy/paste the bucket name and click Empty, Click Exit Click Delete, type or copy/paste the bucket name and click Delete, Click Exit
Move to the API Gateway console https://console.aws.amazon.com/apigateway/main/apis?region=us-east-1 Check the box next to the petcuddleotron API Click Actions and then Delete Click Delete
Move to the lambda console https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions Check the box next to email_reminder_lambda, click Actions, Click Delete, Click Delete Check the box next to api_lambda, click Actions, Click Delete, Click Delete
Move to the Step Functions console https://console.aws.amazon.com/states/home?region=us-east-1#/statemachines Check the box next to PetCuddleOTron, CLick Delete, then Delete state machine
Move to the Pinpoint console https://us-east-1.console.aws.amazon.com/pinpoint/home?region=us-east-1#/sms-account-settings/phoneNumbers Check the box next to the origination number you created in stage 1. Click Remove Phone Number, conform and click Delete
Go to the SES console and verified identities https://us-east-1.console.aws.amazon.com/ses/home?region=us-east-1#/verified-identities Select one of the indentities, Click Delete, then click Confirm Pick the other verified identity, Click Delete, then click Confirm
Move to the cloudformation console https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filteringText=&filteringStatus=active&viewNested=true&hideStacks=false Check the box next to SMROLE , click Delete then Delete Stack Check the box next to LAMBDAROLE , click Delete then Delete Stack
AT this point you have removed all infrastructure used for this AdvancedDemo and have completed the advanced demo itself.
Good job !!

