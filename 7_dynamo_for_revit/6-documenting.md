# Documenting

Editing parameters for documentation follows suit with the lessons learned in prior sections. In this section, we'll look at editing parameters which don't affect the geometric properties of an element, but instead prepare a Revit file for documentation.

### Deviation

In the exercise below, we'll use a basic deviation from plane node to create a Revit sheet for documentation. Each panel on our parametrically defined roof structure has a different value for deviation, and we want to call out the range of values using color and by scheduling out the adaptive points to hand off to a facade consultant, engineer, or contractor.

![deviation](images/6/deviation.jpg)

> The deviation from plane node will calculate the distance that the set of four points varies from the best-fit plane between them. This is a quick and easy way to study constructability.

## Exercise

### Part I: Setting Panels Aperture Ratio Based on Deviation From Plane Node

> Download the example file by clicking on the link below.
>
> A full list of example files can be found in the Appendix.

{% file src="datasets/6/Revit-Documenting.zip" %}

Start with the Revit file for this section (or continue from the previous section). This file has an array of ETFE panels on the roof. We'll reference these panels for this exercise.

![](images/6/documenting-exerciseI-01.jpg)

> 1. Add a _Family Types_ node to the canvas and choose _"ROOF-PANEL-4PT"_.
> 2. Plug this node into a Select _All Elements of Family Type_ node to get all of the elements from Revit into Dynamo.

![](images/6/documenting-exerciseI-02.jpg)

> 1. Query the location of adaptive points for each element with the _AdaptiveComponent.Locations_ node.
> 2. Create a polygon from these four points with the _Polygon.ByPoints_ node. Notice we now have an abstract version of the paneled system in Dynamo without having to import the full geometry of the Revit element.
> 3. Calculate planar deviation with the _Polygon.PlaneDeviation_ node.

Just for kicks, like the previous exercise, let's set the aperture ratio of each panel based on its planar deviation.

![](images/6/documenting-exerciseI-03.jpg)

> 1. Add an _Element.SetParameterByName_ node to the canvas and connect the adaptive components to the _element_ input. Connect a _Code Block_ reading _"Aperture Ratio"_ into the _parameterName_ input.
> 2. We cannot directly connect the deviation results into the value input because we need to remap the values to the parameter range.

![](images/6/documenting-exerciseI-04.jpg)

> 1. Using _Math.RemapRange_, remap the deviation values to a domain between 0.15 and 0\_.\_45 by entering `0.15; 0.45;` into the _Code Block_.
> 2. Plug these results into the value input for _Element.SetParameterByName_.

Back in Revit we can _kind of_ make sense of the change in aperture across the surface.

![Exercise](images/6/13.jpg)

Zooming in, it becomes more clear that the closed panels are weighted towards the corners of the surface. The open corners are towards the top. The corners represent areas of larger deviation while bulge has minimal curvature, so this makes sense.

![Exercise](images/6/13a.jpg)

### Part II: Color and Documentation

Setting the Aperture Ratio doesn't clearly demonstrate the deviation of panels on the roof, and we're also changing the geometry of the actual element. Suppose we just want to study the deviation from the standpoint of fabrication feasibility. It would be helpful to color the panels based on deviation range for our documentation. We can do that with the series of steps below, and in a very similar process to the steps above.

![](images/6/documenting-exerciseII-01.jpg)

> 1. Remove the _Element.SetParameterByName_ and its input nodes and add _Element.OverrideColorInView_.
> 2. Add a _Color Range_ node to the canvas and plug into the color input of _Element.OverrideColorInView_. We still have to connect the deviation values to the color range in order to create the gradient.
> 3. Hovering over the _value_ input, we can see that the values for the input must be between _0_ and _1_ in order to map a color to each value. We need to remap the deviation values to this range.

![](images/6/documenting-exerciseII-02.jpg)

> 1. Using _Math.RemapRange_, remap the planar deviation values to a range between\* 0\* and _1_ (note: you can use the _"MapTo"_ node to define a source domain as well).
> 2. Plug the results into a _Color Range_ node.
> 3. Notice our output is a range of colors instead of a range of numbers.
> 4. If you're set to Manual, hit _Run_. You should be able to get away with being set to Automatic from this point forward.

Back in Revit, we see a much more legible gradient which is representative of planar deviation based on our color range. But what if we want to customize the colors? Notice that the minimum deviation values are represented in red, which seems to be the opposite of what we'd expect. We want to have maximum deviation to be red, with minimum deviation represented by a calmer color. Let's go back to Dynamo and fix this.

![](images/6/09.jpg)

![](images/6/documenting-exerciseII-04.jpg)

> 1. Using a _code block_, add two numbers on two different lines: `0;` and `255;`.
> 2. Create a red and blue color by plugging the appropriate values into two _Color.ByARGB_ nodes.
> 3. Create a list from these two colors.
> 4. Plug this list into the _colors_ input of the _Color Range_, and watch the custom color range update.

Back in Revit, we can now make better sense of areas of maximum deviation in the corners. Remember, this node is for overriding a color in a view, so it can be really helpful if we had a particular sheet in the set of drawings which focuses on a particular type of analysis.

![Exercise](images/6/07(6).jpg)

### Part III: Scheduling

Selecting one ETFE panel in Revit, we see that there are four instance parameters, XYZ1, XYZ2, XYZ3, and XYZ4. These are all blank after they're created. These are text-based parameters and need values. We'll use Dynamo to write the adaptive point locations to each parameter. This helps interoperability if the geometry needs to be sent to an engineer of facade consultant.

![](images/6/documenting-exerciseIII-01.jpg)

In a sample sheet, we have a large, empty schedule. The XYZ parameters are shared parameters in the Revit file, which allows us to add them to the schedule.

![Exercise](images/6/03(8).jpg)

Zooming in, the XYZ parameters are yet to be filled in. The first two parameters are taken care of by Revit.

![Exercise](images/6/02(9).jpg)

To write in these values, we'll do a complex list operation. The graph itself is simple, but the concepts build heavily from the list mapping as discussed in the list chapter.

![](images/6/documenting-exerciseIII-04.jpg)

> 1. Select all the adaptive components with two nodes.
> 2. Extract the location of each point with _AdaptiveComponent.Locations_.
> 3. Convert these points to strings. Remember, the parameter is text-based so we need to input the correct data type.
> 4. Create a list of the four strings which define the parameters to change: _XYZ1, XYZ2, XYZ3,_ and _XYZ4_.
> 5. Plug this list into the _parameterName_ input of _Element.SetParameterByName_.
> 6. Connect _Element.SetParameterByName_ into the the _combinator_ input of _List.Combine._ Connect the _adaptive components_ into _list1_. Connect _String_ from Object into _list2_.

We are list mapping here, because we are writing four values for each element, which creates a complex data structure. The _List.Combine_ node defines an operation one step down in the data hierarchy. This is why element and value inputs of _Element.SetParameterByName_ are left blank. _List.Combine_ is connecting the sublists of its inputs into the empty inputs of _Element.SetParameterByName_, based on the order in which they are connected.

Selecting a panel in Revit, we see now that we have string values for each parameter. Realistically, we would create a simpler format to write a point (X,Y,Z). This can be done with string operations in Dynamo, but we're bypassing that here to stay within the scope of this chapter.

![](../.gitbook/assets/04(5).jpg)

A view of the sample schedule with parameters filled in.

![](../.gitbook/assets/01(9).jpg)

Each ETFE panel now has the XYZ coordinates written for each adaptive point, representing the corners of each panel for fabrication.

![Exercise](../.gitbook/assets/00(8).jpg)
