---
description: JSON Web Tokens configurations
---

# 🌐 JWT

## Global Configuration

Here is the default configuration for our JSON Web Tokens integration:

```javascript
cbsecurity : {
    /**
     * --------------------------------------------------------------------------
     * Json Web Tokens Settings
     * --------------------------------------------------------------------------
     * Here you can configure the JWT services for operation and storage.  In order for your firewall
     * to leverage JWT authentication/authorization you must make sure you use the `JwtAuthValidator` as your
     * validator of choice; either globally or at the module level.
     */
    jwt                     : {
        // The issuer authority for the tokens, placed in the `iss` claim
        issuer                          : "",
        // The jwt secret encoding key, defaults to getSystemEnv( "JWT_SECRET", "" )
        // This key is only effective within the `config/Coldbox.cfc`. Specifying within a module does nothing.
        secretKey               : getSystemSetting( "JWT_SECRET", "" ),
        // by default it uses the authorization bearer header, but you can also pass a custom one as well.
        customAuthHeader        : "x-auth-token",
        // The expiration in minutes for the jwt tokens
        expiration              : 60, 
        // If true, enables refresh tokens, token creation methods will return a struct instead
        // of just the access token. e.g. { access_token: "", refresh_token : "" }
        enableRefreshTokens        : false,
        // The default expiration for refresh tokens, defaults to 30 days
        refreshExpiration          : 10080,
        // The Custom header to inspect for refresh tokens
        customRefreshHeader        : "x-refresh-token",
        // If enabled, the JWT validator will inspect the request for refresh tokens and expired access tokens
        // It will then automatically refresh them for you and return them back as
        // response headers in the same request according to the customRefreshHeader and customAuthHeader
        enableAutoRefreshValidator : false,
        // Enable the POST > /cbsecurity/refreshtoken API endpoint
        enableRefreshEndpoint      : true,
        // encryption algorithm to use, valid algorithms are: HS256, HS384, and HS512
        algorithm               : "HS512",
        // Which claims neds to be present on the jwt token or `TokenInvalidException` upon verification and decoding
        requiredClaims          : [] ,
        // The token storage settings
        tokenStorage            : {
            // enable or not, default is true
            "enabled"       : true
            // A cache key prefix to use when storing the tokens
            "keyPrefix"     : "cbjwt_", 
            // The driver to use: db, cachebox or a WireBox ID
            "driver"        : "cachebox",
            // Driver specific properties
            "properties"    : {
                cacheName : "default"
            }
        }
    }
}
```

### `issuer`

The issuer authority for the tokens is placed in the `iss` claim of the token. If empty, we will use the `event.buildLink()` to create the issuer. By default, our validators also check that tokens are created by the same issuer.

### `secretKey`

The secret key is used to sign the JWT tokens. By default, it will try to load an environment variable called `JWT_SECRET` , if that setting is also empty, we will auto-generate a secret token that will last as long as the ColdFusion application scope lasts. So technically, your secret will rotate only if a secret is not specified.

Also, this key is ignored in modules. To specify a fixed key to be used in your modules, you will have to configure it by adding a cbsecurity key settings in the `moduleSettings` structure within the `config/Coldbox.cfc`.

{% hint style="success" %}
Your secret key will auto-rotate every application scope rotation. Please note that all tokens used after that scope rotation will automatically become invalid.

Please note that we use the `jwt-cfml` library for encoding/decoding tokens. Please [refer to it's documentation](https://forgebox.io/view/jwt-cfml) in order to leverage RS and ES algorithms with certificates.

[https://forgebox.io/view/jwt-cfml](https://forgebox.io/view/jwt-cfml)
{% endhint %}

### `customAuthHeader`

By default, our jwt services will look into the `authorization` header for a bearer token. However, it can also look in a custom header by this name, which defaults to `x-auth-token`. Finally, if not found, it will also look into the `rc` scope for a `rc[ 'x-auth-token' ]` as well.

### `expiration`

The default expiration in minutes for the JWT tokens. Defaults to 60 minutes

### `algorithm`

The encryption algorithm to use for the tokens. The default is **HS512**, but the available ones for are:

* HS256
* HS384
* **HS512**
* RS256
* RS384
* RS512
* ES256
* ES384
* ES512

In the case of the `RS` and `ES` algorithms, asymmetric keys are expected to be provided in unencrypted PEM or JWK format (in the latter case, first deserialize the JWK to a CFML struct). When using PEM, private keys must be encoded in PKCS#8 format.

If your private key is not currently in this format, conversion should be straightforward:

```
$ openssl pkcs8 -topk8 -nocrypt -in privatekey.pem -out privatekey.pk8
```

When decoding tokens, either a public key or certificate can be provided. (If a certificate is provided, the public key will be extracted.)

{% embed url="https://forgebox.io/view/jwt-cfml" %}

### `requiredClaims`

This is an array of claim names that each token MUST have in order to be authenticated. If a token comes in but does not have these claims in the payload structure, it will be deemed invalid.

### `tokenStorage`

By default, our JWT services will store tokens in CacheBox for you in order to be able to invalidate them. We ship with two providers for token storage: `db` and `cachebox`.

#### `Enabled`

By default, the token storage is enabled.

#### `KeyPrefix`

The key prefix to use when storing the keys in permanent storage. Defaults to `cbjwt_`

#### `Driver`

The driver to use. It can be either **db** or **cachebox** or your own WireBox Id for using custom storage.

#### `Properties`

A struct of properties to configure each storage with.

## Refresh Token Configuration

Refresh tokens have several configuration items; check them out in our [refresh token configuration section](../../jwt/refresh-tokens.md#refresh-token-configuration).
