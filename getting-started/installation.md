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

* A database for optional firewall logging
* ColdBox 6+
* ColdBox 7+ for delegates and basic auth support only

## Mixins

The following mixins are registered once the module is installed:

```javascript
/**
 * Retrieve the Jwt Auth Service
 */
function jwtAuth()

/**
 * Retrieve the CBSecurity Service Object
 */
function cbSecure()
```

## Configuration Settings

By default `cbsecurity` is configured to work with `cbauth` as the authentication service.  You only need to provide a user service class that knows how to connect to your database to retrieve and validate credentials.  You can also use the in-built basic authentication users as well.&#x20;

{% hint style="success" %}
You can find much more information about cbauth here: [https://forgebox.io/view/cbauth](https://forgebox.io/view/cbauth)
{% endhint %}

{% content-ref url="configuration/" %}
[configuration](configuration/)
{% endcontent-ref %}
