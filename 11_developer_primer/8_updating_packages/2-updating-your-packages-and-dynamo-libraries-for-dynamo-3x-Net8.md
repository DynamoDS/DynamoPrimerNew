# Updating your Packages and Dynamo Libraries for Dynamo 3.x and NET8

### Introduction <a href="#introduction" id="introduction"></a>

This section contains information on issues you may encounter while migrating your graphs, packages, and libraries to Dynamo 3.x.

Dynamo 3.0 is a major release, and some APIs have been changed or removed. The biggest change that is likely to affect you as a developer or user of Dynamo 3.x is the move to .NET8.

Dotnet/.NET is the runtime that powers the C# language that Dynamo is written in. We have updated to a modern version of this runtime along with the rest of the Autodesk ecosystem.

You can read more in [our blog post](https://dynamobim.org/dynamo-on-net-8/).
***

### Package Compatibility <a href="#package-compatibility" id="package-compatibility"></a>

#### Using Dynamo 2.x Packages in Dynamo 3.x 
Because Dynamo 3.x now runs on the .NET8 runtime, packages that were built for Dynamo 2.x (*using .NET48*) are not guaranteed to work in Dynamo 3.x. When you attempt to download a package in Dynamo 3.x that was published from a Dynamo version less than 3.0, you will get a warning that the package is from an older version of Dynamo. 

**This does not mean the package will not work** It's simply a warning that there could be compatibility issues, and in general it's a good idea to check if there's a newer version that has been built specifically for Dynamo 3.x.

You may also notice this type of warning in your Dynamo log files at package load time. If everything is working correctly, you can ignore it.

#### Using Dynamo 3.x Packages in Dynamo 2.x 

It's very unlikely that a package built for Dynamo 3.x (*using .Net8*) is going to work on Dynamo 2.x. You will also see a warning when downloading packages built for newer versions of Dynamo while using an older version.


