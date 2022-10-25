# Configuration

## Security Settings

By Default, the security module will register itself for you using the module configuration settings you define in the`config/ColdBox.cfc.` Below you can find all the settings with their default value and description.

{% code title="config/Coldbox.cfc" %}
```javascript
// Module Settings
moduleSettings = {
    // CB Security
    cbSecurity : {
        // The global invalid authentication event or URI or URL to go if an invalid authentication occurs
        "invalidAuthenticationEvent"    : "",
        // Default Authentication Action: override or redirect when a user has not logged in
        "defaultAuthenticationAction"    : "redirect",
        // The global invalid authorization event or URI or URL to go if an invalid authorization occurs
        "invalidAuthorizationEvent"        : "",
        // Default Authorization Action: override or redirect when a user does not have enough permissions to access something
        "defaultAuthorizationAction"    : "redirect",
        // You can define your security rules here or externally via a source
        // specify an array for inline, or a string (db|json|xml|model) for externally
        "rules"                            : [],
        // The validator is an object that will validate rules and annotations and provide feedback on either authentication or authorization issues.
        "validator"                        : "CBAuthValidator@cbsecurity",
        // The WireBox ID of the authentication service to use in cbSecurity which must adhere to the cbsecurity.interfaces.IAuthService interface.
        "authenticationService"          : "authenticationService@cbauth",
        // WireBox ID of the user service to use
        "userService"                     : "",
        // The name of the variable to use to store an authenticated user in prc scope if using a validator that supports it.
        "prcUserVariable"                 : "oCurrentUser",
        // If source is model, the wirebox Id to use for retrieving the rules
        "rulesModel"                    : "",
        // If source is model, then the name of the method to get the rules, we default to `getSecurityRules`
        "rulesModelMethod"                : "getSecurityRules",
        // If source is db then the datasource name to use
        "rulesDSN"                        : "",
        // If source is db then the table to get the rules from
        "rulesTable"                    : "",
        // If source is db then the ordering of the select
        "rulesOrderBy"                    : "",
        // If source is db then you can have your custom select SQL
        "rulesSql"                         : "",
        // Use regular expression matching on the rule match types
        "useRegex"                         : true,
        // Force SSL for all relocations
        "useSSL"                        : false,
        // Auto load the global security firewall
        "autoLoadFirewall"                : true,
        // Activate handler/action based annotation security
        "handlerAnnotationSecurity"        : true,
        // Activate security rule visualizer, defaults to false by default
        "enableSecurityVisualizer"        : false,
        // JWT Settings
        "jwt"                             : {
            // The issuer authority for the tokens, placed in the `iss` claim
            "issuer"                  : "",
            // The jwt secret encoding key to use
            "secretKey"               : getSystemSetting( "JWT_SECRET", "" ),
            // by default it uses the authorization bearer header, but you can also pass a custom one as well or as an rc variable.
            "customAuthHeader"        : "x-auth-token",
            // The expiration in minutes for the jwt tokens
            "expiration"              : 60,
            // If true, enables refresh tokens, longer lived tokens (not implemented yet)
            "enableRefreshTokens"     : false,
            // The default expiration for refresh tokens, defaults to 30 days
            "refreshExpiration"       : 43200,
            // encryption algorithm to use, valid algorithms are: HS256, HS384, and HS512
            "algorithm"               : "HS512",
            // Which claims neds to be present on the jwt token or `TokenInvalidException` upon verification and decoding
            "requiredClaims"          : [] ,
            // The token storage settings
            "tokenStorage"            : {
                // enable or not, default is true
                "enabled"       : true,
                // A cache key prefix to use when storing the tokens
                "keyPrefix"     : "cbjwt_",
                // The driver to use: db, cachebox or a WireBox ID
                "driver"        : "cachebox",
                // Driver specific properties
                "properties"    : {
                    "cacheName" : "default"
                }
            }
        }
    }
};
```
{% endcode %}

