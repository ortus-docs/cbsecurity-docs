---
description: Secure your first route in five minutes.
icon: bolt
---

# Quickstart

This takes you from install to a secured route with a passing test.

## 1. Install

```bash
install cbsecurity
```

This also installs `cbauth` (login state) and `cbcsrf`. See [Installation](installation.md) for the supported engines.

## 2. Configure

Tell cbauth where your users live and tell the firewall where to send people who are not logged in:

```javascript
// config/ColdBox.cfc
moduleSettings = {
    cbauth     : { userServiceClass : "models.UserService" },
    cbsecurity : {
        firewall : { invalidAuthenticationEvent : "security.login" }
    }
}
```

## 3. Provide Your Users

Your user service must implement `isValidCredentials()`, `retrieveUserByUsername()` and `retrieveUserById()` (`cbsecurity.interfaces.IUserService`). The user it returns must implement `getId()`, `hasPermission()` and `hasRole()` (`IAuthUser`). You can start from the bundled [`User`](../usage/auth-user.md) object.

## 4. Secure a Route

With route middleware (ColdBox 8.2+), next to the route:

```javascript
// config/Router.cfc
route( "/account" ).middleware( "Authenticated@cbsecurity" ).to( "account.index" )

route( "/admin" )
    .middleware( "Authorized@cbsecurity" )
    .meta( { permissions : "ADMIN" } )
    .to( "admin.index" )
```

Or with an annotation on the handler, which works on any ColdBox version:

```javascript
component secured="ADMIN" {
    function index( event, rc, prc ){}
}
```

Prefer a central list? Use [security rules](../usage/untitled-1.md) instead. See [Overview](overview.md) to choose.

## 5. Test It

```javascript
it( "sends guests to the login", () => {
    var event = get( "/account" )
    expect( event.getValue( "relocate_event" ) ).toBe( "security.login" )
} )
```

Running middleware in `get()` and `post()` needs ColdBox 8.3+. Annotations and rules are tested the same way on any version.

## Where Next

* [How the Firewall Works](how-the-firewall-works.md): what happens when access is denied.
* [Route Middleware](../usage/route-middleware.md): throttling, API keys, IP filtering and more.
* [JWT](../jwt/jwt-services.md): securing a REST API.
* [Full Configuration](configuration/full-configuration.md): every setting.
