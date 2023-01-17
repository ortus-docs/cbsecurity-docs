---
description: Get up and running with CBSecurity in no time!
---

# Installation

Leverage [CommandBox](https://www.ortussolutions.com/products/commandbox) to install into your ColdBox app:

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

By default `cbsecurity` is configured to work with `cbauth` as the authentication service.  You only need to provide a user service class that knows how to connect to your database to retrieve and validate credentials.  You can also use the in-built basic authentication users as well.&#x20;

{% hint style="success" %}
You can find much more information about cbauth here: [https://forgebox.io/view/cbauth](https://forgebox.io/view/cbauth)
{% endhint %}

{% content-ref url="configuration/" %}
[configuration](configuration/)
{% endcontent-ref %}
