---
description: This feature set is provided by the cbcsrf module.
icon: user-visor
---

# Cross Site Request Forgery

Since version 2.4.x we have added the `cbcsrf` module as a dependency of **cbSecurity**.  Below is how you can use it:

{% hint style="info" %}
To verify tokens on only some routes, use the [`VerifyCsrf@cbsecurity`](route-middleware/verify-csrf.md) route middleware instead of the auto verifier.
{% endhint %}

## Settings

`cbcsrf` is a module of its own with its own settings, so you can configure CSRF in either place. Set them in the `csrf` key of your `cbsecurity` settings, or in the `cbcsrf` module settings. If you set the same setting in both, the value in `cbsecurity.csrf` wins.

```javascript
moduleSettings = {
    cbsecurity : {
        csrf : {
            // Verify every non GET request
            enableAutoVerifier     : false,
            // Events to skip, regular expressions allowed
            verifyExcludes         : [],
            // Minutes before a token expires, 0 for never
            rotationTimeout        : 30,
            // Enable the /cbcsrf/generate endpoint
            enableEndpoint         : false,
            // The WireBox ID of the cache storage
            cacheStorage           : "CacheStorage@cbstorages",
            // Rotate tokens when a user logs in or out
            enableAuthTokenRotator : true
        }
    }
};
```

### Which Settings Win

For each setting, the first of these that applies is used:

1. A value you set explicitly in `cbsecurity.csrf`.
2. The value of the `cbcsrf` module, from `moduleSettings.cbcsrf`, `config/modules/cbcsrf.cfc` or its defaults.

Only the keys you set in `cbsecurity.csrf` are applied. The cbsecurity defaults never overwrite a `cbcsrf` setting. It works one setting at a time, so you can mix them:

```javascript
moduleSettings = {
    cbcsrf     : { rotationTimeout : 60, enableAutoVerifier : true },
    cbsecurity : { csrf : { rotationTimeout : 15 } }
}
// rotationTimeout is 15: the cbsecurity.csrf value wins
// enableAutoVerifier is true: you did not set it in cbsecurity.csrf, so the cbcsrf value stays
```

After startup, `cbsecurity.csrf` shows the values that are in effect.

{% hint style="info" %}
Before cbsecurity 3.9.0, `cbsecurity.csrf` always overwrote the `cbcsrf` settings, even for settings you never set in it, so a `cbcsrf` setting you did not repeat in `cbsecurity.csrf` was lost.
{% endhint %}

