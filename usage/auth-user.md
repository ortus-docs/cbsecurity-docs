---
description: CBSecurity comes bundled with a basic authentication User
---

# Auth User

CBSecurity ships with a basic `User` object that already implements the auth and JWT interfaces and gives you basic properties.&#x20;

```
cbsecurity.models.auth.User
```

This powerful component provides a comprehensive representation of users, with robust authorization and JSON Web Token (JWT) capabilities. With this basic user, you can effortlessly manage your application's user authentication and access control. It offers a user-friendly interface for handling user profiles, permissions, and JWT generation, ensuring a secure and seamless experience for developers and end-users.

Check out the ColdBox REST Starter Templates to see it in action.

```bash
coldbox create app name=restapp skeleton=rest
```

{% @github-files/github-code-block url="https://github.com/coldbox-modules/cbsecurity/blob/master/models/auth/User.cfc" %}
