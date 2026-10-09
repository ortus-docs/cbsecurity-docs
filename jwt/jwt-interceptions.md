---
icon: bullhorn
---

# JWT Interceptions

The JWT services announce these events. Listen to them with an [interceptor](https://coldbox.ortusbooks.com/the-basics/interceptors) to audit, log or react to token activity. Each event passes the keys below in `interceptData`.

| Event | Announced when | `interceptData` keys |
| --- | --- | --- |
| `cbSecurity_onJWTCreation` | A new token is generated for a user | `token`, `payload`, `user` |
| `cbSecurity_onJWTInvalidation` | A token is invalidated | `token` |
| `cbSecurity_onJWTValidAuthentication` | A valid token is parsed, tested and authenticated with the authentication services | `token`, `payload`, `user` |
| `cbSecurity_onJWTInvalidUser` | The token's subject is not found: the user service returns null or an invalid user | `token`, `payload` |
| `cbSecurity_onJWTInvalidClaims` | The parsed token does not have the required claims | `token`, `payload` |
| `cbSecurity_onJWTExpiration` | The parsed token has expired | `token`, `payload` |
| `cbSecurity_onJWTStorageRejection` | The parsed token is valid but is not in the permanent token storage | `token`, `payload` |
| `cbSecurity_onJWTValidParsing` | The parsed token passed every validation but has NOT been authenticated yet | `token`, `payload` |

* `token` is the JWT string, `payload` is the decoded claims struct and `user` is the user the token belongs to.

## Example

{% code title="interceptors/SecurityAudit.cfc" %}
```javascript
component extends="coldbox.system.Interceptor"{

    function cbSecurity_onJWTCreation( event, interceptData ){
        // do what you like here
    }

}
```
{% endcode %}
