# Overview

With the ColdBox security module you will be able to **secure** all your incoming ColdBox events from execution either through security rules or discrete annotations. However, as we all know, every application has different requirements and since we are keen on extensibility, the module can be configured to work with whatever security **authentication** and **authorization** mechanism you might have.  We call this, the security validator.

![](https://github.com/ColdBox/cbox-security/wiki/ColdBoxSecurity.jpg)

The module wraps itself around the `preProcess` interception point and will try to validate security rules and/or annotations on the requested handler actions. By default, this module will register the `Security` interceptor **automatically** for you and will inspect your configuration for rules to process and activate annotation driven security.

{% hint style="success" %}
The `preProcess` interception point happens before any event executes in a ColdBox application.
{% endhint %}

## How Does Validation Happen?

How does the interceptor know a user doesn't or does have access? Well, here is where you register a Validator CFC \(`validator` setting\) with the interceptor that implements two validation functions: `ruleValidator()` and `annotationValidator()` that will allow the module to know if the user is logged in and has the right authorizations to continue with the execution.

{% hint style="info" %}
You can find an interface for these methods in `cbsecurity.interfaces.IUserValidator`
{% endhint %}

The validator's job is to tell back to the firewall if they are allowed access and if they don't, what type of validation they broke: **authentication** or **authorization**.

> `Authentication` is when a user is NOT logged in

> `Authorization` is when a user does not have the right permissions to access an event/handler or action.

## Validation Process

Once the firewall has the results and the user is **NOT** allowed access. Then the following will occur:

* The request that was blocked will be logged via LogBox with the offending IP and extra metadata
* The current requested URL will be flashed as `_securedURL` so it can be used in relocations
* If using a rule, the rule will be stored in `prc` as `cbsecurity_matchedRule`
* The validator results will be stored in `prc` as `cbsecurity_validatorResults`
* If the type of invalidation is `authentication` the `cbSecurity_onInvalidAuthentication` interception will be announced
* If the type of invalidation is `authorization` the `cbSecurity_onInvalidAuthorization` interception will be announced
* If the type is `authentication` the default action \(`defaultAuthenticationAction`\) for that type will be executed \(An override or a relocation\) will occur against the setting `invalidAuthenticationEvent` which can be an event or a destination URL.
* If the type is `authorization` the default action \(`defaultAuthorizationAction`\) for that type will be executed \(An override or a relocation\) `invalidAuthorizationEvent` which can be an event or a destination URL.

{% hint style="warning" %}
If you are securing a module, then the module has the capability to override the global settings if it declares them in its `ModuleConfig.cfc`
{% endhint %}

## Security Rules

Global Rules can be declared in your `config/ColdBox.cfc` in plain CFML or in any module's `ModuleConfig.cfc` or they can come from the following global sources:

* A json file
* An xml file
* The database by adding the configuration settings for it
* A model by executing a `getSecurityRules()` method from it

### Rule Anatomy

A rule is a struct that can be composed of the following elements. All of them are optional except the `secureList`.

```javascript
rules = [
	{
		"whitelist" 	: "", // A list of white list events or Uri's
		"securelist"	: "", // A list of secured list events or Uri's
		"match"			: "event", // Match the event or a url
		"roles"			: "", // Attach a list of roles to the rule
		"permissions"	: "", // Attach a list of permissions to the rule
		"redirect" 		: "", // If rule breaks, and you have a redirect it will redirect here
		"overrideEvent"	: "", // If rule breaks, and you have an event, it will override it
		"useSSL"		: false, // Force SSL,
		"action"		: "", // The action to use (redirect|override) when no redirect or overrideEvent is defined in the rule.
		"module"		: "" // metadata we can add so mark rules that come from modules
	};
]
```

### Global Rules

Rules can be declared globally in your `config/ColdBox.cfc` or they can also be place in any custom module in your application:

{% code-tabs %}
{% code-tabs-item title="config/Coldbox.cfc" %}
```javascript
// CB Security
cbSecurity : {
	// Global Relocation when an invalid access is detected, instead of each rule declaring one.
	"invalidAuthenticationEvent" 	: "main.index",
	// Global override event when an invalid access is detected, instead of each rule declaring one.
	"invalidAuthorizationEvent"		: "main.index",
	// Default invalid action: override or redirect when an invalid access is detected, default is to redirect
	"defaultAuthorizationAction"	: "redirect",
	// The global security rules
	"rules" 						: [
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
{% endcode-tabs-item %}
{% endcode-tabs %}

## Annotation Security

The firewall will inspect handlers for the `secured` annotation. This annotation can be added to the entire handler or to an action or both. The default value of the `secured` annotation is a Boolean `true`. Which means, we need a user to be authenticated in order to access it.

{% code-tabs %}
{% code-tabs-item title="handlers" %}
```javascript
// Secure this handler
component secured{

	function index(event,rc,prc){}
	function list(event,rc,prc){}

}

// Same as this
component secured=true{
}

// Not the same as this
component secured=false{
}
// Or this
component{

	function index(event,rc,prc) secured{

	}
	
	function list(event,rc,prc) secured="list"{

	}

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Authorization Context

You can also give the annotation some value, which can be anything you like: A list of roles, a role, a list of permissions, metadata, etc. Whatever it is, this is the **authorization context** and the user validator must be able to not only authenticate but authorize the context or an invalid authorization will occur.

{% code-tabs %}
{% code-tabs-item title="handler/users.cfc" %}
```javascript
// Secure this handler
component secured="admin,users"{

	function index(event,rc,prc) secured="list"{

	}
	
	function save(event,rc,prc) secured="write"{

	}

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Cascading Security

By having the ability to annotate the handler and also the action you create a cascading security model where they need to be able to access the handler first and only then will the action be evaluated for access as well.

## Security Validations

As we mentioned at the beginning of this overview, the security module will use a Validator object in order to determine if the user has authentication/authorization or not.  This setting is the `validator` setting and will point to the WireBox ID that implements the following methods: `ruleValidator() and annotationValidator().`

{% code-tabs %}
{% code-tabs-item title="models/MyValidator.cfc" %}
```javascript
/**
 * This function is called once an incoming event matches a security rule.
 * You will receive the security rule that matched and an instance of the ColdBox controller.
 *
 * You must return a struct with two keys:
 * - allow:boolean True, user can continue access, false, invalid access actions will ensue
 * - type:string(authentication|authorization) The type of block that ocurred.  Either an authentication or an authorization issue.
 *
 * @return { allow:boolean, type:string(authentication|authorization) }
 */
struct function ruleValidator( required rule, required controller );

/**
 * This function is called once access to a handler/action is detected.
 * You will receive the secured annotation value and an instance of the ColdBox Controller
 *
 * You must return a struct with two keys:
 * - allow:boolean True, user can continue access, false, invalid access actions will ensue
 * - type:string(authentication|authorization) The type of block that ocurred.  Either an authentication or an authorization issue.
 *
 * @return { allow:boolean, type:string(authentication|authorization) }
 */
struct function annotationValidator( required securedValue, required controller );
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Each validator must return a `struct` with the following keys:

* `allow:boolean` A Boolean indicator if authentication or authorization was violated
* `type:stringOf(authentication|authorization)` A string that indicates the type of violation: authentication or authorization.

### CFValidator

ColdBox security ships with a default authentication and authorization validator called `CFSecurity` which is the default validator with the following WireBox ID: `CFValidator@cbsecurity` and can be found at `cbsecurity.models.CFSecurity`

The default security is based on what ColdFusion gives you for basic security using their [security functionality](https://cfdocs.org/cfloginuser). You basically use `cfloginuser` to log in a user and set their appropriate **roles** in the system. The module can then match to these roles via the security rules you have created.

{% embed url="https://cfdocs.org/cfloginuser" %}

{% embed url="https://cfdocs.org/cflogin" %}

{% code-tabs %}
{% code-tabs-item title="cbsecurity/models/CFValidator.cfc" %}
```javascript
struct function ruleValidator( required rule, required controller ){
	return validateSecurity( arguments.rule.roles );
}

struct function annotationValidator( required securedValue, required controller ){
	return validateSecurity( arguments.securedValue );
}

private function validateSecurity( required roles ){
	var results = { "allow" : false, "type" : "authentication" };

	// Are we logged in?
	if( isUserLoggedIn() ){

		// Do we have any roles?
		if( listLen( arguments.roles ) ){
			results.allow 	= isUserInAnyRole( arguments.roles );
			results.type 	= "authorization";
		} else {
			// We are satisfied!
			results.allow.true;
		}
	}

	return results;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



### Custom Validators

The second method of authentication is based on your custom security logic. You will be able to register a validation object with the module. Once a rule is matched, the module will call your validation object, send in the rule/annotation value and ask if the user can access it or not. It will be up to your logic to determine if the rule is satisfied or not.  Below is a sample permission based security validator:

{% code-tabs %}
{% code-tabs-item title="models/MySecurity.cfc" %}
```javascript
component singleton{

	struct function ruleValidator( required rule, required controller ){
		return permissionValidator( rule.permissions, controller, rule );
	}
	
	struct function annotationValidator( required securedValue, required controller ){
		return permissionValidator( securedValue, controller );
	}
	
	private function permissionValidator( permissions, controller, rule ){
		var results = { "allow" : false, "type" : "authentication" };
		var user 	= getCurrentUser();
	
		// First check if user has been authenticated.
		if( user.isLoaded() AND user.isLoggedIn() ){
			// Do we have the right permissions
			if( len( arguments.permissions ) ){
				results.allow 	= user.checkPermission( arguments.permission );
				results.type 	= "authorization";
			} else {
				results.allow = true;
			}
		}
	
		return results;
	}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Authentication vs Authorization

The security module can distinguish between authentication issues and authorization issues.  Once these actions are identified, the security module can act upon the result of these actions.  These actions are based on the following 4 settings, but they all come down to two outcomes: 

* a relocation to another event or URL
* an event override

| Setting | Default | Description |
| :--- | :--- | :--- |
| `invalidAuthenticationEvent` | --- | The global invalid authentication event or URI or URL to go if an invalid authentication occurs |
| `defaultAuthenticationAction` | **redirect** | Default Authentication Action: override or redirect when a user has not logged in |
| `invalidAuthorizationEvent` | --- | The global invalid authorization event or URI or URL to go if an invalid authorization occurs |
| `defaultAuthorizationAction` | **redirect** | Default Authorization Action: override or redirect when a user does not have enough permissions to access something |

## Interceptions

When invalid authentication or authorizations occur the interceptor will announce the following events:

* `cbSecurity_onInvalidAuthentication`
* `cbSecurity_onInvalidAuthorization`

You will receive the following data in the `interceptData` struct:

* `ip` : The offending IP address
* `rule` : The security rule intercepted or empty if annotations
* `settings` : The firewall settings
* `validatorResults` : The validator results
* `annotationType` : The annotation type intercepted, `handler` or `action` or empty if rule driven
* `processActions` : A Boolean indicator that defaults to **true**. If you change this to **false**, then the interceptor won't fire the invalid actions. Usually this means, you manually will do them.

You can use these security listeners to do auditing, logging, or even override the result of the operation.

## Security Visualizer

This module also ships with a security visualizer that will document all your security rules and your settings in a nice panel. In order to activate it you must add the `enableSecurityVisualizer` setting to your config and mark it as `true`. Once enabled you can navigate to: `/cbsecurity` and you will be presented with the visualizer.

{% hint style="danger" %}
**Important** The visualizer is disabled by default and if it detects an environment of production, it will disable itself.
{% endhint %}

![](https://raw.githubusercontent.com/coldbox-modules/cbsecurity/development/test-harness/visualizer.png)



