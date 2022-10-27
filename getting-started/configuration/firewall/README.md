---
description: Configuring the security firewall
---

# 🧱 Firewall

Here are the default settings for configuring the security firewall in CBSecurity:

```javascript
/**
 * --------------------------------------------------------------------------
 * Firewall Settings
 * --------------------------------------------------------------------------
 * The firewall is used to block/check access on incoming requests via security rules or via annotation on handler actions.
 * Here you can configure the operation of the firewall and especially what Validator will be in charge of verifying authentication/authorization
 * during a matched request.
 */
firewall : {
	// Auto load the global security firewall automatically, else you can load it a-la-carte via the `Security` interceptor
	"autoLoadFirewall"            : true,
	// The Global validator is an object that will validate the firewall rules and annotations and provide feedback on either authentication or authorization issues.
	"validator"                   : "CBAuthValidator@cbsecurity",
	// Activate handler/action based annotation security
	"handlerAnnotationSecurity"   : true,
	// The global invalid authentication event or URI or URL to go if an invalid authentication occurs
	"invalidAuthenticationEvent"  : "",
	// Default Auhtentication Action: override or redirect when a user has not logged in
	"defaultAuthenticationAction" : "redirect",
	// The global invalid authorization event or URI or URL to go if an invalid authorization occurs
	"invalidAuthorizationEvent"   : "",
	// Default Authorization Action: override or redirect when a user does not have enough permissions to access something
	"defaultAuthorizationAction"  : "redirect",
	// Firewall database event logs.
	"logs" : {
		"enabled"    : false,
		"dsn"        : "",
		"schema"     : "",
		"table"      : "cbsecurity_logs",
		"autoCreate" : true
	}
	// Firewall Rules, this can be a struct of detailed configuration
	// or a simple array of inline rules
	"rules"                       : {
		// Use regular expression matching on the rule match types
		"useRegex" : true,
		// Force SSL for all relocations
		"useSSL"   : false,
		// A collection of default name-value pairs to add to ALL rules
		// This way you can add global roles, permissions, redirects, etc
		"defaults" : {},
		// You can store all your rules in this inline array
		"inline"   : [],
		// If you don't store the rules inline, then you can use a provider to load the rules
		// The source can be a json file, an xml file, model, db
		// Each provider can have it's appropriate properties as well. Please see the documentation for each provider.
		"provider" : { "source" : "", "properties" : {} }
	}
},
```

### AutoLoadFirewall

By default, the security firewall is always enabled, but you can disable it globally if you like.

```javascript
autoLoadFirewall : false
```

### Annotation Security

By default, annotation security is enabled. This will inspect ALL incoming event executions for the security annotation `secured`. If you do not want to use annotation security we recommend you turn it off to avoid the inspection of events.

```javascript
handlerAnnotationSecurity : false
```

### Validator

This is the global validator that will be used to validate authentication/authorization.  The default is `CBAuthValidator@cbsecurity`.  This object needs to match the interface: `cbsecurity.interfaces.ISecurityValidator` .  The available validators we ship are:

