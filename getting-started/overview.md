---
description: >-
  In this page you will find a thorough overview of the capabilities of the
  ColdBox Security module.
---

# Overview

## Authentication/Authorization

For any security system you need to know **who** is authenticated \(authentication\) and **what** \(authorization\) this user is allowed to do. `cbsecurity` is no different, so it provides an:

* **Authentication system** which performs the following functions:
  * Validates user credentials
  * Logs them in and out
  * Tracks their security in sessions or any custom storage
* **Authorization** system which:
  * validates permissions or roles or both

![](../.gitbook/assets/image%20%281%29.png)

## ColdBox Security Firewall

With the ColdBox security module you will be able to **secure** all your incoming ColdBox events from execution either through security rules or discrete annotations within your code. You will also be able to leverage our `CBSecurity` service model to secure any code context anywhere.

![ColdBox Security Firewall](../.gitbook/assets/image.png)

The module wraps itself around the `preProcess` interception point \(The first execution of a ColdBox request\) and will try to validate if the request has been authenticated and authorized to execute.  This is done via security rules and/or annotations on the requested handler actions through a CBSecurity `Validator` .  The job of the validator is to make sure user requests have been authenticated and authorized:

* **CBAuth Validator**: this is the default \(and recommended\) validator, which makes use of the [cbauth](https://cbauth.ortusbooks.com/) module. It provides authentication and _permission_ based security.
* **CFML Security Validator:** Coldbox security has had this validator since version 1,  and it will talk to the ColdFusion engine's security methods \(`cflogin,cflogout`\). It provides authentication and _roles_ based security.
* **JWT Validator**: If you want to use Json Web Tokens the JWT Validator provides authorization and authentication by validating incoming access/refresh tokens for RESTFul APIs.
* **Custom Validator:** You can define your own authentication and authorization engines and plug them in to the cbsecurity framework.

## How Does Validation Happen?

How does the interceptor know a user doesn't or does have access? Well, here is where you register a Validator CFC \(`validator` setting\) with the interceptor that implements two validation functions: `ruleValidator()` and `annotationValidator()` that will allow the module to know if the user is logged in and has the right authorizations to continue with the execution.

{% hint style="info" %}
You can find an interface for these methods in `cbsecurity.interfaces.ISecurityValidator`
{% endhint %}

The validator has two options to determine if the user will be allowed access:

* The `ruleValidator`\(\) function will evaluate configured [security rules](../usage/untitled-1.md)
* The  `annotationValidator()` function will look at [security annotations](../usage/security-annotations.md) in your handler and handler actions.

You can use rules, annotations or even both. Rules are much more flexible, but more complex. Rules will be evaluated before annotations.

The validators' job is to tell back to the firewall if they are allowed access and if they don't, what type of validation they broke: **authentication** or **authorization**.

> `Authentication` is when a user is NOT logged in
>
> `Authorization` is when a user does not have the right permissions to access an event/handler or action.

## Validation Process

Once the firewall has the results and the user is **NOT** allowed access, the following will occur:

* The request that was blocked will be logged via LogBox with the offending IP and extra metadata
* The current requested URL will be flashed as `_securedURL` so it can be used in relocations
* If using a rule, the rule will be stored in `prc` as `cbsecurity_matchedRule`
* The validator results will be stored in `prc` as `cbsecurity_validatorResults`
* If the type of invalidation is `authentication` the `cbSecurity_onInvalidAuthentication` interception will be announced
* If the type of invalidation is `authorization` the `cbSecurity_onInvalidAuthorization` interception will be announced
* If the type is `authentication` the default action \(`defaultAuthenticationAction`\) for that type will be executed \(An override or a relocation\) will occur against the setting `invalidAuthenticationEvent` which can be an event or a destination URL.
* If the type is `authorization` the default action \(`defaultAuthorizationAction`\) for that type will be executed \(An override or a relocation\) `invalidAuthorizationEvent` which can be an event or a destination URL.

## Security Rules vs Annotation Security

{% code title="Security Rules" %}
```javascript
{
    "whitelist"     : "", 
    "securelist"    : "", 
    "match"            : "event",  // or url
    "roles"            : "", 
    "permissions"    : "", 
    "redirect"         : "", 
    "overrideEvent"    : "", 
    "useSSL"        : false, 
    "action"        : "redirect", // or override 
    "module"        : ""
};
```
{% endcode %}

{% code title="Annotations" %}
```javascript
// Secure the entire handler
component secured{

	function index(event,rc,prc){}
	function list(event,rc,prc){}

}
// Same as this
component secured=true{
}

// Do NOT secure the handler
component secured=false{
}
// Same as this, no annotation!
component{

	function index(event,rc,prc) secured{
	}

	function list(event,rc,prc) secured="list"{

	}
	 
```
{% endcode %}

Your application can be secured with security rules or handler and method annotations. Before making your choice, you should take the following arguments into consideration:

* annotations are directly visible in your code, but very static. 
* annotations can protect events. Rules can protect events and incoming Url's.
* rules allow you to change your action \(override or redirect\) and target on each rule. With annotations you can only use your configured default action and target.
* when stored in a file or database, rules can be edited by admins at runtime.

### Security Rules

Global Rules can be declared in your `config/ColdBox.cfc` in plain CFML or in any module's `ModuleConfig.cfc` or they can come from the following global sources:

* A json file
* An xml file
* The database by adding the configuration settings for it
* A model by executing a `getSecurityRules()` method from it

#### Rule Anatomy

A rule is a struct that can be composed of the following elements. All of them are optional except the `secureList`.

```javascript
rules = [
    {
        "whitelist"     : "", // A list of white list events or Uri's
        "securelist"    : "", // A list of secured list events or Uri's
        "match"            : "event", // Match the event or a url
        "roles"            : "", // Attach a list of roles to the rule
        "permissions"    : "", // Attach a list of permissions to the rule
        "redirect"         : "", // If rule breaks, and you have a redirect it will redirect here
        "overrideEvent"    : "", // If rule breaks, and you have an event, it will override it
        "useSSL"        : false, // Force SSL,
        "action"        : "", // The action to use (redirect|override) when no redirect or overrideEvent is defined in the rule.
        "module"        : "" // metadata we can add so mark rules that come from modules
    };
]
```

#### Global Rules

Rules can be declared globally in your `config/ColdBox.cfc` or they can also be place in any custom module in your application:

{% code title="config/Coldbox.cfc" %}
```javascript
// CB Security
cbSecurity : {
    // Global Relocation when an invalid access is detected, instead of each rule declaring one.
    "invalidAuthenticationEvent"     : "main.index",
    // Global override event when an invalid access is detected, instead of each rule declaring one.
    "invalidAuthorizationEvent"        : "main.index",
    // Default invalid action: override or redirect when an invalid access is detected, default is to redirect
    "defaultAuthorizationAction"    : "redirect",
    // The global security rules
    "rules"                         : [
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

### Annotation Security

The firewall will inspect handlers for the `secured` annotation. This annotation can be added to the entire handler or to an action or both. The default value of the `secured` annotation is a Boolean `true`. Which means, we need a user to be authenticated in order to access it.

{% code title="handlers" %}
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
{% endcode %}

#### Authorization Context

You can also give the annotation some value, which can be anything you like: A list of roles, a role, a list of permissions, metadata, etc. Whatever it is, this is the **authorization context** and the user validator must be able to not only authenticate but authorize the context or an invalid authorization will occur.

{% code title="handler/users.cfc" %}
```javascript
// Secure this handler
component secured="admin,users"{

    function index(event,rc,prc) secured="list"{

    }

    function save(event,rc,prc) secured="write"{

    }

}
```
{% endcode %}

#### Cascading Security

By having the ability to annotate the handler and also the action you create a cascading security model where they need to be able to access the handler first and only then will the action be evaluated for access as well.

## Security Validator

As we mentioned at the beginning of this overview, the security module will use a Validator object in order to determine if the user has authentication/authorization or not. This setting is the `validator` setting and will point to the WireBox ID that implements the following methods: `ruleValidator() and annotationValidator().`

{% code title="models/MyValidator.cfc" %}
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
{% endcode %}

Each validator must return a `struct` with the following keys:

* `allow:boolean` A Boolean indicator if authentication or authorization was violated
* `type:stringOf(authentication|authorization)` A string that indicates the type of violation: authentication or authorization.
* `messages:string` Info or debugging messages

### CBAuthValidator

ColdBox security ships with the `CBAuthValidator@cbsecurity` which is the default validator in the configuration setting `validator` setting.

```javascript
cbsecurity = {
    validator = "CBAuthValidator@cbsecurity"
}
```

{% hint style="warning" %}
When using the default `CBAuthValidator@cbsecurity` you also have to configure the cbauth module.
{% endhint %}

```javascript
  cbAuth: {
    userServiceClass: "UserService"
  }
```

### CFValidator

ColdBox security ships also with a CFML authentication and authorization validator called `CFSecurity` which has the following WireBox ID: `CFValidator@cbsecurity` and can be found at `cbsecurity.models.CFSecurity`

You basically use `cfloginuser` to log in a user and set their appropriate **roles** in the system. The module can then match to these roles via the security rules you have created.

{% embed url="https://cfdocs.org/cfloginuser" caption="" %}

{% embed url="https://cfdocs.org/cflogin" caption="" %}

{% code title="cbsecurity/models/CFValidator.cfc" %}
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
            results.allow     = isUserInAnyRole( arguments.roles );
            results.type     = "authorization";
        } else {
            // We are satisfied!
            results.allow.true;
        }
    }

    return results;
}
```
{% endcode %}

### Custom Validators

The second method of authentication is based on your custom security logic. You will be able to register a validation object with the module. Once a rule is matched, the module will call your validation object, send in the rule/annotation value and ask if the user can access it or not. It will be up to your logic to determine if the rule is satisfied or not. Below is a sample permission based security validator:

{% code title="models/MySecurity.cfc" %}
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
        var user     = getCurrentUser();

        // First check if user has been authenticated.
        if( user.isLoaded() AND user.isLoggedIn() ){
            // Do we have the right permissions
            if( len( arguments.permissions ) ){
                results.allow     = user.checkPermission( arguments.permission );
                results.type     = "authorization";
            } else {
                results.allow = true;
            }
        }

        return results;
    }
}
```
{% endcode %}

## Authentication vs Authorization

The security module can distinguish between authentication issues and authorization issues. Once these actions are identified, the security module can act upon the result of these actions. These actions are based on the following 4 settings, but they all come down to two outcomes:

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

{% hint style="success" %}
There are many more interception points available to you, check them out in our [Interceptions](../usage/interceptions.md) page.
{% endhint %}

## CBSecurity Model

The `CBSecurity` model was introduced in version 2.3.0 and it provides you with a way to provide authorization checks and contexts anywhere you like: handlers, layouts, views, interceptors and even models.

Getting access to the model is easy via our `cbSecure()` mixin \(handlers/layouts/views/interceptors\) or injecting it via WireBox:

```javascript
// Mixin approach
cbSecure()

