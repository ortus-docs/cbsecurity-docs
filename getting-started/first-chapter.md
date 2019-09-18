# Configuration

## Security Settings

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

## Automatic Firewall

Please note that by default, the security firewall will be auto-registered for you.  If you do NOT want the firewall to be automatically registered for you, then use the `autoLoadFirewall` setting and make it false.  Then you can use the **Custom Firewall** approach below to register the firewall manually in the order of the interceptors that you would like.

```javascript
autoLoadFirewall : false
```

## Annotation Security

By default, annotation security is enabled.  This will inspect ALL incoming event executions for the security annotations.  If you do not want to use annotation security we recommend you turn it off to avoid the inspection of events.

```javascript
handlerAnnotationSecurity : false
```

## Security Visualizer

ColdBox security comes with a nice graphical visualizer for all the registered security rules and settings in your global firewall.  You can enable it by using the enableSecurityVisualizer setting.  

```javascript
enableSecurityVisualizer :  true
```

You can then visit the `/cbsecurity` URL and you will be presented with this magical tool:

![](https://raw.githubusercontent.com/coldbox-modules/cbsecurity/development/test-harness/visualizer.png)

{% hint style="danger" %}
**Important** The visualizer is **disabled** by default and if it detects an environment of production, it will disable itself.
{% endhint %}

## Custom Firewalls

You can also register multiple instances of the `cbsecurity` module using different configurations by just adding them to your app's config or even your module's configuration.  This will register a NEW firewall apart from the global security firewall registered using the global settings as defined above.

{% code-tabs %}
{% code-tabs-item title="config/Coldbox.cfc" %}
```javascript
interceptors = [

    {
        class="cbsecurity.interceptors.Security",
        name="FirewallName",
        properties={
            // All Settings from above
        }

]
```
{% endcode-tabs-item %}
{% endcode-tabs %}





