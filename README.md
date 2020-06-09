

#### [Using OAuth 2.0 for Server to Server Applications](https://developers.google.com/identity/protocols/OAuth2ServiceAccount)

> The Google OAuth 2.0 system supports server-to-server interactions such as those between a web application and a Google service. For this scenario you need a service account, which is an account that belongs to your application instead of to an individual end user. Your application calls Google APIs on behalf of the service account, so users aren't directly involved.

## Setup 

- Login to [**Google API Console**](https://console.developers.google.com/) with an **admin** account
- Navigate to [**IAM and admin**](https://console.developers.google.com/iam-admin)
- Select [**Service accounts**](https://console.developers.google.com/iam-admin/serviceaccounts)
- Select or create a project

<img width="500" src="https://user-images.githubusercontent.com/1725068/44443436-a012aa00-a612-11e8-996f-1d36f14d1d76.png" />

- Create a new service account
  - Service account name: ``(any)``
  - Service account ID: ``(any, will auto-fill)``
  - Click **Create**
  <img width="1372" alt="create-service-account-1" src="https://user-images.githubusercontent.com/10867016/83682531-067f7700-a5b2-11ea-8572-761e5d1079b6.png">
  
- Choose the role
  - **Project->Owner**
  - Click **Continue**
<img width="1354" alt="create-service-account-2" src="https://user-images.githubusercontent.com/10867016/83682535-08e1d100-a5b2-11ea-9f1d-89a93064e068.png">

- Click **Create Key**
<img width="1917" alt="create-service-account-3" src="https://user-images.githubusercontent.com/10867016/83682539-0a12fe00-a5b2-11ea-8e76-43a2c2c2eeaf.png">

- Choose the key options
  - Key type: **JSON**
  - Click **Create**
<img width="1899" alt="create-service-account-4" src="https://user-images.githubusercontent.com/10867016/83682540-0aab9480-a5b2-11ea-9ec6-d743ef174bb2.png">
  

- Store the ``json`` file locally (you will never be able to generate it again)
- Navigate to [**APIs & Services**](https://console.developers.google.com/apis)
- Click **ENABLE APIS AND SERVICES**

<img width="500" src="https://user-images.githubusercontent.com/1725068/44443863-f7b21500-a614-11e8-8269-7e37069b6d2e.png" />

- Find the necessary [**SDK**](https://developers.google.com/identity/protocols/oauth2/scopes)

  - For example, to use the [**Directory API**](https://developers.google.com/admin-sdk/directory/) to manage users, find **Admin SDK**
  
<img width="500" src="https://user-images.githubusercontent.com/1725068/44443895-0f899900-a615-11e8-8ecc-823634f29d11.png" />

- Enable the **SDK**

<img width="500" src="https://user-images.githubusercontent.com/1725068/44443946-4cee2680-a615-11e8-849f-e48057c28ab3.png" />

- Delegate your app to act on behalf of any user in your domain
  - Login to [**Google Admin Console**](https://admin.google.com/) with an admin account

  <img width="500" src="https://user-images.githubusercontent.com/1725068/44444060-d43b9a00-a615-11e8-89a5-e60cc3fe6097.png" />

  - Select **Security**

  <img width="500" src="https://user-images.githubusercontent.com/1725068/44444126-42805c80-a616-11e8-968a-a6465da7cb66.png" />

  - Select **API Controls->Domain-Wide Delegation**

  <img width="750" src="https://user-images.githubusercontent.com/10867016/84146179-423e9480-aa29-11ea-92f9-874f03aeec02.png" />
- Click **Add New**
<img width="500" src="https://user-images.githubusercontent.com/10867016/84146700-1a036580-aa2a-11ea-83f2-c594f5df64e8.png"/>

  - Register the necessary [**scopes**](https://developers.google.com/identity/protocols/googlescopes)
    - Client Name: either the service account or the client ID
    - Enter the API scopes to grant.  **Note they are comma-delimited here** **You will need these later**.  For example, to use the [**Directory API**](https://developers.google.com/admin-sdk/directory/) to manage users, register the following:
    `https://apps-apis.google.com/a/feeds/domain/,https://www.googleapis.com/auth/admin.directory.user`

## Storing Your Key And Keeping It Safe
**Your key should remain private.  If you are using version control, such as *git*, you should keep the key out of your repository so that you do not accidentally share it.**
One way to do this is to put it in a private folder that will not be included in your repository.
1. In the **Resources** folder for your project, add a folder called **Private**
2. In your **.gitignore** file for your project, add the follwing line:
``Resources/Private``
3. Put your key into that folder
4. Check the updates to your repository to make sure that your key does not appear in the list of added/updated files.

## Modules

Several standard functions are required for the client application.

* [URL_Escape (Project Method in this repo)](https://github.com/miyako/4d-tips-google-service-account/blob/master/URL_Escape.txt)

* [Unix_Timestamp (Project Method in this repo)](https://github.com/miyako/4d-tips-google-service-account/blob/master/Unix_Timestamp.txt)

* [JWT Plugin (JSON Web Token) with ``RS256``](https://github.com/miyako/4d-plugin-jwt)

## How to make a service account call

1. Load ``private_key`` from the downloaded ``json`` file into a variable

  ***Example:***
```
    $keyFile:=File("/RESOURCES/Private/<yourfilename>.json")
    ASSERT ($keyFile.exists)
```
2. Use the key to create a signed JWT using the [JWT  Plugin](https://github.com/miyako/4d-plugin-jwt)

  | Field | Description |
  |--|--|
  | iss | ``client_email`` from the downloaded ``json`` file |
  | scope | **space-delimited** list of scope URL's the application requests |
  | aud | ``token_uri`` from the downloaded ``json`` file |
  | iat | ``Unix_timestamp //Project Method`` |
  | exp |  `` iat + 3600`` |  
  | sub | The email address of the user the app is using for this operation|
  | endpoint | ``https://oauth2.googleapis.com/token``|
  | grant_type | ``urn:ietf:params:oauth:grant-type:jwt-bearer``|
  | kid | ``private_key_id``  from the downloaded ``json`` file

  ***Example:***
```
  $username:="<EMAIL OF USER ACCOUNT YOU ARE ACTING ON BEHALF OF>"
  $ohead:=New object("alg";"RS256";"typ";"JWT") //header

    //<build the JWT>
    $keyJSON:=JSON Parse($keyFile.getText())
    $ojwt:=New object()
    $ojwt.iss:=$keyJSON.client_email
    $ojwt.scope:="https://www.googleapis.com/auth/admin.directory.user"
    $ojwt.aud:=$keyJSON.token_uri
    $ojwt.iat:=Unix_Timestamp
    $ojwt.exp:=$ojwt.iat+3600  // an hour from now
    $ojwt.sub:=$username
    $ojwt.endpoint:="https://oauth2.googleapis.com/token"
    $ojwt.grantType:="urn:ietf:params:oauth:grant-type:jwt-bearer"
    $ojwt.kid:=$keyJSON.private_key_id

    $privateKey:=$keyJSON.private_key
    $assertion:=JWT Sign (JSON Stringify($ohead);JSON Stringify($ojwt);$privateKey)
    //</build the JWT>
```
  
4. Request an access token.
  - This is done with an  HTTP ``POST`` request to 
    ``https://oauth2.googleapis.com/token``
  - The header should include
     `Content-Type:application/x-www-form-urlencoded`

  ***Example:***
```
  ARRAY TEXT($aHeaderNames;1)
  ARRAY TEXT($aHeaderValues;1)
  $aHeaderNames{1}:="Content-Type"
  $aHeaderValues{1}:="application/x-www-form-urlencoded"

  $body:="grant_type="+URL_Escape ("urn:ietf:params:oauth:grant-type:jwt-bearer")+"&assertion="+$assertion
  $url:="https://oauth2.googleapis.com/token"
  
  C_OBJECT($token)
  $status:=HTTP Request(HTTP POST method;$url;$body;$token;$aHeaderNames;$aHeaderValues)
  
  // if $status = 200, $token will contain your access token
```
 
5. Follow-on interaction with the server includes the access token in the header.

  ***Example:***
    `$header:= "Authorization: " + $token.token_type + " "+$token.access_token`

6. Plan for the access token to expire.  Your code should assume that it will occasionally need to repeat from step 2.

# References
- [Using OAuth 2.0 to Access Google APIs](https://developers.google.com/identity/protocols/oauth2)
- [Using OAuth 2.0 for Server to Server Applications](https://developers.google.com/identity/protocols/oauth2/service-account#httprest)
- [Control G Suite API access with domain-wide delegation](https://support.google.com/a/answer/162106)
