# Point Group Management

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

Working with COGO Points and Point Groups in Civil 3D is a core element of many field-to-finish processes. Dynamo really shines when it comes to data management, and we'll demonstrate one potential use case in this example. &#x20;

## Goal

> :dart: Create a Point Group for each unique COGO Point description.&#x20;

## Key Concepts

> * Working with Lists
> * Grouping similar objects with the **List.GroupByKey** node
> * Showing custom output in Dynamo Player

## Version Compatibility

{% hint style="success" %}
This graph will run on **Civil 3D 2020** and above.
{% endhint %}

## Dataset

Start by downloading the sample files below and then opening the DWG file and Dynamo graph.

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## Solution

Here's an overview of the logic in this graph.

> 1. Get all of the COGO Points in the Document
> 2. Group the COGO Points by description
> 3. Create Point Groups
> 4. Output a summary to Dynamo Player

Let's go!

### Get COGO Points

Our first step to get all of the Point Groups in the Document, then get all of the COGO Points within each group. This will give us a _nested list_ or "list of lists," which will be easier to work with later if we flatten everything down to a single list with the **List.Flatten** node.

{% hint style="info" %}
If you're new to working with Lists, take a look at the [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention") section.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>Get all Point Groups and COGO Points </p></figcaption></figure>

### Group Points by Description

Now that we have all the COGO Points, we need to separate them into groups based on their descriptions. This is exactly what the **List.GroupByKey** node does. It essentially groups together any items that share the same key.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>Grouping the COGO Points by description</p></figcaption></figure>

### Create Point Groups

The hard work is done! The final step is to create new Civil 3D Point Groups from the grouped COGO Points.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>Create new Point Groups</p></figcaption></figure>

### Output Summary

When you run the graph, there's nothing to see in the Dynamo background preview because we aren't working with any geometry. So the only way to see if the graph executed properly is to check the Toolspace, or to look at the node output previews. However, if we run the graph using **Dynamo Player**, then we can provide more feedback about the graph's results by outputting a summary of the Point Groups that were created. All you have to do is right-click on a node and set it to _Is Output_. In this case, we use a renamed **Watch** node to view the results.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>Setting a node to <em>Is Output</em> will display its contents in the Dynamo Player output</p></figcaption></figure>

### Result

Here's an example of running the graph using **Dynamo Player**.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>Running the graph using Dynamo Player and seeing the results in the Toolspace</p></figcaption></figure>

{% hint style="info" %}
If Dynamo Player is new to you, take a look at the [dynamo-player.md](../../dynamo-player.md "mention") section.
{% endhint %}

> :tada: Mission accomplished!

## Ideas

Here are some ideas for how you could expand the capabilities of this graph.

{% hint style="info" %}
Modify the point grouping to be based on **full description** instead of raw description.
{% endhint %}

{% hint style="info" %}
Group the points by some other **pre-defined categories** that you choose (e.g., "Ground shots," "Monuments," etc.)
{% endhint %}

{% hint style="info" %}
Automatically create TIN Surfaces for points in certain groups.
{% endhint %}
