---
description: Security rules in an XML file
---

# XML Rules

You can place all your security rules inside of an XML file and then tell CBSecurity where they are:

{% code title="config/Coldbox.cfc" %}
```javascript
// CB Security
cbSecurity : {
  firewall : {
    rules : {
      provider : {
        "source" : "config/security.xml.cfm"
      }
    }
  }
}
```
{% endcode %}

Then your XML file can look like this:

{% code title="config/security.xml.cfm" %}
```markup
<?xml version="1.0" encoding="ISO-8859-1"?>
<-- <
Declare as many rule elements as you want, order is important 
Remember that the securelist can contain a list of regular
expressions if you want

ex: All events in the user handler
 user\..*
ex: All events
 .*
ex: All events that start with admin
 ^admin

If you are not using regular expressions, just write the text
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
{% endcode %}
