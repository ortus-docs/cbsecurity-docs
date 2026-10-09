---
description: Force HTTPS on routes with the EnsureHttps middleware.
---

# Ensure HTTPS

`EnsureHttps@cbsecurity` makes sure the request is secure.

* `GET` and `HEAD` requests are redirected to the HTTPS URL with a `301`.
* Any other method is denied with a `403`. A redirect would drop the request body, and the data was already sent in the clear.

```javascript
group( { pattern : "/account", middleware : [ "EnsureHttps@cbsecurity" ] }, () => {
    route( "/login" ).to( "sessions.new" )
} )
```

Detection uses `event.isSSL()`, which also honors `X-Forwarded-Proto` and `X-Scheme`, so it works behind a load balancer that terminates TLS.

## Options

| Route meta key | Setting | Default | Description |
| --- | --- | --- | --- |
| `redirectToHttps` | `middleware.ensureHttps.redirect` | `true` | Set to `false` to deny `GET` requests with a `403` instead of redirecting. Good for APIs. |

To send `Strict-Transport-Security` headers, see [Security Headers](../../getting-started/configuration/security-headers.md).
