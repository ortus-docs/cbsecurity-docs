---
description: Configuration for basic authentication
---

# 🥸 Basic Auth

The `basicAuth` key is used to store user credentials that will be used with [Basic Authentication](../../usage/basic-authentication.md) and how the passwords are stored in memory.

```javascript
/**
 * --------------------------------------------------------------------------
 * Basic Auth
 * --------------------------------------------------------------------------
 * These settings are used so you can configure the hashing patterns of the user storage
 * included with cbsecurity.  These are only used if you are using the `BasicAuthUserService` as
 * your service of choice alongside the `BasicAuthValidator`
 */
basicAuth : {
	// Hashing algorithm to use
	hashAlgorithm  : "SHA-512",
	// Iterates the number of times the hash is computed to create a more computationally intensive hash.
	hashIterations : 5,
	// User storage: The `key` is the username. The value is the user credentials that can include
	// { roles: "", permissions : "", firstName : "", lastName : "", password : "" }
	users          : {}
}
```

### HashAlgorithm

This is the default algorithm used when hashing the user storage passwords in memory.  The default is `SHA-512`

```javascript
hashAlgorithm  : "SHA-256",
```

### HashIterations

Iterates the number of times the hash is computed to create a more computationally intensive hash.  The default is `5`

```javascript
hashIterations  : 10,
```

### Users

This is the in-memory user storage system.  It's a `struct` and each **key** represents a unique `username` in the storage system.  Each user can then have the following attributes, but in reality, you can add as many attributes as you want.

* `password` - The only mandatory attribute.
* `firstName`
* `lastName`
* `roles`
* `permissions`

```javascript
users : {
    "lmajano" : { password : "test", permissions : "read,write",
    "guest" : { password : "guest", permissions : "read" }
}
```
