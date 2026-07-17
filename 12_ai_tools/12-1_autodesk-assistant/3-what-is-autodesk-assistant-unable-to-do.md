# What is Autodesk Assistant not yet capable of

### Updating packages

Autodesk Assistant can not yet download and install packages into Dynamo

![](../../.gitbook/assets/Aa_Update.jpg)

As can be seen in the image above, Autodesk Assistant is not able to modify, install or uninstall external packages associated with Dynamo. It is able to provide instructions so that you may install them yourself.


### Accessing the hard disk

Autodesk Assistant is currently not able to access file systems on the general hard disk. 
In the example below there is some geometry created on the canvas. Autodesk Assistant is asked to take the points that make up the geometry and export them as a JSON. Whilst the Autodesk Assistant is not able to generate this JSON directly, it does suggest that a dynamo script could be made that does this.

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

### Directly changing the Revit document

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Autodesk Assistant for Dynamo is not able to directly alter the Revit document or model. Revit has its own version of Autodesk Assistant that makes changes to the Revit model and document, the Assitant for Dynamo will notify the user that it is unable to directly alter the Revit document.

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

Autodesk Assistant in Dynamo can only alter the Revit document and model via scripting.

![](../../.gitbook/assets/Aa_viewScript.jpg)

