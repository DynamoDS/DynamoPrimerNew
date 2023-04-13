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

The Package Manager web client is used exclusively for searching and viewing package data such as versioning and download statistics.

The Package Manager web client can be accessed at this link: [https://dynamopackages.com/](https://dynamopackages.com)

![Package manager web client](images/packagemanager-browser.jpg)
