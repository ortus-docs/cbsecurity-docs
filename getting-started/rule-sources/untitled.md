# DB Rules

The following are properties used when the source of the rules is db or coming from the database.

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `rulesDSN` | string | true if rulesSource = db | --- | The dsn to use if the rules are coming from a database |
| `rulesTable` | string | true if rulesSource = db | --- | The table where the rules are |
| `rulesSQL` | string | false | `select* from rulesTable` | The custom SQL statement to use to retrieve the rules according to the rulesTable property. If not set, the default of select\* from rulesTable will be used. |
| `rulesOrderBy` | string | false | --- | The column to order the rules by. If not chosen, the interceptor will not order the query, just select it. |

```javascript
interceptors = [
    {class="cbsecurity.interceptors.Security", name="ApplicationSecurity", properties={
        useRegex = true, rulesSource = "db", validatorModel = "SecurityService",
        rulesDSN = "myApp", rulesTable = "securityRules", rulesOrderBy = "order asc"
    }}
];
```

