---
description: Create tamper-proof, expiring links with signed URLs.
---

# Signed URLs

A signed URL carries a signature of its path and query string. Nobody can change the path or a parameter, or extend the expiration, without breaking it. No login is needed, which suits email verification, password resets and temporary download links.

```text
/invoices/42/download?expires=1760000000&signature=9f2c...
```

## Setup

Set a secret. Use a long random value, keep it out of source control and rotate it if it leaks (every existing link stops working).

```bash
CBSECURITY_SIGNING_SECRET=a-long-random-value
```

```javascript
// or in config/ColdBox.bx
moduleSettings = { cbsecurity : { signedUrls : { secret : getSystemSetting( "CBSECURITY_SIGNING_SECRET" ) } } }
```

Without a secret, signing throws `cbsecurity.SigningSecretMissing`.

## Protect the Route

```javascript
route( pattern = "/invoices/:id/download", name = "invoice.download" )
    .middleware( "Signed@cbsecurity" )
    .to( "invoices.download" )
```

A missing, altered or expired link gets a `403`. The reason (`missing`, `invalid` or `expired`) is in `prc.cbSecurity_signatureStatus` if you want to react to it.

## Create Links

These helpers are available in handlers, views, layouts and interceptors:

| Helper | Description |
| --- | --- |
| `signedRoute( name, params, expiresIn )` | Link to a named route. Params that match a route segment fill the pattern, the rest go in the query string. |
| `signedUrl( to, queryString, expiresIn )` | Link to an event or route path, like `event.buildLink()` but with a real query string. |
| `hasValidSignature()` | Does the current request carry a valid signature? |

`expiresIn` is in seconds. `0` (the default) never expires.

```javascript
// handler
var link = signedRoute( "invoice.download", { id : 42, ref : "email" }, 3600 )
```

Outside handlers and views (a model, a scheduled task), inject `UrlSigner@cbsecurity`:

```javascript
property name="urlSigner" inject="UrlSigner@cbsecurity";

var link = urlSigner.sign( "https://example.com/verify/42", 86400 )
urlSigner.isValid( link )     // true or false
urlSigner.check( link )       // valid, missing, invalid or expired
```

## What Is Signed

* The path and **every** query parameter. Adding or changing one invalidates the link, including tracking parameters that a mail tool appends.
* Parameter names are case insensitive and the order does not matter. A trailing slash on the path is ignored.
* The scheme and host are ignored, so links keep working behind proxies and load balancers.

{% hint style="warning" %}
A signed URL is a bearer link: anyone who has it can use it until it expires. Give sensitive links a short expiration and do not log them.
{% endhint %}
