---
description: Welcome to the ColdBox Security, the best way to secure your ColdBox apps.
---

# 🔏 Introduction

<figure><img src=".gitbook/assets/CBSecurity-S-darkbg.png" alt=""><figcaption><p>The Best Way To Secure Your Applications</p></figcaption></figure>

The ColdBox `cbsecurity` module is a collection of modules to help secure your ColdBox applications.



<figure><img src=".gitbook/assets/cbsecurity-3-visualizer (1) (1).png" alt=""><figcaption><p>Security Visualizer</p></figcaption></figure>



The major areas of concern are:

* A **security authentication/authorization firewall** ( `cbsecurity` ) which can secure your application based on:
  * Security rules and a rule engine for validation of incoming events or URL patterns
  * Handler annotations
* Security **** service for explicit authorizations ( `cbsecurity` ) to provide you with functional approaches to security context authorization in **any** layer of your application.
* A **JWT generator, decoder, and authentication services** ( `jwtcfml` )&#x20;
* Cross-Site Request Forgery **(CSRF) Protection** ( `cbcsrf` )
* An **authentication manage**r ( `cbauth` ) which can be plug-and-play with your own or third-party modules
* Basic Authentication services that provide basic user credential storage and browser challenges
* A graphical user interface for visualizing the firewall and operational settings we lovingly call the **CBSecurity Visualizer**
* Industry-standard response headers to protect against XSS, clickjacking, frame busting, and much more
* Generate secure and random passwords

## Module composition

![CBSecurity consumes several other modules and leverages cbstorages for storage.](.gitbook/assets/cbSecurity-Modules.png)

## Features

* Ability to have global security rules
* The ability for modules to add their own security rules and action overrides
* Ability to distinguish between authentication and authorization issues
* Annotation-driven cascading security for handlers and actions
* A functional security service that can be injected anywhere to provide you with authorizations
* Security rules can exist in:
  * XML File
  * JSON File
  * Database
  * Models
* The rules can be configured to use regular expressions or simple snippets
* You can use ColdFusion authentication security
* Can leverage any custom authentication provider
* Plug any Authentication service or can leverage [cbauth](https://github.com/elpete/cbauth) by default
* Ability to distinguish between invalid authentication and authorization and determine the process's outcome. &#x20;
* Ability to load/unload security rules from contributing modules.
* The ability for each module to define its own `validator`
* JWT Access and Refresh Tokens Native support

## Versioning <a href="#versioning" id="versioning"></a>

The ColdBox Security Module is maintained under the [Semantic Versioning](http://semver.org/) guidelines as much as possible. Releases will be numbered in the following format:

```
<major>.<minor>.<patch>
```

And constructed with the following guidelines:

* Breaking backward compatibility bumps the major (and resets the minor and patch)
* New additions without breaking backward compatibility bumps the minor (and resets the patch)
* Bug fixes and misc changes bumps the patch

## License <a href="#license" id="license"></a>

Apache 2 License: [http://www.apache.org/licenses/LICENSE-2.0](https://www.apache.org/licenses/LICENSE-2.0)​

## Important Links <a href="#important-links" id="important-links"></a>

* Code: [https://github.com/coldbox-modules/cbsecurity](https://github.com/coldbox-modules/cbsecurity)​
* Issues: [https://github.com/coldbox-modules/cbsecurity/issues](https://github.com/coldbox-modules/cbsecurity/issues)

## Professional Open Source <a href="#professional-open-source" id="professional-open-source"></a>

![Ortus Solutions, Corp](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LA-UVvG0NM7NpDzssBL%2F-LA-Uaei0WzTH7Su5CR7%2F-LA-UqN1BRXynZ7RUVO7%2Fortussolutions\_button.png?generation=1523647999385555\&alt=media)

The ColdBox Security Module is a professional open-source software backed by [Ortus Solutions, Corp](http://www.ortussolutions.com/services) offering services like:

* Custom Development
* Professional Support & Mentoring
* Training
* Server Tuning
* Security Hardening
* Code Reviews
* [Much More](http://www.ortussolutions.com/services)

### Discussion & Help

The Box products and modules community for discussion and help can be found here:

[https://community.ortussolutions.com/c/box-modules/cbsecurity/](https://community.ortussolutions.com/c/box-modules/cbsecurity/26)

### HONOR GOES TO GOD ABOVE ALL <a href="#honor-goes-to-god-above-all" id="honor-goes-to-god-above-all"></a>

Because of His grace, this project exists. If you don't like this, then don't read it; it's not for you.

> "Therefore being justified by **faith**, we have peace with God through our Lord Jesus Christ: By whom also we have access by **faith** into this **grace** wherein we stand, and rejoice in hope of the glory of God." Romans 5:5
