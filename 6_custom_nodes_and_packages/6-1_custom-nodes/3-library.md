# Publishing to Your Library

We've just created a custom node and applied it to a specific process in our Dynamo graph. And we like this node so much, we want to keep it in our Dynamo library to reference in other graphs. To do this, we'll publish the node locally. This is a similar process to publishing a package, which we'll walk through in more detail in the next chapter.

By publishing a node locally, the node will be accessible in your Dynamo library when you open a new session. Without publishing a node, a Dynamo graph which references a custom node must also have that custom node in its folder (or the custom node must be imported into Dynamo using _File > Import Library_).

{% hint style="warning" %}
You can publish custom nodes and packages from Dynamo Sandbox in version 2.17 and newer, as long as they have no host API dependencies. In older versions, publishing custom nodes and packages is only enabled in Dynamo for Revit and Dynamo for Civil 3D.&#x20;
{% endhint %}

## Exercise: Publishing a Custom Node Locally

> Download the example file by clicking on the link below.
>
> A full list of example files can be found in the Appendix.

{% file src="../datasets/6-1/3/PointsToSurface.dyf" %}

Let's move forward with the custom node that we created in the previous section. Once the PointsToSurface custom node is opened, we see the graph in the Dynamo Custom Node Editor. You can also open up a custom node by double clicking on it in the Dynamo Graph Editor.

![](../images/6-1/3/publishcustomnodelocally01.jpg)

To Publish a custom node locally, simply right click on the canvas and select _"Publish This Custom Node..."_

![](../images/6-1/3/publishcustomnodeexercise-02.jpg)

Fill out the relevant information similar to the image above and select _"Publish Locally"._ Note that the Group field defines the main element accessible from the Dynamo menu.

<figure><img src="../../.gitbook/assets/publish_a_package.png" alt=""><figcaption></figcaption></figure>

Choose a folder to house all of the custom nodes that you plan on publishing locally. Dynamo will check this folder each time it loads, so make sure the folder is in a permanent place. Navigate to this folder and choose _"Select Folder"._ Your Dynamo node is now published locally, and will remain in your Dynamo library each time you load the program!

![](../images/6-1/3/publishcustomnodeexercise-04.jpg)

To check on the custom node folder location, go to _Dynamo > Preferences > Package Settings > Node and Package Paths._

<figure><img src="../../.gitbook/assets/settings.png" alt="" width="520"><figcaption></figcaption></figure>

In this window we see a list of paths.

<figure><img src="../../.gitbook/assets/package-locations.png" alt=""><figcaption></figcaption></figure>

> 1. _Documents\DynamoCustomNodes..._ refers to the location of custom nodes we've published locally.
> 2. _AppData\Roaming\Dynamo..._ refers to the default location of Dynamo Packages installed online.
> 3. You may want to move your local folder path down in the list order (by clicking on the down arrow to the left of the path name). The top folder is the default path for package installs. So by keeping the default Dynamo package install path as the default folder, online packages will be separated from your locally published nodes.

We switched the order of the path names in order to have Dynamo's default path as the package install location.

<figure><img src="../../.gitbook/assets/updated-package-locations.png" alt=""><figcaption></figcaption></figure>

Navigating to this local folder, we can find the original custom node in the _".dyf"_ folder, which is the extension for a Dynamo Custom Node file. We can edit the file in this folder and the node will update in the UI. We can also add more nodes to the main _DynamoCustomNode_ folder and Dynamo will add them to your library at restart!

![](../images/6-1/3/publishcustomnodeexercise-08.jpg)

Dynamo will now load each time with "PointsToSurface" in the "DynamoPrimer" group of your Dynamo library.

![](../images/6-1/3/publishcustomnodeexercise-09.jpg)
