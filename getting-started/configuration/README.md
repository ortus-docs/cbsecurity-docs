---
description: How to configure cbsecurity. Every setting has a default, so only set what you change.
icon: square-sliders-vertical
---

# Configuration

## Where to Configure

cbsecurity registers itself from the `cbsecurity` key of your `moduleSettings` in `config/ColdBox.cfc`. In ColdBox 7 and later you can instead create `config/modules/cbsecurity.cfc` (see below). You only need to set the keys you change, the rest keep their defaults.

## Settings at a Glance

This is every top-level key with the values most apps set. Each section links to its page for the full list of options.

```javascript
moduleSettings = {
    // The user service cbauth uses to validate credentials
    cbauth : { userServiceClass : "models.UserService" },

    cbsecurity : {
        authentication : {
            provider        : "authenticationService@cbauth",
            userService     : "",
            prcUserVariable : "oCurrentUser"
        },
        firewall : {
            autoLoadFirewall            : true,
            validator                   : "AuthValidator@cbsecurity",
            handlerAnnotationSecurity   : true,
            invalidAuthenticationEvent  : "security.login",
            defaultAuthenticationAction : "redirect",
            invalidAuthorizationEvent   : "security.notAuthorized",
            defaultAuthorizationAction  : "redirect",
            rules                       : [ { secureList : "^admin", roles : "admin" } ],
            logs                        : { enabled : false }
        },
        jwt             : { secretKey : getSystemSetting( "JWT_SECRET", "" ) },
        csrf            : { enableAutoVerifier : false },
        basicAuth       : { users : {} },
        securityHeaders : { enabled : true },
        visualizer      : { enabled : false },
        middleware      : { trustedProxies : [] },
        signedUrls      : { secret : getSystemSetting( "CBSECURITY_SIGNING_SECRET", "" ) }
    }
}
```

## Settings Pages

| Key | What it configures | Page |
| --- | --- | --- |
| `authentication` | The service that logs users in and out | [Authentication](authentication.md) |
| `firewall` | The validator, invalid actions, rules and logs | [Firewall](firewall/README.md) |
| `jwt` | JSON Web Token creation, storage and refresh | [JWT](jwt.md) |
| `csrf` | The `cbcsrf` module | [CSRF](csrf.md) |
| `basicAuth` | Basic Auth hashing and in-config users | [Basic Auth](basic-auth.md) |
| `securityHeaders` | Host, IP, SSL, HSTS and other protections | [Security Headers](security-headers.md) |
| `visualizer` | The rule debugging panel | [Visualizer](visualizer.md) |
| `middleware`, `signedUrls` | Route middleware defaults and signed URLs | [Route Middleware](middleware.md) |

## ColdBox 7 Module Config File

In ColdBox 7 and later you can keep the settings in their own file, `config/modules/cbsecurity.cfc`, with a `configure()` method that returns the same structure:

```javascript
component {

    function configure(){
        return {
            authentication : { provider : "authenticationService@cbauth" },
            firewall       : {
                validator                  : "AuthValidator@cbsecurity",
                invalidAuthenticationEvent : "security.login"
            }
        };
    }

}
```

## Module Settings

Each module can also have its own CBSecurity settings which override or collaborate with the global settings. So what can a module do:

* Have its own validator
* Have its own security rules
* Have its own invalid authentication event and action
* Have its own invalid authorization event and action

You will create a `cbsecurity` struct within the module's `settings` struct in the `ModuleConfig.cfc`

{% code title="module/ModuleConfig.cfc" %}
```javascript
settings = {
    cbsecurity : {
        firewall : {
            // Where to go when a user is not logged in, and how: redirect, override or block
            invalidAuthenticationEvent  : "api:Home.onInvalidAuth",
            defaultAuthenticationAction : "override",
            // Where to go when a user lacks permissions, and how
            invalidAuthorizationEvent   : "api:Home.onInvalidAuthorization",
            defaultAuthorizationAction  : "override",
            // The validator for this module
            validator                   : "JwtAuthValidator@cbsecurity",
            // Rules as a simple array. You can also use the full form with
            // `inline`, `defaults` and `provider`, see the Firewall page.
            rules                       : [ { secureList : "api:Secure\.*" } ]
        }
    }
}
```
{% endcode %}

{% hint style="danger" %}
Please note that a module's security rules will be **PREPENDED** to the global rules
{% endhint %}

### Loading and Unloading

Also note that if modules are loaded dynamically, it will still inspect them and register them if cbsecurity settings are found. The same goes for unloading, the entire security rules for that module will cease to exist.