{% hint style="warning" %}
`cbcsrf` is a separate module. If you upgrade it, check its own [documentation](https://forgebox.io/view/cbcsrf) for new settings.
{% endhint %}

### `enableAutoVerifier`

Off by default. When on, every non-GET request must carry a valid CSRF token, in the `csrf` request key or the `x-csrf-token` header. See [Automatic Token Verifier](#automatic-token-verifier).

### `verifyExcludes`

Regular expressions matched against the incoming event. A match skips the auto verifier for that event.

```javascript
verifyExcludes : [ "stripe\.", "logout" ]
```

### `rotationTimeout`

Tokens live for 30 minutes by default. Change it in minutes, or use `0` for tokens that never expire.

### `enableEndpoint`

Enables `GET /cbcsrf/generate` so AJAX and UI-only applications can fetch tokens for secured users. See the `/cbcsrf/generate` Endpoint section below.

{% hint style="danger" %}
The endpoint is protected with a `secured` annotation, so the firewall's annotation security must be enabled.
{% endhint %}

### `cacheStorage`

The WireBox ID that stores tokens. The default is `CacheStorage@cbstorages`. Use any [cbstorages](https://forgebox.io/view/cbstorages) storage that matches its API, for example `SessionStorage@cbstorages`.

### `enableAuthTokenRotator`

On by default. Rotates a user's tokens when they log in or out through any authentication service registered with cbsecurity.

## Mixins

This module will add the following UDF mixins to handlers, interceptors, layouts and views:

* `csrfToken()` : To generate a token, using the `default` or a custom key
* `csrfVerify()` : Verify a valid token or not
* `csrf()` : To generate a hidden field (`csrf`) with the token
* `csrfField()` : Generate a random token in a hidden form element and javascript that will refresh the page automatically when the token expires
* `csrfRotate()` : To wipe and rotate the tokens for the user

Here are the method signatures:

```javascript
/**
 * Provides a random token and stores it in the coldbox cache storages. You can also provide a specific key to store.
 *
 * @key A random token is generated for the key provided.
 * @forceNew If set to true, a new token is generated every time the function is called. If false, in case a token exists for the key, the same key is returned.
 *
 * @return csrf token
 */
string function csrfToken( string key='', boolean forceNew=false )

/**
 * Validates the given token against the same stored in the session for a specific key.
 *
 * @token Token that to be validated against the token stored in the session.
 * @key The key against which the token be searched.
 *
 * @return Valid or Invalid Token
 */
boolean function csrfVerify( required string token='', string key='' )

/**
 * Generate a random token and build a hidden form element so you can submit it with your form
 *
 * @key A random token is generated for the key provided.
 * @forceNew If set to true, a new token is generated every time the function is called. If false, in case a token exists for the key, the same key is returned.
 *
 * @return HTML of the hidden field (csrf)
 */
string function csrf( string key='', boolean forceNew=false )

/**
 * Generate a random token in a hidden form element and javascript that will refresh the page automatically when the token expires
 * 
 * @key A random token is generated for the key provided. CFID is the default
 * @forceNew If set to true, a new token is generated every time the function is called. If false, in case a token exists for the key, the same key is returned.
 *
 * @return HTML of the hidden field (csrf)
 */
string function csrfField( string key='', boolean forceNew=false )

/**
 * Clears out all csrf token stored
 */
function csrfRotate()
```

## Mappings

The module also registers the following mapping in WireBox: `@cbcsrf` so you can call our service model directly.

```java
component{

    property name="cbcsrf" inject="@cbcsrf";
    
}
```

## Automatic Token Expiration

By default, the module is configured to rotate all user csrf tokens **every 30 minutes**. This means that every token that gets created has a maximum life-span of `{rotationTimeout}` minutes. If you do NOT want the tokens to EVER expire during the user's logged in session, then use the value of `0` zero.

> It is recommended to rotate your keys often, in case your token get's compromised.

## Token Rotation

We have provided several methods to rotate or clear out all of a user's tokens. If you are using `cbAuth` as your module of choice for authentication, then we will listen to **logins** and **logouts** and rotate the keys for you if you have enabled the `enableAuthTokenRotator` setting.

```javascript
moduleSettings = {
    cbcsrf : {
        // Enable/Disable the cbAuth login/logout listener in order to rotate keys
        enableAuthTokenRotator : true
    }
};
```

If you are NOT using `cbAuth` then we recommend you leverage the `csrfRotate()` mixin or the `cbcsrf.rotate()` method on the `@cbsrf` model and do the manual rotation yourself.

{% code title="handlers/security.cfc" %}
```javascript
component{

    function doLogin( event, rc, prc ){
    
        if( valid login ){
            // login user
            csrfRotate();
        }
    }
    
    function logout( event, rc, prc  ){
        csrfRotate();
    }

}
```
{% endcode %}

### Simple Example

Below is a simple example of manually verifying tokens in your handlers:

{% code title="registration.cfc" %}
```javascript
component extends="coldbox.system.EventHandler"{

     function signUp( event, rc, prc ){
        // Store this in a hidden field in the form
        prc.token = csrfGenerate();
        event.setView( "registration/signup" );
    }

     function signUpProcess( event, rc, prc ){
        // Verify CSFR token from form
        if( csrfVerify( rc.token ?: '' ) {
            // save form
        } else {
            // Something isn't right
            relocate( 'handler.signup' );
        }
    }
}
```
{% endcode %}

## Automatic Token Verifier

We have included an interceptor that if loaded will verify all incoming requests to make sure the token has been passed or it will throw an exception.

The settings for this feature are:

```javascript
cbcsrf : {
    // Enable the verifier
    enableAutoVerifier : true,
    
    // A list of events to exclude from csrf verification, regex allowed: e.g. stripe\..*
    verifyExcludes : [
    
    ]
}
```

You can also register an array of regular expressions that will be tested against the incoming event and if matched, it will allow the request through with no verification.

The verification process is as follows:

* If we are doing an integration test, then skip verification
* If the incoming HTTP Method is a `get,options or head` skip verification
* If the incoming event matches any of the `verifyExcludes` setting, then skip verification
* If the action is marked with a `skipCsrf` annotation, then skip verification
*   If no `rc.csrf` exists and no `x-csrf-token` header exists, throw a&#x20;

    `TokenNotFoundException` exception
* If the token is invalid then throw a `TokenMismatchException` exception

Please note that this verifier will check the following locations for the token:

1. The request collection (`rc`) via the `cbcsrf` key
2. The request HTTP header (`x-csrf-token`) key

### `skipCsrf` Annotation

You can also annotate your event handler actions with a `skipCsrf` annotation and the verifier will also skip the verification process for those actions.

```javascript
component{

    function doTestSave( event, rc, prc ) skipCsrf{


    }

}
```

## `/cbcsrf/generate` Endpoint

This module also allows you to turn on the generation HTTP endpoint via the `enableEndpoint` boolean setting. When turned on the module will register the following route: `GET /cbcsrf/generate/:key?`. You can use this endpoint to generate tokens for your users via AJAX or UI only applications. Please note that you can pass an optional `/:key` URL parameter that will generate the token for that specific key.

This endpoint should be secured, so we have annotated it with a `secured` annotation so if you are using `cbSecurity` or `cbGuard` this endpoint will only be available to logged in users.
