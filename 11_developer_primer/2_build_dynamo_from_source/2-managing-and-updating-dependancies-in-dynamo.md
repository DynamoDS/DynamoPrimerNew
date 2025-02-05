# How To Update or Add a New Dependency to Dynamo

### When does this wiki apply
While working on a new feature, or simply updating an existing dependency you should evaluate the following before bringing a new dependency into the Dynamo repo.

### Considerations
1. What is the license of the new or updated dependency - only some open source licenses are approved without speaking with ADSK legal.
    * After resolving license, make sure the dep and version are recorded in the internal wiki.
    * If license is `LGPL`, `GPL` or `Apache`, the license file must be copied into the _"Open Source Licenses"_ sub-folder of the Dynamo build.
    * If license is `LGPL`, the full source code for all third-party components, along with text copies of their appropriate open source licenses, must be uploaded to [www.autodesk.com/lgplsource](https://www.autodesk.com/company/legal-notices-trademarks/open-source-distribution)
2. If updating, has the license type changed from the previous version?
3. Is the dependency cross platform? 
    * Does it have native components (like `CEFSharp` or `ImageMagick`)? This will make it harder to deploy cross platform
    * Does it have windows only references? If so it should not be as dependency of DynamoCore or other cross platform parts of Dynamo (The model layer).
4. Is the dependency correctly bundled into the bin folder on build with all of its required dependencies?
    * If updating, are some files removed as a consequence of updating? Is this version of Dynamo intended for a point release of host products? If so you'll need to keep the old binaries around until a global launch year to support patch installers. See [here](https://github.com/DynamoDS/Dynamo/tree/master/extern/legacy_remove_me).
5. Does the dependency or its dependency tree conflict with other existing dependencies in Dynamo?
6. ?? Does the dependency or its dependency tree conflict with existing dependencies in products that integrate Dynamo in process (Revit, Civil etc) - **This is important, as these issues can only be discovered at integration time unless work is done upfront.**