---
description: >-
  Get access to several security-focused methods to assist in your security
  journey.
---

# Utility Methods

The CBSecurity model also has some methods that assist developers in their security needs

### Generating Passwords

You can use the `createPassword( length:32, letters:true, numbers:true, symbols:true )`&#x20;

```java
// Generate a random password 32 characters in length
cbsecure().createPassword()

// Generate with no symbols and 16 characters
cbsecure().createPassword( length: 16, symbols: false )

// Generate with no numbers and 12 characters
cbsecure().createPassword( length: 12, numbers: false )
```

### The Real Slim Shady!

You can also leverage our internal utility methods to get the current request's IP address and hostname.   Please remember that the default is to trust the upstream headers and check those first.

* `getRealIP( trustUpstream : true )` : Get a request's actual IP address
* `getRealHost( trustUpstream : true )` : Get a request's actual hostname used
