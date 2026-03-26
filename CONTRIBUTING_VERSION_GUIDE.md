# Contributing to Multi-Version Documentation

## Purpose
This document provides guidelines for contributors to maintain version-specific content in the Dynamo Primer across multiple Dynamo versions (2.13+).

## Our Approach
The Dynamo Primer uses a **unified documentation approach** with **inline version indicators**. We avoid maintaining separate documentation trees for each version, instead marking version-specific content where it appears.

## When to Add Version Indicators

### Always Mark These Scenarios:
1. **Breaking Changes**: API changes, removed features, syntax changes
2. **New Features**: Features available only in certain versions
3. **Runtime Changes**: .NET Framework version differences (.NET 4.8 → .NET 8 → .NET 10)
4. **Package Compatibility**: Changes affecting third-party packages
5. **Python Engine Changes**: IronPython2 vs PythonNet3

### Examples of Version-Specific Content:
- Package migration guides (e.g., updating from 2.x to 3.x)
- Python engine selection (IronPython2 vs PythonNet3 in Dynamo 4.x)
- .NET runtime requirements for developers
- Host application integration changes

## How to Mark Version-Specific Content

### Use GitBook Hint Boxes
GitBook provides native hint syntax that renders as colored callout boxes:

#### For General Information
```markdown
{% hint style="info" %}
**Version Note**: This feature is available in Dynamo 3.0 and later.
{% endhint %}
```

#### For Important Warnings
```markdown
{% hint style="warning" %}
**Compatibility**: This syntax changed in Dynamo 2.0. For older versions, use the legacy syntax.
{% endhint %}
```

#### For Positive Messages
```markdown
{% hint style="success" %}
**New Feature**: Dynamo 4.0 introduces significant performance improvements!
{% endhint %}
```

#### For Critical Alerts
```markdown
{% hint style="danger" %}
**Breaking Change**: This API was removed in Dynamo 4.0. Use the replacement API instead.
{% endhint %}
```

### Placement Guidelines
1. **Page Level**: Add hints near the top of pages that are entirely version-specific
2. **Section Level**: Add hints before sections describing version-specific features
3. **Inline**: Use bold text for brief inline version notes: `**Dynamo 3.0+:**`

### Version Reference Format
Use these formats consistently:
- **Single version**: "Dynamo 2.13", "Dynamo 3.0"
- **Version range**: "Dynamo 2.x (2.13-2.19)", "Dynamo 3.0+"
- **Multiple versions**: "Dynamo 2.x and 3.x"

## Version Guide Maintenance

### Updating VERSION_GUIDE.md
When a new Dynamo version is released:

1. Update the version table with the new version
2. Add any major changes to the "Major Version Differences" section
3. Update the "Host Application Integration" table if needed
4. Link to new migration guides if created

### Creating Migration Guides
For major version changes, create a new migration guide following the pattern:
- `6-X-updating-your-packages-and-dynamo-libraries-for-dynamo-Xx.md`
- Include: runtime changes, API changes, migration steps, known issues
- Add a version badge at the top linking to other version guides

## Best Practices

### DO:
✅ Link to the VERSION_GUIDE.md for comprehensive version information  
✅ Test instructions across multiple versions when possible  
✅ Use consistent version notation  
✅ Update version tables when versions change  
✅ Mark breaking changes clearly  

### DON'T:
❌ Duplicate version information across multiple pages  
❌ Use vague version references like "recent versions" or "older versions"  
❌ Remove content for older versions unless truly obsolete  
❌ Add version notes to core concepts that apply across all versions  
❌ Create separate documentation trees per version  

## Testing Your Changes

Before submitting a PR with version-specific content:

1. **Clarity Check**: Is it immediately clear which version(s) this applies to?
2. **Context Check**: Does the reader have enough context to understand the difference?
3. **Link Check**: Are related version guides properly linked?
4. **Consistency Check**: Does the formatting match other version indicators?

## Examples from the Primer

### Good Example 1: Page-Level Version Context
```markdown
# Updating your Packages and Dynamo Libraries for Dynamo 4.x

{% hint style="info" %}
**Applies to**: Dynamo 4.x (4.0+)  
**Target Runtime**: .NET 10  
**Key Changes**: PythonNet3 default, obsolete API removal

For other versions, see: [Dynamo 2.x](link) | [Dynamo 3.x](link)
{% endhint %}
```

### Good Example 2: Section-Level Warning
```markdown
### Installing Packages

{% hint style="warning" %}
**Package Compatibility**: Packages built for different Dynamo versions may not be 
compatible due to runtime changes. Check for version-specific builds.
{% endhint %}
```

### Good Example 3: Inline Version Note
```markdown
The Python engine defaults to IronPython2 in Dynamo 2.x and 3.x, but **Dynamo 4.0+** 
uses PythonNet3 for all new Python nodes.
```

## Questions?
If you're unsure whether to add version indicators, ask yourself:
- "Will this work the same way in all supported Dynamo versions?"
- "Could a user from a different version be confused by this?"
- "Is there a breaking change or new feature here?"

If the answer is "no" to the first question or "yes" to the others, add a version indicator.

## Resources
- Main version guide: [VERSION_GUIDE.md](VERSION_GUIDE.md)
- Migration guides: See `11_developer_primer/3_developing_for_dynamo/6-*` files
- GitBook hint syntax: https://docs.gitbook.com/content-editor/blocks/hint

---
Last updated: 2026-01-26  
Maintainers: See [CONTRIBUTING.md] for general contribution guidelines
