#### [Using OAuth 2.0 for Server to Server Applications](https://developers.google.com/identity/protocols/OAuth2ServiceAccount)

> The Google OAuth 2.0 system supports server-to-server interactions such as those between a web application and a Google service. For this scenario you need a service account, which is an account that belongs to your application instead of to an individual end user. Your application calls Google APIs on behalf of the service account, so users aren't directly involved.

#### Checklist 

- Login to [**Google API Console**](https://console.developers.google.com/) with an **admin** account

- Navigate to [**IAM and admin**](https://console.developers.google.com/iam-admin)

- Select [**Service accounts**](https://console.developers.google.com/iam-admin/serviceaccounts)

- Select or create a project

<img width="500" src="https://user-images.githubusercontent.com/1725068/44443436-a012aa00-a612-11e8-996f-1d36f14d1d76.png" />

- Create a new service account
  - Service account name: ``any``
  - Service account ID: ``any``
  - Project role: **Owner**
  - Furnish a new private key: **true**
    - Key type: **JSON**
  - Enable G Suite Domain-wide Delegation: **true**
