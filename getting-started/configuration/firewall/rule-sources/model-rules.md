---
description: Security rules from a model's method call
---

# Model Rules

If you prefer to store your rules your way, CBSecurity can talk to any WireBox ID or model and get the rules from them by using the `model` source in the rule provider.

{% code title="config/Coldbox.cfc" %}
```javascript
// CB Security
cbSecurity : {
  firewall : {
    rules : {
      provider : {
        "source" : "model",
        "properties" : {
            "model" : "SecurityService",
            "method" : "getSecurityRules"
        }
      }
    }
  }
}
```
{% endcode %}

* The `model` property is any WireBox ID or classpath
* The `method` property is the name of the method to call to get an array of rules back\