* **CBAuth Validator**: this is the default validator, which makes use of the [cbauth](https://cbauth.ortusbooks.com/) module. It provides authentication and _permission-_based security.
* **CFML Security Validator:** Coldbox security has had this validator since version 1,  and it will talk to the ColdFusion engine's security methods (`cflogin,cflogout`). It provides authentication and _role-based_ security.
* **Basic Auth Validator:** This validator secures your app by doing [basic authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication) browser challenges to incoming requests. It can also work with the `BasicAuthUserService` and provide you a basic user credentials storage within your configuration file.&#x20;
* **JWT Validator**: If you want to use JSON Web Tokens the JWT Validator provides authorization and authentication by validating incoming access/refresh tokens via headers for RESTFul API communications.
* **Custom Validator:** You can define your own authentication and authorization engines and plug them into the cbsecurity framework.

```javascript
validator : "BasicAuthValidator@cbsecurity"
```

### InvalidAuthenticationEvent

This setting is used to set the global event that will be executed or redirected to if an invalid authentication is detected.  Usually, you want to direct users to a login screen.

```javascript
"invalidAuthenticationEvent"  : "security.login",
```

### DefaultAuthenticationAction

We set the default event above, but how do we get there? This setting is the action the firewall will take when an invalid authentication event is detected. &#x20;

1. `redirect` - Redirect them to the `invalidAuthenticationEvent`
2. `override` - Override the incoming event to the `invalidAuthenticationEvent`
3. `block` - Block the request entirely with a 401 Not Authorized response.

### InvalidAuthorizationEvent

This setting is used to set the global event that will be executed or redirected to if an invalid authorization is detected.  Usually, you could direct them to a not authorized page.

```javascript
"invalidAuthorizationEvent"  : "dashboard.notAuthorized",
```

### DefaultAuthorizationAction

We set the default event above, but how do we get there? This setting is the action the firewall will take when an invalid authorization event is detected. &#x20;

1. `redirect` - Redirect them to the `invalidAuthorizationEvent`
2. `override` - Override the incoming event to the `invalidAuthorizationEvent`
3. `block` - Block the request entirely with a 401 Not Authorized response.

### Logs

You can enable the firewall logs and CBSecurity will log all blocks the firewall detects.  By default it is disabled, but if you enable the logs, we will create the table for you.

```javascript
"logs" : {
    "enabled"    : false,
    "dsn"        : "",
    "schema"     : "",
    "table"      : "cbsecurity_logs",
    "autoCreate" : true
}
```

{% hint style="info" %}
The `dsn` key is optional and CBSecurity will inspect the Application settings for a default datasource.
{% endhint %}

### Rules

This key is used to define where rules come from and how they interact with the firewall.  The `rules` key can be of two types:

* An array of rules
* A struct of configuration with a rule source

#### Array of Rules

This is the shorthand way of defining global rules.

```javascript
"rules"                      : [
	// should use direct action and do a global redirect
	{
		"whitelist"   : "",
		"securelist"  : "admin",
		"match"       : "event",
		"roles"       : "admin",
		"permissions" : "",
		"action"      : "redirect",
		"httpMethods" : "*"
	},
	// Match only put/post
	{
		"whitelist"   : "",
		"securelist"  : "putpost",
		"match"       : "event",
		"roles"       : "",
		"permissions" : "",
		"action"      : "block",
		"httpMethods" : "put,post"
	},
	{
		"whitelist"   : "",
		"securelist"  : "cfide",
		"match"       : "url",
		"roles"       : "",
		"permissions" : "",
		"action"      : "redirect",
		"allowedIPs"  : "10.0.0.1"
	},
	// no action, use global default action
	{
		"whitelist"   : "",
		"securelist"  : "noAction",
		"match"       : "url",
		"roles"       : "admin",
		"permissions" : "",
		"httpMethods" : "*"
	}
]
```

#### Rule Configuration

If this setting is a struct, then you can configure how the rules behave and where they come from: JSON, XML, database, model, etc.

```javascript
"rules" : {
    // Use regular expression matching on the rule match types
    "useRegex" : true,
    // Force SSL for all relocations
    "useSSL"   : false,
    // A collection of default name-value pairs to add to ALL rules
    // This way you can add global roles, permissions, redirects, etc
    "defaults" : {},
    // You can store all your rules in this inline array
    "inline"   : [],
    // If you don't store the rules inline, then you can use a provider to load the rules
    // The source can be a json file, an xml file, model, db
    // Each provider can have it's appropriate properties as well. Please see the documentation for each provider.
    "provider" : { "source" : "", "properties" : {} }
}
```

#### useRegex

This is `true` by default.   It tells the fireall to use regular expression matching against white and secure lists in a rule.

#### useSSL

If `true` then if a security rule needs to do a redirect, it will force the redirect to SSL.

#### defaults

This is a collection of name-value pairs that each security rule will inherit by default.

```javascript
defaults : {
    name : "",
    action : "block"
}
```
