## Step 1 (optional)
Create new Google Script Project [Click here](http://script.google.com/)

* To use other Google APIs with Google Script Project, you need to change the Google Script Project type from default to standard under Project Settings.
* To change project type you need to enter Google Cloud Project number. 

## Step 2
If you already creted one, get project from [this link](https://console.cloud.google.com/iam-admin/settings)
or Create a new Google Cloud Project [visit](https://console.cloud.google.com/)

## Step 3
* Update Concent Screen Deatils [Click here](https://console.cloud.google.com/apis/credentials/consent)
* Add desired Scopes. All Oauth Scope List : [View](https://developers.google.com/identity/protocols/oauth2/scopes)

If you see "Needs verification", don't worry, On your login screen you will se "Not Verified by google", but app will work fine.

## Step 4
Add Scopes to Google App Script Project in appmanifest.json file [See Refrence](https://developers.google.com/apps-script/concepts/scopes)

## Step 5 (Optional)
Add aditional scopes desired by app [from this list](https://developers.google.com/identity/protocols/oauth2/scopes#script)

## Step 6 (Optional)
If using UrlFetchApp, add `"https://www.googleapis.com/auth/script.external_request"` to your appmanifest.json scopes.

## Conratulations !! 
### You have successfully integrated Google App Script with additional Google APIs.

