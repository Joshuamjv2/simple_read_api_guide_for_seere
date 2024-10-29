# SEERE API STRUCTURE
## Databse Tables
1. users
2. company
3. invites
4. role
5. staff
6. team
7. team_member

### 1. Users
## These are the different user types
1. admin - manages company
2. super - under seere
3. staff - under company

- All users are invited by email unless they are super users.
- When authenticated, you can call /auth/me to view all the user's information
  including the user_type.
  
**Besides super users, all users must belong to a company. Ensure to create a company
before inviting (check section 3 for invites) a company user**.

**Only superusers can use "/auth/signup", the rest have to be invited.**
This endpoint will be removed eventually when we have the initial super user so that no one can access it.

#### Signup Pyload
```python
{
  "first_name": "string",
  "last_name": "string",
  "email": "string",
  "company_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "password": "stringst",
  "user_type": "staff"
}
```


### 2. Company
You must be a registered superuser to create a company.
After creating a company, send an invite to an admin who will manage it (check section 3 for invites)
- **User permission type**: super
- **Authentication required**: yes

#### Payload
```python
{
  "name": "string",
  "address": "string",
  "cap": "string",
  "piva": "stringstrin",
  "city": "string",
  "country": "string"
}
```

### 3. Invites
For users that will belong to a company, ensure to have an existing company and add the **company_id**
- **Authentication required**: yes
- **User permission type**: super, admin

If you're a super user inviting an admin user for a company, ensure to specify "admin" for the user_type in the
request body. When users accept the invitation, they will create an account and automatically be added to the
specified company in the invitation request.

To test the invitation onboarding, we're doing this manually for now since we don't have a page where a user can set up
their account. So when an invitation is received;
1. Copy the token in the link
2. Go to **/invites/accept_invitation** and add the token as your "token" query parameter
3. Add your first_name, last_name, password to the body and submit. If request is successful, you can log into
your account.

#### Payload
```python
{
  "notes": "",
  "email": "string",
  "company_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6", # Optional
  "user_type": "staff"
}
```
**The company id is always required for users that belong to a company**

### 4. Role
- **Authentication required**: yes
- **User permission type**: super

All roles are created by the super user and can be accessed by company admins.

### 5. Staff
- **Authentication required**: yes
- **User permission type**: admin

When a company is created, and users have been added to it,
some of the users can be added into the company's staff and assigned roles
by the company admin.
#### Payload
```python
{
  "user_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "company_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "role_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```

### 6. Team
- **Authentication required**: yes
- **User permission type**: admin

An admin can add teams to their existing company
#### Payload
```python
{
  "name": "string",
  "company_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```

### 7. Team Member
- **Authentication required**: yes
- **User permission type**: admin

When a team is created, the admin can add members to it.
Each member must be an existing user in the company the platform.
#### Payload
```python
{
  "user_id": uuid,
  "team_id": uuid,
  "role_id": uuid
}
```









