# Documenting

Editing parameters for documentation follows suit with the lessons learned in prior sections. In this section, we'll look at editing parameters which don't affect the geometric properties of an element, but instead prepare a Revit file for documentation.

### Deviation

In the exercise below, we'll use a basic deviation from plane node to create a Revit sheet for documentation. Each panel on our parametrically defined roof structure has a different value for deviation, and we want to call out the range of values using color and by scheduling out the adaptive points to hand off to a facade consultant, engineer, or contractor.

![deviation](../.gitbook/assets/deviation.jpg)

> The deviation from plane node will calculate the distance that the set of four points varies from the best-fit plane between them. This is a quick and easy way to study constructability.

## Exercise

> Download the example files that accompanies this exercise (Right click and "Save Link As..."). A full list of example files can be found in the Appendix.

1. [Documenting.dyn](https://github.com/h-iL/ForkedDynamoPrimerReorganized/blob/main/08\_Dynamo-for-Revit/datasets/8-6/Documenting.dyn)
2. [ARCH-Documenting-BaseFile.rvt](https://github.com/h-iL/ForkedDynamoPrimerReorganized/blob/main/08\_Dynamo-for-Revit/datasets/8-6/ARCH-Documenting-BaseFile.rvt)

Start with the Revit file for this section (or continue from the previous section). This file has an array of ETFE panels on the roof. We'll reference these panels for this exercise.

![Exercise](../.gitbook/assets/17.jpg)

> 1. Add a _Family Types_ node to the canvas and choose _"ROOF-PANEL-4PT"_.

1. Plug this node into a Select _All Elements of Family Type_ node to get all of the elements from Revit into Dynamo.

![Exercise](../.gitbook/assets/16.jpg)

> 1. Query the location of adaptive points for each element with the _AdaptiveComponent.Locations_ node.

1. Create a polygon from these four points with the _Polygon.ByPoints_ node. Notice we now have an abstract version of the paneled system in Dynamo without having to import the full geometry of the Revit element.
2. Calculate planar deviation with the _Polygon.PlaneDeviation_ node.

![Exercise](<../.gitbook/assets/15 (1).jpg>)

> Just for kicks, like the previous exercise, let's set the \*aperture ratio \*of each panel based on its planar deviation.

1. Add an _Element.SetParameterByName_ node to the canvas and connect the adaptive components to the _element_ input. Connect a _code block_ reading _"Aperture Ratio"_ into the _parameterName_ input.
2. We cannot directly connect the deviation results into the value input because we need to remap the values to the parameter range.

![Exercise](<../.gitbook/assets/14 (1).jpg>)

> 1. Using _Math.RemapRange_, remap the deviation values to a domain between \*.15 \*and _.45_.

1. Plug these results into the value input for _Element.SetParameterByName_.

![Exercise](../.gitbook/assets/13.jpg)

> Back in Revit we can _kind of_ make sense of the change in aperture across the surface.

![Exercise](../.gitbook/assets/13a.jpg)

> Zooming in, it becomes more clear that the closed panels are weighted towards the corners of the surface. The open corners are towards the top. The corners represent areas of larger deviation while bulge has minimal curvature, so this makes sense.

\###Color and Documentation Setting the Aperture Ratio doesn't clearly demonstrate the deviation of panels on the roof, and we're also changing the geometry of the actual element. Suppose we just want to study the deviation from the standpoint of fabrication feasibility. It would be helpful to color the panels based on deviation range for our documentation. We can do that with the series of steps below, and in a very similar process to the steps above.

![Exercise](<../.gitbook/assets/11 (3).jpg>)

> 1. Remove the _Element.SetParameterByName_ nodes and add _Element.OverrideColorInView_.

1. Add a _Color Range_ node to the canvas and plug into the color input of _Element.OverrideColorInView_. We still have to connect the deviation values to the color range in order to create the gradient.
2. Hovering over the _value_ input, we can see that the values for the input must be between _0_ and _1_ in order to map a color to each value. We need to remap the deviation values to this range.

![Exercise](<../.gitbook/assets/10 (3).jpg>)

> 1. Using _Math.RemapRange_, remap the planar deviation values to a range between\* 0\* and _1_ (note: you can use the _"MapTo"_ node to define a source domain as well).

1. Plug the results into a _Color Range_ node.
2. Notice our output is a range of colors instead of a range of numbers.
3. If you're set to Manual, hit _Run_. You should be able to get away with being set to Automatic from this point forward.

![Exercise](../.gitbook/assets/09.jpg)

> Back in Revit, we see a much more legible gradient which is representative of planar deviation based on our color range. But what if we want to customize the colors? Notice that the minimum deviation values are represented in red, which seems to be the opposite of what we'd expect. We want to have maximum deviation to be red, with minimum deviation represented by a calmer color. Let's go back to Dynamo and fix this.

![Exercise](<../.gitbook/assets/08 (4).jpg>)

> 1. Using a _code block_, add two numbers on two different lines: `0;` and `255;`.

1. Create a red and blue color by plugging the appropriate values into two _Color.ByARGB_ nodes.
2. Create a list from these two colors.
3. Plug this list into the _colors_ input of the _Color Range_, and watch the custom color range update.

![Exercise](<../.gitbook/assets/07 (6).jpg>)

> Back in Revit, we can now make better sense of areas of maximum deviation in the corners. Remember, this node is for overriding a color in a view, so it can be really helpful if we had a particular sheet in the set of drawings which focuses on a particular type of analysis.

\###Scheduling

![Exercise](<../.gitbook/assets/06 (2).jpg>)

> 1. Selecting one ETFE panel in Revit, we see that there are four instance parameters, _XYZ1, XYZ2, XYZ3,_ and _XYZ4_. These are all blank after they're created. These are text-based parameters and need values. We'll use Dynamo to write the adaptive point locations to each parameter. This helps interoperability if the geometry needs to be sent to an engineer of facade consultant.

![Exercise](<../.gitbook/assets/03 (8).jpg>)

> In a sample sheet, we have a large, empty schedule. The XYZ parameters are shared parameters in the Revit file, which allows us to add them to the schedule.

![Exercise](<../.gitbook/assets/02 (9).jpg>)

> Zooming in, the XYZ parameters are yet to be filled in. The first two parameters are taken care of by Revit.

![Exercise](<../.gitbook/assets/05 (3).jpg>)

> To write in these values, we'll do a complex list operation. The graph itself is simple, but the concepts build heavily from the list mapping as discussed in the list chapter.

1. Select all the adaptive components with two nodes.
2. Extract the location of each point with _AdaptiveComponent.Locations_.
3. Convert these points to strings. Remember, the parameter is text-based so we need to input the correct data type.
4. Create a list of the four strings which define the parameters to change: _XYZ1, XYZ2, XYZ3,_ and _XYZ4_.
5. Plug this list into the _parameterName_ input of _Element.SetParameterByName_.
6. Connect _Element.SetParameterByName_ into the the _combinator_ input of _List.Combine._
7. Connect the _adaptive components_ into _list1_.
8. Connect _String_ from Object into _list2_.
9. We are list mapping here, because we are writing four values for each element, which creates a complex data structure. The _List.Combine_ node defines an operation one step down in the data hierarchy. This is why element and value inputs are left blank. _List.Combine_ is connecting the sublists of its inputs into the empty inputs of _List.SetParameterByName_, based on the order in which they are connected.

![Exercise](<../.gitbook/assets/04 (5).jpg>)

> Selecting a panel in Revit, we see now that we have string values for each parameter. Realistically, we would create a simpler format to write a point (X,Y,Z). This can be done with string operations in Dynamo, but we're bypassing that here to stay within the scope of this chapter.

![Exercise](<../.gitbook/assets/01 (9).jpg>)

> A view of the sample schedule with parameters filled in.

![Exercise](<../.gitbook/assets/00 (8).jpg>)

> Each ETFE panel now has the XYZ coordinates written for each adaptive point, representing the corners of each panel for fabrication.
