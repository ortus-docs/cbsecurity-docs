---
description: Every cbsecurity setting with its default value, in one copy-paste block.
icon: file-code
---

# Full Configuration

This is the complete `cbsecurity` configuration with every default. You never need all of it: copy the keys you want to change and leave the rest out, the defaults fill in the gaps. Each section links to the page that explains it.

{% hint style="info" %}
Put it in `moduleSettings.cbsecurity` in `config/ColdBox.cfc`, or return it from `configure()` in `config/modules/cbsecurity.cfc` on ColdBox 7 and later. See [Configuration](README.md).
{% endhint %}

```javascript
moduleSettings = {
    // The user service class that cbauth uses to validate credentials
    cbauth : { userServiceClass : "" },

    cbsecurity : {

        // --------------------------------------------------------------
        // Authentication. See authentication.md
        // --------------------------------------------------------------
        authentication : {
            // The WireBox ID of the service that adheres to IAuthService
            provider        : "authenticationService@cbauth",
            // The WireBox ID of the user service. Detected from cbauth or Basic Auth when empty
            userService     : "",
            // The prc variable that holds the authenticated user on every secured request
            prcUserVariable : "oCurrentUser"
        },

        // --------------------------------------------------------------
        // Basic Auth. See basic-auth.md
        // --------------------------------------------------------------
        basicAuth : {
            hashAlgorithm  : "SHA-512",
            hashIterations : 5,
            // The key is the username. The value can hold
            // { roles, permissions, firstName, lastName, password }
            users          : {}
        },

        // --------------------------------------------------------------
        // CSRF, passed to the cbcsrf module. Settings you set on the cbcsrf module itself win
        // over these. See usage/cross-site-request-forgery-cbcsrf.md
        // --------------------------------------------------------------
        csrf : {
            // Verify the token on every non GET request
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
        },

        // --------------------------------------------------------------
        // Firewall. See firewall/README.md
        // --------------------------------------------------------------
        firewall : {
            // Register the global firewall interceptor. Route middleware needs it
            autoLoadFirewall            : true,
            // The validator that authenticates and authorizes requests
            validator                   : "AuthValidator@cbsecurity",
            // Secure handlers and actions with annotations
            handlerAnnotationSecurity   : true,
            // Where to go when a user is not logged in, and how: redirect, override or block
            invalidAuthenticationEvent  : "",
            defaultAuthenticationAction : "redirect",
            // Where to go when a user lacks permissions, and how
            invalidAuthorizationEvent   : "",
            defaultAuthorizationAction  : "redirect",
            rules : {
                // Regular expression matching on rule match types
                useRegex : true,
                // Force SSL for all relocations
                useSSL   : false,
                // Name-value pairs added to every rule
                defaults : {},
                // Rules defined inline
                inline   : [],
                // Or loaded from a source: a JSON file, XML file, model or database
                provider : { source : "", properties : {} }
            },
            // Firewall event logs in a database
            logs : {
                enabled    : false,
                dsn        : "",
                schema     : "",
                table      : "cbsecurity_logs",
                autoCreate : true
            }
        },

        // --------------------------------------------------------------
        // JSON Web Tokens. See jwt.md
        // --------------------------------------------------------------
        jwt : {
            // The "iss" claim
            issuer                     : "",
            // The signing key. Load it from the environment
            secretKey                  : getSystemSetting( "JWT_SECRET", "" ),
            // HS256, HS384 or HS512
            algorithm                  : "HS512",
            // Minutes before an access token expires
            expiration                 : 60,
            // Header to read the access token from
            customAuthHeader           : "x-auth-token",
            // Claims that must be present or the token is rejected
            requiredClaims             : [],
            // Issue refresh tokens as well as access tokens
            enableRefreshTokens        : false,
            // Minutes before a refresh token expires, 10080 is 7 days
            refreshExpiration          : 10080,
            // Header to read the refresh token from
            customRefreshHeader        : "x-refresh-token",
            // Refresh expired tokens automatically and return the new ones as headers
            enableAutoRefreshValidator : false,
            // Enable the POST /cbsecurity/refreshtoken endpoint
            enableRefreshEndpoint      : true,
            tokenStorage               : {
                enabled    : true,
                keyPrefix  : "cbjwt_",
                // db, cachebox or a WireBox ID
                driver     : "cachebox",
                properties : { cacheName : "default" }
            }
        },

        // --------------------------------------------------------------
        // Security headers. See security-headers.md
        // --------------------------------------------------------------
        securityHeaders : {
            enabled                : true,
            // Read forwarded headers from a trusted upstream first
            trustUpstream          : false,
            contentSecurityPolicy  : { enabled : false, policy : "" },
            contentTypeOptions     : { enabled : true },
            customHeaders          : {},
            frameOptions           : { enabled : true, value : "SAMEORIGIN" },
            hsts                   : {
                enabled           : true,
                "max-age"         : "31536000",
                preload           : false,
                includeSubDomains : false
            },
            hostHeaderValidation   : { enabled : false, allowedHosts : "" },
            ipValidation           : { enabled : false, allowedIPs : "" },
            referrerPolicy         : { enabled : true, policy : "same-origin" },
            secureSSLRedirects     : { enabled : false },
            xssProtection          : { enabled : true, mode : "block" }
        },

        // --------------------------------------------------------------
        // Visualizer. See visualizer.md
        // --------------------------------------------------------------
        visualizer : {
            enabled      : false,
            secured      : false,
            securityRule : {}
        },

        // --------------------------------------------------------------
        // Route middleware. See middleware.md
        // --------------------------------------------------------------
        middleware : {
            // IPs or CIDR ranges allowed to set X-Forwarded-For
            trustedProxies : [],
            // Redirect GET and HEAD to HTTPS. Other methods are denied
            ensureHttps    : { redirect : true },
            apiKey         : {
                header    : "x-api-key",
                param     : "apiKey",
                keys      : [],
                // A WireBox ID of an object with isValidKey( key, event )
                validator : ""
            },
            honeypot       : { field : "website_url", silent : true },
            throttle       : {
                maxAttempts   : 60,
                decaySeconds  : 60,
                cacheProvider : "default",
                // Named limiters: { login : { maxAttempts : 5, decaySeconds : 60 } }
                limiters      : {}
            }
        },

        // --------------------------------------------------------------
        // Signed URLs. See middleware.md
        // --------------------------------------------------------------
        signedUrls : {
            // Required to sign URLs. Load it from the environment
            secret         : getSystemSetting( "CBSECURITY_SIGNING_SECRET", "" ),
            signatureParam : "signature",
            expiresParam   : "expires"
        }
    }
}
```
