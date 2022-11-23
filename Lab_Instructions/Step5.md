# Step 5

In this step, we will create an S3 bucket and static website hosting which will host the application frontend. You will download the source files for the frontend, configure it to connect to the specific API gateway and then upload them to S3. 

Finally, we will run some application tests to verify its functionality.



## CREATE THE S3 BUCKET

- Go to the `S3` Console https://s3.console.aws.amazon.com/s3/home?region=us-east-1# 

- Click `Create bucket`

- Choose a unique bucket name Ensure the region is set to US East (N.Virginia) `us-east-1`

- Scroll Down and UNTICK `Block all public access`

- Tick the box under Turning off block all public access might result in this bucket and the objects within becoming public to acknowledge you understand that you can make the bucket public.

- Scroll Down to the bottom and click `Create bucket`




## SET THE BUCKET AS PUBLIC

- Go into the bucket you just created.

- Click the `Permissions` tab.

- Scroll down and in the `Bucket Policy` area, click `Edit`.

- in the box, `paste` the code below
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicRead",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "REPLACE_WITH_YOUR_BUCKET_ARN/*"
        }
    ]
}
```




## ENABLE STATIC HOSTING

Next you need to enable `static hosting` on the `S3` bucket so that it can be used as a front end website.

- Click on the `Properties` Tab

- Scroll down and locate `Static website hosting`

- Click `Edit`

- Select Enable Select Host a static website

- For both Index Document and Error Document enter `index.html` Click `Save Changes`

- Scroll down and locate `Static website hosting` again.

- Under `Bucket Website Endpoint` copy and note down the bucket endpoint `URL`.




## DOWNLOAD AND EDIT THE FRONT END FILES

Download and extract this ZIP file https://learn-cantrill-labs.s3.amazonaws.com/aws-serverless-pet-cuddle-o-tron/serverless_frontend.zip 

Inside the serverless_frontend folder are the front end files for the serverless website :-
* `index.html` .. the main index page
* `main.css` .. the stylesheet for the page
* `whiskers.png` .. an image of whiskers !!
* `serverless.js` .. the JS code which runs in your browser. It responds when buttons are clicked, and passes the text from the boxes when it calls the API Gateway endpoint.

Open the `serverless.js` in a code/text editor. 

Locate the placeholder `REPLACEME_API_GATEWAY_INVOKE_URL` . replace it with your API Gateway `Invoke URL` at the end of this URL.. add `/petcuddleotron` it should look something like this `https://somethingsomething.execute-api.us-east-1.amazonaws.com/prod/petcuddleotron` 

`Save` the file.




## UPLOAD AND TEST


- Return to the `S3` console Click on the `Objects` Tab.

- Click `Upload`

- Drag the 4 files from the `serverless_frontend` folder onto this tab, including the `serverless.js` file you just edited. MAKE SURE ITS THE EDITED VERSION

- Click `Upload` and wait for it to complete.

- Click `Exit`

- Verify All 4 files are in the `Objects` area of the bucket.


- Open the `PetCuddleOTron URL` you just noted down in a new tab.

What you are seeing is a simple HTML web page created by the HTML file itself and the main.css stylesheet. When you click `buttons`.. that calls the `.js` file which is the starting point for the serverless application

Ok to test the application: 

- Enter an amount of time until the next cuddle ...I suggest `120 seconds `

- Enter a `message`, i suggest `JUST A MESSAGE FROM ME`

- Then enter the PetCuddleOTron Customer Address in the email box, this is the email which you verified right at the start as the customer for this application.

- Then enter your cell/mobile number in full international format in the next box. Before you do the next step and click the button on the application, if you want to see how the application works do the following open a new tab to the `Step functions` console https://console.aws.amazon.com/states/home?region=us-east-1#/statemachines

- Click on `PetCuddleOTron`

- Click on the `Logging` tab, you will see no logs, CLick on the `Executions` tab, you will see no executions..

- Move back to the web application tab (s3 bucket), then click on `LEVEL3 : ALL THE THINGS` to send both an `email` and `SMS` reminder

- Got back to the `Step functions` console make sure the `Executions` Tab is selected click the `Refresh` Icon, Click the `execution`

- Watch the graphic .. see how the `Timer` state is highlighted. The step function is now executing and it has its own state ... its a serverless flow. Keep waiting, and after `120 seconds` the visual will update showing the flow through the state machine
* `Timer` .. waits 120 seconds
* `ChoiceState` ... moves to the parallel state because you clicked on the LEVEL3 ALL THE THINGS button to send email and SMS
* Then `ParallelSMS` uses SNS directly to send a text message
* `ParallelEmail` invokes the lambda function to send an email
* `NextState` is then moved through, then finally END

- Scroll to the top, click `ExecutionInput` and you can see the information entered on the webpage. This was sent, via the JS running in browser, to the API gateway, to the api_lambda then through to the statemachine

- Click `PetCuddleOTron` at the top of the page

- Click on the `Logging` Tab

Because the roles you created had CWLogs permissions the state machine is able to log to CWLogs. Review the logs and ensure you are happy with the flow.


YOU ARE DONE DEPLOYING AND LAUNCHING A SERVERLESS APPLICATION

At this point thats everything .. you now have a fully functional serverless application
* Loads HTML & JS From S3 & Static hosting
* Communicates via JS to API Gateway
* uses api_lambda as backing resource
* runs a statemachine passing in parameters
* state machine sends email, SMS or both
* state machine terminates



## CLEAN UP IN STEP 6

