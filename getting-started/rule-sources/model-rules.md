# Model Rules

If you prefer to store your rules your way, then that's perfectly fine.  Just make your `rules` setting point to `model` and then provide us with the object to get the rules from.

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `rulesModel` | string | true | --- | The WireBox ID of the object that we will use to retrieve the rules from |
| `rulesModelMethod` | string | false | `getSecurityRules` | The name of the method to call on the object. |



{% code title="config/Coldbox.cfc" %}
```javascript
moduleSettings = {
	// CB Security
	cbSecurity : {
		"rules" 		: "model",
		"rulesModel" 	: "SecurityService"
	}
};
```
{% endcode %}

  


