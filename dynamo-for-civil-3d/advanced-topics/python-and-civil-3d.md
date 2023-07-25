# Python and Civil 3D

While Dynamo is extremely powerful as a [visual programming](../../a\_appendix/a-1\_visual-programming-and-dynamo.md) tool, it is also possible to go beyond nodes and wires and write code in textual form. There are two ways that you can do this:

1. Write **DesignScript** using a Code Block
2. Write **Python** using a Python node

This section will focus on how to leverage Python in the Civil 3D environment to take advantage of the AutoCAD and Civil 3D .NET APIs.

{% hint style="info" %}
Take a look at the [8-3\_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention") section for more general information about using Python in Dynamo.
{% endhint %}

## API Documentation

Both AutoCAD and Civil 3D have several APIs available that enable developers like you to extend the core product with custom functionality. In the context of Dynamo, it is the **Managed .NET APIs** that are relevant. The following links are essential to understanding the structure of the APIs and how they work.

[AutoCAD .NET API Developer's Guide](https://help.autodesk.com/view/OARX/2024/ENU/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2)

[AutoCAD .NET API Reference Guide](https://help.autodesk.com/view/OARX/2024/ENU/?guid=OARX-ManagedRefGuide-What\_s\_New)

[Civil 3D .NET API Developer's Guide](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0)

[Civil 3D .NET API Reference Guide](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331)

{% hint style="info" %}
As you move through this section, there may be some concepts that you are unfamiliar with, such as databases, transactions, methods, properties, etc. Many of these concepts are core to working with the .NET APIs and are not specific to Dynamo or Python. It is beyond the scope of this section of the Primer to discuss these items in detail, so we recommend frequently referring to the above links for more information.
{% endhint %}

## Code Template

When you first edit a new Python node, it will be pre-populated with template code to get you started. Here's a breakdown of the template with explanations about each block.

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>The default Python template in Civil 3D</p></figcaption></figure>

> 1. Imports the `sys` and `clr` modules, both of which are necessary for the Python interpreter to function properly. In particular, the `clr` module enables .NET namespaces to be treated essentially as Python packages.
> 2. Loads the standard assemblies (i.e., DLLs) for working with the managed .NET APIs for AutoCAD and Civil 3D.
> 3. Adds references to standard AutoCAD and Civil 3D namespaces. These are equivalent to the `using` or `Imports` directives in C# or VB.NET (respectively).
> 4. The node's input ports are accessible using a pre-defined list called `IN`. You can access the data in a specific port using its index number, for example `dataInFirstPort = IN[0]`.
> 5. Gets the active Document and Editor.
> 6. Locks the Document and initiates a Database transaction.
> 7. This where you should place the bulk of your script's logic.
> 8. Uncomment this line to commit the transaction after your main work is done.
> 9. If you want to output any data from the node, assign it to the `OUT` variable at the end of your script.

{% hint style="info" %}
**Want to customize?**\
You can modify the default Python template by editing the `PythonTemplate.py` file located in `C:\ProgramData\Autodesk\C3D <version>\Dynamo`.
{% endhint %}

## Example

Let's work through an example to demonstrate some of the essential concepts of writing Python scripts in Dynamo for Civil 3D.

### Goal

> :dart: Get the boundary geometry of all Catchments in a drawing.

### Dataset

Here are examples files that you can reference for this exercise.

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### Solution Overview

Here's an overview of the logic in this graph.

> 1. Review the Civil 3D API documentation
> 2. Select all of the Catchments in the document by layer name
> 3. "Unwrap" the Dynamo Objects to access the internal Civil 3D API members
> 4. Create Dynamo points from AutoCAD points
> 5. Create PolyCurves from the points

Let's go!

### Review API Documentation

Before we start building our graph and writing code, it's a good idea to take a look at the Civil 3D API documentation and get a sense of what the API makes available to us. In this case, there's a [property in the Catchment class](https://help.autodesk.com/view/CIV3D/2024/ENU/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c) that will return the boundary points of the Catchment. Note that this property returns a `Point3dCollection` object, which is not something that Dynamo will know what to do with. In other words, we won't be able to create a PolyCurve from a `Point3dCollection`, so eventually we'll need to convert everything to Dynamo points. More on that later.

### Get All Catchments

Now we can start building our graph logic. The first thing to do is get a list of all the Catchments in the Document. There are nodes available for this, so we don't need to include it in the Python script. Using nodes provides better visibility for someone else that might read the graph (versus burying lots of code in a Python script), and it also keeps the Python script focused on one thing: returning the boundary points of the Catchments.

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>Getting all of the Catchments in the Document by layer</p></figcaption></figure>

Note here that the output from the **All Objects on Layer** node is a list of CivilObjects. This is because Dynamo for Civil 3D doesn't currently have any nodes for working with Catchments, which is the whole reason why we need to access the API through Python.

### Unwrapping Objects

Before we go further, we need to briefly touch on an important concept. In the [node-library.md](../node-library.md "mention") section, we discussed how Objects and CivilObjects are related. To add a little more detail to this, a **Dynamo Object** is a wrapper around an **AutoCAD Entity**. Similarly, a **Dynamo CivilObject** is a wrapper around a **Civil 3D Entity**. You can "unwrap" an Object by accessing its `InternalDBObject` or `InternalObjectId` properties.

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Dynamo Type</th><th width="373">Wraps</th></tr></thead><tbody><tr><td><strong>Object</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>Entity</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>Entity</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %}
As a rule of thumb, it is generally safer to get the Object ID using the `InternalObjectId` property and then access the wrapped object in a transaction. This is because the `InternalDBObject` property will return an AutoCAD DBObject that is not in a writable state.
{% endhint %}

### Python Script

Here's the complete Python script that does the work of accessing the internal Catchment objects are getting their boundary points. The highlighted lines represent those that are modified/added from the default template code.

{% hint style="info" %}
Click on the underlined text in the script for an explanation of each line.
{% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># Load the Python Standard and DesignScript Libraries
import sys
import clr

# Add Assemblies for AutoCAD and Civil3D
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# Import references from AutoCAD
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Import references from Civil3D
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# The inputs to this node will be stored as a list in the IN variables.
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a> 
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit("The input is null or empty.")</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-8">if not isinstance(objs, list):</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-9">objs = [objs]</a>
</strong>    
adoc = Application.DocumentManager.MdiActiveDocument
editor = adoc.Editor

with adoc.LockDocument():
    with adoc.Database as db:
        
        with db.TransactionManager.StartTransaction() as t:
<strong>            <a data-footnote-ref href="#user-content-fn-10">for obj in objs:</a>              
</strong><strong>                <a data-footnote-ref href="#user-content-fn-11">id = obj.InternalObjectId</a>
</strong><strong>                <a data-footnote-ref href="#user-content-fn-12">aeccObj = t.GetObject(id, OpenMode.ForRead)</a>                
</strong><strong>                <a data-footnote-ref href="#user-content-fn-13">if isinstance(aeccObj, Catchment):</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-14">catchment = aeccObj</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-15">acPnts = catchment.BoundaryPolyline3d</a>                    
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-16">dynPnts = []</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">for acPnt in acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>            
            # Commit before end transaction
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# Assign your output to the OUT variable.
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %}
As a rule of thumb, it is a best practice to include the bulk of your script logic inside a transaction. This ensures safe access to the objects that your script is reading/writing. In many cases, omitting a transaction can cause a fatal error.
{% endhint %}

### Create PolyCurves

At this stage, the Python script should output a list of Dynamo points that you can see in the background preview. The last step is to simply create PolyCurves from the points. Note that this could also be accomplished directly in the Python script, but we've intentionally put it outside the script in a node so that it is more visible. Here's what the final graph looks like.

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>The final graph</p></figcaption></figure>

### Result

And here's the final Dynamo geometry.

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>The resulting Dynamo PolyCurves for the Catchment boundaries</p></figcaption></figure>

> :tada: Mission accomplished!

## IronPython vs. CPython

Just a quick note here before we wrap up. Depending on which version of Civil 3D you are using, the Python node may be configured differently. In **Civil 3D 2020 and 2021**, Dynamo used a tool called **IronPython** to move data between .NET objects and Python scripts. In **Civil 3D 2022**, however, Dynamo transitioned to use the standard native Python interpreter (aka **CPython**) instead that uses Python 3. Benefits of this transition include access to popular modern libraries and new platform features, essential maintenance, and security patches.

{% hint style="info" %}
You can read more about this transition and how to upgrade legacy scripts on the [Dynamo Blog](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/). If you want to keep using IronPython, then you will simply need to install the **DynamoIronPython2.7** package using the Dynamo Package Manager.
{% endhint %}

[^1]: By default, the Dynamo geometry library is not added to the Python environment. Our goal with this script is to output a list of Dynamo points for the Catchment boundaries, so we need to add this line in order to create the points later on.

[^2]: This lines gets the specific class that we need from the Dynamo geometry library. Note that we specify `import Point as DynPoint` here instead of `import *` as the latter would introduce naming clashes.

[^3]: Here we rename the default `dataEnteringNode` variable to `objs` so it is more descriptive.

[^4]: Here we specify exactly which input port contains the data that we want instead of the default `IN`, which refers to the entire list of all inputs.

[^5]: This creates an empty list that we'll add our output data to later on.

[^6]: This is to protect against a potential empty or null input to our script.

[^7]: Instead of breaking the script, we'll output a helpful message to explain why the script cannot continue.

[^8]: The remainder of the script assumes that we have a list of objects to work with (as opposed to a single object). But just in case there is only one object (i.e., a drawing with a single Catchment), then we still want the script to be able to run.

[^9]: If the input is not a list (i.e., a single object), then we simply create a new list with the object as the only item.

[^10]: Initiate a loop for each object in the list.

[^11]: "Unwrap" the Dynamo object by getting its Object ID.

[^12]: Retrieve the "wrapped" object from the AutoCAD database. Notice that the OpenMode is set to `ForRead` here because we're not planning on making any edits to the objects. We're simplying "querying" data.

[^13]: It's possible that the input list of objects contains a mix of Catchments and other non-Catchment item. We need to check for this situation and handle it appropriately (i.e., only continue this iteration of the loop if the item is indeed a Catchment).

[^14]: If the script makes it this far, then we know that the object is indeed a Catchment. We'll add a new variable here just to keep the naming clean and understandable.

[^15]: Here's where we retrieve the Catchment boundary points using the appropriate property that we looked up earlier in the API docs. As mentioned earlier, this will give us a `Point3dCollection` object, which is essentially a list of AutoCAD points. We will need to "convert" those to Dynamo points so that they are useful.

[^16]: Create an empty list to store the boundary points for this Catchment.

[^17]: Initiate a loop for each `Point3d` object in the `Point3dCollection`.

[^18]: Create a Dynamo point using the coordinates of the AutoCAD point.

[^19]: Add the point to the list.

[^20]: Once we've finished "converting" all of the AutoCAD points to Dynamo points, we add the resulting list to our output. After this, the outer loop will move on to the next object in the input list.

[^21]: Uncomment this line to commit the transaction.

[^22]: And last but not least, we output the list of Dynamo points.
