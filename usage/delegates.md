---
description: Delegate yourself!
---

# Delegates

CBSecurity comes bundled with the following delegate objects that you can use in your user objects and provide them with security capabilities.

<table><thead><tr><th width="292">Delegate</th><th>Description</th></tr></thead><tbody><tr><td><code>Auth@cbsecurity</code></td><td>This delegate enables your application objects to deal with authentication features via delegation.</td></tr><tr><td><code>Authorizable@cbsecurity</code></td><td>This delegate adds authorization capabilities to your objects</td></tr><tr><td><code>JwtSubject@cbsecurity</code></td><td>This delegate adds JWT Subject methods to a target</td></tr></tbody></table>

### Auth@cbsecurity

This delegate enables your application objects to deal with authentication features via delegation.

* `jwtAuth()`
* `cbSecure()`
* `auth()`

{% code lineNumbers="true" %}
```cfscript
component name="User" delegates="auth@cbSecurity"{

}
```
{% endcode %}

### Authorizable@cbsecurity

This delegate adds authorization capabilities to your objects.

* `hasPermission()`
* `hasRole()`
* `isLoggedIn()`
* `guest()`
* `hasAll()`
* `hasNone()`
* `sameUser()`

```cfscript
component name="User" delegates="Authorizable@cbSecurity"{

}
```

### JwtSubject@cbsecurity

This delegate adds JWT Subject methods to a target.

* `getJWTCustomClaims()`
* `getJWTScopes()`

```cfscript
component name="User" delegates="JwtSubject@cbSecurity"{

}
```
