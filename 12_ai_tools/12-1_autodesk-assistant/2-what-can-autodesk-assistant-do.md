# What can Autodesk Assistant do?

Autodesk Assistant is capable of many operations and will utilize library nodes, code blocks containing designscript and custom python nodes. Autodesk Assistant is also capable of organising and annotating dynamo scripts using coloured regions and text.

### Basic list management

Autodesk Assistant is perfect for automating small simple tasks that are otherwise repetitive and tedious. In this example, Autodesk Assistant creates two lists of points and then connects them with lines with on offset of two. 
The exact prompt is: *I would like two sets of points in a line parallel to each other. The sets should each have ten points. Then connect the points with lines and offset the points being connected by two.*



### Debugging

Autodesk Assistant can debug or resolve any warnings or errors that may be occuring in a Dynamo script. In the following script it can be seen that there is a mismatch in the input type expected in a **List.GetItemAtIndex** node. 



In this instance Autodesk Assistant has been used to debug the script and fix the input issue. The exact prompt was: *please fix the warning on the canvas*



Autodesk Assistant identified that a string was being used as input when a number or integer should have been used and rectified the issue.

### Geometry

Autodesk Assistant can create complex geometry and will try as best as possible to match a given query. Concise instructions are key to getting the desired results. In this example, Autodesk Assistant is being asked to create 10 spheres that increase in diameter by 1 unit each time, starting with a diameter of 1. The spheres are to be placed in a spiral that trends upwards. The exact prompt reads: *I would like 10 spheres each with a diameter that is 1 unit larger than the lat, starting at one. Place them in a spiral that rises upwards.*

![](../../.gitbook/assets/Aa_Geom_query.jpg)


Autodesk Assistant best tries to match this request as can be seen by the geometry created here. 
![](<../../.gitbook/assets/Aa_Geom_geometry.jpg>)


The Autodesk Assistant is able to generate nodes, custom code blocks and python nodes but if not specified will tend towards code blocks.

![](<../../.gitbook/assets/Aa_Geom_script.jpg>)

If requested, the Autodesk Assistant can rewrite a script to prioritise using a particular method if that is what is wanted.

![](../../.gitbook/assets/Aa_Geom_script_rewrite.jpg)

The script will be re-written as desired.

![](../../.gitbook/assets/Aa_Geom_script_nodes.jpg)

However the results may be slightly different, especially if certain variables were not given in the first place.

![](../../.gitbook/assets/Aa_Geom_script_results.jpg)

As mentioned Autodesk Assistant can also create python nodes if that is what is desired.

![](../../.gitbook/assets/Aa_Geom_script_CodeRequest.jpg)

The Autodesk Assistant will again rewrite the script in the requested format, with some minor changes in the results being possible once again.

![](../../.gitbook/assets/Aa_Geom_script_CodeResults.jpg)

### Data manipulation

 Autodesk Assistant can use revit nodes to search through the Revit document attached to the Dynamo file in order to extract information about the Revit model. In this particular example, a sample revit project has been loaded into the program and Autodesk Assistant will be used to make queries about the model. 

![](../../.gitbook/assets/Aa_Data_SampleProject.jpg)

The prompt here was: *Please get the combined lengths of all walls in this model that are less than 200mm thick*. As can be seen the Autodesk Assistant utilised a script in order to gather the requested information about the model and then displayed it within the Autodesk Assistant chat window.

![](../../.gitbook/assets/Aa_Data_Results.jpg)

This script could also then be taken and applied to other models if desired.

![](../../.gitbook/assets/Aa_Data_Script.jpg)

### Search functions

> 1. Autodesk Assistant can use the web to search for scripts that may already exist elsewhere and recreate or copy them into the canvas



### Real world references

As Autodesk Assistant has access to the internet it can also search for real world references in order to recreate buildings or forms that already exist. In this example, Autodesk Assistant has been asked to create a geometry form that matches the iconic Gherkin building (also known as 30 St Marys Axe) in London. 

![](../../.gitbook/assets/Aa_Search_Gherkin.jpg)

For simplicity, the prompt was just to create a geometric form, not full Revit elements. 
The full prompt was: *I would like a script that recreates the gherkin building, also known as 30 st mary axe, in London. The script can just create geometry for now, I don't need revit elements yet.*

![](../../.gitbook/assets/Aa_Search_Script.jpg)

![](../../.gitbook/assets/Aa_Search_Geometry.jpg)

As can be seen, the geometry does not quite match the exact form and requires some additional nodes in order to reach the correct orientation. 

![](../../.gitbook/assets/Aa_Search_FlipScript.jpg)

Once the form was flipped though, it somewhat resembles the form of the Gherkin building and particularly given that there are inputs which allow for micro adjustments, it is not hard to arrive at a form which is recognisable as the Gherkin. 

![](../../.gitbook/assets/Aa_Search_FlipGeometry.jpg)