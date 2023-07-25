# Service Placement

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

The engineering design of a typical housing development involves working with several underground utilities, such as sanitary sewer, storm drainage, potable water, or others. This example will demonstrate how Dynamo can be used to draw the service connections from a distribution main to a given lot (i.e., parcel). It is common for every lot to require a service connection, which introduces significant tedious work to place all of the services. Dynamo can speed up the process by automatically drawing the necessary geometry with precision, as well as providing flexible inputs that can be adjusted to suit local agency standards.

## Goal

> :dart: Place water service meter Block References at specified offsets from a lot line, and draw a Line for each service connection perpendicular to the distribution main.

## Key Concepts

> * Using the **Select Object** node for user input
> * Working with Coordinate Systems
> * Using geometric operations like **Geometry.DistanceTo** and **Geometry.ClosestPointTo**
> * Creating Block References
> * Controlling object binding settings

## Version Compatibility

{% hint style="success" %}
This graph will run on **Civil 3D 2020** and above.
{% endhint %}

## Dataset

Before getting started, download the sample files below.

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## Solution

Here's an overview of the logic in this graph.

> 1. Get the curve geometry for the distribution main
> 2. Get the curve geometry for a user-selected lot line, reversing if necessary
> 3. Generate the insertion points for the service meters
> 4. Get the points on the distribution main that are closest to the service meter locations
> 5. Create Block References and Lines in Model Space

Let's go!

### Get Distribution Main Geometry

Our first step is to get the geometry for the distribution main into Dynamo. Instead of selecting individual Lines or Polylines, we'll instead get all of the objects on a certain layer and join them together as a Dynamo PolyCurve.

{% hint style="info" %}
If Dynamo curve geometry is new to you, take a look at the [4-curves.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "mention") section.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>Getting the objects from Civil 3D and joining everything together into a single PolyCurve</p></figcaption></figure>

### Get Lot Line Geometry

Next, we need to get the geometry for a selected lot line into Dynamo so we can work with it. The right tool for the job is the **Select Object** node, which allows the user of the graph to pick a specific object in Civil 3D.

We also need to handle a potential issue that may arise. The lot line has a start point and an end point, which means that it has a direction. In order for the graph to produce consistent results, we need all of the lot lines to have a consistent direction. We can account for this condition directly in the graph logic, which makes the graph more resilient.&#x20;

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>Selecting a lot line and ensuring that it is the right direction</p></figcaption></figure>

> 1. Get the start and end points of the lot line.
> 2. Measure the distance from each point to the distribution main, then figure out which distance is greater.
> 3. The desired result is that the start point of the line is closest to the distribution main. If that isn't then case, then we reverse the direction of the lot line. Otherwise we simply return the original lot line.

### Generate Insertion Points

It's time to figure out where the service meters are going to be placed. Typically the placement is determined by local agency requirements, so we'll just provide input values that can be changed to suit various conditions. We're going to use a **Coordinate System** along the lot line as the reference for creating the points. This makes it really easy to define offsets relative to the lot line, not matter its orientation.

{% hint style="info" %}
If Coordinate Systems are new to you, take a look at the [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") section.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>Creating the insertion points for the service meters</p></figcaption></figure>

### Get Connection Points

Now we need to get points on the distribution main that are closest to the service meter locations. This will allow us to draw the service connections in Model Space so that they are always perpendicular to the distribution main. The **Geometry.ClosestPointTo** node is the perfect solution.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>Getting perpendicular points on the distribution main</p></figcaption></figure>

> 1. This is the distribution main PolyCurve
> 2. These are the service meter insertion points

### Create Objects

The last step is to actually create objects in Model Space. We'll use the insertion points that we generated previously to create Block References, and then we'll use the points on the distribution main to draw Lines to the service connections.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### Result

When you run the graph you should see new Block References and service connection lines in Model Space. Try changing some of the inputs and watching everything update automatically!

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>Adjusting the input parameters in Dynamo and immediately seeing the results in Civil 3D</p></figcaption></figure>

### Bonus: Enable Sequential Placement

You may notice that after placing the objects for one lot line, selecting a different lot line results in the objects being "moved."

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>Behavior when object binding is turned on</p></figcaption></figure>

This is Dynamo's default behavior, and it is very useful in many cases. However, you may find want to place several service connections sequentially and have Dynamo create new objects with each run instead of modifying the original ones. You can control this behavior by changing the object binding settings.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Dynamo's object binding sesttings</p></figcaption></figure>

{% hint style="info" %}
Take a look at the [object-binding.md](../../advanced-topics/object-binding.md "mention") section for more information.
{% endhint %}

Changing this setting will force Dynamo to "forget" the objects that it creates with each run. Here's an example of running the graph with object binding turned off using **Dynamo Player**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>Running the graph using Dynamo Player and seeing the results in Civil 3D</p></figcaption></figure>

{% hint style="info" %}
If Dynamo Player is new to you, take a look at the [dynamo-player.md](../../dynamo-player.md "mention") section.
{% endhint %}

> :tada: Mission accomplished!

## Ideas

Here are some ideas for how you could expand the capabilities of this graph.

{% hint style="info" %}
Place **multiple service connections** simultaneously instead of selecting each lot line.
{% endhint %}

{% hint style="info" %}
Adjust the inputs to instead place **sewer cleanouts** instead of water service meters.
{% endhint %}

{% hint style="info" %}
**Add a toggle** to allow for placing a single service connection on a particular side of the lot line instead of both sides.
{% endhint %}
