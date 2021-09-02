# Introduction

The ColdBox `cbsecurity` module is a collection of modules to help secure your applications.

The major areas of concern are:

* A **security authentication/authorization firewall** \( `cbsecurity` \) which can secure your application based on:
  * Security rules and a rule engine for validation incoming events or URL's
  * Handler annotations
* A **security service** for explicit authorizations \( `cbsecurity` \) to provide you with functional approaches to security context authorization in **any** layer of your application.
* A **JWT generator, decoder and authentication services** \( `jwtcfml` \) 
* Cross Site Request Forgery **\(CSRF\) Protection** \( `cbcsrf` \)
* An **authentication manage**r \( `cbauth` \)

## Module composition

![Cbsecurity consumes several other modules and leverages cbstorages for storage.](.gitbook/assets/cbsecurity-modules.png)

## Features

* Ability to have global security rules
* Ability for modules to add their own security rules and action overrides
* Ability to distinguish between authentication and authorization issues
* Annotation driven cascading security for handlers and actions
* A functional security service that can be injected anywhere to provide you with authorizations
* Security rules can exist in:
  * XML File
  * JSON File
  * Database
  * Models
* The rules can be configured to use regular expressions or simple snippets
* Can use ColdFusion authentication security
* Can leverage any custom authentication provider
* Plug any Authentication service or can leverage [cbauth](https://github.com/elpete/cbauth) by default
* Capability to distinguish between invalid **authentication** and invalid **authorization** and determine an outcome of the process.  
* Ability to load/unload security rules from contributing modules.
* Ability for each module to define it's own `validator`

## Versioning <a id="versioning"></a>

The ColdBox Security Module is maintained under the [Semantic Versioning](http://semver.org/) guidelines as much as possible.  Releases will be numbered with the following format:

```text
<major>.<minor>.<patch>
```

And constructed with the following guidelines:

* Breaking backward compatibility bumps the major \(and resets the minor and patch\)
* New additions without breaking backward compatibility bumps the minor \(and resets the patch\)
* Bug fixes and misc changes bumps the patch

## License <a id="license"></a>

Apache 2 License: [http://www.apache.org/licenses/LICENSE-2.0](https://www.apache.org/licenses/LICENSE-2.0)​

## Important Links <a id="important-links"></a>

* Code: [https://github.com/coldbox-modules/cbsecurity](https://github.com/coldbox-modules/cbsecurity)​
* Issues: [https://github.com/coldbox-modules/cbsecurity/issues](https://github.com/coldbox-modules/cbsecurity/issues)

## Professional Open Source <a id="professional-open-source"></a>

![Ortus Solutions, Corp](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LA-UVvG0NM7NpDzssBL%2F-LA-Uaei0WzTH7Su5CR7%2F-LA-UqN1BRXynZ7RUVO7%2Fortussolutions_button.png?generation=1523647999385555&alt=media)

The ColdBox Security Module is a professional open source software backed by [Ortus Solutions, Corp](http://www.ortussolutions.com/services) offering services like:

* Custom Development
* Professional Support & Mentoring
* Training
* Server Tuning
* Security Hardening
* Code Reviews
* [Much More](http://www.ortussolutions.com/services)

### Discussion & Help 

The Box products and modules community for  discussion and help can be found here: 

[https://community.ortussolutions.com/c/box-modules/cbsecurity/](https://community.ortussolutions.com/c/box-modules/cbsecurity/26)

### HONOR GOES TO GOD ABOVE ALL <a id="honor-goes-to-god-above-all"></a>

Because of His grace, this project exists. If you don't like this, then don't read it, it's not for you.

> "Therefore being justified by **faith**, we have peace with God through our Lord Jesus Christ: By whom also we have access by **faith** into this **grace** wherein we stand, and rejoice in hope of the glory of God." Romans 5:5

