# IOC Properties

The following are properties used when the source of the rules is ioc or coming from an IoC module

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `rulesBean` | string | true if rulesSource = ioc | --- | The bean name to ask the IoC module for that has the rules |
| `rulesBeanMethod` | string | true if rulesSource = ioc | --- | The method in the bean to call that will return a query of rules |
| `rulesBeanArgs` | string | false | --- | A comma-delimited list of arguments to send into the method. This is an optional argument and if not set, the method will be called with no arguments |

```javascript
interceptors = [
    {class="cbsecurity.interceptors.Security", name="ApplicationSecurity", properties={
        useRegex = true, rulesSource = "ioc", validatorModel = "SecurityService",
        rulesBean = "SecurityService", rulesBeanMethod = "getRules", rulesBeanArgs = "sorting=true"
    }}
];
```