## InvalidAuthentication-/ InvalidAuthorization events and default actions

The `invalidAuthenticationEvent` and `invalidAuthorizationEvent` keys can be used to provide default events when Authentication or Authorization failed. The defaultAuthenticationAction and defaultAuthorizationAction determine whether there will be a redirection or override. The default action is `redirect`, but especially for API's an `override` will be more appropriate. When using rule-based security you can override these keys for any individual rule.

## Validator

You can place a global validator in the configuration settings, but you can also override the validator on a module by module basis as well. The default validator is using the [CBAuth Validator.](../../security-validators/cbauth-validator.md)

## Authentication Services

cbsecurity ships with the [cbauth](https://github.com/elpete/cbauth) module that can provide you with a nice interface for authentication services. If you use the default `authenticationService` authenticationService@cbauth, you have to define the UserServiceClass in the cbauth module.\
However, you can plug in any WireBox ID and select your own authentication services.

{% hint style="warning" %}
If you are using cbauth as your `authenticationService` (the default), you also need to [configure cbauth.](https://cbauth.ortusbooks.com/installation-and-usage)
{% endhint %}

## User Services

cbsecurity will also require a user service if you will be dealing with any JWT security tokens. Just add your WireBox ID to the user service of your choice. If you are using cbauth, you have to define the UserServiceClass in the cbauth module.

## Automatic Firewall

Please note that by default, the security firewall will be auto-registered for you. If you do NOT want the firewall to be automatically registered for you, then use the `autoLoadFirewall` setting and make it false. Then you can use the **Custom Firewall** approach below to register the firewall manually in the order of the interceptors that you would like.

```javascript
autoLoadFirewall : false
```

## Annotation Security

By default, annotation security is enabled. This will inspect ALL incoming event executions for the security annotations. If you do not want to use annotation security we recommend you turn it off to avoid the inspection of events.

```javascript
handlerAnnotationSecurity : false
```

## Security Visualizer

ColdBox security comes with a nice graphical visualizer for all the registered security rules and settings in your global firewall. You can enable it by using the enableSecurityVisualizer setting.

```javascript
enableSecurityVisualizer :  true
```

You can then visit the `/cbsecurity` URL and you will be presented with this magical tool:

![](https://raw.githubusercontent.com/coldbox-modules/cbsecurity/development/test-harness/visualizer.png)

{% hint style="danger" %}
**Important** The visualizer is **disabled** by default and if it detects an environment of production, it will disable itself.
{% endhint %}

## Module Settings

Each module can override some settings for cbsecurity according to its needs. You will create a `cbsecurity` struct within the module's `settings` struct in the `ModuleConfig.cfc`

{% code title="module/ModuleConfig.cfc" %}
```javascript
settings = {
    // CB Security Module Settings
    cbsecurity : {
        // Module Relocation when an invalid access is detected, instead of each rule declaring one.
        "invalidAuthenticationEvent"  : "api:Home.onInvalidAuth",
        // Default Auhtentication Action: override or redirect when a user has not logged in
        "defaultAuthenticationAction" : "override",
        // Module override event when an invalid access is detected, instead of each rule declaring one.
        "invalidAuthorizationEvent"   : "api:Home.onInvalidAuthorization",
        // Default invalid action: override or redirect when an invalid access is detected, default is to redirect
        "defaultAuthorizationAction"  : "override",
        // The validator to use for this module
        "validator"                   : "JWTService@cbsecurity",
        // You can define your security rules here or externally via a source
        "rules"                       : [ { "secureList" : "api:Secure\.*" } ]
    }
}
```
{% endcode %}

The settings you see above are the only ones that module's support as of now.

## Custom Firewalls

You can also register multiple instances of the `cbsecurity` module using different configurations by just adding them to your app's config or even your module's configuration. This will register a NEW firewall apart from the global security firewall registered using the global settings as defined above.

{% code title="config/Coldbox.cfc" %}
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
{% endcode %}
