---
description: Security rules from a database
---

# DB Rules

CBSecurity also allows you to store your security rules in a database as long as all the columns match the keys of the rules as we saw in the [rule anatomy.](../../../overview.md#rule-anatomy)

You will use the `db` as the `source` and fill out the available db properties:

{% code title="config/Coldbox.cfc" %}
```javascript
// CB Security
cbSecurity : {
  firewall : {
    rules : {
      provider : {
        "source" : "db",
        "properties" : {
            "dsn" : "myapp",
            "sql" : "",
            "table" : "securityRules",
            "orderBy" : "order asc"
        }
      }
    }
  }
}
```
{% endcode %}

* The `dsn` property is the name of the datasource to use
* The `table` property is what table the rules are stored in
* The `orderBy` property is what order by SQL to use, by default it is empty
* The `sql` property is what SQL to execute to retrieve the rules.  The default is `select * from ${table}`\
