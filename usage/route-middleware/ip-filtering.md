---
description: Allow or block requests by IP address with AllowedIPs and DenyIPs.
---

# IP Filtering

| Middleware | Behavior |
| --- | --- |
| `AllowedIPs@cbsecurity` | Only the listed IPs get in. Everyone else gets a `403`. |
| `DenyIPs@cbsecurity` | The listed IPs get a `403`. Everyone else passes. |

Lists accept IPv4, IPv6 and CIDR ranges, as a list or an array, in the route `meta`:

```javascript
// Only the office and the VPN
route( "/admin" )
    .middleware( "AllowedIPs@cbsecurity" )
    .meta( { allowedIps : "203.0.113.7,10.8.0.0/16" } )
    .to( "admin.index" )

// Block a range
route( "/api" )
    .middleware( "DenyIPs@cbsecurity" )
    .meta( { denyIps : [ "198.51.100.0/24" ] } )
    .to( "api.index" )
```

A route without its list throws `cbsecurity.MiddlewareMisconfigured` instead of silently allowing or blocking everyone.

## Behind a Proxy or Load Balancer

By default the client IP is the address that made the connection. Forwarded headers can be faked, so `X-Forwarded-For` is only honored when the connection comes from a proxy you trust:

```javascript
moduleSettings = {
    cbsecurity : {
        middleware : { trustedProxies : [ "10.0.0.0/8" ] }
    }
}
```

The header is read from the right and the first address that is not a trusted proxy is the client.

Combine IP filtering with [authentication middleware](../route-middleware.md). An IP is not an identity.
