---
description: Secure routes and route groups with cbsecurity middleware, declared right next to the route.
icon: route
---

# Route Middleware

CBSecurity ships ready-made [ColdBox route middleware](https://coldbox.ortusbooks.com/the-basics/routing/routing-dsl/middleware) so you can secure a route, or a whole group of routes, where it is declared. It uses the same validators, invalid actions, interception points and logging as the firewall, so you do not need security rules.

```javascript
// config/Router.bx
route( "/account" ).middleware( "Authenticated@cbsecurity" ).to( "account.index" )

route( "/admin" )
    .middleware( "Authorized@cbsecurity" )
    .meta( { permissions : "ADMIN" } )
    .to( "admin.index" )
```

{% hint style="info" %}
Route middleware needs ColdBox **8.2+**. Group-level `meta` and running middleware in integration tests (`execute()`, `get()`, etc.) need ColdBox **8.3+**.
{% endhint %}

## The Middleware

| WireBox ID | What it verifies |
| --- | --- |
| `Authenticated@cbsecurity` | The user is logged in. Route `meta` is ignored. |
| `Authorized@cbsecurity` | The user is logged in and satisfies the `permissions` and/or `roles` in the route `meta`. |
| `JwtAuth@cbsecurity` | Like `Authorized`, but authenticating with a JWT through the `JwtAuthValidator`. |
| `BasicAuth@cbsecurity` | Like `Authorized`, but authenticating with HTTP Basic credentials through the `BasicAuthValidator`. |

`Authenticated` and `Authorized` use the validator configured in `firewall.validator`.

### More Protection

These do not authenticate users. They guard routes against abuse and answer denied requests directly, with a JSON error and the right status code, without the firewall's invalid actions. Each is configured in the route `meta()`, with defaults in the [module settings](../getting-started/configuration/middleware.md).

| WireBox ID | What it does |
| --- | --- |
| [`Throttle@cbsecurity`](route-middleware/throttle.md) | Rate limits requests with named or inline limiters. |
| [`ApiKey@cbsecurity`](route-middleware/api-key.md) | Requires an API key in a header or request key. |
| [`AllowedIPs@cbsecurity`](route-middleware/ip-filtering.md) | Only listed IPs and CIDR ranges get in. |
| [`DenyIPs@cbsecurity`](route-middleware/ip-filtering.md) | Blocks listed IPs and CIDR ranges. |
| [`EnsureHttps@cbsecurity`](route-middleware/ensure-https.md) | Redirects to HTTPS, or denies non `GET` requests. |
| [`VerifyCsrf@cbsecurity`](route-middleware/verify-csrf.md) | Verifies a CSRF token on unsafe requests. |
| [`Honeypot@cbsecurity`](route-middleware/honeypot.md) | Catches spam bots with a hidden field. |
| [`Signed@cbsecurity`](route-middleware/signed-urls.md) | Only lets valid, unexpired [signed URLs](route-middleware/signed-urls.md) through. |

Stack them as needed. Middleware runs in the order you declare it, so put cheap checks first:

```javascript
route( "/api/orders" )
    .middleware( [ "DenyIPs@cbsecurity", "Throttle@cbsecurity", "JwtAuth@cbsecurity" ] )
    .meta( { denyIps : "198.51.100.0/24", throttle : "api", permissions : "ORDERS_READ" } )
    .to( "orders.index" )
```

## Declaring Permissions and Roles

`Authorized`, `JwtAuth` and `BasicAuth` read their requirements from the route `meta()`:

| Meta key | Description |
| --- | --- |
| `permissions` | One permission, a list or an array. |
| `roles` | One role, a list or an array. Any one role is enough. |
| `mode` | How `permissions` are verified: `any` (default, one is enough), `all` (every one is required) or `none` (the user must have none of them). |

```javascript
// Any one of the permissions
route( "/reports" )
    .middleware( "Authorized@cbsecurity" )
    .meta( { permissions : [ "REPORTS_VIEW", "ADMIN" ] } )
    .to( "reports.index" )

// Every permission is required
route( "/billing" )
    .middleware( "Authorized@cbsecurity" )
    .meta( { permissions : "BILLING_READ,BILLING_WRITE", mode : "all" } )
    .to( "billing.index" )

// Roles
route( "/editor" )
    .middleware( "Authorized@cbsecurity" )
    .meta( { roles : "editor,admin" } )
    .to( "editor.index" )
```

## Securing a Group

Attach the middleware and the `meta` to a `group()` and every route inside inherits them. A route's own `meta()` wins over the group's.

```javascript
group(
    {
        pattern    : "/admin",
        middleware : [ "Authorized@cbsecurity" ],
        meta       : { permissions : "ADMIN" }
    },
    () => {
        route( "/users" ).to( "admin.users" )
        route( "/settings" ).to( "admin.settings" )
        // This one needs a different permission
        route( "/audit" ).meta( { permissions : "AUDITOR" } ).to( "admin.audit" )
        // This one is public
        route( "/status" ).withoutMiddleware( "Authorized@cbsecurity" ).to( "admin.status" )
    }
)
```

Use `middlewareGroup()` to name a bundle once and reuse it:

```javascript
middlewareGroup( "secured", [ "Authenticated@cbsecurity" ] )

route( "/dashboard" ).middleware( "secured" ).to( "dashboard.index" )
```

## APIs With JWT

```javascript
group( { pattern : "/api", middleware : [ "JwtAuth@cbsecurity" ] }, () => {
    route( "/orders" ).to( "orders.index" )
    route( "/orders/:id" ).meta( { permissions : "ORDERS_WRITE" } ).to( "orders.update" )
} )
```

{% hint style="warning" %}
The `JwtAuthValidator` verifies **permissions** only (the token scopes or the user's permissions). A `roles` value in the route `meta` is not evaluated for JWT requests.
{% endhint %}

## When Access Is Denied

A denied request goes through the firewall's invalid access flow, so it behaves exactly like a firewall rule or a [security annotation](security-annotations.md):

* A guest is an **authentication** failure. A logged in user without the required permissions or roles is an **authorization** failure.
* The firewall settings decide the response: `defaultAuthenticationAction`, `invalidAuthenticationEvent`, `defaultAuthorizationAction` and `invalidAuthorizationEvent` (`redirect`, `override` or `block`), including the per-module overrides.
* The `cbSecurity_onInvalidAuthentication` and `cbSecurity_onInvalidAuthorization` [interceptions](interceptions.md) are announced with `annotationType` set to `middleware`.
* The validator results are stored in `prc.cbSecurity_validatorResults` and the secured URL is flashed to `_securedURL`.

When access is allowed, the authenticated user is stored in the `prc` using the `authentication.prcUserVariable` setting, just like the firewall does.

{% hint style="warning" %}
Route middleware needs the firewall interceptor, so keep `firewall.autoLoadFirewall` set to `true` (the default). Without it, the middleware throws a `cbsecurity.MiddlewareRequiresFirewall` exception. You do not need any firewall rules.
{% endhint %}

## Firewall Rules, Annotations or Middleware?

They share the same validators and invalid actions, so choose by where you want the rule to live. When combined, the firewall rules and annotations run first, then the route middleware.

| Approach | Rules live in | Best for |
| --- | --- | --- |
| [Firewall rules](security-rules.md) | Config, JSON, XML, a database or a model | Central, data-driven policies and rules contributed by modules. |
| [Annotations](security-annotations.md) | On the handler or action | Security that must travel with the code. |
| Route middleware | Next to the route | Securing routes and groups in `config/Router`, with no rules file. |

## Custom Middleware

Extend `cbsecurity.models.middleware.Guard` to build your own named middleware. Its constructor accepts `permissions`, `roles`, `mode`, `validator` (`auth`, `cbauth`, `jwt`, `basic` or a WireBox ID) and `useMeta`:

```javascript
// models/middleware/AdminOnly.bx : WireBox ID "AdminOnly"
class extends="cbsecurity.models.middleware.Guard" singleton {
    function init(){
        return super.init( permissions : "ADMIN", useMeta : false )
    }
}
```

```javascript
route( "/admin" ).middleware( "AdminOnly" ).to( "admin.index" )
```

## Testing

With ColdBox 8.3+, `execute()`, `get()`, `post()` and the other integration test helpers run route middleware, so you can test secured routes end to end:

```javascript
it( "redirects guests", () => {
    var event = get( "/admin" )
    expect( event.getValue( "relocate_event" ) ).toBe( "main.login" )
} )
```
