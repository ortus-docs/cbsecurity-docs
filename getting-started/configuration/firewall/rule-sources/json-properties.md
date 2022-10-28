---
description: Security rules in a JSON file
---

# JSON Rules

You can place all your security rules inside of a JSON file and then tell CBSecurity where they are:

{% code title="config/Coldbox.cfc" %}
```javascript
// CB Security
cbSecurity : {
  firewall : {
    rules : {
      provider : {
        "source" : "config/security.json.cfm"
      }
    }
  }
}
```
{% endcode %}

Then your file can be something like this:

{% code title="config/security.json.cfm" %}
```javascript
[
    {
        "whitelist": "user\\.login,user\\.logout,^main.*",
        "securelist": "^user\\.*, ^admin",
        "match": "event",
        "roles": "admin",
        "permissions": "",
        "redirect": "user.login",
        "useSSL": false
    },
    {
        "whitelist": "",
        "securelist": "^shopping",
        "match": "url",
        "roles": "",
        "permissions": "shop,checkout",
        "redirect": "user.login",
        "useSSL": true
    }
]
```
{% endcode %}
