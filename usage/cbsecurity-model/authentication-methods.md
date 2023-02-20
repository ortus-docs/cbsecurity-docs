---
description: >-
  These methods assist a developer in getting insight into the authentication
  framework.
---

# Authentication Methods

You can leverage the CBSecurity model to get insight into some aspects of the authentication process or do authentication via the configured authentication service if needed.

### Configured Services

* `getAuthService()` - Get access to the configured authentication service
* `getUserService()` - Get access to the configured user service

### Authentication Context

* `authenticate( username, password )` - Authenticate a request
* `getUser()` - Get the authenticated user of the current request
* `isLoggedIn()` - Verify if the current request has authenticated
* `logout()` - Logout the authenticated user
