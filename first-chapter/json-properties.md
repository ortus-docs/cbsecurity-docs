# JSON Properties

The following are properties used when the source of the rules is json

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `rulesfile` | string | true if rulesSource = JSON | --- | The location of the security rules json file |

```javascript
interceptors = [
    {class="cbsecurity.interceptors.Security", name="ApplicationSecurity", properties={
        useRegex = true, rulesSource = "json", validatorModel = "SecurityService",
        rulesFile = "config/security.json.cfm"
    }}
];
```

  


