---
description: September 21, 2026
---

# What's new With 3.8.0

#### Changed

* Upgraded `cbauth` to `^7.0.0`. This version makes the authentication service startup thread-safe.

#### Fixed

* Fixed the `Strict-Transport-Security` header, which was rendered with the wrong separators. It is now built the way browsers expect:

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

`includeSubDomains` and `preload` are only added when their [security headers](../../getting-started/configuration/security-headers.md) settings are turned on. If you worked around the old output in a proxy or a test, you can remove that workaround.
