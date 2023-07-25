# Getting Started

Now that you know a little more about the big picture, let's jump right in and build your first Dynamo graph in Civil 3D!

## Open Dynamo

The first thing to do is open up an empty document in Civil 3D. Once you're there, navigate to the **Manage** tab in the Civil 3D ribbon and look for the **Visual Programming** panel.

![](<../.gitbook/assets/image (7).png>)

Click on the **Dynamo** button, which will launch Dynamo in a separate window.

{% hint style="info" %}
**What's the difference between Dynamo and Dynamo Player?**

Dynamo is what you use to build and run graphs. Dynamo Player is an easy way to run graphs without having to open them in Dynamo.

Head to the [dynamo-player.md](dynamo-player.md "mention") section when you're ready to try it out.
{% endhint %}

## Start a New Graph

Once Dynamo is open, you'll see the start screen. Click on **New Graph** to open up a blank workspace.

<figure><img src="../.gitbook/assets/c3d-start.png" alt=""><figcaption><p>Dynamo start screen</p></figcaption></figure>

{% hint style="info" %}
**What about the samples?**

Dynamo for Civil 3D comes with a few pre-built graphs that can help spark some more ideas for how you can use Dynamo. We recommend taking a look at those at some point, as well as the [sample-workflows](sample-workflows/ "mention") here in the Primer.
{% endhint %}

## Add Nodes

You should now be looking at an empty workspace. Let's see Dynamo in action! Here's our goal:

> &#x20;:dart: **Build a Dynamo graph that will insert Text into Model Space.**

Pretty simple, right? But before we start, we need to cover a few fundamentals.

The core building blocks of a Dynamo graph are called **nodes**. A node is like a little machine - you put data into it, it does some work on that data, and it outputs a result. Dynamo for Civil 3D has a **library** of nodes that you can connect together with **wires** to form a **graph** that does bigger and better things than any one node can do by itself.

{% hint style="info" %}
**Wait, what if I've never used Dynamo before?**

Some of this might be pretty new for you, and that's OK! These sections will help.

[3\_user\_interface](../3\_user\_interface/ "mention")\
[4\_nodes\_and\_wires](../4\_nodes\_and\_wires/ "mention")\
[5\_essential\_nodes\_and\_concepts](../5\_essential\_nodes\_and\_concepts/ "mention")
{% endhint %}

OK, let's build our graph. Here's a list of all the nodes that we'll need.

<figure><img src="../.gitbook/assets/c3d-create-text-node-list.png" alt=""><figcaption></figcaption></figure>

You can find these nodes by typing their names into the search bar in the library, or by right-clicking anywhere in the canvas and searching there.

<figure><img src="../.gitbook/assets/c3d-create-text-node-placement.gif" alt=""><figcaption><p>Nodes can be placed from the library, or by right-clicking in the canvas</p></figcaption></figure>

{% hint style="info" %}
**How do I know which nodes to use and where to find them?**

Nodes in the library are grouped into logical categories based on what they do. Take a look at the [node-library.md](node-library.md "mention") section for a more in-depth tour.
{% endhint %}

Here's what your final graph should look like.

<figure><img src="../.gitbook/assets/c3d-text-create-final (2).png" alt=""><figcaption><p>The finished graph</p></figcaption></figure>

Let's summarize what we've done here:

> 1. We chose which Document to work in. In this case (and many cases), we want to work in the active Document in Civil 3D.
> 2. We defined the destination Block where our Text object should be created (Model Space in this case).
> 3. We used a _String_ node to specify which layer the Text should be placed on.
> 4. We created a point using the _Point.ByCoordinates_ node to define the position where the Text should be placed.
> 5. We defined the X and Y coordinates of the Text insertion point using two _Number Slider_ nodes.
> 6. We used another _String_ node to define the contents of the Text object.
> 7. And finally, we created the Text object.

Let's see the results of our shiny new graph!

## See the Result

Back in Civil 3D, make sure that the **Model** tab is selected. You should see the new Text object that Dynamo created.

{% hint style="info" %}
If you can't see the Text, you may need to run the ZOOM -> EXTENTS command to zoom to the right spot.
{% endhint %}

<figure><img src="../.gitbook/assets/c3d-create-text-result.png" alt="" width="413"><figcaption></figcaption></figure>

Cool! Now let's make some updates to the Text.

Back in your Dynamo graph, go ahead and change a few of the input values, such as the text string, insertion point coordinates, etc. You should see the Text automatically update in Civil 3D. Also notice that if you unplug one of the input ports, the Text is removed. If you plug everything back in, the Text is created again.&#x20;

<div data-full-width="false">

<figure><img src="../.gitbook/assets/c3d-create-text.gif" alt=""><figcaption><p>The finished graph in action</p></figcaption></figure>

</div>

{% hint style="info" %}
**Why doesn't Dynamo insert a new Text object every time the graph is run?**

By default, Dynamo will "remember" the objects that it creates. If you change the node input values, the objects in Civil 3D are updated instead of creating brand new objects. You can read more about this behavior in the [object-binding.md](advanced-topics/object-binding.md "mention") section.
{% endhint %}

> :tada: Mission accomplished!

## Next Steps

This example just scratches the surface of what you can do with Dynamo for Civil 3D. Keep reading to learn more!
