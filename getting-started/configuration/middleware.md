---
description: Configure the cbsecurity route middleware and signed URLs.
---

# 🚦 Route Middleware

Defaults for the [route middleware](../../usage/route-middleware.md) live under the `middleware` and `signedUrls` keys of your `cbsecurity` module settings. Every value that has a route `meta` key can be overridden per route.

```javascript
moduleSettings = {
    cbsecurity : {
        middleware : {
            // IPs or CIDR ranges allowed to set X-Forwarded-For
            trustedProxies : [],
            ensureHttps    : { redirect : true },
            apiKey         : {
                header    : "x-api-key",
                param     : "apiKey",
                keys      : [],
                validator : ""
            },
            honeypot       : { field : "website_url", silent : true },
            throttle       : {
                maxAttempts   : 60,
                decaySeconds  : 60,
                cacheProvider : "default",
                limiters      : {}
            }
        },
        signedUrls : {
            secret         : getSystemSetting( "CBSECURITY_SIGNING_SECRET", "" ),
            signatureParam : "signature",
            expiresParam   : "expires"
        }
    }
}
```

You only need to set the keys you change. The rest keep their defaults.

| Setting | Used by | See |
| --- | --- | --- |
| `middleware.trustedProxies` | `AllowedIPs`, `DenyIPs`, `Throttle` | [IP Filtering](../../usage/route-middleware/ip-filtering.md) |
| `middleware.ensureHttps` | `EnsureHttps` | [Ensure HTTPS](../../usage/route-middleware/ensure-https.md) |
| `middleware.apiKey` | `ApiKey` | [API Keys](../../usage/route-middleware/api-key.md) |
| `middleware.honeypot` | `Honeypot` | [Honeypot](../../usage/route-middleware/honeypot.md) |
| `middleware.throttle` | `Throttle` | [Throttle](../../usage/route-middleware/throttle.md) |
| `signedUrls` | `Signed`, `UrlSigner`, signed URL helpers | [Signed URLs](../../usage/route-middleware/signed-urls.md) |
