# Securing Views

You can also use our handy `event.secureView()` method in the request context to pivot between views according to user permissions.

{% hint style="info" %}
cbSecurity injects the `secureView()` method into the request context via the `preProcess` interception point.
{% endhint %}

```javascript
event.secureView( permissions, successView, failView )
```

This will allow you to set the `successView` if the user has the permissions or the `failView` if they don't.
