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

{% code-tabs %}
{% code-tabs-item title="interceptors/SecurityAudit.cfc" %}
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
{% endcode-tabs-item %}
{% endcode-tabs %}

## Stop Processing Actions

The intercept data has a key called `processActions` which defaults to **true**.  This Boolean indicator tells the firewall to process the invalid authentication/authorization procedures.  If you change this value to **false**, then the firewall will do NOTHING because it is expecting for YOU to have done the actions.

