# n-Dimensional Lists

Further down the rabbit-hole, let's add even more tiers to hierarchy. Data structure can expand far beyond a two-dimensional list of lists. Since lists are items in and of themselves in Dynamo, we can create data with as many dimensions as possible.

The analogy we'll work with here are Russian Nesting Dolls. Each list can be regarded as one container holding multiple items. Each list has its own properties and is also regarded as its own object.

![Dolls](../images/5-4/4/145493363\_fc9ff5164f\_o.jpg)

> A set of Russian Nesting Dolls (Photo by [Zeta](https://www.flickr.com/photos/beppezizzi/145493363)) is an analogy for n-Dimensional lists. Each layer represents a list, and each list contains items within it. In Dynamo's case, each container can have multiple containers inside (representing the items of each list).

n-Dimensional lists are difficult to explain visually, but we've set up a few exercises in this chapter which focus on working with lists which venture beyond two dimensions.

### Mapping and Combinations

Mapping is arguably the most complex part of data management in Dynamo, and is especially relevant when working with complex hierarchies of lists. With the series of exercises below, we'll demonstrate when to use mapping and combinations as data becomes multi-dimensional.

Preliminary introductions to **List.Map** and **List.Combine** can be found in the previous section. In the last exercise below, we'll use these nodes on a complex data structure.

## Exercise - 2D Lists - Basic

> Download the example file by clicking on the link below.
>
> A full list of example files can be found in the Appendix.

{% file src="../datasets/5-4/4/n-Dimensional-Lists.zip" %}

This exercise is the first in a series of three which focuses on articulating imported geometry. Each part in this series of exercises will increase in the complexity of data structure.

![Exercise](../images/5-4/4/n-dimensionallists-2dlistsbasic01.jpg)

> 1. Let's begin with the .sat file in the exercise file folder. We can grab this file using the **File Path** node.
> 2. With **Geometry.ImportFromSAT**, the geometry is imported into our Dynamo preview as two surfaces.

For this exercise, we want to keep it simple and work with one of the surfaces.

![](../images/5-4/4/n-dimensionallists-2dlistsbasic02.jpg)

> 1. Let's select the index of 1 to grab the upper surface. We do this with **List.GetItemAtIndex** node.
> 2. Switch off the geometry preview from **Geometry.ImportFromSAT** preview.

The next step is to divide the surface into a grid of points.

![](../images/5-4/4/n-dimensionallists-2dlistsbasic03.jpg)

> 1\. Using **Code Block**, insert these two lines of code: `0..1..#10;` `0..1..#5;`
>
> 2\. With the **Surface.PointAtParameter**, connect the two code block values to u and _v_. Change the _lacing_ of this node to _"Cross Product"_.
>
> 3\. The output reveals the data structure, which is also visible in the Dynamo preview.

Next, used the Points from last step to generate ten curves along the surface.

![](../images/5-4/4/n-dimensionallists-2dlistsbasic04.jpg)

> 1. To get a look at how the data structure is organized, let's connect a **NurbsCurve.ByPoints** to the output of **Surface.PointAtParameter**.
> 2. You may switch off the preview from the **List.GetItemAtIndex** Node for now for a clearer result.

![](../images/5-4/4/n-dimensionallists-2dlistsbasic05.jpg)

> 1. A basic **List.Transpose** will flip the columns and rows of a list of lists.
> 2. Connecting the output of **List.Transpose** to **NurbsCurve.ByPoints**, we now get five curves running horizontally across the surface.
> 3. You may switch off the preview from the **NurbsCurve.ByPoints** Node in the previous step to achieve the same result in the image.

## Exercise - 2D Lists - Advanced

Let's increase the complexity. Suppose we wanted to perform an operation on the curves created from the previous exercise. Perhaps we want to relate these curves to another surface and loft between them. This requires more attention to data structure, but the underlying logic is the same.

![](../images/5-4/4/n-dimensionallists-2dlistsadvance01.jpg)

> 1. Begin with a step from the previous exercise, isolating the upper surface of the imported geometry with the **List.GetItemAtIndex** node.

![](../images/5-4/4/n-dimensionallists-2dlistsadvance02.jpg)

> 1. Using **Surface.Offset**, offset the surface by a value of _10_.

![](../images/5-4/4/n-dimensionallists-2dlistsadvance03.jpg)

> 1. In the same manner as the previous exercise, define a _code block_ with these two lines of code: `0..1..#10;` `0..1..#5;`
> 2. Connect these outputs to two **Surface.PointAtParameter** nodes, each with _lacing_ set to _"Cross Product"_. One of these nodes is connected to the original surface, while the other is connected to the offset surface.

![](../images/5-4/4/n-dimensionallists-2dlistsadvance04.jpg)

> 1. Switch off the preview of these Surfaces.
> 2. As in the previous exercise, connect the outputs to two **NurbsCurve.ByPoints** nodes. The result show curves corresponding to two surfaces.

![](../images/5-4/4/n-dimensionallists-2dlistsadvance05.jpg)

> 1. By using **List.Create**, we can combine the two sets of curves into one list of lists.
> 2. Notice from the output, we have two lists with ten items each, representing each connect set of Nurbs curves.
> 3. By performing a **Surface.ByLoft**, we can visually make sense of this data structure. The node lofts all of the curves in each sublist.

![](../images/5-4/4/n-dimensionallists-2dlistsadvance06.jpg)

> 1. Switch off the preview from **Surface.ByLoft** Node in previous step.
> 2. By using **List.Transpose**, remember, we are flipping all of the columns and rows. This node will transfer two lists of ten curves into ten lists of two curves. We now have each nurbs curve related to the neighboring curve on the other surface.
> 3. Using **Surface.ByLoft**, we arrive at a ribbed structure.

Next, we will demonstrate an alternative process to achieve this result

![](../images/5-4/4/n-dimensionallists-2dlistsadvance07.jpg)

> 1. Before we start, switch off the **Surface.ByLoft** preview in previous step to avoid confusion.
> 2. An alternative to **List.Transpose** uses **List.Combine**. This will operate a _"combinator"_ on each sublist.
> 3. In this case, we're using **List.Create** as the _"combinator"_, which will create a list of each item in the sublists.
> 4. Using the **Surface.ByLoft** node, we get the same surfaces as in the previous step. Transpose is easier to use in this case, but when the data structure becomes even more complex, **List.Combine** is more reliable.

![](../images/5-4/4/n-dimensionallists-2dlistsadvance08.jpg)

> 1. Stepping back a few steps, if we want to switch the orientation of the curves in the ribbed structure, we want to use a **List.Transpose** before connect to **NurbsCurve.ByPoints**. This will flip the columns and rows, giving us 5 horizontal ribs.

## Exercise - 3D Lists

Now, we're going to go even one step further. In this exercise, we'll work with both imported surfaces, creating a complex data hierarchy. Still, our aim is to complete the same operation with the same underlying logic.

Begin with the imported file from previous exercise.

![](../images/5-4/4/n-Dimensional-Lists-3dlist01.jpg)

![](../images/5-4/4/n-Dimensional-Lists-3dlist02.jpg)

> 1. As in the previous exercise, use the **Surface.Offset** node to offset by a value of _10_.
> 2. Notice from the output, that we've created two surfaces with the offset node.

![](../images/5-4/4/n-Dimensional-Lists-3dlist03.jpg)

> 1. In the same manner as the previous exercise, define a **Code Block** with these two lines of code: `0..1..#20;` `0..1..#20;`
> 2. Connect these outputs to two **Surface.PointAtParameter** nodes, each with lacing set to _"Cross Product"_. One of these nodes is connected to the original surfaces, while the other is connected to the offset surfaces.

![](../images/5-4/4/n-Dimensional-Lists-3dlist04.jpg)

> 1. As in the previous exercise, connect the outputs to two **NurbsCurve.ByPoints** nodes.
> 2. Looking at the output of the **NurbsCurve.ByPoints,** notice that this is a list of two lists, which is more complex than the previous exercise. The data is categorized by the underlying surface, so we've added another tier to the data structured.
> 3. Notice that things become more complex in the **Surface.PointAtParameter** node. In this case we have a list of lists of lists.

![](../images/5-4/4/n-Dimensional-Lists-3dlist05.jpg)

> 1. Before we proceed, switch off the preview from the existing surfaces.
> 2. Using the **List.Create** node, we merge the Nurbs curves into one data structure, creating a list of lists of lists.
> 3. By connecting a **Surface.ByLoft** node, we get a version of the original surfaces, as they each remain in their own list as created from the original data structure.

![](../images/5-4/4/n-Dimensional-Lists-3dlist06.jpg)

> 1. In the previous exercise, we were able to use a **List.Transpose** to create a ribbed structure. This won't work here. A transpose should be used on a two-dimensional list, and since we have a three-dimensional list, an operation of "flipping columns and rows" won't work as easily. Remember, lists are objects, so **List.Transpose** will flip our lists with out sublists, but won't flip the nurbs curves one list further down in the hierarchy.

![](../images/5-4/4/n-Dimensional-Lists-3dlist07.jpg)

> 1. **List.Combine** will work better for us here. We want to use **List.Map** and **List.Combine** nodes when we get to more complex data structures.
> 2. Using **List.Create** as the _"combinator"_, we create a data structure that will work better for us.

![](../images/5-4/4/n-Dimensional-Lists-3dlist08.jpg)

> 1. The data structure still needs to be transposed at one step down on the hierarchy. To do this we'll use **List.Map**. This is working like **List.Combine**, except with one input list, rather than two or more.
> 2. The function we'll apply to **List.Map** is **List.Transpose**, which will flip the columns and rows of the sublists within our main list.

![](../images/5-4/4/n-Dimensional-Lists-3dlist09.jpg)

> 1. Finally, we can loft the Nurbs curves together with a proper data hierarchy, giving us a ribbed structure.

![](../images/5-4/4/n-Dimensional-Lists-3dlist10.jpg)

> 1. Let's add some depth to the geometry with a **Surface.Thicken** Node with the input settings as shown.

![](../images/5-4/4/n-Dimensional-Lists-3dlist11.jpg)

> 1. It'll be nice to add a surface backing two this structure, so add another **Surface.ByLoft** node and use the first output of **NurbsCurve.ByPoints** from an earlier step as input.
> 2. As the preview is getting cluttered, switch off preview for these nodes by right clicking on each of them and uncheck 'preview' to see the result better.

![](../images/5-4/4/n-Dimensional-Lists-3dlist12.jpg)

> 1. And thickening these selected surfaces, our articulation is complete.

Not the most comfortable rocking chair ever, but it's got a lot of data going on.

![](../images/5-4/4/n-Dimensional-Lists-3dlist13.jpg)

Last step, let's reverse the direction of the striated members. As we used transpose in the previous exercise, we'll do something similar here.

![](../images/5-4/4/n-Dimensional-Lists-3dlist14.jpg)

> 1. Since we have one more tier to the hierarchy, so we need to use **List.Map** with a **List.Transpose** function to change the direction of our Nurbs curves.

![](../images/5-4/4/n-Dimensional-Lists-3dlist15.jpg)

> 1. We may want to increase the number of treads, so we can change the **Code Block** to `0..1..#20;` `0..1..#30;`

The first version of the rocking chair was sleek, so our second model offers an off-road, sport-utility version of recumbency.

![](../images/5-4/4/n-Dimensional-Lists-3dlist16.jpg)
