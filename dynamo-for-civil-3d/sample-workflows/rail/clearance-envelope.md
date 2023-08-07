# Clearance Envelope

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption></figcaption></figure>

Developing kinematic envelopes for clearance validation is an important part of rail design. Dynamo can be used to generate solids for the envelope instead of creating and managing complex Corridor subassemblies to do the job.

## Goal

> :dart: Use a vehicle profile Block to generate clearance envelope 3D solids along a Corridor.

## Key Concepts

> * Working with Corridor Feature Lines
> * Transforming geometry between Coordinate Systems
> * Creating solids by lofting
> * Controlling node behavior with lacing settings

## Version Compatibility

{% hint style="success" %}
This graph will run on **Civil 3D 2020** and above.
{% endhint %}

## Dataset

Start by downloading the sample files below and then opening the DWG file and Dynamo graph.

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope (1).dyn" %}

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dwg" %}

## Solution

Here's an overview of the logic in this graph.

> 1. Get Feature Lines from the specified Corridor Baseline
> 2. Generate Coordinate Systems along the Corridor Feature Line at the desired spacing
> 3. Transform the profile Block geometry to the Coordinate Systems
> 4. Loft a solid between the profiles
> 5. Create the solids in Civil 3D

Let's go!

### Get Corridor Data

Our first step is to get Corridor data. We'll select the Corridor model by its name, get a specific Baseline within the Corridor, and then get a Feature Line within the Baseline by its point code.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_GetCorridorData.png" alt=""><figcaption><p>Selecting the Corridor, baseline, and feature line</p></figcaption></figure>

### Generate Coordinate Systems

What we're going to do now is generate **Coordinate Systems** along the Corridor Feature Lines between a given start and end station. These Coordinate Systems will be used to align the vehicle profile Block geometry to the Corridor.

{% hint style="info" %}
If Coordinate Systems are new to you, take a look at the [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") section.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_CreateCoordinateSystems.png" alt=""><figcaption><p>Getting Coordinate Systems along the Corridor Feature Lines</p></figcaption></figure>

> 1. Notice the little **XXX** in the bottom-right corner of the node. This means that the node's lacing settings are set to _Cross Product_, which is necessary to generate the Coordinate Systems at the same station values for both Feature Lines.

{% hint style="info" %}
If node lacing is new to you, take a look at the [1-whats-a-list.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md "mention") section.
{% endhint %}

### Transform Block Geometry

Now we need to somehow create an array of the vehicle profiles along the Feature Lines. What we're going to do is transform the geometry from the vehicle profile Block definition using the **Geometry.Transform** node. This is a tricky concept to visualize, so before we look at the nodes, here's a graphic that shows what is going to happen.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_TransformAnimation.gif" alt=""><figcaption><p>A visualization of transforming geometry between Coordinate Systems.</p></figcaption></figure>

So essentially we're taking the Dynamo geometry from a _single_ Block definition and moving/rotating it, all while creating an array along the Feature Line. Cool stuff! Here's what the node sequence looks like.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Transform.png" alt=""><figcaption></figcaption></figure>

> 1. This gets the Block definition from the Document.
> 2. These nodes get the Dynamo geometry of the Objects within the Block.
> 3. These nodes essentially define the Coordinate System that we are transforming the geometry _from._
> 4. And finally, this node does the actual work of transforming the geometry.
> 5. Note the _Longest_ lacing on this node.

And here's what we get in Dynamo.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Profiles.png" alt=""><figcaption><p>The vehicle profile Block geometry after transforming</p></figcaption></figure>

### Generate Solids

Good news! The hard work is done. All we need to do now is generate solids between the profiles. This is easily accomplished with the **Solid.ByLoft** node.

<figure><img src="../../../.gitbook/assets/Rail_PlaceTies_SolidByLoft.png" alt="" width="325"><figcaption></figcaption></figure>

And here's the result. Remember that these are Dynamo solids - we still need to create them in Civil 3D.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Solids.png" alt=""><figcaption><p>The Dynamo solids after lofting</p></figcaption></figure>

### Output Solids to Civil 3D

Our final step is to output the generated solids into Model Space. We'll also give them a color to make them very easy to see.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_SolidsToC3D.png" alt=""><figcaption><p>Outputting the solids to Civil 3D</p></figcaption></figure>

### Result

Here's an example of running the graph using **Dynamo Player**.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption><p>Running the graph using Dynamo Player and seeing the results in Civil 3D</p></figcaption></figure>

{% hint style="info" %}
If Dynamo Player is new to you, take a look at the [dynamo-player.md](../../dynamo-player.md "mention") section.
{% endhint %}

> :tada: Mission accomplished!

## Ideas

Here are some ideas for how you could expand the capabilities of this graph.

{% hint style="info" %}
Add the ability to use **different station ranges** separately for each track.
{% endhint %}

{% hint style="info" %}
**Split the solids** into smaller segments that could be analyzed individually for clashes.
{% endhint %}

{% hint style="info" %}
Check to see if the envelope solids **intersect with features** and color those that clash.
{% endhint %}
