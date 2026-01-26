# Version Guide

## About This Guide

The Dynamo Primer covers features and functionality across multiple versions of Dynamo (v2.13 through v4.0+). This guide helps you understand which content applies to your version of Dynamo and how to navigate version-specific information.

## Supported Versions

This Primer provides documentation for the following Dynamo versions:

| Version Range | Key Features | Documentation Status |
|---------------|-------------|---------------------|
| **Dynamo 2.x** (2.13 - 2.17) | XML to JSON file format, .NET Framework 4.8 | Fully Documented |
| **Dynamo 3.x** (3.0 - 3.6) | .NET 8 runtime, improved stability, Python updates | Fully Documented |
| **Dynamo 4.x** (4.0+) | .NET 10 runtime, PythonNet3 default, significant performance improvements | Fully Documented |

## Understanding Version Compatibility

### Core Concepts and Features
Most core Dynamo concepts (nodes, wires, visual programming fundamentals, geometry) remain consistent across all versions. These sections apply to all users regardless of version.

### Version-Specific Content
Throughout this Primer, you'll encounter content marked with version indicators:

{% hint style="info" %}
**Version Note:** This feature requires Dynamo 3.0 or later
{% endhint %}

{% hint style="warning" %}
**Compatibility:** This applies to Dynamo 2.x only. See the updated approach for 3.x+
{% endhint %}

## Major Version Differences

### Dynamo 2.x → 3.x Migration
- **Runtime Change**: .NET Framework 4.8 → .NET 8
- **Package Compatibility**: Packages may need rebuilding
- **File Format**: Improved JSON format with better traceability
- **Python**: Enhanced Python support

**Learn more:** [Updating for Dynamo 3.x](11_developer_primer/3_developing_for_dynamo/6-1-updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)

### Dynamo 3.x → 4.x Migration
- **Runtime Change**: .NET 8 → .NET 10
- **Python**: PythonNet3 becomes default for all new Python nodes
- **Performance**: Significant speed improvements
- **API Cleanup**: Removal of APIs marked obsolete in 1.x

**Learn more:** [Updating for Dynamo 4.x](11_developer_primer/3_developing_for_dynamo/6-2-updating-your-packages-and-dynamo-libraries-for-dynamo-4x.md)

## Which Version Am I Using?

To find your Dynamo version:

1. **In Dynamo Sandbox**: Check the splash screen or go to Help > About
2. **In Dynamo for Revit**: Launch Dynamo and check the top-right corner of the window
3. **In Dynamo for Civil 3D**: Similar to Revit, check after launching from the Manage tab

## Getting the Right Version

### Latest Stable Release
Visit the [official download page](https://dynamobim.com/download/) for the latest stable version.

### Version-Specific Downloads
Access previous versions and development builds at [Dynamo Builds](https://dynamobuilds.com).

### Host Application Integration
| Host Application | Bundled Dynamo Version |
|-----------------|----------------------|
| Revit 2025-2026 | Dynamo 3.x |
| Revit 2027+ | Dynamo 4.x (expected) |
| Civil 3D 2025-2026 | Dynamo 3.x |
| FormIt | Check application documentation |

## Navigation Tips

### For Beginners
Start with the core sections which apply to all versions:
- [Introduction](1_introduction/README.md)
- [Setup for Dynamo](2_setup_for_dynamo/README.md)
- [User Interface](3_user_interface/README.md)
- [Nodes and Wires](4_nodes_and_wires/README.md)
- [Essential Nodes & Concepts](5_essential_nodes_and_concepts/README.md)

### For Package Developers
Focus on version-specific development guides:
- [Developing for Dynamo](11_developer_primer/3_developing_for_dynamo/README.md)
- [Updating for Dynamo 2.x](11_developer_primer/3_developing_for_dynamo/6-0-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
- [Updating for Dynamo 3.x](11_developer_primer/3_developing_for_dynamo/6-1-updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)
- [Updating for Dynamo 4.x](11_developer_primer/3_developing_for_dynamo/6-2-updating-your-packages-and-dynamo-libraries-for-dynamo-4x.md)

### For Advanced Users
Version-specific features and changes:
- [Language Changes](8_coding_in_dynamo/8-4_language-changes/1-dynamo-2.0-language-changes.md)
- [Python in Different Versions](8_coding_in_dynamo/8-3_python/README.md)

## Reporting Issues

If you encounter content that's unclear about version compatibility, please report it on our [GitHub Issues page](https://github.com/DynamoDS/DynamoPrimerNew/issues).

## Contributing

When contributing to this Primer, please:
1. Clearly mark version-specific content
2. Test instructions across multiple versions when possible
3. Include version compatibility notes in your pull requests
4. Reference this guide for formatting standards

---

**Quick Reference:**
- Dynamo 2.x: .NET 4.8, XML→JSON transition
- Dynamo 3.x: .NET 8, Python improvements  
- Dynamo 4.x: .NET 10, PythonNet3 default, performance boost

[Return to Main Page](README.md)
