---
description: What's new with CBSecurity 3.4.2
---

# What's New With 3.4.2

CBSecurity 3.4.2 is a maintenance release that addresses database compatibility issues and improves documentation standards.

## Database Compatibility Improvements

### Oracle Database Support

- **Fixed**: Updated security logs columns to work with Oracle databases using `clob` data type
- This enhancement ensures CBSecurity's logging functionality works seamlessly with Oracle database environments
- Improves enterprise database compatibility for security audit trails

### Security Logs Configuration

- **Fixed**: `cbsecurity_logs` table name is now properly referenced instead of being hard-coded
- This change ensures the module setting for the logs table name is properly respected
- Provides better flexibility for custom table naming conventions

## Documentation Improvements

### Markdown Rules Updates

- **Fixed**: Updated markdown rules to eliminate duplicate headers
- Improved documentation consistency and readability
- Enhanced GitBook compatibility and navigation structure

## System Requirements

- **ColdBox Framework**: 6+
- **CFML Engines**: Adobe ColdFusion 2018+, Lucee 5+
- **CommandBox**: 5.0+
- **Database**: Any supported database (Oracle compatibility enhanced)

## Compatibility Notes

This release maintains full backward compatibility with existing CBSecurity 3.x installations while improving database compatibility across different database engines.

## Migration Notes

No migration steps are required for this release. Simply update your CBSecurity module dependency:

```bash
# Update to latest version
box update cbsecurity

# Or install specific version
box install cbsecurity@3.4.2
```

### Oracle Database Users

If you're using Oracle and experiencing issues with security logs, this update will resolve column type compatibility issues. No manual database changes are required.

## Related Resources

- [CBSecurity Documentation](https://coldbox-security.ortusbooks.com/)
- [Source Code](https://github.com/coldbox-modules/cbsecurity)
- [Issue Tracker](https://ortussolutions.atlassian.net/projects/BOX/issues)
- [Database Configuration Guide](../../getting-started/configuration/firewall/rule-sources/untitled.md)