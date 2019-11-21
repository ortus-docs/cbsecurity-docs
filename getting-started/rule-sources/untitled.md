# DB Rules

If you have your security rules in a database, then cbsecurity can read the rules from the database for you.  Just make the `rules` key  equal to `db` and fill out the extra configuration keys shown below:



| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `rulesDSN` | string | true | --- | The dsn to use if the rules are coming from a database |
| `rulesTable` | string | true | --- | The table where the rules are |
| `rulesSQL` | string | false | `select* from rulesTable` | The custom SQL statement to use to retrieve the rules according to the rulesTable property. If not set, the default of select\* from rulesTable will be used. |
| `rulesOrderBy` | string | false | --- | The column to order the rules by. If not chosen, the interceptor will not order the query, just select it. |

{% code title="config/Coldbox.cfc" %}
```javascript
moduleSettings = {
	// CB Security
	cbSecurity : {
		rules        : "db", // Rules are in the database
		rulesDSN     : "myDatasource", // The datasource
		rulesTable   : "securityRules", // The table that has the rules
		rulesOrderBy : "order asc" // An optional ordering
	}
};
```
{% endcode %}

