---
description: >-
  The BasicAuth validator leverages HTTP Basic Authentication for authentication
  and role-permission-based authorization for you.
---

# BasicAuth Validator

This validator will challenge users with browser-based basic authentication.  It will use whatever authentication system and user service you have configured. &#x20;

If you don't change the default settings, then CBSecurity will switch to using the `BasicAuthUserService` which allows you to store user credentials in your configuration file.  You can check out our [Basic Authentication ](../usage/basic-authentication.md)section for further information.

```json
cbsecurity = {

    firewall : {
        validator = "BasicAuthValidator@cbsecurity"
    }

}
```

{% hint style="success" %}
Remember that a validator can exist globally and on a per ColdBox Module level.
{% endhint %}