// Injection
property name="cbSecurity" inject="@CBSecurity";
```

Once injected you can leverage it using our awesome methods listed below:

### **Blocking Methods**

When certain permission context is met, if not throws `NotAuthorized`

* `secure( permissions, [message] )`
* `secureAll( permissions, [message] )`
* `secureNone( permissions, [message] )`
* `secureWhen( context, [message] )`

```javascript
// Only allow access to user_admin
cbSecure().secure( "USER_ADMIN" );

// Only allow access if you have all of these permissions
cbSecure().secureAll( "EDITOR, POST_PUBLISH" )

// YOu must not have this permission, if you do, kick you out
cbSecure().secureNone( "FORGEBOX_USER" )

// Secure using security evaluations
// Kick out if you do not have the AUTHOR_ADMIN or you are not the same incoming author
cbSecurity.secureWhen( 
    cbSecurity.none( "AUTHOR_ADMIN" ) && 
    !cbSecurity.sameUser( oAuthor )  
)

// Secure using a closure 
cbSecurity.secureWhen( ( user ) => !user.isConfirmed() );
```

### **Action Context Methods**

When certain permission context is met, execute the success function/closure, else if a `fail` closure is defined, execute that instead.

* `when( permissions, success, fail )`
* `whenAll( permissions, success, fail )`
* `whenNone( permissions, success, fail )`

```javascript
var oAuthor = authorService.getOrFail( rc.authorId );
prc.data = userService.getData();

