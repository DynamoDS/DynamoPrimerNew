# Migrating Node Icons for .NET 10

## Overview

Dynamo 4.0 runs on .NET 10. If your package includes node icons, you may need to update how those icons are stored. Packages that still use the old icon format may fail to build, or icons may not appear in the node library.

This guide explains what changed, why, and how to update your package.

## Why icons broke

Node icons in Dynamo packages are stored in a `*Images.resx` file, compiled into a `YourPackage.customization.dll`, and loaded at runtime by Dynamo.

Historically, many packages embedded icons as `System.Drawing.Bitmap` objects inside the `.resx` file. That format relies on `BinaryFormatter` for serialization. **`BinaryFormatter` was removed in .NET 10**, so:

- **Build time:** `GenerateResource` may fail when processing old `.resx` entries.
- **Runtime:** `ResourceManager.GetObject()` may throw when Dynamo tries to load icons.

Dynamo's built-in libraries were migrated in [#16478](https://github.com/DynamoDS/Dynamo/pull/16478) by **avoiding `BinaryFormatter` entirely** — icons are now stored as raw PNG `byte[]` data via `ResXFileRef`, not as serialized `Bitmap` objects.

There is no drop-in `BinaryFormatter` replacement. The supported approach is to migrate your icon resources to the new format.

## Who needs to act

You need to migrate if your package:

- Targets Dynamo 4.0 / .NET 10, **and**
- Has a `YourPackageImages.resx` (or similar) with entries like:

```xml
<data name="MyNode.Large" type="System.Drawing.Bitmap, System.Drawing"
      mimetype="application/x-microsoft.net.object.bytearray.base64">
  <value>...long base64 string...</value>
</data>
```

You do **not** need to change:

- String localization `.resx` files (`Properties/Resources.resx`) — these are unaffected.
- Packages with no node icons.
- The overall `*.customization.dll` pipeline — only the **content format** inside the `.resx` changes.

## How Dynamo loads icons (unchanged)

The loading pipeline is the same as before:

1. Your node library builds `YourPackage.dll`.
2. A post-build step compiles `YourPackageImages.resx` → `YourPackage.customization.dll`.
3. Both DLLs are placed in `package/bin/`.
4. Dynamo loads icons via `ResourceManager("{AssemblyName}Images", customizationAssembly)`.

What changed is the **type of data** stored in the resource assembly: `byte[]` (PNG bytes) instead of serialized `Bitmap`.

---

## Migration steps

### 1. Extract icons as PNG files

For each embedded icon in your `.resx`, save it as a standalone `.png` file. A typical layout:

```
MyPackage/
├── bin/
│   ├── MyPackage.dll
│   └── MyPackage.customization.dll   ← generated at build
├── Images/
│   ├── MyCompany.MyPackage.MyNode.Large.png
│   └── MyCompany.MyPackage.MyNode.Small.png
├── MyPackageImages.resx
└── MyPackage.csproj
```

**Naming rules:**

- Each node needs a **Small** and **Large** icon.
- Resource keys use the format `{FullyQualifiedNodeName}.Small` and `{FullyQualifiedNodeName}.Large`.
- Match the fully qualified type name Dynamo uses for the node (e.g. `MyCompany.MyPackage.Nodes.MyNode`).
- Use **exact case** in file names and `.resx` paths — Linux builds are case-sensitive.

### 2. Update the `.resx` file

Replace embedded `Bitmap` entries with `ResXFileRef` entries pointing at your PNG files.

**Before (does not work on .NET 10):**

```xml
<assembly alias="System.Drawing" name="System.Drawing, Version=4.0.0.0, ..." />
<data name="MyCompany.MyPackage.MyNode.Large"
      type="System.Drawing.Bitmap, System.Drawing"
      mimetype="application/x-microsoft.net.object.bytearray.base64">
  <value>
    iVBORw0KGgoAAAANSUhEUgAA...
  </value>
</data>
```

**After (.NET 10 compatible):**

```xml
<assembly alias="System.Windows.Forms"
          name="System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
<data name="MyCompany.MyPackage.MyNode.Large"
      type="System.Resources.ResXFileRef, System.Windows.Forms">
  <value>Images\MyCompany.MyPackage.MyNode.Large.png;System.Byte[], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
</data>
<data name="MyCompany.MyPackage.MyNode.Small"
      type="System.Resources.ResXFileRef, System.Windows.Forms">
  <value>Images\MyCompany.MyPackage.MyNode.Small.png;System.Byte[], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
</data>
```

> **Tip:** In Visual Studio, open `MyPackageImages.resx`, remove the old embedded images, then drag PNG files into the resource editor. VS generates the `ResXFileRef` entries automatically.

### 3. Confirm the build target (`.csproj`)

The customization DLL build step is unchanged. Ensure your `.csproj` still contains something like:

```xml
<Target Name="GenerateFiles" AfterTargets="ResolveSateliteResDeps"
        Condition="$(RuntimeIdentifier.Contains('win'))">
  <GenerateResource SdkToolsPath="$(TargetFrameworkSDKToolsDirectory)"
                    UseSourcePath="true"
                    Sources="$(ProjectDir)MyPackageImages.resx"
                    OutputResources="$(ProjectDir)MyPackageImages.resources" />
  <AL SdkToolsPath="$(TargetFrameworkSDKToolsDirectory)"
     TargetType="library"
     EmbedResources="$(ProjectDir)MyPackageImages.resources"
     OutputAssembly="$(OutDir)MyPackage.customization.dll"
     Version="%(AssemblyInfo.Version)" />
</Target>
```

