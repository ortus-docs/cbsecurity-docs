---
description: >-
  The auth validator leverages authentication and role-permission-based
  authorization for you.
---

# Auth Validator

ColdBox security ships with this validator that knows how to talk to the authentication service and validate authentication and authorization via permissions and roles.  All you need to do is use the WireBox ID of `AuthValidator@cbsecurity` in your `validator` setting:

```javascript
cbsecurity = {

    firewall : {
        validator = "AuthValidator@cbsecurity"
    }

}
```

{% hint style="info" %}
**AuthValidator** is the default validator for ColdBox Security
{% endhint %}

{% hint style="danger" %}
The configured authentication service must adhere to our `IAuthService` interface and the User object must adhere to the `IAuthUser` interface.
{% endhint %}

{% hint style="success" %}
Remember that a validator can exist globally and on a per ColdBox Module level.
{% endhint %}
