# JWT Interceptions

The JWT Services will announce some key events for you to listen to

* `cbSecurity_onJWTCreation` - Whenever a new token is generated for a user
* `cbSecurity_onJWTInvalidation` - Whenever an invalidation occurs for a token
* `cbSecurity_onJWTValidAuthentication` - Whenever a valid JWT token is parsed, tested and authenticated with the authentication services
* `cbSecurity_onJWTInvalidUser` - When trying to find the token's subject and the user service returns null or not a valid user
* `cbSecurity_onJWTInvalidClaims` - When the parsed token does not adhere to the required claims
* `cbSecurity_onJWTExpiration` - When the parsed token has expired
* `cbSecurity_onJWTStorageRejection` - When the parsed token is valid but cannot be found in the permanent storage
* `cbSecurity_onJWTValidParsing` - When the parsed token has passed all validation procedures but has NOT been authenticated yet.

## cbSecurity\_onJWTCreation

This event has the following data in the `interceptData` struct

| Key       | Description                            |
| --------- | -------------------------------------- |
| `token`   | The JWT token                          |
| `payload` | The payload that was used to create it |
| `user`    | The user it belongs to                 |

## cbSecurity\_onJWTInvalidation

This event has the following data in the `interceptData` struct

| Key     | Description                        |
| ------- | ---------------------------------- |
| `token` | The JWT token that was invalidated |

## cbSecurity\_onJWTValidAuthentication

This event has the following data in the `interceptData` struct

| Key       | Description                   |
| --------- | ----------------------------- |
| `token`   | The JWT token that was parsed |
| `payload` | The payload that was decoded  |
| `user`    | The authenticated user        |

## cbSecurity\_onJWTInvalidUser

This event has the following data in the `interceptData` struct

| Key       | Description                     |
| --------- | ------------------------------- |
| `token`   | The JWT token that was parsed   |
| `payload` | The JWT payload that was parsed |

## cbSecurity\_onJWTInvalidClaims

This event has the following data in the `interceptData` struct

| Key       | Description                     |
| --------- | ------------------------------- |
| `token`   | The JWT token that was parsed   |
| `payload` | The JWT payload that was parsed |

## cbSecurity\_onJWTExpiration

This event has the following data in the `interceptData` struct

| Key       | Description                     |
| --------- | ------------------------------- |
| `token`   | The JWT token that was parsed   |
| `payload` | The JWT payload that was parsed |

## cbSecurity\_onJWTStorageRejection

This event has the following data in the `interceptData` struct

| Key       | Description                     |
| --------- | ------------------------------- |
| `token`   | The JWT token that was parsed   |
| `payload` | The JWT payload that was parsed |

## cbSecurity\_onJWTValidParsing

This event has the following data in the `interceptData` struct

| Key       | Description                     |
| --------- | ------------------------------- |
| `token`   | The JWT token that was parsed   |
| `payload` | The JWT payload that was parsed |

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
