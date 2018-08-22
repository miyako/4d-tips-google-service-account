# 4d-tips-google-service-account
Service account authorization without OAuth from 4D

[Using OAuth 2.0 for Server to Server Applications](https://developers.google.com/identity/protocols/OAuth2ServiceAccount)

The Google OAuth 2.0 system supports server-to-server interactions such as those between a web application and a Google service. For this scenario you need a service account, which is an account that belongs to your application instead of to an individual end user. Your application calls Google APIs on behalf of the service account, so users aren't directly involved. This scenario is sometimes called "two-legged OAuth," or "2LO."

