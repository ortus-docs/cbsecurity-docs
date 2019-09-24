# What's New With 2.0.0

## New Features

* Adobe 2016,2018 Support
* Settings transferred to ColdBox 4/5 `moduleSettings` approach instead of root approach \(See compat section\)
* The `rulesModelMethod` now defaults to `getSecurityRules()`
* ColdFusion security validator has an identity now `CFValidator@cbsecurity` instead of always being inline.
* You can now add an `overrideEvent` element to a rule. If that is set, then we will override the incoming event via `event.overrideEvent()` instead of doing a relocation using the `redirect` rule element.
* You can now declare your rules inline in the configuration settings using the `rules` key. This will allow you to build the rules in your config instead of a rule source.
* We now can distinguish between invalid auth and invalid authorizations
* New interception block points `cbSecurity_onInvalidAuthentication`, `cbSecurity_onInvalidAuhtorization`
* You now have a `defaultAuthorizationAction` setting which defaults to `redirect`
* You now have a `invalidAuthenticationEvent` setting that can be used
* You now have a `defaultAuthenticationAction` setting which defaults to `redirect`
* You now have a `invalidAuthorizationEvent` setting that can be used
* If a rule is matched, we will store it in the `prc` as `cbSecurity_matchedRule` so you can see which security rule was used for processing invalid access actions.
* If a rule is matched we will store the validator results in `prc` as `cbSecurity_validatorResults`
* Ability for modules to register cbSecurity rules and setting overrides by registering a `settings.cbSecurity` key.
* New security rule visualizer for graphically seeing you rules and configuration.  Can be locked down via the `enableSecurityVisualizer` setting. Disabled by default.
* Annotation based security for handlers and actions using the `secured` annotation.  Which can be boolean or a list of permissions, roles or whatever you like.
* You can disable annotation based security by using the `handlerAnnotationSecurity` boolean setting.
* JWT Token Security Support

## Improvements

* SSL Enforcement now cascades according to the following lookup: Global, rule, request
* Interfaces documented for easier extension `interfaces.*`
* Migration to script and code modernization
* New Module Layout
* Secured rules are now logged as `warn()` with the offending Ip address.
* New setting to turn on/off the loading of the security firewall: `autoLoadFirewall`. The interceptor will auto load and be registered as `cbsecurity@global` in WireBox.

## Compat

* Adobe 11 Dropped
* Lucee 4.5 Dropped
* Migrate your root `cbSecurity` settings in your `config/ColdBox.cfc` to inside the `moduleSettings`
* IOC rules support dropped
* OCM rules support dropped
* `validatorModel` dropped in favor of just `validator` to be a WireBox Id
* Removed `preEventSecurity` it was too chatty and almost never used
* The function `userValidator` has been renamed to `ruleValidator` and also added the `annotationValidator` as well.
* `rulesSource` removed you can now use the `rules` setting
  * The `rules` can be: `array, db, model, filepath`
  * If the `filepath` has `json` or `xml` in it, we will use that as the source style
* `rulesFile` removed you can now use the `rules` setting.

