---
description: What cbsecurity does and which part of it to reach for.
icon: head-side-gear
---

# Overview

For any security system you need to know **who** the user is (authentication) and **what** they are allowed to do (authorization). cbsecurity provides both:

* **Authentication**: validates credentials, logs users in and out, and tracks them in sessions or any custom storage.
* **Authorization**: validates permissions, roles, both or neither.

![](<../.gitbook/assets/image (2) (1).png>)

## Ways to Secure Your App

Pick the one that matches where you want the rule to live. They share the same validators, invalid actions, interceptions and logging, so you can mix them.

| Approach | Where the rule lives | Best for |
| --- | --- | --- |
| [Security rules](../usage/security-rules.md) | Config, JSON, XML, a database or a model | Central, data-driven policies. Rules can protect events **and** URLs, and admins can edit them at runtime. |
| [Annotations](../usage/security-annotations.md) | On the handler or action | Security that travels with the code. |
| [Route middleware](../usage/route-middleware.md) | Next to the route in `config/Router` | Securing routes and groups with no rules file, plus throttling, API keys, IP filtering and more. |
| [The `cbSecurity` model](../usage/cbsecurity-model/README.md) | In your code | Securing any code context: services, views, blocks of logic. |

When combined, rules run first, then annotations, then route middleware.

![ColdBox Security Firewall](<../.gitbook/assets/image (1) (1).png>)

## Validators

A **validator** knows how to authenticate and authorize a request. cbsecurity ships with:

| Validator | Use it for |
| --- | --- |
| [Auth Validator](../security-validators/auth-validator.md) | The default. Authentication and permission security through `IAuthService` and `IAuthUser`. |
| [CFML Security Validator](../security-validators/default-security.md) | The engine's `cflogin` security. Authentication and role security. |
| [Basic Auth Validator](../security-validators/basicauth-validator.md) | HTTP Basic challenges, with optional in-config user storage. |
| [JWT Validator](../jwt/jwt-validator.md) | JSON Web Tokens for REST APIs. |
| [Custom Validator](../security-validators/custom-security-validator-object.md) | Your own authentication and authorization engine. |

<figure><img src="../.gitbook/assets/CBSecurity Validators.png" alt=""><figcaption><p>Security Validators Process Flow</p></figcaption></figure>

## How a Request Is Checked

The firewall asks the validator at `preProcess`. If the answer is no, it logs the block, announces an interception and runs a redirect, an override or a block. See [How the Firewall Works](how-the-firewall-works.md).

## Next Steps

* New to cbsecurity? Start with [Installation](installation.md) and [Configuration](configuration/README.md).
* Securing a REST API? See [JWT](../jwt/jwt-services.md).
* Protecting forms? See [Cross Site Request Forgery](../usage/cross-site-request-forgery-cbcsrf.md).
* Debugging rules? Turn on the [Visualizer](configuration/visualizer.md).
* Looking up an event? See [Interceptions](../usage/interceptions.md).
