# Efficiently Working With Large Data Sets In Dynamo

This page introduces you to some rules of thumb for efficiently working with large data sets in Dynamo. Hopefully you can use the tips to identify bottlenecks in your graphs, so that your graph will execute in a matter of minutes, rather than a matter of hours.

Contents:
* Geometry generation vs tessellation
* Memory use
* Revit API

### Geometry generation vs tessellation

In Dynamo, creating a piece of geometry and drawing it are two completely different events. In general, creating geometry is much quicker and uses less memory than drawing the object. You can think of the geometry as a list of measurements to make a suit, while the tessellation is the suit itself. You can tell quite a lot about the suit from its measurements: how long the arms are, how much it costs, etc., but you almost always need to see and try on the finished suit to see if it is correct or not. Similarly, with untessellated geometry, you can determine its bounding box, area, volume; intersect it with other geometry; and export it to SAT or Revit. However, you almost always have to tessellate the geometry to get a feel it it is correct or not. 

If your Dynamo graph has many objects, and is slowing down during execution, you may be able to remove the tessellation steps from your graph to speed things up.  

Geometry nodes in Dynamo are always tessellated*. This leaves you with two options to work with untessellated geometry: Python nodes and ZeroTouch nodes. As long as you don't return a geometry object out of your Python or ZeroTouch node, the geometry will not be tessellated. For instance, if your graph has several point nodes, connected to several line nodes, connected to several loft nodes, connected to several thicken nodes, the geometry will be tessellated at each step. Instead, you can bundle this logic into a Python or ZeroTouch node, and only return the final object out of the node.

More information about using ZeroTouch nodes can be found in the [Developing for Dynamo](11\_developer\_primer/3\_developing\_for\_dynamo/README.md) section of this Primer.

### Memory Use

If you are no longer tessellating geometry, you may run into a memory bottleneck from excess geometry accumulation. Geometry objects in Dynamo consume a minor, but not insignificant amount of memory on creation. If you are working with hundreds of thousands or millions of objects, this can add up and cause Dynamo or Revit to crash. In Dynamo version 2.5 and later this is handled implicitly by disposing off the unused objects, but if you are using a version older than 2.5 then one way to avoid creating lots of geometry is to dispose of the objects when you are done with them. For instance, let's say you were creating hundreds of thousands of NurbsCurves, each requiring dozens of Points. One way to create them is a 2 dimensional list in Dynamo, and feed that into a NurbsCurve.ByPoints node. However, that requires creating millions of Points. Another way is to use a Python or ZeroTouch node. In this node, you can create a dozen points, feed that into NurbsCurve.ByPoints, and then dispose of the dozen points with the .Dispose() method call. More information about using ZeroTouch nodes can be found in the [Developing for Dynamo](11\_developer\_primer/3\_developing\_for\_dynamo/README.md) section of this Primer. Disposing of your geometry objects after you create them can drastically cut down on the amount of memory you use in certain circumstances, and although we do handle this for users on Dynamo 2.5 and later, we do recommend that the user still disposes geometry explicitly, if the use case requires to cut down on memory at a deterministic time. See [Dynamo Geometry Stability Improvements](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297) to read more about the new stability features introduced in Dynamo 2.5.

### Revit API

If you are aggressively Disposing objects in a ZeroTouch or Python node, and still running into memory or performance problems, it may be necessary to bypass Dynamo entirely and create Revit objects directly through the API. For instance, you may parse an Excel file for point information, and use this information to create XYZ, and other Revit Elements, through their API. At this point, Revit will become the ultimate bottleneck, which can't be avoided.