Also update your target framework:

```xml
<TargetFramework>net10.0-windows</TargetFramework>
```

### 4. Update custom icon-loading code (if any)

Most packages rely on Dynamo's built-in icon loading and need no code changes. If your package loads icons directly from a `ResourceManager`, update the deserialization.

**Before:**

```csharp
var rm = new ResourceManager("MyPackageImages", customizationAssembly);
var bitmap = (Bitmap)rm.GetObject("MyCompany.MyPackage.MyNode.Small");
```

**After:**

```csharp
using System.Drawing;
using System.IO;
using System.Resources;

var rm = new ResourceManager("MyPackageImages", customizationAssembly);
Bitmap bitmap = null;

var imageBytes = rm.GetObject("MyCompany.MyPackage.MyNode.Small") as byte[];
if (imageBytes != null)
{
    using var ms = new MemoryStream(imageBytes);
    bitmap = new Bitmap(ms);
}
```

For base64 encoding (e.g. a web-based library view):

```csharp
byte[] imageBytes = rm.GetObject(iconName) as byte[];
string base64 = Convert.ToBase64String(imageBytes);
```

This matches how Dynamo loads icons internally as of [#16478](https://github.com/DynamoDS/Dynamo/pull/16478).

### 5. Rebuild and verify

1. Rebuild the package on the .NET 10 SDK.
2. Confirm `MyPackage.customization.dll` is produced in `bin/`.
3. Install the package in Dynamo 4.0 Sandbox.
4. Check that icons appear in the library for all nodes.
5. If you support cross-platform builds, verify PNG file names match `.resx` paths exactly (case-sensitive).

---

## Complete minimal example

**Project layout:**

```
SamplePackage/
├── Images/
│   ├── SamplePackage.HelloWorld.Large.png
│   └── SamplePackage.HelloWorld.Small.png
├── SamplePackageImages.resx
├── SamplePackage.csproj
└── HelloWorld.cs
```

**`SamplePackageImages.resx` (excerpt):**

```xml
<data name="SamplePackage.HelloWorld.Large" type="System.Resources.ResXFileRef, System.Windows.Forms">
  <value>Images\SamplePackage.HelloWorld.Large.png;System.Byte[], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
</data>
<data name="SamplePackage.HelloWorld.Small" type="System.Resources.ResXFileRef, System.Windows.Forms">
  <value>Images\SamplePackage.HelloWorld.Small.png;System.Byte[], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
</data>
```

**`SamplePackage.csproj` (excerpt):**

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net10.0-windows</TargetFramework>
    <UseWPF>true</UseWPF>
  </PropertyGroup>

  <Target Name="GenerateCustomizationDll" AfterTargets="Build"
          Condition="$(RuntimeIdentifier.Contains('win'))">
    <GenerateResource SdkToolsPath="$(TargetFrameworkSDKToolsDirectory)"
                      UseSourcePath="true"
                      Sources="$(ProjectDir)SamplePackageImages.resx"
                      OutputResources="$(ProjectDir)SamplePackageImages.resources" />
    <AL SdkToolsPath="$(TargetFrameworkSDKToolsDirectory)"
       TargetType="library"
       EmbedResources="$(ProjectDir)SamplePackageImages.resources"
       OutputAssembly="$(OutDir)SamplePackage.customization.dll"
       Version="1.0.0.0" />
  </Target>
</Project>
```

**Published package layout:**

```
SamplePackage/
├── pkg.json
├── bin/
│   ├── SamplePackage.dll
│   └── SamplePackage.customization.dll
└── extra/   (optional)
```

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|--------|--------------|-----|
| Build fails on `GenerateResource` | Old embedded `Bitmap` entries remain | Convert all icon entries to `ResXFileRef` |
| Icons missing at runtime, no build error | Stale `customization.dll` from pre-migration build | Clean and rebuild; republish package |
| Icons work on Windows, fail on Linux CI | Case mismatch between `.resx` path and PNG file name | Align casing exactly |
| `InvalidCastException` on `GetObject()` | Code still casts to `Bitmap` | Read `byte[]` and construct `Bitmap` from `MemoryStream` |
| Wrong / default icon shown | Resource key doesn't match node name | Use fully qualified node name + `.Small`/`.Large` suffix |

## What not to do

- Do not try to re-enable `BinaryFormatter` — it is removed in .NET 10 by design.
- Do not embed new icons as `System.Drawing.Bitmap` in `.resx` files.
- Do not omit the `customization.dll` from the published `bin/` folder.

## References

- [Dynamo .NET 10 migration (PR #16478)](https://github.com/DynamoDS/Dynamo/pull/16478)
- [Initial icon migration research (PR #15933)](https://github.com/DynamoDS/Dynamo/pull/15933)
- [Microsoft: BinaryFormatter removed in .NET 10](https://learn.microsoft.com/en-us/dotnet/core/compatibility/serialization/10.0/binaryformatter-removed)
- [Microsoft: Creating resource files](https://learn.microsoft.com/en-us/dotnet/framework/resources/creating-resource-files-for-desktop-apps)
- [Dynamo developer resources](https://developer.dynamobim.org/)
