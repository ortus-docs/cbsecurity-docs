# Declaring the Interceptor

In order to enable ColdBox security you must register the Security interceptor in your parent or other module configuration's `interceptors` section:

```javascript
interceptors = [
    { 
        class       = "cbSecurity.interceptors.Security", 
        name        = "ApplicationSecurity", 
        properties  = {
            // Security properties go here.
        }
    }
];
```

{% hint style="info" %}
**IMPORTANT** If you are using SES or URL mappings in your ColdBox 4 application, make sure that you declare the security interceptor after the SES interceptor. Interceptors require order, so security needs for the URL to be translated first. In coldbox 5 SES is handled by the Routing service, so you don't need this SES interceptor.
{% endhint %}

## Global Properties

| Property | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `useRegex` | boolean | false | true | By default all secure and white lists are matched using regular expressions. You can disable it if you like and use plain old string matching. |
| `queryChecks` | boolean | false | false | Flag that tells the interceptor to validate the columns in the security rules. This makes sure all columns have the same columns. By default it is in relaxed mode so all columns are used. |
| `preEventSecurity` | boolean | false | false | This turns on the `preEvent`execution point that will make sure that before any event is fired internally, that its verified against the security rules. Only use this if you really want to secure all internal events, else this can hinder performance. |
| `ruleSource` | string | true | --- | Where to look for the rules as described above, this value has to be a choice from the following list `xml,json,db,model,ioc or ocm`. |
| `validator` | string | false | --- | The class path of the validator object to use. The interceptor will create the object for you and cache it internally. If the object has an `init()` method, the interceptor will call it for you. |
| `validatorModel` | string | false | --- | The model name of the security validator to use for custom validations. The interceptor will call `getModel()` with the name of this property to be retrieved via [WireBox](http://wirebox.ortusbooks.com/) |
| `validatorIOC` | string | false | --- | The bean name of the security validator to use for custom validations. The interceptor will ask the IoC module for the bean according to this property |

