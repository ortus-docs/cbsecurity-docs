# Module Rules

Every module in ColdBox has the capability to contribute their own rules to `cbsecurity` by registering them in the `ModuleConfig.cfc` within the `settings` struct. Just create another struct called `cbsecurity` with the following allowed keys:

{% code title="ModuleConfig.cfc" %}
```javascript
settings = {
    // CB Security Rules to prepend to global rules
    cbsecurity = {
        // Module Relocation when an invalid access is detected, instead of each rule declaring one.
        "invalidAuthenticationEvent"     : "mod1:secure.index",
        // Default Authentication Action: override or redirect when a user has not logged in
        "defaultAuthenticationAction"    : "redirect",
        // Module override event when an invalid access is detected, instead of each rule declaring one.
        "invalidAuthorizationEvent"    : "mod1:secure.auth",
        // Default Authorization Action: override or redirect when a user does not have enough permissions to access something
        "defaultAuthorizationAction"    : "redirect",
        // You can define your security rules here
        "rules"                            : [
            {
                "secureList"     : "mod1:home"
            },
            {
                "secureList"     : "mod1/modOverride",
                "match"            : "url",
                "action"        : "override"
            }
        ]
    }
};
```
{% endcode %}

As you can see each module can have it's own overrides for authentication and authorization events as well as their own rules.

{% hint style="danger" %}
Please note that these security rules will be **PREPENDED** to the global rules
{% endhint %}

## Rule Sources

As with the global rules defined in `config/Coldbox.cfc`, the module `cbsecurity.rules` setting supports multiple rule sources:

* [DB](https://github.com/ortus-docs/cbsecurity-docs/tree/ea50160182c145dae1aa01e169fc434048a3c911/getting-started/first-chapter/rule-sources/untitled/README.md)
* [Inline](https://github.com/ortus-docs/cbsecurity-docs/tree/ea50160182c145dae1aa01e169fc434048a3c911/getting-started/first-chapter/rule-sources/inline-rules/README.md)
* [JSON](https://github.com/ortus-docs/cbsecurity-docs/tree/ea50160182c145dae1aa01e169fc434048a3c911/getting-started/first-chapter/rule-sources/json-properties/README.md)
* [Model](https://github.com/ortus-docs/cbsecurity-docs/tree/ea50160182c145dae1aa01e169fc434048a3c911/getting-started/first-chapter/rule-sources/model-rules/README.md)
* [XML](https://github.com/ortus-docs/cbsecurity-docs/tree/ea50160182c145dae1aa01e169fc434048a3c911/getting-started/first-chapter/rule-sources/xml-properties/README.md)

For example, you can load security rules specific to a module from a JSON file stored in your module:

{% code title="ModuleConfig.cfc" %}
```

```
{% endcode %}

```javascript
settings = {
    cbsecurity = {
        "rules" : "#modulePath#/config/firewallRules.json"
        // other config here... <---
    }
};
```

## Loading/Unloading

Also note that if modules are loaded dynamically, it will still inspect them and register them if cbsecurity settings are found. The same goes for unloading, the entire security rules for that module will cease to exist.

