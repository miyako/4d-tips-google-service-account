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

<img width="500" src="https://user-images.githubusercontent.com/1725068/44443658-d7ce2180-a613-11e8-8991-fe406d9f0390.png" />

- Store the ``json`` file locally (you will never be able to generate it again))
- Navigate to [**APIs & Services**](https://console.developers.google.com/apis)
- Click **ENABLE APIS AND SERVICES**

<img width="500" src="https://user-images.githubusercontent.com/1725068/44443863-f7b21500-a614-11e8-8269-7e37069b6d2e.png" />

- Find **Admin SDK**

<img width="500" src="https://user-images.githubusercontent.com/1725068/44443895-0f899900-a615-11e8-8ecc-823634f29d11.png" />

- Enable **Admin SDK**

<img width="500" src="https://user-images.githubusercontent.com/1725068/44443946-4cee2680-a615-11e8-849f-e48057c28ab3.png" />

- Login to [**Google Admin Console**](https://admin.google.com/) with an admin account

<img width="500" src="https://user-images.githubusercontent.com/1725068/44444060-d43b9a00-a615-11e8-89a5-e60cc3fe6097.png" />

- Select **Security**

<img width="500" src="https://user-images.githubusercontent.com/1725068/44444126-42805c80-a616-11e8-968a-a6465da7cb66.png" />

- Select **Advanced settings**

<img width="500" src="https://user-images.githubusercontent.com/1725068/44444180-9428e700-a616-11e8-9271-b81c446a4c44.png" />

- Select [**Manage API client access**](https://admin.google.com/ManageOauthClients)

- 

<img width="500" src="https://user-images.githubusercontent.com/1725068/44444294-19140080-a617-11e8-940d-13d14463c664.png" />

```
https://apps-apis.google.com/a/feeds/domain/,https://www.googleapis.com/auth/admin.directory.user
```






