---
description: How cbsecurity decides if a request can run, and what happens when it cannot.
icon: shield-check
---

# How the Firewall Works

The firewall wraps the `preProcess` interception point, the first thing that runs in a ColdBox request. It asks a **validator** whether the request is allowed and acts on the answer.

## How Validation Happens

You register a validator (the `firewall.validator` setting) that implements two functions from `cbsecurity.interfaces.ISecurityValidator`:

* `ruleValidator()` evaluates your [security rules](../usage/untitled-1.md).
* `annotationValidator()` evaluates the [security annotations](../usage/security-annotations.md) on your handlers and actions.

[Route middleware](../usage/route-middleware.md) calls the same validator for the secured route. Rules run before annotations, and you can use any mix of the three.

The validator answers one of three things: allowed, or not allowed with a type, **authentication** or **authorization**. A validator can also tell the firewall to block the request.

> `Authentication` is when a user is NOT logged in.
>
> `Authorization` is when a user is logged in but does not have the right permissions or roles.

{% hint style="info" %}
A validator can also challenge the user to log in. The `BasicAuthValidator` does this by sending a header that prompts the browser for credentials.
{% endhint %}

## When Access Is Denied

When the validator says no, the firewall does the following, whether the check came from a rule, an annotation or middleware:

* Logs the blocked request with LogBox, including the IP and extra metadata.
* Logs the block to the database when [firewall logs](configuration/firewall/README.md) are on, so the [visualizer](configuration/visualizer.md) can show it.
* When the action is a redirect, flashes the requested URL as `_securedURL`, so you can send the user back after login.
* Stores the matched rule in `prc.cbSecurity_matchedRule` (rules only) and the validator results in `prc.cbSecurity_validatorResults`.
* Announces `cbSecurity_onInvalidAuthentication` or `cbSecurity_onInvalidAuthorization` depending on the type. See [Interceptions](../usage/interceptions.md).
* Runs the default action for that type.

## The Default Actions

Each type has its own action, set by these four settings. The action is always one of three outcomes: relocate to another event or URL, override the event, or answer with a firewall `401 Not Authorized` block.

| Setting | Default | Description |
| --- | --- | --- |
| `invalidAuthenticationEvent` | none | The event, URI or URL to go to when a user is not logged in. |
| `defaultAuthenticationAction` | `redirect` | `redirect`, `override` or `block` when a user is not logged in. |
| `invalidAuthorizationEvent` | none | The event, URI or URL to go to when a user lacks permissions. |
| `defaultAuthorizationAction` | `redirect` | `redirect`, `override` or `block` when a user lacks permissions. |

A rule can set its own `action`, `redirect` and `overrideEvent`, which win over these defaults.
