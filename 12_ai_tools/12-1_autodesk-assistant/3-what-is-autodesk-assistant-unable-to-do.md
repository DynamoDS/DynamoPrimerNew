# What is Autodesk Assistant not yet capable of

### Updating packages

Autodesk Assistant can not yet download and install packages into Dynamo

![](../../.gitbook/assets/Aa_Update.jpg)

As can be seen in the image above, Autodesk Assistant is not able to modify, install or uninstall external packages associated with Dynamo. It is able to provide instructions so that you may install them yourself.


### Reading and writing files directly

Autodesk Assistant works with Dynamo graph files: it can open a graph, insert one into your canvas, run one, and save your work. It cannot read or write other files on your hard disk on its own.

In the example below there is some geometry created on the canvas. Autodesk Assistant is asked to take the points that make up the geometry and export them as a JSON. Whilst the Autodesk Assistant is not able to generate this JSON directly, it does suggest that a dynamo script could be made that does this.

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

That suggestion is the pattern to remember. Anything outside the graph is reached through nodes, not by Assistant itself.

### Changing the Revit model without a graph

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Autodesk Assistant for Dynamo has no direct connection to the Revit model. Ask it to edit the model the way you would ask Assistant in Revit, and it will tell you that it cannot.

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

It can still change your Revit model. It does this the way you would: it places Revit nodes, wires them together, and the model changes when the graph runs.

![](../../.gitbook/assets/Aa_viewScript.jpg)

Two things follow from working this way:

- **Nothing changes until the graph runs.** You can read the graph and adjust values first. The model is only touched when the graph is run, either by you or by Assistant as part of your request.
- **The change is repeatable.** The graph stays on the canvas. Save it, and you can run the same task again on another model, or share it with your team.

If you want a single, direct edit with no graph involved, use Autodesk Assistant in Revit.
