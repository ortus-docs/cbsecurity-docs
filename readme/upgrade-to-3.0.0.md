# Upgrade to 3.0.0

CBSecurity 3 is a major release and it will require some updates in order for you to fully upgrade your previous versions.

### Adobe 2016

This engine is no longer supported

### JwtService Validator Deprecated

In the previous releases the validator for JWT was `JwtService@cbsecurity`.  This has now changed to `JwtAuthValidator@cbsecurity`.  So make sure you update your configurations.

### CBAuthValidator Deprecated

The `CBAuthValidator` has been renamed to just `AuthValidator`.  This validator is now not `cbauth` focused but `IAuthService` focused.  It also supports role and permission based authorization.

### Settings Structure

The entire settings structure has been redesigned to support many features in a a more concise and block approach.  All top-level settings have been removed and added to specific sections.  Please review the [Configuration](../getting-started/configuration/) section in detail to see where the new settings belongs to.