// Run Security Contexts
cbSecure()
    // Only user admin can change to the incoming role
    .when( "USER_ADMIN", ( user ) => oAuthor.setRole( roleService.get( rc.roleID ) ) )
    // The system admin can set a super admin
    .when( "SYSTEM_ADMIN", ( user ) => oAuthor.setRole( roleService.getSystemAdmin() ) )
    // Filter the data to be shown to the user
    .when( "USER_READ_ONLY", ( user ) => prc.data.filter( ( i ) => !i.isClassified ) )

// Calling with a fail closure
cbSecurity.when(
    "USER_ADMIN",
    ( user ) => user.setRole( "admin" ), //success
    ( user ) => relocate( "Invaliduser" ) //fail
);
```

### **Verification Methods**

Verify permissions or user equality

* `has( permissions ):boolean`
* `all( permissions ):boolean`
* `none( permissions ):boolean`
* `sameUser( user ):boolean`

```javascript
function edit( event, rc, prc ){
    var oUser = userService.getOrFail( rc.id ?: "" );
    if( !sameUser( oUser ) ){
        relocate( "/users" );
    }
}

<cfif cbsecure().all( "USER_ADMIN,USER_EDITOR" )>
    This is only visible to user admins!
</cfif>

<cfif cbsecure().has( "SYSTEM_ADMIN" )>
    <a href="/user/impersonate/#prc.user.getId()#">Impersonate User</a>
</cfif>

<cfif cbsecure().sameUser( prc.user )>
    <i class="fa fa-star">This is You!</i>
</cfif>
```

### **Request Context Methods**

* `secureView( permissions, successView, failView )`

{% code title="handlers/users.cfc" %}
```javascript
component{

    function index( event, rc, prc ){
     event.secureView( "USER_ADMIN", "users/admin/index", "users/index" ); 
    }

}
```
{% endcode %}

## Security Visualizer

This module also ships with a security visualizer that will document all your security rules and your settings in a nice panel. In order to activate it you must add the `enableSecurityVisualizer` setting to your config and mark it as `true`. Once enabled you can navigate to: `/cbsecurity` and you will be presented with the visualizer.

{% hint style="danger" %}
**Important** The visualizer is disabled by default and if it detects an environment of production, it will disable itself.
{% endhint %}

![](https://raw.githubusercontent.com/coldbox-modules/cbsecurity/development/test-harness/visualizer.png)

## JSON Web Tokens \(JWT\) REST Security

ColdBox Security offers a comprehensive feature set for RESTFul applications that require JSON web tokens.  We offer both access and refresh token capabilities.  Check out our [JWT Section](../jwt/jwt-services.md) for an in-depth overview.

