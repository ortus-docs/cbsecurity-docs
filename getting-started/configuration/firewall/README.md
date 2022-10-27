# Firewall



## InvalidAuthentication-/ InvalidAuthorization events and default actions

The `invalidAuthenticationEvent` and `invalidAuthorizationEvent` keys can be used to provide default events when Authentication or Authorization failed. The defaultAuthenticationAction and defaultAuthorizationAction determine whether there will be a redirection or override. The default action is `redirect`, but especially for API's an `override` will be more appropriate. When using rule-based security you can override these keys for any individual rule.

## Validator

You can place a global validator in the configuration settings, but you can also override the validator on a module by module basis as well. The default validator is using the [CBAuth Validator.](../../../security-validators/cbauth-validator.md)



## Automatic Firewall

Please note that by default, the security firewall will be auto-registered for you. If you do NOT want the firewall to be automatically registered for you, then use the `autoLoadFirewall` setting and make it false. Then you can use the **Custom Firewall** approach below to register the firewall manually in the order of the interceptors that you would like.

```javascript
autoLoadFirewall : false
```

## Annotation Security

By default, annotation security is enabled. This will inspect ALL incoming event executions for the security annotations. If you do not want to use annotation security we recommend you turn it off to avoid the inspection of events.

```javascript
handlerAnnotationSecurity : false
```
