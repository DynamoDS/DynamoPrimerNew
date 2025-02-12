# Package Introduction

Dynamo offers a vast number of features out of the box and also maintains an extensive package library that can significantly extend Dynamo’s capability. A package is a collection of custom nodes or additional functionality. The Dynamo Package Manager is a portal for the community to download any package that has been published online. These toolsets are developed by third parties to extend Dynamo's core functionality, accessible to all, and ready to download at the click of the button.

![Package Manager Site](../images/6-2/1/dpm.jpg)

An open-source project such as Dynamo thrives on this type of community involvement. With dedicated third-party developers, Dynamo is able to extend its reach to workflows across a range of industries. For this reason, the Dynamo team has made concerted efforts to streamline package development and publishing (which will be discussed in more detail in the following sections).

### Installing a Package

The easiest way to install a package is by using the Packages menu option in your Dynamo interface. Let's jump right into it and install a package now. In this quick example, we'll install a popular package for creating quad panels on a grid.

In Dynamo, go to _Packages > Package Manager..._

<figure><img src="../../.gitbook/assets/package-manager-menu.png" alt=""><figcaption></figcaption></figure>

In the search bar, let's search for "quads from rectangular grid". After a few moments, you should see all of the packages which match this search query. We want to select the first package with the matching name.

Click Install to add this package to your library, then accept the confirmation. Done!

<figure><img src="../../.gitbook/assets/quads-from-rectangular-grid.png" alt=""><figcaption></figcaption></figure>

Notice that we now have another group in our Dynamo library called "buildz". This name refers to the developer of the package, and the custom node is placed in this group. We can begin to use this right away.

![](../images/6-2/1/packageintroduction-installingapackage03.jpg)

Use **Code Block** to quickly define a rectangular grid, output the result to a **Polygon.ByPoints** Node, subsequently a **Surface.ByPatch** Node to view the list of rectangular panels you have just created.

![](../images/6-2/1/packageintroduction-installingapackage04.jpg)

### Installing Package Folder - DynamoUnfold

The example above focuses on a package with one custom node, but you use the same process for downloading packages with several custom nodes and supporting data files. Let's demonstrate that now with a more comprehensive package: Dynamo Unfold.

As in the example above, begin by selecting _Packages > Package Manager.._.

This time, we'll search for _"DynamoUnfold"_, one word. When we see the packages, download by clicking on Install to add Dynamo Unfold to your Dynamo Library.

<figure><img src="../../.gitbook/assets/unfold.png" alt=""><figcaption></figcaption></figure>

In the Dynamo Library, we have a _DynamoUnfold_ Group with multiple categories and custom nodes.

![](../images/6-2/1/packageintroduction-installingpackagefolder02.jpg)

Now, let's take a look at the package's file structure.&#x20;

1. First, go to Packages > Package Manager > Installed Packages.
2. Next to DynamoUnfold, select the options menu <img src="../images/6-2/1/packageintroduction-verticaldotsmenu.jpg" alt="" data-size="line">.
3. Then, click Show Root Directory to open the root folder for this package.

<figure><img src="../../.gitbook/assets/view-root-directory.png" alt=""><figcaption></figcaption></figure>

This will take us to the package's root directory. Notice that we have 3 folders and a file.

![](../images/6-2/1/packageintroduction-installingpackagefolder05.jpg)

> 1. The _bin_ folder houses .dll files. This Dynamo package was developed using Zero-Touch, so the custom nodes are held in this folder.
> 2. The _dyf_ folder houses the custom nodes. This package was not developed using Dynamo custom nodes, so this folder is empty for this package.
> 3. The extra folder houses all additional files, including our example files.
> 4. The pkg file is a basic text file defining the package settings. We can ignore this for now.

Opening the "extra" folder, we see a bunch of example files that were downloaded with the install. Not all packages have example files, but this is where you can find them if they are part of a package.

Let's open up "SphereUnfold".

