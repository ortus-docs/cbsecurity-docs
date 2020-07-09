# Inline Rules

Inline rules will be used by declaring them in your configuration for **cbsecurity** in the `config/ColdBox.cfc.`  This is done by making the `rules` key an array of rule structures.

{% code title="config/Coldbox.cfc" %}
```javascript
moduleSettings = {
	// CB Security
	cbSecurity : {
		// The global security rules
		"rules" : [
			// should use direct action and do a global redirect
			{
				"whitelist": "",
				"securelist": "admin",
				"match": "event",
				"roles": "admin",
				"permissions": "",
				"action" : "redirect"
			},
			// no action, use global default action
			{
				"whitelist": "",
				"securelist": "noAction",
				"match": "url",
				"roles": "admin",
				"permissions": ""
			},
			// Using overrideEvent only, so use an explicit override
			{
				"securelist": "ruleActionOverride",
				"match": "url",
				"overrideEvent": "main.login"
			},
			// direct action, use global override
			{
				"whitelist": "",
				"securelist": "override",
				"match": "url",
				"roles": "",
				"permissions": "",
				"action" : "override"
			},
			// Using redirect only, so use an explicit redirect
			{
				"securelist": "ruleActionRedirect",
				"match": "url",
				"redirect": "main.login"
			}
		]
	}
};
```
{% endcode %}

