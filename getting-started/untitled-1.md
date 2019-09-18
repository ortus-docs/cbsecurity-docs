# Overview

The topic of security is always an interesting and essential one. However, most MVC frameworks offer very loose security when it comes down to an event-oriented architecture. ColdBox Interceptors change this behavior as we have the ability to intercept requests in any point in time during our application executions. With this feature we introduce our Security Module that implements what we call **ColdBox Security**. 

With the ColdBox security module you will be able to secure all your ColdBox events from execution. However, as we all know, every application has different requirements and since we are keen on extensibility, the module can be configured to work with whatever security authentications and authorization mechanism you might have.  



![](https://github.com/ColdBox/cbox-security/wiki/ColdBoxSecurity.jpg)

The module wraps itself around the `preProcess` interception point and will try to validate security rules and/or annotations on the requested handler actions. By default, this module will register the `Security` interceptor **automatically** for you and will inspect your configuration for rules to process and activate annotation driven security.

## How Does Validation Happen?

How does the interceptor know a user doesn't have access? Well, here is where you register a Validator CFC \(`validator` setting\) with the interceptor that implements two validation functions: `ruleValidator() and annotationValidator()`.

{% hint style="info" %}
You can find an interface for these methods in `cbsecurity.interfaces.IUserValidator`
{% endhint %}

The validator's job is to tell back to the firewall if they are allowed access and if they don't, what type of validation they broke: **authentication** or **authorization**.

> `Authentication` is when a user is NOT logged in

> `Authorization` is when a user does not have the right permissions to access an event/handler or action.

## Validation Process

Once the firewall has the results and the user is NOT allowed access. Then the following will occur:

* The request will be logged via LogBox
* The current URL will be flashed as `_securedURL` so it can be used in relocations
* If using a rule, the rule will be stored in `prc` as `cbsecurity_matchedRule`
* The validator results will be stored in `prc` as `cbsecurity_validatorResults`
* If the type of invalidation is `authentication` the `cbSecurity_onInvalidAuthentication` interception will be announced
* If the type of invalidation is `authorization` the `cbSecurity_onInvalidAuthorization` interception will be announced
* If the type is `authentication` the default action for that type will be executed \(An override or a relocation\) `invalidAuthenticationEvent`
* If the type is `authorization` the default action for that type will be executed \(An override or a relocation\) `invalidAuthorizationEvent`

{% hint style="warning" %}
If you are securing a module, then the module has the capability to override the global settings if it declares them in its ModuleConfig.cfc
{% endhint %}

## Security Rules

Global Rules can be declared in your `config/ColdBox.cfc` or in any module's `ModuleConfig.cfc` inline, or they can come from the following sources:

* A json file
* An xml file
* The database by adding the configuration settings for it
* A model by executing the `getSecurityRules()` method from it

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

The firewall will inspect handlers for the `secured` annotation. This annotation can be added to the entire handler or to an action or both. The default value of the `secured` annotation is a boolean `true`. Which means, we need a user to be authenticated in order to access it.

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

The two available authentication algorithms are the following:

1. \(**default**\) By using `cflogin, cfloginuser, cflogout`
2. Security Validation Object that you create and implement.

The default security is based on what ColdFusion gives you for basic security using their security tags. You basically use `cfloginuser` to log in a user and set their appropriate **roles** in the system. The module can then match to these roles via the security rules you have created.

The second method of authentication is based on your custom security logic. You will be able to register a validation object with the module. Once a rule is matched, the module will call your validation object, send in the rule and ask if the user can access it or not. It will be up to your logic to determine if the rule is satisfied or not.

