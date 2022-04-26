# Interceptions

The security firewall will announce some interception events when invalid access or authorizations occur within the system:

* `cbSecurity_onInvalidAuthentication`
* `cbSecurity_onInvalidAuthorization`

You will receive the following data in the `interceptData` struct in each interception call:

* `ip` : The offending IP address
* `rule` : The security rule intercepted or empty if annotations
* `settings` : The firewall settings
* `validatorResults` : The validator results
* `annotationType` : The annotation type intercepted, `handler` or `action` or empty if rule driven
* `processActions` : A Boolean indicator that defaults to true. If you change this to false, then the interceptor won't fire the invalid actions. Usually this means, you manually will do them.

With these interceptions you can build a nice auditing system, login tracking and much more.

{% code title="interceptors/SecurityAudit.cfc" %}
```javascript
component extends="coldbox.system.Interceptor"{

    function cbSecurity_onInvalidAuthentication( event, interceptData ){
        // do what you like here
    }
    
    function cbSecurity_onInvalidAuthorization( event, interceptData ){
        // do what you like here
    }

}
```
{% endcode %}

## Stop Processing Actions

The intercept data has a key called `processActions` which defaults to **true**.  This Boolean indicator tells the firewall to process the invalid authentication/authorization procedures.  If you change this value to **false**, then the firewall will do NOTHING because it is expecting for YOU to have done the actions.

## JWT Interception

If you are using our [JWT facilities](../jwt/jwt-services.md), then we will announce the following interceptions during JWT usage:

* `cbSecurity_onJWTCreation`
* `cbSecurity_onJWTInvalidation`
* `cbSecurity_onJWTValidAuthentication`
* `cbSecurity_onJWTInvalidUser`
* `cbSecurity_onJWTInvalidClaims`
* `cbSecurity_onJWTExpiration`
* `cbSecurity_onJWTStorageRejection`
* `cbSecurity_onJWTValidParsing`
* `cbSecurity_onJWTInvalidateAllTokens`

Check them all out in our [JWT Interceptions Page](../jwt/jwt-interceptions.md).

## CBAuth Interceptions

You can always find the latest interception points here:

{% embed url="https://cbauth.ortusbooks.com/interception-points" %}

cbauth announces several custom interception points. You can use these interception points to change request data or add additional values to session or request scopes. The `preAuthentication` and `postAuthentication` events fire during the standard `authenticate()` method call with a username and password. The `preLogin` and `postLogin` events fire during the `login()` method call. The `preLogout` and `postLogout` events fire during the `logout()` method call.

{% hint style="success" %}
The `preLogin` and `postLogin` interception points will be called during the course of `authenticate()`. The order of the calls then are `preAuthentication` -> `preLogin` -> `postLogin` -> `postAuthentication`.
{% endhint %}
