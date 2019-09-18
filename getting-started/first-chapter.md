# Configuration

By Default, the security module will register itself for you using the module configuration settings you define in the`config/ColdBox.cfc.`  Below you can find all the settings with their default value and description.

{% code-tabs %}
{% code-tabs-item title="config/Coldbox.cfc" %}
```javascript
// Module Settings
moduleSettings = {
	// CB Security
	cbSecurity : {
		// The global invalid authentication event or URI or URL to go if an invalid authentication occurs
		"invalidAuthenticationEvent"	: "",
		// Default Auhtentication Action: override or redirect when a user has not logged in
		"defaultAuthenticationAction"	: "redirect",
		// The global invalid authorization event or URI or URL to go if an invalid authorization occurs
		"invalidAuthorizationEvent"		: "",
		// Default Authorization Action: override or redirect when a user does not have enough permissions to access something
		"defaultAuthorizationAction"	: "redirect",
		// You can define your security rules here or externally via a source
		"rules"							: [],
		// The validator is an object that will validate rules and annotations and provide feedback on either authentication or authorization issues.
		"validator"						: "CFValidator@cbsecurity",
		// If source is model, the wirebox Id to use for retrieving the rules
		"rulesModel"					: "",
		// If source is model, then the name of the method to get the rules, we default to `getSecurityRules`
		"rulesModelMethod"				: "getSecurityRules",
		// If source is db then the datasource name to use
		"rulesDSN"						: "",
		// If source is db then the table to get the rules from
		"rulesTable"					: "",
		// If source is db then the ordering of the select
		"rulesOrderBy"					: "",
		// If source is db then you can have your custom select SQL
		"rulesSql" 						: "",
		// Use regular expression matching on the rule match types
		"useRegex" 						: true,
		// Force SSL for all relocations
		"useSSL"						: false,
		// Auto load the global security firewall
		"autoLoadFirewall"				: true,
		// Activate handler/action based annotation security
		"handlerAnnotationSecurity"		: true,
		// Activate security rule visualizer, defaults to false by default
		"enableSecurityVisualizer"		: false
	}
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Custom Interceptor

You can also register multiple instances of the `cbsecurity` module using different configurations by just adding them to your app's config or even your module's configuration.  This will register a NEW firewall apart from the global security firewall registered using the global settings as defined above.

{% code-tabs %}
{% code-tabs-item title="config/Coldbox.cfc" %}
```javascript
interceptors = [

    {
        class="cbsecurity.interceptors.Security",
        properties={
            // All Settings from above
        }

]
```
{% endcode-tabs-item %}
{% endcode-tabs %}





