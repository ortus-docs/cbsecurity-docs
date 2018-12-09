---
description: ColdBox OCM (Object Cache Manager)
---

# OCM Properties

The following are properties used when the source of the rules is `ocm` or coming from the CacheBox

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `rulesOCMKey` | string | true | --- | The cache key to use to retrieve the rules from the ColdBox default cache provider |

```javascript
interceptors = [
    {class="cbsecurity.interceptors.Security", name="ApplicationSecurity", properties={
        useRegex = true, rulesSource = "ocm", validatorModel = "SecurityService",
        rulesOCMKey = "qSecurityRules"
    }}
];
```

