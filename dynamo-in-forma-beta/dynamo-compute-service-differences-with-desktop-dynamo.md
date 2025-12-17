# Dynamo cloud compute differences with Desktop Dynamo

This page highlights the differences you should be aware of when writing Dynamo programs to execute in the Dynamo compute service cloud context.

## What is DaaS?

You may have seen documentation referring to DaaS, Dynamo as a Service, or Dynamo Cloud Compute. These all refer to the same thing: Dynamo's core runtime executing in a cloud context. This means your graph is not executing on your machine. DaaS can currently be accessed only through the Dynamo Player extension for Forma, where users can upload and manage `.dyn` files created in the desktop environment, run `.dyn` files shared by their colleagues through the extension, or use pre-loaded `.dyn` routines provided by Autodesk as samples.

Because your graphs run in this cloud context, and not on your machine, DaaS currently cannot directly use traditional Dynamo host contexts (Revit, Civil 3D, etc.). If you want to use types from those programs in your graph, you will need to serialize (save) them into the graph using the `Data.Remember` node or other in-graph serialization techniques. These are similar to the workflows you need to use when writing graphs for Generative Design in Revit.

For more information on the Dynamo compute service see the [Dynamo compute service docs](../dynamo-compute-service/README.md).

## What version of Dynamo is executing my code?

The version is based on 3.x and is updated frequently based on Dynamo's open-source master branch.

## What packages/nodes are available in this version of Dynamo?

* Most of the core nodes, see the next section for some specific limitations.
* `DynamoFormaBeta` package for interacting with the Forma API.
* `VASA` for voxelization / efficient analysis.
* `MeshToolKit` for mesh manipulation. Mesh toolkit is also available out-of-the-box starting in Dynamo 3.4.
* `RefineryToolkit` for useful algorithms that allow clash test, view distance, shortest path, isovist, etc.

## What should I be aware of while I write graphs for DaaS?

* Python nodes will not work. These will _currently_ just fail to execute.
* Custom packages cannot be used.
* The UI/view layer of UI nodes will not execute. We don't anticipate this to be a problem with core functionality, but it's good to keep in mind if you see a bug related to a node with custom UI.
* Windows-only functionality will not work. For example, if you try to use the Windows registry or WPF, this will fail.
* View extensions will not be loaded.
* Filesystem nodes are not going to be very useful. Any files you reference on your local machine will not exist when running in DaaS.
* Excel/DSOffice interop nodes will not function. Open XML nodes should function.
* Network requests in general will not function, though you can make calls to the Forma API.

## How am I supposed to remember all of that? What if it changes?

* In the future, we intend to provide tooling inside Dynamo for the desktop that will make it easier to ensure your graph runs the same in both contexts.

## How much does this cost?

* During this Beta, compute time is not currently charged.

## How do I get started?

Check out the [blog post](https://dynamobim.org/dynamo-as-a-service-powers-up-dynamo-player-in-forma/), [YouTube series](https://www.youtube.com/playlist?list=PLY-ggSrSwbZqlbQG1i45bpT8clCJp08wD) or the Samples in the Forma Extension to get started. These will guide you through:

* Getting access to Autodesk Forma.
* Installing the DynamoFormaBeta for Dynamo on Desktop and the Dynamo Extension in Forma.
* Writing your first graph.

## Security

* Be aware your shared graphs are stored in Forma.
* Max graph execution time is currently less than 30 minutes. This value may change.
* Execution requests are rate limited, so you may see errors if you make many compute requests in too short a period.
