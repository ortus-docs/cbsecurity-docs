# Table of contents

* [Introduction](README.md)
  * [Release History](readme/release-history/README.md)
    * [What's new With 3.9.0](readme/release-history/whats-new-with-3.9.0.md)
    * [What's new With 3.8.0](readme/release-history/whats-new-with-3.8.0.md)
    * [What's new With 3.7.0](readme/release-history/whats-new-with-3.7.0.md)
    * [What's New With 3.6.0](readme/release-history/whats-new-with-3.6.0.md)
    * [What's New With 3.5.0](readme/release-history/whats-new-with-3.5.0.md)
    * [What's New With 3.4.3](readme/release-history/whats-new-with-3.4.3.md)
    * [What's New With 3.4.2](readme/release-history/whats-new-with-3.4.2.md)
    * [What's New With 3.4.1](readme/release-history/whats-new-with-3.4.1.md)
    * [What's New With 3.4.0](readme/release-history/whats-new-with-3.4.0.md)
    * [What's New With 3.3.0](readme/release-history/whats-new-with-3.3.0.md)
    * [What's New With 3.2.0](readme/release-history/whats-new-with-3.2.0.md)
    * [What's New With 3.1.0](readme/release-history/whats-new-with-3.1.0.md)
    * [What's New With 3.0.0](readme/release-history/whats-new-with-3.0.0.md)
  * [Upgrade to 3.0.0](readme/upgrade-to-3.0.0.md)
  * [About This Book](readme/about-this-book/README.md)
    * [Author](readme/about-this-book/author.md)

## Get Started

* [Overview](getting-started/overview.md)
* [Quickstart](getting-started/quickstart.md)
* [Installation](getting-started/installation.md)
* [How the Firewall Works](getting-started/how-the-firewall-works.md)
* [Configuration](getting-started/configuration/README.md)
  * [Full Configuration](getting-started/configuration/full-configuration.md)
  * [🔏 Authentication](getting-started/configuration/authentication.md)
  * [🥸 Basic Auth](getting-started/configuration/basic-auth.md)
  * [🌐 JWT](getting-started/configuration/jwt.md)
  * [🧱 Firewall](getting-started/configuration/firewall/README.md)
    * [DB Rules](getting-started/configuration/firewall/rule-sources/db-rules.md)
    * [JSON Rules](getting-started/configuration/firewall/rule-sources/json-properties.md)
    * [Model Rules](getting-started/configuration/firewall/rule-sources/model-rules.md)
    * [XML Rules](getting-started/configuration/firewall/rule-sources/xml-properties.md)
  * [🚦 Route Middleware](getting-started/configuration/middleware.md)
  * [🔬 Visualizer](getting-started/configuration/visualizer.md)

## Secure Your App

* [Security Rules](usage/security-rules.md)
* [Security Annotations](usage/security-annotations.md)
* [Route Middleware](usage/route-middleware.md)
  * [Throttle](usage/route-middleware/throttle.md)
  * [API Keys](usage/route-middleware/api-key.md)
  * [IP Filtering](usage/route-middleware/ip-filtering.md)
  * [Ensure HTTPS](usage/route-middleware/ensure-https.md)
  * [Verify CSRF](usage/route-middleware/verify-csrf.md)
  * [Honeypot](usage/route-middleware/honeypot.md)
  * [Signed URLs](usage/route-middleware/signed-urls.md)
* [Secured URL](usage/secured-url.md)

## Authentication

* [Authentication Services](usage/authentication-services.md)
* [Basic Authentication](usage/basic-authentication.md)
* [Auth User](usage/auth-user.md)
* [Delegates](usage/delegates.md)

## JWT

* [JWT Services](jwt/jwt-services.md)
* [JWT Validator](jwt/jwt-validator.md)
* [Refresh Tokens](jwt/refresh-tokens.md)
* [Token Storage](jwt/jwt-token-storage.md)
* [JWT Interceptions](jwt/jwt-interceptions.md)

## CSRF and Headers

* [Cross Site Request Forgery](usage/cross-site-request-forgery-cbcsrf.md)
* [☢️ Security Headers](getting-started/configuration/security-headers.md)

## Reference

* [cbSecurity Model](usage/cbsecurity-model/README.md)
  * [Authentication Methods](usage/cbsecurity-model/authentication-methods.md)
  * [Authorization Contexts](usage/cbsecurity-model/authorization-contexts.md)
  * [Blocking Methods](usage/cbsecurity-model/secure-blocking-methods.md)
  * [Securing Views](usage/cbsecurity-model/securing-views.md)
  * [Utility Methods](usage/cbsecurity-model/utility-methods.md)
  * [Verification Methods](usage/cbsecurity-model/verification-methods.md)
* [Interceptions](usage/interceptions.md)
* [Auth Validator](security-validators/auth-validator.md)
* [BasicAuth Validator](security-validators/basicauth-validator.md)
* [CFML Security Validator](security-validators/default-security.md)
* [Custom Validator](security-validators/custom-security-validator-object.md)

## External links

* [Issue Tracker](https://ortussolutions.atlassian.net/projects/BOX/issues)
* [Source code](https://github.com/coldbox-modules/cbsecurity)
* [Sponsor Us](https://patreon.com/ortussolutions)
