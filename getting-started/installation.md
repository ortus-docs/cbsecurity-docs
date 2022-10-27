---
description: Get up and running with CBSecurity in no time!
---

# Installation

Leverage CommandBox to install into your ColdBox app:

```bash
# Latest version
install cbsecurity

# Bleeding Edge
install cbsecurity@be
```

## System Requirements

* Lucee 5.x+
* ColdFusion 2018+
* A database for optional firewall logging

## Configuration Settings

By default `cbsecurity` is configured to work with `cbauth` as the authentication service.  You would only need to provide a user service class that knows how to connect to your database to retrieve and validate credentials.  You can also use the in-built basic authentication services as well.&#x20;

{% content-ref url="configuration/" %}
[configuration](configuration/)
{% endcontent-ref %}
