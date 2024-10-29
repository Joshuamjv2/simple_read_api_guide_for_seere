# Seere API Guide

## API User Guide


### 1. Introduction
This document provides a step-by-step walkthrough to help you interact with our API using FastAPI and Swagger documentation. 
This guide covers the basics, from accessing the documentation to executing requests and interpreting responses.
**Swagger Documentation URL:** [Click here](http://85.235.151.17:8001/docs)

### 2. Prerequisites
To get started, ensure you have:
- **Browser:** Access to a web browser to open the Swagger documentation
- **API Authorization:** An API token or credentials (**covered in section 4**)
- **Tools (Optional):** Tools like Postman or cURL for testing requests outside Swagger.

### 3. Signup
To be able to use the platform, you must be a registered user. There is a **/signup** endpoint for super
users who are not invited, however, the rest of the users must be invited to create an account.

##### Here are the steps to signup if you're **not** an invited super user:
1. got to /auth/signup and provide the required information as stated in the swagger documentation
2. if your signup is successful, you will receive a link in your registered email address for you to verify your account
3. For now since we don't have a user interface, copy the token in the link(the long string value after **token=**)
4. Back to the API, go to **/auth/verify_email** and paste the token there
5. If this is successful, your account is verified, and you can now login

##### Here are the steps to signup if you're an invited super user:
Once you receive an invite to join Seere, you can create an account. Again, since we don't have a UI
to do this, we shall do it from the API directly:
1. Copy the token in the invitation link
2. Go to **/invites/accept_invitation** and past the token there as a query parameter, then provide your **first_name**, **last_name**, and **password**
3. Once submitted and you have a successful request, your account has been created and you can log in with your email and password

### 4. Authentication
Go to **/auth/login** and provide your email and password to receive an **access token** and a "**referesh token**".
The access token has a way shorter life span than a refresh token. Once the access token is expired, you can use the refresh token to renew it
from **/auth/refresh_token**

When you have the access token, ensure to have it in your request headers, for example your headers can look like:
```python
{
  "Content-type": "application/json",
  "Authorization: "Bearer <your_access_token>"
}
```
**To refresh the access token from /auth/refresh_token, ensure to replace the access token in your headers with the refresh token**
If your requests are valid, you can make API calls to protected endpoints and get valid responses.

### 5. Invites
Users get onto the platform by invitation. You must be invited by an admin user or super user.
To invite a user, make sure you're authenticated first as described in section 4. Users invited to a company must have a company ID.
The only users who don't belong to a company are super users.
Go to **/invites/send_invite**, fill in the invited user's information and submit the request. Once a user receives your invite,
they can set up their account as described in section 3.
```python
# Here is an example request body for an invite
{
  "notes": "", # Optional
  "email": "string",
  "company_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6", # Optional
  "user_type": "staff"
}
```

### 6. Companies
In order to create a company; 
1. ensure you're logged in as a super user
2. create company at **/company** as a post request,
```python
# Here is an example request body for a company
{
  "name": "string",
  "address": "string",
  "cap": "string",
  "piva": "stringstrin",
  "city": "string",
  "country": "string"
}
```
3. You access other endpoints like:
- **/company** (get) - view all companies if you're a super user
- **/company/{company_id}** (get) - view single company
- **/company/{company_id}** (patch) - Update a single company
- **/company/{company_id}** (delete) - Delete a single company if youre a super user

Under the company object, you will receive information about the teams, users and invites in a company.


