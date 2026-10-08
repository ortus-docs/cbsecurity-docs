---
description: Rate limit routes with the Throttle middleware and the RateLimiter model.
---

# Throttle

`Throttle@cbsecurity` limits how many requests a client can make in a time window. Over the limit it answers `429 Too Many Requests` with a `Retry-After` header. Allowed requests get `X-RateLimit-Limit` and `X-RateLimit-Remaining` headers.

## Named Limiters

Define limiters once in your `cbsecurity` module settings and reference them by name:

```javascript
// config/ColdBox.bx
moduleSettings = {
    cbsecurity : {
        middleware : {
            throttle : {
                limiters : {
                    login : { maxAttempts : 5,   decaySeconds : 60 },
                    api   : { maxAttempts : 120, decaySeconds : 60, by : "user" }
                }
            }
        }
    }
}
```

```javascript
// config/Router.bx
route( "/login" ).middleware( "Throttle@cbsecurity" ).meta( { throttle : "login" } ).to( "sessions.create" )
```

An unknown limiter name throws `cbsecurity.MiddlewareMisconfigured`.

## Inline Limits

```javascript
route( "/search" )
    .middleware( "Throttle@cbsecurity" )
    .meta( { throttle : { maxAttempts : 30, decaySeconds : 60 } } )
    .to( "search.index" )
```

## Limiter Options

| Option | Default | Description |
| --- | --- | --- |
| `maxAttempts` | `60` | Requests allowed per window. |
| `decaySeconds` | `60` | The window length in seconds. |
| `cacheProvider` | `default` | The name of the CacheBox cache that holds the counters. |
| `by` | `ip` | `ip`, or `user` to count each logged in user. Guests fall back to their IP. |
| `name` | limiter name or route pattern | Routes that share a `name` share one counter. |

Defaults for `maxAttempts`, `decaySeconds` and `cacheProvider` live in `middleware.throttle` and apply to anything you do not set.

{% hint style="warning" %}
Counters live in the CacheBox cache you choose. With more than one server, point `cacheProvider` at a shared cache (Redis, Couchbase or a database), otherwise every server counts on its own.
{% endhint %}

## RateLimiter

Use the model directly for limits that are not routes, like password resets or calls inside a service.

```javascript
limiter = getInstance( "RateLimiter@cbsecurity" )

if ( limiter.tooManyAttempts( "reset:#rc.email#", 3 ) ) {
    return event.renderData( statusCode : 429, data : "Try again in #limiter.availableIn( 'reset:#rc.email#' )# seconds" )
}
limiter.hit( "reset:#rc.email#", 3600 )
```

| Method | Description |
| --- | --- |
| `hit( key, decaySeconds, cacheProvider )` | Record an attempt and return the attempts in the window. |
| `tooManyAttempts( key, maxAttempts, cacheProvider )` | Has the key reached its limit? |
| `attempts( key, cacheProvider )` | Attempts in the current window. |
| `remaining( key, maxAttempts, cacheProvider )` | Attempts left, never below zero. |
| `availableIn( key, cacheProvider )` | Seconds until the window resets. |
| `clear( key, cacheProvider )` | Reset a key, for example after a successful login. |

The window is fixed: it starts with the first hit and resets after `decaySeconds`.
