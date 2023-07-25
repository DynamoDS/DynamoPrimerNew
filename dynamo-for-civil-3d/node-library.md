# Node Library

We mentioned earlier that **nodes** are the core building blocks of a Dynamo graph, and they are organized into logical groups in the **library**. In Dynamo for Civil 3D, there are two categories (or **shelves**) in the library that contain dedicated nodes for working with AutoCAD and Civil 3D objects, such as Alignments, Profiles, Corridors, Block References, etc. The rest of the library contains nodes that are more generic in nature and are consistent between all "flavors" of Dynamo (for example, Dynamo for Revit, Dynamo Sandbox, etc.).

{% hint style="info" %}
Check out the [2-library.md](../3\_user\_interface/2-library.md "mention") section for more information about how the nodes in the core Dynamo library are organized.
{% endhint %}

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>The node library in Dynamo for Civil 3D</p></figcaption></figure>

> 1. Specific nodes for working with AutoCAD and Civil 3D objects
> 2. General-purpose nodes
> 3. Nodes from third-party **packages** that you can install separately

{% hint style="warning" %}
By using the nodes found under the AutoCAD and Civil 3D shelves, your Dynamo graph will only work in Dynamo for Civil 3D. If a Dynamo for Civil 3D graph is opened elsewhere (in Dynamo for Revit, for example), these nodes will be flagged with a warning and will not run.
{% endhint %}

{% hint style="info" %}
**Why are there two separate shelves for AutoCAD and Civil 3D?**

This organization distinguishes the nodes for native AutoCAD objects (Lines, Polylines, Block References, etc.) from the nodes for Civil 3D objects (Alignments, Corridors, Surfaces, etc.). And from a technical standpoint, AutoCAD and Civil 3D are two separate things - AutoCAD is the base application, and Civil 3D is built on top of it.
{% endhint %}

## Node Hierarchy

In order to work with the AutoCAD and Civil 3D nodes, it's important to have a solid understanding of the object hierarchy within each shelf. Remember the taxonomy from Biology? Kingdom, Phylum, Class, Order, Family, Genus, Species? AutoCAD and Civil 3D objects are categorized in a similar manner. Let's work through some examples to explain.

### Civil Objects

Let's use an Alignment as an example.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

Say your goal is to change the name of the Alignment. From here, the next node you would add is a **CivilObject.SetName** node.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

At first, this may not seem very intuitive. What is a **CivilObject**, and why does the library not have an **Alignment.SetName** node? The answer is related to _reusability_ and _simplicity._ If you think about it, the process of changing the name of a Civil 3D object is the same whether the object is an Alignment, Corridor, Profile, or something else. So instead of having repetitive nodes that essentially all do the same thing (e.g., **Alignment.SetName, Corridor.SetName, Profile.SetName**, etc.), it would make sense to wrap up that functionality into a single node. That's exactly what **CivilObject.SetName** does!

Another way to think about this is in terms of _relationships_. An Alignment and a Corridor are both types of **Civil Objects**, just like an apple and a pear are both types of fruit. Civil Object nodes apply to any type of Civil Object, just like you might want to use a single peeler for peeling both an apple and a pear. Your kitchen would get pretty messy if you had a separate peeler for every type of fruit! In that sense, the Dynamo node library is just like your kitchen.

### Objects

Now, let's take this one step further. Say you want to change the layer of the Alignment. The node you would use is the **Object.SetLayer** node.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

Why isn't there a node called **CivilObject.SetLayer**? The same principles of reusability and simplicity that we discussed earlier apply here. The _layer_ property is something that is common to any object in AutoCAD that can be drawn or inserted, such a Line, Polyline, Text, Block Reference, etc. Civil 3D objects like Alignments and Corridors fall under the same category, and so any node that applies to an **Object** can also be used with any **Civil Object**.

