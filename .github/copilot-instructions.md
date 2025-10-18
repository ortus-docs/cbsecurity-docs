### Quick orientation — cbsecurity-docs (GitBook Documentation)

This repository contains the comprehensive documentation for the CBSecurity ColdBox module, organized as a GitBook project that covers security, authentication, authorization, JWT handling, CSRF protection, and more.

Keep guidance short and actionable. Prefer small, verifiable edits that maintain consistency with the existing documentation structure and GitBook formatting.

## 1) Big picture
- **GitBook-structured documentation**: Uses `SUMMARY.md` for navigation structure and individual `.md` files for content
- **Comprehensive coverage**: Documents all aspects of CBSecurity including firewall rules, JWT services, authentication providers, security validators, and configuration
- **Cross-referenced content**: Links between sections using relative paths and GitBook-style references
- **Version-aware**: Tracks release history and upgrade guides for major versions (3.x series)

## 2) Documentation structure & where to make changes
- **Entry point**: `README.md` - main introduction and overview
- **Navigation**: `SUMMARY.md` - defines the GitBook table of contents and page hierarchy
- **Getting Started**: `getting-started/` - installation, overview, and configuration guides
- **Configuration guides**: `getting-started/configuration/` - specific setup for authentication, JWT, CSRF, firewall rules, etc.
- **Usage examples**: `usage/` - practical implementation guides and code examples
- **Security validators**: `security-validators/` - detailed docs on different validator types
- **JWT documentation**: `jwt/` - comprehensive JWT implementation guides
- **Release notes**: `readme/release-history/` - version-specific changes and upgrade guides

## 3) Content patterns & conventions to follow
- **GitBook frontmatter**: Each `.md` file starts with YAML frontmatter containing `description` field
- **Image handling**: Images stored in `.gitbook/assets/` with descriptive names and proper captions using `<figure>` tags
- **Code blocks**: Use appropriate language tags (```bash, ```javascript, ```cfml, etc.)
- **Cross-references**: Use relative paths for internal links, maintain GitBook's link structure
- **Consistent terminology**:
  - "CBSecurity" (not "cbsecurity" in prose)
  - "ColdBox" (proper capitalization)
  - "CommandBox" for the CLI tool
  - "Firewall" for security rules engine
  - "Validator" for authentication/authorization components

## 4) Documentation maintenance workflows
- **Content updates**: Edit individual `.md` files directly, ensure frontmatter is preserved
- **Navigation changes**: Update `SUMMARY.md` when adding/removing/reorganizing pages
- **Image updates**: Place new images in `.gitbook/assets/` with descriptive names
- **Version updates**: Add new release notes to `readme/release-history/` and update version references throughout
- **Cross-reference validation**: Ensure internal links work when moving or renaming content

## 5) GitBook-specific formatting
- **Hints and callouts**: Use GitBook hint blocks: `{% hint style="info" %}`, `{% hint style="warning" %}`, etc.
- **Code tabs**: Use GitBook code tabs for multi-language examples
- **Figure captions**: Wrap images in `<figure>` tags with `<figcaption>` for proper GitBook rendering
- **Table of contents**: Let GitBook auto-generate from `SUMMARY.md` structure

## 6) Content areas and their focus
- **Getting Started**: Focus on quick setup and basic configuration
- **Configuration**: Deep-dive into specific feature setup with code examples
- **Usage**: Practical implementation patterns and real-world examples
- **Security Validators**: Technical details on different authentication/authorization strategies
- **JWT**: Complete guide to token-based authentication implementation
- **Release History**: Version-specific changes, breaking changes, and migration guides

## 7) Small, high-value tasks for AI agents
- **Update code examples**: Ensure all CFML/JavaScript examples are current and functional
- **Cross-reference validation**: Check and fix broken internal links between documentation sections
- **Consistency improvements**: Standardize terminology, formatting, and code block languages
- **Content gaps**: Identify missing examples or unclear explanations in existing sections
- **Version updates**: Add new features documentation when CBSecurity module is updated

## 8) Content quality guidelines
- **Practical examples**: Every configuration option should have a working code example
- **Clear explanations**: Technical concepts should be explained for both beginners and advanced users
- **Up-to-date references**: Ensure all links to external resources (GitHub, CommandBox, etc.) are current
- **Visual aids**: Use diagrams and screenshots to illustrate complex concepts like security flow
- **Progressive complexity**: Start with simple examples and build to more advanced use cases

## 9) GitBook project specifics
- **Assets management**: Images and other assets in `.gitbook/assets/` should be optimized and properly named
- **Markdown compatibility**: Use standard Markdown with GitBook extensions, avoid HTML when possible
- **Search optimization**: Use clear headings and descriptive content for better GitBook search functionality
- **Mobile-friendly**: Consider how content renders on mobile devices through GitBook

## 10) Integration with main project
- **Source synchronization**: Documentation should reflect the current state of the CBSecurity module codebase
- **API documentation**: Keep method signatures and configuration options in sync with actual module code
- **Example validation**: Code examples should be tested against the actual CBSecurity module functionality

If anything above is unclear or missing (GitBook publishing workflow, content review process, or integration patterns), tell me which area to expand and I will iterate.

## Documentation Standards (GitBook)

### Code Block Formatting
When documenting code examples, use the following GitBook syntax:

```
{% code title="filename.ext" overflow="wrap" lineNumbers="true" %}

{% code language="javascript|groovy|cfml" %}
your code here
{% endcode %}

{% endcode %}
```

### Language Tabs (BoxLang + CFML)
For examples supporting both BoxLang and CFML, always use tab interface with BoxLang first:

```
{% tabs %}

{% tab title="BoxLang" %}

BoxLang example code:

{% code title="Test.bx" overflow="wrap" lineNumbers="true" %}

{% code language="groovy" %}
// BoxLang code
{% endcode %}

{% endcode %}

{% endtab %}

{% tab title="CFML" %}

CFML example code:

{% code title="Test.cfc" overflow="wrap" lineNumbers="true" %}

{% code language="javascript" %}
// CFML code (uses cfscript syntax)
{% endcode %}

{% endcode %}

{% endtab %}

{% endtabs %}
```

### Supported Platforms
- **BoxLang** 1.0+ (Preferred)
- **Adobe ColdFusion** 2023+
- **Lucee** 5.x+
- **Dependencies**: ColdBox 7+, cbi18n 3.0+

### Documentation Priority
When writing new documentation:
1. Always provide BoxLang example first, then CFML
2. Use language-appropriate syntax highlighting (groovy for BoxLang, javascript for CFML)
3. Include practical, copy-paste ready examples
4. Link to related sections and external resources
5. Include version compatibility notes when applicable