# Rename Structures

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

When adding Pipes and Structures to a Pipe Network, Civil 3D uses a template to automatically assign names. This is usually sufficient during initial placement, but inevitably the names will have to change in the future as the design evolves. In addition, there are many different naming patterns that might be required, for example naming Structures sequentially within a pipe run starting from the furthest downstream Structure, or following a naming pattern that aligns with a local agency's data schema. This example will demonstrate how Dynamo can be used to define any type of naming strategy apply it consistently.

## Goal

> :dart: Rename Pipe Network Structures in order based on the stationing of an Alignment.

## Key Concepts

> * Working with Bounding Boxes
> * Filtering data using the **List.FilterByBoolMask** node
> * Sorting data using the **List.SortByKey** node
> * Generating and modifying text strings

## Version Compatibility

{% hint style="success" %}
This graph will run on **Civil 3D 2020** and above.
{% endhint %}

## Dataset

Before getting started, download the sample files below.

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## Solution

Here's an overview of the logic in this graph.

> 1. Select the Structures by layer
> 2. Get the Structure locations
> 3. Filter the Structures by offset, then sort them by station
> 4. Generate the new names
> 5. Rename the Structures

Let's go!

### Select Structures

The first thing we need to do is select all of the Structures that we plan to work with. We'll do this by simply selecting all of the objects on a certain layer, which means that we can select Structures from different Pipe Networks (assuming they share the same layer).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>Selecting the Structures on a given layer</p></figcaption></figure>

> 1. This node ensures that we don't accidentally retrieve any undesirable object types that might share the same layer as the Structures.

### Get Structure Locations

Now that we have the Structures, we need to figure out their position in space so that we can sort them according to their location. To do this, we'll take advantage of the Bounding Box of each object. The **Bounding Box** of an object is the minimum-sized box that fully contains the geometric extents of the object. By computing the center of the Bounding Box, we get a pretty good approximation of the Structure's insertion point.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>Using Bounding Boxes to get the approximate insertion point of each Structure</p></figcaption></figure>

We'll use these points to get the station and offset of the Structures relative to a selected Alignment.

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### Filter and Sort

Here's where things start to get a little tricky. At this stage, we have a big list of all the Structures on the layer that we specified, and we chose an Alignment that we wanted to sort them along. The trouble is that there may be Structures in the list that we don't want to rename. For example, they may not be part of the particular run that we are interested in.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. The selected Alignment
> 2. The Structures that we want to rename
> 3. The Structures that should be ignored

So, we need to filter the list of Structures so that we don't consider those that are greater than a certain offset from the Alignment. This is best accomplished using the **List.FilterByBoolMask** node. After filtering the list of Structures, we use the **List.SortByKey** node to sort them by their station values.

{% hint style="info" %}
If you're new to working with Lists, take a look at the [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention") section.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>Filtering and sorting the Structures</p></figcaption></figure>

> 1. Check to see if the Structure's offset is less than the threshold value
> 2. Replace any null values with _false_
> 3. Filter the list of Structures and stations
> 4. Sort the Structures by the stations

### Generate New Names

The last bit of work that we need to do is create the new names for the Structures. The format that we'll use is `<alignment name>-STRC-<number>`. There's a few extra nodes here to pad the numbers with extra zeros if desired (e.g., "01" instead of "1").

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>Generating the new Structure names</p></figcaption></figure>

### Rename Structures

And, last but not least, we rename the Structures.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>Set the names for the Structures</p></figcaption></figure>

### Result

Here's an example of running the graph using **Dynamo Player**.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>Running the graph using Dynamo Player and seeing the results in Civil 3D</p></figcaption></figure>

{% hint style="info" %}
If Dynamo Player is new to you, take a look at the [dynamo-player.md](../../dynamo-player.md "mention") section.
{% endhint %}

> :tada: Mission accomplished!

### Bonus: Visualizing in Dynamo

It can be helpful to take advantage of Dynamo's 3D background preview to visualize the graph's intermediate outputs instead of just the final result. One easy thing we can do is show the Bounding Boxes for the Structures. In addition, this particular dataset has a Corridor in the document, so we can bring the Corridor Feature Line geometry into Dynamo to provide some context for where the Structures are located in space. If the graph is used on a dataset that doesn't have any Corridors, then these nodes will simply do nothing.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>Visualizing the geometry for Structures and Corridor Feature Lines</p></figcaption></figure>

Now we can better understand how the process of filtering the Structures by offset works.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>Adjusting the Alignment offset threshold value and visualizing the affected Structures in Dynamo</p></figcaption></figure>

## Ideas

Here are some ideas for how you could expand the capabilities of this graph.

{% hint style="info" %}
Rename the structures based on their **closest Alignment** rather than selecting a specific Alignment.
{% endhint %}

{% hint style="info" %}
**Rename the Pipes** in addition to the Structures.
{% endhint %}

{% hint style="info" %}
**Set the layers** of the Structures based on their run.
{% endhint %}
