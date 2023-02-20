---
description: >-
  This object is used to provide you with human, fluent and explicit security
  authorizations, authentication insight, utility and contexts.
---

# cbSecurity Model

## Explicit Authorizations

The `cbSecurity` model is a specialized service that will allow you to do explicit authorizations in any layer of your ColdBox application.

Sometimes, you will need authorization checks outside of the incoming request rules or the handler annotations. This can be from within interceptors, models, layouts, or views. For this, we have provided the `cbSecurity` model so you can do explicit authorization checks anywhere you like.

## `cbSecurity` Model

You can inject our model, or you can use our handy `cbsecure()` mixin (handlers/layouts/views) and then call the appropriate security functions:

```javascript
// Mixin: Handlers/Layouts/Views
cbsecure()

// Injection
property name="cbSecurity" inject="@cbSecurity"
```

{% hint style="danger" %}
All security methods will call the application's configured Authentication Service to retrieve the currently logged-in user. If the user is not logged in, an immediate `NoUserLoggedIn` exception will be thrown by all methods.
{% endhint %}

You can now discover our sections for securing using `cbSecurity`

{% content-ref url="authentication-methods.md" %}
[authentication-methods.md](authentication-methods.md)
{% endcontent-ref %}

{% content-ref url="authorization-contexts.md" %}
[authorization-contexts.md](authorization-contexts.md)
{% endcontent-ref %}

{% content-ref url="secure-blocking-methods.md" %}
[secure-blocking-methods.md](secure-blocking-methods.md)
{% endcontent-ref %}

{% content-ref url="securing-views.md" %}
[securing-views.md](securing-views.md)
{% endcontent-ref %}

{% content-ref url="utility-methods.md" %}
[utility-methods.md](utility-methods.md)
{% endcontent-ref %}

{% content-ref url="verification-methods.md" %}
[verification-methods.md](verification-methods.md)
{% endcontent-ref %}





