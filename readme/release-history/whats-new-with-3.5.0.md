---
description: What's new with CBSecurity 3.5.0
---

# What's New With 3.5.0

CBSecurity 3.5.0 is a significant modernization release that brings enhanced platform support, improved development workflows, and comprehensive AI assistance capabilities.

## BoxLang Certification

CBSecurity 3.5.0 has been fully certified for **BoxLang**, the modern dynamic JVM language. This includes:

- Complete compatibility testing with BoxLang runtime
- Validated functionality across all CBSecurity features
- Updated examples and documentation for BoxLang syntax
- Full test harness coverage for BoxLang environments

## ColdBox 8 Support

This release adds official support and certification for **ColdBox 8**, ensuring CBSecurity works seamlessly with the latest ColdBox framework features and improvements.

## Enhanced Development Workflows

### GitHub Actions Updates

The project has migrated to modern GitHub Actions workflows, providing:

- Improved CI/CD pipeline reliability
- Better cross-platform testing coverage
- Automated testing across multiple CFML engines
- Enhanced security scanning and dependency management

### Test Harness Improvements

The test harness has been significantly upgraded to provide:

- Better local development experience
- Enhanced integration testing capabilities
- Improved TestBox runner configuration
- Streamlined server startup and configuration

## AI-Powered Development Assistance

### GitHub Copilot Instructions

CBSecurity 3.5.0 introduces comprehensive AI assistance through:

- **`.github/copilot-instructions.md`** - Detailed guidance for AI agents covering:
  - Module architecture and component relationships
  - Security validator patterns and implementation
  - Interceptor flow and event handling
  - Development workflows and best practices
  - Test harness setup and TestBox runner details

This enhancement enables AI tools like GitHub Copilot to provide more accurate and contextual assistance when working with CBSecurity.

## Developer Experience Improvements

### Documentation Enhancements

- Documented test-harness structure and usage patterns
- Enhanced TestBox runner details for local integration testing
- Improved developer workflow documentation
- Better guidance for module extension and customization

### Local Development Setup

The release includes improved documentation and tooling for:

- Setting up local development environments
- Running integration tests via `test-harness/tests/runner.cfm`
- Using `box.json` scripts for common development tasks
- Server configuration for different CFML engines

## System Requirements

- **ColdBox Framework**: 6+ (ColdBox 8 certified)
- **CFML Engines**: Adobe ColdFusion 2018+, Lucee 5+, BoxLang 1.0+
- **CommandBox**: 5.0+

## Compatibility Notes

This release maintains full backward compatibility with existing CBSecurity 3.x installations. No breaking changes have been introduced.

## Migration Notes

No migration steps are required for this release. Simply update your CBSecurity module dependency:

```bash
# Update to latest version
box update cbsecurity

# Or install specific version
box install cbsecurity@3.5.0
```

## Related Resources

- [CBSecurity Documentation](https://coldbox-security.ortusbooks.com/)
- [Source Code](https://github.com/coldbox-modules/cbsecurity)
- [Issue Tracker](https://ortussolutions.atlassian.net/projects/BOX/issues)
- [BoxLang Documentation](https://boxlang.ortusbooks.com/)