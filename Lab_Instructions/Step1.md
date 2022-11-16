# Step 1

## VERIFY SES APPLICATION SENDING EMAIL ADDRESS


The Pet-Cuddle-O-Tron application is going to send reminder messages via `SMS` and `Email`. It will use the `Simple Email Service` or `SES`. Because this is just a demo, we won’t go into the nitty-gritty of SES. In other words, to keep things quick - we will verify the `sender address` and the `receiver address`. Ordinary in production, it could be configured to allow sending from the application email, to any users of the application. But lets keep it simple and quick for the sake of this demo.

- Ensure you are logged into an `AWS` account, have `admin privileges` and are in the `us-east-1 / N. Virginia Region`

- Move to the `SES console` https://console.aws.amazon.com/ses/home?region=us-east-1#

- Click on `Verified Identities` under `Configuration` Click `Create Identity`

- Check the `'Email Address'` checkbox


Ideally you will need a `sending email address` for the application and a `receiving email address` for your test customer. But you can use the same email for both.

- Enter whatever email you want to use to send in the box `(it needs to be a valid address as it will be checked)`

- Click `Create Identity`


You will receive an email to this address containing a `link` to click

- Click that `link`


You should see a `Congratulations!` message

- Return to the `SES` console and `Refresh` your browser, the `verification status` should now be `verified`

- Record this `address` somewhere save as the `PetCuddleOTron Sending Address`


## VERIFY SES APPLICATION CUSTOMER EMAIL ADDRESS

If you want to use a different email address for the test customer (recommended), follow the steps below

- Click `Create Identity`

- Check the `'Email Address'` checkbox For my application email ... the email for my test customer is <yingikeme@gmail.com>

- Enter whatever email you want to use to send in the box `(it needs to be a valid address as it will be checked)`

- Click `Create Identity`

You will receive an email to this address containing a `link` to click

- Click that `link`

You should see a `Congratulations!` message

- Return to the `SES` console and refresh your browser, the `verification status` should now be `verified`

- Record this `address` somewhere save as the `PetCuddleOTron Customer Address`


## VERIFY AN SMS NUMBER (THERE IS A DIFFERENT CONFIGURATION FOR USA AND OTHER COUNTRIES REQUIRING AN ORIGINATING NUMBER)

- Move to the `SNS` Console https://us-east-1.console.aws.amazon.com/sns/v3/home?region=us-east-1#/dashboard

- Click the `'Hamburger'` menu icon on the `left`.

- Click `Text Messaging (SMS)` under `Mobile`.

- Scroll down and in `Sandbox destination phone numbers` click `Add phone number`.

- Enter the `international number format` of a phone number you control.

- Pick the `verification message language` you want to use. Click `Add phone Number`.

- If you get an error here saying `no origination entities available to send`, click `Cancel`

- You will recieve a `verification number` on your phone, enter it onto the `Verification code box` and click `Verify phone number`.


## DONE WITH STEP 1

At this point you have `whitelisted` 2 email addresses for use with SES.

* the PetCuddleOTron `Sending Address`.

* the PetCuddleOTron `Customer Address`.

You have also configured `SMS number` within `SNS`. These will be configured and used by the application in later stages


