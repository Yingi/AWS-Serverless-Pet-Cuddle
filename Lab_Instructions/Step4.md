# Step 4


In this stage you will be creating the front end API for the serverless application. The frontend loads from `S3`, runs in your browser and communicates with this API.
It uses `API Gateway` for the API Endpoint, and this uses `Lambda` to provide the backend compute.

First you will create the supporting `API_LAMBDA` and then the `API Gateway`





## CREATE API LAMBDA FUNCTION WHICH SUPPORTS API GATEWAY

- Go to the `Lambda` console https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions

- Click on `Create Function`

- For `Function Name` use `api_lambda`

- For `Runtime` use `Python 3.9`

- Expand `Change default execution role`

- Select `Use an existing role`

- Choose the `LambdaRole` from the dropdown

- Click `Create Function`

This is the lambda function which will support the API Gateway




## CONFIGURE THE LAMBDA FUNCTION (Using the current UI)

- Scroll down, and remove all the code from the `lambda_function` text box

- Open this link in a new tab https://learn-cantrill-labs.s3.amazonaws.com/aws-serverless-pet-cuddle-o-tron/api_lambda.py 
- depending on your browser it might download the .py file, if so, open it in either your code editor, or notepad on windows, or textedit on a mac and copy it all into your clipboard 

- Move back to the `Lambda` console.

- Select the existing lambda code and delete it.

- `Paste` the code into the lambda fuction.

This is the function which will provide compute to API Gateway. Its job is to be called by API Gateway when its used by the serverless front end part of the application (loaded by S3) It accepts some information from you, via API Gateway and then it starts a state machine execution - which is the logic of the application.

- You need to locate the `YOUR_STATEMACHINE_ARN` placeholder and replace this with the `State Machine ARN` you noted down in the previous step.

- Click `Deploy` to save the lambda function and configuration.





## CONFIGURE THE LAMBDA FUNCTION (Using the preview/NEW UI)

- Under `Aliases` click `Latest` Click the `Code Tab`

- Open this link in a new tab https://learn-cantrill-labs.s3.amazonaws.com/aws-serverless-pet-cuddle-o-tron/api_lambda.py depending on your browser it might download the .py file, if so, open it in either your code editor, or notepad on windows, or textedit on a mac and copy it all into your clipboard 

- Move back to the `Lambda` console.

- Select the existing lambda code and delete it.

- `Paste` the code into the lambda fuction.

This is the function which will provide compute to API Gateway. Its job is to be called by API Gateway when its used by the serverless front end part of the application (loaded by S3) It accepts some information from you, via API Gateway and then it starts a state machine execution - which is the logic of the application.

You need to locate the `YOUR_STATEMACHINE_ARN` placeholder and replace this with the `State Machine ARN` you noted down in the previous step.

- Click `Deploy as latest`




## CREATE API

Now we have the `api_lambda` function created, the next step is to create the API Gateway, API and Method which the front end part of the serverless application will communicate with.

- Move to the `API Gateway` console https://console.aws.amazon.com/apigateway/main/apis?region=us-east-1

- Click `APIs` on the menu on the `left`

- Locate the `REST API` box, and click `Build` (being careful not to click the build button for any of the other types of API ... REST API is the one you need) If you see a popup dialog `Create your first API` dismiss it by clicking `OK`

- Under `Create new API` ensure `New API` is selected.

- For API name* enter `petcuddleotron`

- For `Endpoint Type` pick `Regional` Click `create API`




## CREATE RESOURCE

- Click the `Actions` dropdown and Click `Create Resource`

- Under `resource name` enter `petcuddleotron`

- Make sure that `Configure as proxy resource` is NOT ticked - this forwards everything as is, through to a lambda function, because we want some control, we DONT want this ticked.

- Towards the bottom MAKE SURE TO TICK `Enable API Gateway CORS`. This relaxes the restrictions on things calling on our API with a different DNS name, it allows the code loaded from the S3 bucket to call the API gateway endpoint. if you DONT check this box, the API will fail

- Click `Create Resource`




## CREATE METHOD

Ensure you have the `/petcuddleotron` resource selected, click `Actions` dropdown and click `create method`

- In the small dropdown box which appears below `/petcuddleotron` select `POST` and click the `tick symbol` next to it. This method is what the front end part of the application will make calls to. It's what the `api_lambda` will provide services for.

- Ensure for `Integration Type` that `Lambda Function` is selected.

- Make sure `us-east-1` is selected for Lambda Region

- In the Lambda Function box.. start typing `api_lambda` and it should autocomplete, click this `auto complete` (Make sure you pick `api_lambda` and not email reminder lambda)

- Make sure that Use `Default Timeout` box IS ticked.

- Make sure that Use `Lambda Proxy integration` box IS ticked, this makes sure that all of the information provided to this API is sent on to lambda for processing in the event data structure. If you don't tick this box, the API will fail

- Click `Save`

You may see a dialogue stating "You are about to give API Gateway permission to invoke your Lambda function:. AWS is asking for your OK to adjust the resource policy on the lambda function to allow API Gateway to invoke it". This is a different policy to the execution role policy which controls the permissions lambda gets.




## DEPLOY API

Now the API, Resource and Method are configured - you now need to deploy the API out to API gateway, specifically an API Gateway STAGE.

- Click `Actions` Dropdown and `Deploy API`

- For `Deployment Stage` select `New Stage`

- For `stage name` and `stage description` enter `prod`

- Click `Deploy`

At the top of the screen will be an `Invoke URL` .. note this down somewhere safe, you will need it in the next STAGE. This URL will be used by the client side component of the serverless application and this will be unique to you.




## DONE WITH STEP 4

At this point you have configured the last part of the AWS side of the serveless application.

You now have :-
* SES Configured
* An Email Lambda function to send email using SES
* A State Machine configured which can send EMAIL, SMS or BOTH after a certain time period when invoked.
* An API, Resource & Method, which use a lambda function for backing deployed out to the PROD stage of API Gateway

In `STEP 5` you will configure the client side of the application (loaded from S3, running in a browser) so that it communicates to API Gateway.



