# XML Rules

The following are properties used when the source of the rules is xml

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `rulesfile` | string | true if rulesSource = xml | --- | The location of the security rules xml file |

```javascript
interceptors = [
    {class="cbsecurity.interceptors.Security", name="ApplicationSecurity", properties={
        useRegex = true, rulesSource = "xml", validatorModel = "SecurityService",
        rulesFile = "config/security.xml.cfm"
    }}
];
```



