---
description: What's new with CBSecurity 3.4.1
---

# What's New With 3.4.1

CBSecurity 3.4.1 is a targeted maintenance release that addresses a specific database compatibility issue with Microsoft SQL Server.

## Database Compatibility Fix

### Microsoft SQL Server Support

- **Fixed**: Added proper parenthesis on `TOP` statements for Microsoft SQL Server in the `DBLogger`
- This fix resolves SQL syntax errors that were occurring when using CBSecurity's database logging features with MSSQL Server
- Thanks to @irvirv for identifying and helping resolve this issue

## System Requirements

- **ColdBox Framework**: 6+
- **CFML Engines**: Adobe ColdFusion 2018+, Lucee 5+
- **CommandBox**: 5.0+
- **Database**: Any supported database (MSSQL Server compatibility enhanced)

## Compatibility Notes

This release maintains full backward compatibility with existing CBSecurity 3.x installations while fixing a specific SQL syntax issue affecting Microsoft SQL Server users.

## Migration Notes

No migration steps are required for this release. Simply update your CBSecurity module dependency:

```bash
# Update to latest version
box update cbsecurity

# Or install specific version
box install cbsecurity@3.4.1
```

### Microsoft SQL Server Users

If you're using Microsoft SQL Server and experiencing SQL syntax errors in the DBLogger, this update will resolve those issues. No manual database changes are required.

## Related Resources

- [CBSecurity Documentation](https://coldbox-security.ortusbooks.com/)
- [Source Code](https://github.com/coldbox-modules/cbsecurity)
- [Issue Tracker](https://ortussolutions.atlassian.net/projects/BOX/issues)
- [Database Configuration Guide](../../getting-started/configuration/firewall/rule-sources/untitled.md)