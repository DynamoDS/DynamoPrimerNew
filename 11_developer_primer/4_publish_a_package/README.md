# Publish a Package

### Publish a Package <a href="#publish-a-package" id="publish-a-package"></a>

Packages are a convenient way to store and share nodes with the Dynamo community. A package can contain everything from Custom Nodes created in the Dynamo workspace to NodeModel derived nodes. Packages are published and installed using the Package Manager. In addition to this page the [Primer](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/1-introduction) has a general guide on packages.

#### What is a Package Manager? <a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

The Dynamo Package Manager is a Software Registry (similar to npm) that can be accessed from Dynamo or in a web browser. The Package Manager includes installing, publishing, updating, and viewing packages. Like npm, it maintains different versions of packages. It also helps to manage the dependencies of your project.

In the browser, search for packages and view statistics: [https://dynamopackages.com/](https://dynamopackages.com)

* In Dynamo, the Package Manager includes install, publish, and update packages.

![Searching for packages](images/dynamopackagemanager.jpg)

> 1. Search for packages online: `Packages > Search for a Package...`
> 2. View/edit installed packages: `Packages > Manage Packages...`
> 3. Publish a new package: `Packages > Publish New Package...`

#### Publishing a package <a href="#publishing-a-package" id="publishing-a-package"></a>

Packages are published from Package Manager within Dynamo. The recommended process is to publish locally, test the package and then publish online to share with the community. Using the NodeModel Case Study, we will go through the necessary steps to publish the RectangularGrid node as a package locally and then on online.

Start Dynamo and select `Packages > Publish New Package...` to open the `Publish a Package` window.

![Publishing a package](images/dyn-publish-package-add-files.jpg)

> 1. Select `Add file...` to browse for files to add to the package
> 2. Select the two `.dll` files from the NodeModel Case Study
> 3. Select `Ok`

With the files added to the package contents, give the package a name, description, and version. Publishing a package using Dynamo automatically creates a `pkg.json` file.

![Package settings](images/dyn-publish-package.jpg)

> A package ready to be published.
>
> 1. Supply the required information for name, description, and version.
> 2. Publish by clicking "Publish Locally" and select Dynamo's package folder: `AppData\Roaming\Dynamo\Dynamo Core\1.3\packages` to have the node available in Core. Always publish locally until the package is ready to share.

After publishing a package, the nodes will be available in the Dynamo Library under the category`CustomNodeModel`.

![Package in Dynamo Library](images/dyn-publish-package-library.jpg)

> 1. The package we just created in the Dynamo Library

Once the package is ready to publish online, open the Package Manager and choose `Publish` and then `Publish Online`.

![Publish a package in Package Manager](images/dyn-publish-package-directory.jpg)

> 1. To see how Dynamo has formatted the package, click on the three vertical dots to the right of "CustomNodeModel" and choose "Show Root Directory"
> 2. Select `Publish` then `Publish Online` in the "Publish a Dynamo Package" window.
> 3. To delete a package, select `Delete`.

#### How do I update a package? <a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

Updating a package is a similar process to publishing. Open the Package Manager and select `Publish Version...` on the package that needs to be updated and enter a higher version.

![Publish a package version](images/dyn-publish-package-version.jpg)

> 1. Select `Publish Version` to update an existing package with new files in the root directory, then choose whether it should be published locally or online.

#### Package Manager web client <a href="#package-manager-web-client" id="package-manager-web-client"></a>

The Package Manager web client allows users to search for and view package data, including versioning, download statistics, and other relevant information. Additionally, package authors can log in to update their package details, such as compatibility information, directly through the web client.

For more information about these features, see the blog post here: [https://dynamobim.org/discover-the-new-dynamo-package-management-experience/](https://dynamobim.org/discover-the-new-dynamo-package-management-experience/).

The Package Manager web client can be accessed at this link: [https://dynamopackages.com/](https://dynamopackages.com)

![Package manager web client](images/packagemanager-browser.jpg)

##### Updating Package Details

Authors can edit their package description, website link, and repository link by following these steps:  

> 1. Under **My Packages**, select the package and click **Edit package details**.  
> 2. Add or modify the **Website** and **Repository** links using the respective fields.  
> 3. Update the **Package Description** as needed.  
> 4. Click **Save changes** to apply updates.  

 **Note**: Updates may take up to 15 minutes to refresh in the Package Manager within Dynamo, as server updates take some time. Efforts are underway to reduce this delay.  

 ![New UI to update Package Details for Published Packages](images/Package-Manager_Image_5.png)

##### Edit Compatibility Information for Published Package Versions  

Compatibility information can be updated retroactively for previously published package versions. Follow these steps:  

![Edit Compatibility Info for Published Packages – Step 1](images/Package-Manager_Image_6.png)

**Step 1:**  

1. Click on the package version you want to update.  
2. The **Depends on** list will be auto-populated with the packages your package depends on.  
3. Click the pencil icon next to **Compatibility** to open the **Edit Compatibility Information** workflow.  

**Step 2:**  

Follow the below flow chart and refer to the table below to help you understand which option works best for your package.

![Which Option to Choose for “Edit Compatibility Info” workflow](images/Package-Manager_Image_7.png)

Let’s use some examples to walk through some scenarios:

**Example Package # 1**- Civil Connection: This package has API dependencies with both Revit & Civil 3D and does not include a collection of core nodes (eg: geometry functions, math functions, and/or list management). So, in this case, the ideal option would be to go with Option 1. The package will be shown as Compatible in Revit and Civil 3D that matches the version range and/or individual version list.

**Example Package # 2** – Rhythm: This package is a collection of Revit specific nodes along with a collection of Core nodes. In this case, the package has host dependencies. But also includes core nodes that will work in Dynamo Core. So, in this case, the ideal option would be Option 2. The package will be shown as Compatible in Revit and Dynamo Core (also called Dynamo Sandbox) environment that matches the version range and/or individual version list.

**Example Package # 3** – Mesh Toolkit: This package is a Dynamo Core package which is a collection of geometry nodes that has no host dependencies. So, in this case, the ideal option would be Option 3. The package will be shown as Compatible in Dynamo and all host environments that matches the version range and/or individual version list.

![Edit Compatibility Info Options](images/Package-Manager_Image_8.png)

Depending on the option selected, Dynamo and/or Host specific fields will pop-up as shown in the image below.

![Edit Compatibility Info – Step 2](images/Package-Manager_Image_9.png)