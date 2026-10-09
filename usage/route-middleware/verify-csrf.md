---
description: Verify CSRF tokens on routes with the VerifyCsrf middleware.
---

# Verify CSRF

`VerifyCsrf@cbsecurity` verifies a CSRF token on every request that is not `GET`, `HEAD` or `OPTIONS`. It uses the bundled [cbcsrf](../cross-site-request-forgery-cbcsrf.md) module. A missing or invalid token gets a `403`.

```javascript
route( "/account/email" ).middleware( "VerifyCsrf@cbsecurity" ).to( "account.updateEmail" )
```

Generate the token in your form with the cbcsrf helpers:

```html
<form method="post" action="/account/email">
    #csrfField()#
    ...
</form>
```

The token is read from the `csrf` request key (what `csrfField()` creates) or the `x-csrf-token` header, the same places the cbcsrf auto verifier uses.

| Route meta key | Default | Description |
| --- | --- | --- |
| `csrfKey` | `default` | The key the token was generated for, see `csrfToken( key )`. |

## Auto Verifier or Middleware?

The cbcsrf auto verifier (`csrf.enableAutoVerifier`) checks every request and uses exclusion patterns. The middleware checks only the routes you name, so it suits apps where most routes are APIs with token authentication and a few are browser forms. Unlike the auto verifier, the middleware also runs in integration tests.
