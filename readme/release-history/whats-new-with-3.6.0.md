---
description: '2025-12-08'
---

# What's New With 3.6.0

#### Security

* **CRITICAL**: Fixed open redirect vulnerability in `_securedURL` handling. The `saveSecuredUrl()` method now validates redirect URLs to ensure they belong to the same host as the current request, preventing attackers from crafting malicious URLs that redirect users to external sites after login. Added `isSafeRedirectUrl()` validation  `java.net.URI` to compare hosts.

#### Fixed

* BOX-164 Allow Visualizer to show settings when `firewall.logging` not enabled
* JWT Handler improperly returns a value, causing it to skip ColdBox's RestHandler's response formatting logic. This results in the entire response object being returned rather than just invoking getDataPacket()
