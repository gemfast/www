---
title: Local Auth
subtitle: Local Authentication uses a username and password to authenticate users and provide a JWT token
tags: [features]
author: alex
---

### Admin User

By default, the `admin` user will be created with a generated password which is written to the log at startup.

```
{"level":"info","detail":"b91oPVmf20ydTt4n3w6qI5WUHzODAj87","time":"2023-07-11T15:01:25-04:00","message":"generated admin password"}
```

Alternatively, the `admin_password` can be provided in the `/etc/gemfast/gemfast.hcl` configuration file:

```
auth "local" {
    admin_password = "mysupersecretpassword"
}
```

Providing this will overwrite the previously generated password.

### Logging In

To login to the Gemfast server run the following:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{"username": "admin", "password":"mysupersecretpassword"}' \
https://gemfast.example.com/admin/api/v1/login
```

This will return an JSON object with a JWT token to use in subsuquent API calls:

```json
{
  "code": 200,
  "expire": "2023-07-12T03:18:11-04:00",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2ODkxNDYyOTEsImlkIjoiYWRtaW4iLCJvcmlnX2lhdCI6MTY4OTEwMzA5MSwicm9sZSI6ImFkbWluIn0.PZ_B8pqxqN9-hQfKQ5F0rHXmlNybQYByaeLSMHPaMog"
}
```

### Creating Additional users

This auth strategy allows administrators to specify a list of users in the `/etc/gemfast/gemfast.hcl` configuration file and also provide them a role. If no role is provided, the user will have the `read` role assigned.

```
auth "local" {
    admin_password = "mysupersecretpassword"
    user {
        username = "bobvance"
        password = "mypassword"
        // Will have "read"
    }
    user {
        username = "creed"
        password = "mypassword2"
        role     = "write"
    }
    user {
        username = "stanley"
        password = "mypassword3"
        role     = "admin"
    }
}
```

### Roles

Each `local` user will have one of the following roles assigned:

* `read` - the default role which provides read-only access to Gemfast APIs
* `write` - provides read and write access to Gemfast APIs (can upload gems)
* `admin` - provides unlimited access to Gemfast APIs

The default role can be changed via the `default_user_role` setting.

The access control policies are available to modify at `/opt/gemfast/etc/gemfast/gemfast_acl.csv`. The default ACL is the following:

| role | path | action |
| ---- | ---- | ------ |
| **admin** | * | * |
| **anonymous** | /admin/api/v1/login | * |
| **write** | /admin/api/v1/refresh-token | * |
| **write** | /admin/api/v1/token | * |
| **write** | /admin/api/v1/* | GET |
| **write** | /private/* | * |
| **read** | /admin/api/v1/refresh-token | * |
| **read** | /admin/api/v1/token | * |
| **read** | /admin/api/v1/* | GET |
| **read** | /private/* | GET |
