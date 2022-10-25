# CBAuth Validator

ColdBox security ships with the **cbauth** validator that knows how to talk to the authentication service and validate authentication and authorization via permissions.  All you need to do is use the WireBox ID of `CBAuthValidator@cbsecurity` in your `validator` setting:

```javascript
cbsecurity = {

    validator = "CBAuthValidator@cbsecurity"

}
```

{% hint style="info" %}
**CBAuthValidator** is the default validator for ColdBox Security
{% endhint %}

Just make sure your User object adheres to our interface of [IAuthUser](../usage/authentication-services.md#user-interface).