![](../images/6-2/1/rd2.jpg)

After opening the file and hitting "Run" on the solver, we have an unfolded sphere! Example files like these are helpful for learning how to work with a new Dynamo package.

![](<../images/6-2/1/packageintroduction-installingpackagefolder07 (1) (2).jpg>)

### Browsing and Viewing Package Information

In the Package Manager, you can browse for packages by using the sorting and filtering options in the Search for Packages tab. There are several filters available for host program, status (new, deprecated, or undeprecated), and whether or not the package has dependencies.

By sorting the packages, you can identify highly rated or most downloaded packages, or find packages with recent updates.&#x20;

You can also access more detail on each package by clicking View Details. This opens a side panel in the Package Manager, where you can find information such as versioning and dependencies, website or repository URL, license information, etc.

### Dynamo Package Manager Website

Another way to discover Dynamo packages is to explore the [Dynamo Package Manager](http://dynamopackages.com) website. Here, you can find package dependencies and host/version compatibility information provided by Package Authors. You can also download the package files from the Dynamo Package Manager, but doing so directly from Dynamo is a more seamless process.

![](../images/6-2/1/dpm2.jpg)

### Where are Packages Files Stored Locally?

If you would like to see where your package files are kept, in the top navigation click on Dynamo > Preferences > Package Settings > Node and Package File Locations, you can find your current root folder directory from here.

![](../images/6-2/1/packageintroduction-installingpackagefolder08.jpg)

By default, packages are installed in a location similar to this folder path: _C:/Users/\[username]/AppData/Roaming/Dynamo/\[Dynamo Version]_.

### Setting up a Shared Location for Packages in an Office

For users who are asking if it is possible to deploy Dynamo (in any form) with pre-attached packages:
The approach that will solve this issue and allow for control at a central location for all users with Dynamo installs is to add a custom package path to each installation.

**Adding a network folder where the BIM manager or others could supervise the stocking of the folder with office approved packages**  

In the UI of an individual application, go to *Dynamo -> Preferences -> Package Settings -> Node and Package file locations*.  In the dialog, press the "Add Path" button and browse to the network location for the shared package resource. 
 
As an automated process, it would involve adding information to the configuration file that is installed with Dynamo:  
`C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]\DynamoSettings.xml`

By default, the configuration for Dynamo for Revit is:
 
`<CustomPackageFolders>`  

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`  

`</CustomPackageFolders>`

Adding a custom location would look like:  

`<CustomPackageFolders>`  

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`  

`<string>N:\OfficeFiles\Dynamo\Packages_Limited</string>`  

`</CustomPackageFolders>`

The central management for this folder can also be controlled by simply making the folder read only.

### Loading Packages with Binaries from a Network Location

#### Scenario

An organization might want to standardize the packages installed by different workstations and users. A way to do this, could be to install these packages from *Dynamo -> Preferences -> Package Settings -> Node and Package file locations*, selecting a network folder as the install location, and get workstations to add that path to `Manage Node and Package Paths`.

#### Problem

While the scenario works properly for packages that contain only custom nodes, it might not work for packages containing binaries, like zero-touch nodes. This issue is caused by [security measures](https://stackoverflow.com/questions/5328274/load-assembly-from-network-location) the .NET framework places over loading assemblies when they come from a network location. Unfortunately, using the `loadFromRemoteSources` configuration element, as suggested in the linked thread, is not a possible solution for Dynamo, because it is distributed as a component rather than an application.

#### Workaround

One possible workaround is to use a mapped network drive pointing to the network location, and have workstations reference that path instead. The steps to create a mapped network drive are described [here](https://support.microsoft.com/en-us/help/4026635/windows-10-map-a-network-drive).

### Going Further with Packages

The Dynamo community is constantly growing and evolving. By exploring the Dynamo Package Manager from time to time, you'll find some exciting new developments. In the following sections, we'll take a more in-depth look at packages, from the end-user perspective to authorship of your own Dynamo Package.
