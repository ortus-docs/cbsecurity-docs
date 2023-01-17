---
description: Configuring CBSecurity for cross site request forgery attacks
---

# 🙈 CSRF

CBSecurity ships with the `cbsrf` module and can be configured in line with the `cbsecurity` key.

{% hint style="warning" %}
Please note that if any update is made to that module, verify its settings in the module's configuration documentation: [https://forgebox.io/view/cbcsrf](https://forgebox.io/view/cbcsrf)
{% endhint %}

```
/**
 * --------------------------------------------------------------------------
 * CSRF - Cross Site Request Forgery Settings
 * --------------------------------------------------------------------------
 * These settings configures the cbcsrf module. Look at the module configuration for more information
 */
csrf : {
	// By default we load up an interceptor that verifies all non-GET incoming requests against the token validations
	enableAutoVerifier     : false,
	// A list of events to exclude from csrf verification, regex allowed: e.g. stripe\..*
	verifyExcludes         : [],
	// By default, all csrf tokens have a life-span of 30 minutes. After 30 minutes, they expire and we aut-generate new ones.
	// If you do not want expiring tokens, then set this value to 0
	rotationTimeout        : 30,
	// Enable the /cbcsrf/generate endpoint to generate cbcsrf tokens for secured users.
	enableEndpoint         : false,
	// The WireBox mapping to use for the CacheStorage
	cacheStorage           : "CacheStorage@cbstorages",
	// Enable/Disable the cbAuth login/logout listener in order to rotate keys
	enableAuthTokenRotator : true
},
```

### EnableAutoVerifier

By default, this setting is turned off.  If you turn it on, then every non-GET request will be verified that it contains a valid incoming csrf token via a header or the incoming rc.

### VerifyExcludes

A list of regex patterns that will match against the incoming event. If matched, then that event will be excluded from the auto-verifier.

```javascript
verifyExcludes : [ "stripe\.", "logout" ],
```

### RotationTimeout

All csrf tokens have a life span of 30 minutes.  But you can control how long they live with this setting.

```javascript
rotationTimeout : 60,
```

### EnableEndpoint

This setting enables the `GET /cbcsrf/generate` endpoint to generate csrf tokens for secured users.  You can use this endpoint to generate user tokens via AJAX or UI-only applications. Please note that you can pass an optional `/:key` URL parameter that will generate the token for that specific key.

{% hint style="danger" %}
**IMPORTANT:** This endpoint is secured via a `secured` annotation, so make sure the firewall has annotation-driven rules enabled.
{% endhint %}

### CacheStorage

The WireBox ID to use for storing the tokens.  The default is the `CacheStorage@cbstorages` object.  However, you can use any ColdBox storage or your own as long as it matches the CBStorages API: [https://forgebox.io/view/cbstorages](https://forgebox.io/view/cbstorages).

```javascript
cacheStorage : "SessionStorage@cbstorages"
```

### EnableAuthTokenRotator

This setting is enabled by default and what it does is that it will rotate a user's secret keys when they login/logout via any authentication service registered with CBSecurity.

```javascript
enableAuthTokenRotator : false
```

