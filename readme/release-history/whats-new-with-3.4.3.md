---
description: What's new with CBSecurity 3.4.3
---

# What's New With 3.4.3

CBSecurity 3.4.3 is a maintenance release that addresses ColdBox 7 compatibility requirements.

## ColdBox 7 Compliance

### View Rendering Method Update

The primary change in this release addresses a breaking change in ColdBox 7:

- **Fixed**: Renamed `renderView()` to `view()` to be ColdBox 7 compliant
- This change ensures CBSecurity works properly with ColdBox 7's updated view rendering methods
- Maintains backward compatibility with earlier ColdBox versions

## System Requirements

- **ColdBox Framework**: 6+ (ColdBox 7 compliant)
- **CFML Engines**: Adobe ColdFusion 2018+, Lucee 5+
- **CommandBox**: 5.0+

## Compatibility Notes

This release maintains full backward compatibility with existing CBSecurity 3.x installations while ensuring forward compatibility with ColdBox 7.

## Migration Notes

No migration steps are required for this release. Simply update your CBSecurity module dependency:

```bash
# Update to latest version
box update cbsecurity

# Or install specific version
box install cbsecurity@3.4.3
```

## Related Resources

- [CBSecurity Documentation](https://coldbox-security.ortusbooks.com/)
- [Source Code](https://github.com/coldbox-modules/cbsecurity)
- [Issue Tracker](https://ortussolutions.atlassian.net/projects/BOX/issues)
- [ColdBox Framework](https://coldbox.ortusbooks.com/)