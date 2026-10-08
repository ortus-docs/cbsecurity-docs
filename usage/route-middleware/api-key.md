---
description: Protect routes with API keys using the ApiKey middleware.
---

# API Keys

`ApiKey@cbsecurity` requires an API key. It reads the `x-api-key` header first and then the `apiKey` request key. A missing or wrong key gets a `401`.

```javascript
route( "/webhooks/billing" )
    .middleware( "ApiKey@cbsecurity" )
    .meta( { apiKeys : [ getSystemSetting( "BILLING_WEBHOOK_KEY" ) ] } )
    .to( "webhooks.billing" )
```

{% hint style="info" %}
`ApiKey` identifies a **client**. If you need to know **who** the user is, with permissions and expiration, use [JwtAuth](../route-middleware.md) instead.
{% endhint %}

## Where Keys Come From

The middleware checks these, in order:

1. The route `meta` key `apiKeys` (a list or an array).
2. The `middleware.apiKey.keys` setting.
3. A validator service from `middleware.apiKey.validator`: a WireBox ID of an object with `boolean isValidKey( required string key, required event )`. Use it to look keys up in a database.

If none is configured the middleware throws `cbsecurity.MiddlewareMisconfigured`. Keys are compared in constant time. Keep them out of source control and load them from environment variables.

## Changing Where It Looks

| Route meta key | Setting | Default |
| --- | --- | --- |
| `apiKeyHeader` | `middleware.apiKey.header` | `x-api-key` |
| `apiKeyParam` | `middleware.apiKey.param` | `apiKey` |

```javascript
route( "/partners" )
    .middleware( "ApiKey@cbsecurity" )
    .meta( { apiKeys : "abc,def", apiKeyHeader : "x-partner-token", apiKeyParam : "token" } )
    .to( "partners.index" )
```

Prefer the header. A key in the query string ends up in access logs and browser history.
