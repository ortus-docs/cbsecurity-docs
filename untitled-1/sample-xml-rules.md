# Sample XML Rules

```markup
<?xml version="1.0" encoding="ISO-8859-1"?>
<-- <
Declare as many rule elements as you want, order is important 
Remember that the securelist can contain a list of regular
expression if you want

ex: All events in the user handler
 user\..*
ex: All events
 .*
ex: All events that start with admin
 ^admin

If you are not using regular expression, just write the text
that can be found in an event.
-->
<rules>
    <rule>
        <match>event</match>
        <whitelist>user\.login,user\.logout,^main.*</whitelist>
        <securelist>^user\..*, ^admin</securelist>
        <roles>admin</roles>
        <permissions>read,write</permissions>
        <redirect>user.login</redirect>
    </rule>

    <rule>
       	<match>event</match>
        <whitelist></whitelist>
        <securelist>^moderator</securelist>
        <roles>admin,moderator</roles>
        <permissions>read</permissions>
        <redirect>user.login</redirect>
    </rule>

    <rule>
       	<match>url</match>
        <whitelist></whitelist>
        <securelist>/secured.*</securelist>
        <roles>admin,paid_subscriber</roles>
        <permissions></permissions>
        <redirect>user.pay</redirect>
    </rule>
</rules>
```

> **IMPORTANT** Please remember to white list your main events \(implicit\), login and logout events if you will be securing the entire application.

### First Rule Analysis

As you can see from the sample, the first rule has the following elements

```text
<match>event</match>
```

So it will match the icoming event.

```text
<whitelist>user\.login,user\.logout,^main.*</whitelist>
```

This means that the following events will not be verified for security: user.login, user.logout and any event that starts with main will be let through, if they match the secure list pattern.

```text
<securelist>^user\..*, ^admin</securelist>
```

This means that any event that starts with the word user. will be secured and anything that starts with the word admin will also be secured, unless the incoming event matches a pattern in the whitelist element.

```text
<roles>admin</roles>
```

This means that only a user with admin role will be allowed to visit the securelist events.

```text
<permissions>read,write</permissions>
```

This probably means that I am doing my own security validation and apart from having the user have a role of admin, he/she must also have the read and write permissions. My own validator will validate this logic.

```text
<redirect>user.login</redirect>
```

Then if it does not validate it will use this redirect element to relocate via `setNextEvent()`

### Second Rule Analysis

The second rule has the following elements:

```text
<match>event</match>
```

So it will match the icoming event.

```text
<whitelist></whitelist>
```

No white listed events are defined.

```text
<securelist>^moderator</securelist>
```

This means that any event that starts with the word moderator will be secured and validated against the user's credentials.

```text
<roles>admin,moderator</roles>
```

This means that users with roles of admin and moderator can execute events that are in the securelist.

```text
<permissions>read</permissions>
```

This probably means that I am doing my own security validation and apart from having the user have a role of admin or moderator, he/she must also have the read permission. My own validator will validate this logic.

```text
<redirect>user.login</redirect>
```

Then if it does not validate it will use this redirect element to relocate via `setNextEvent()`

### Third Rule Analysis

The third rule has the following elements:

```text
<match>URL</match>
```

So it will match the incoming URL pattern after the domain name and application location.

```text
<whitelist></whitelist>
```

No white listed events are defined.

```text
<securelist>/secured.*</securelist>
```

It will secure any incoming URI that starts with /secured

```text
<roles>admin,paid_subscriber</roles>
```

This means that users with roles of admin and paid\_subscriber can visit URLs with /secured in them

```text
<permissions></permissions>
```

No permissions

```text
<redirect>user.pay</redirect>
```

Then if it does not validate it will use this redirect element to relocate via `setNextEvent()`

