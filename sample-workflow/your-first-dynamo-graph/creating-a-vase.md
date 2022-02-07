---
description: suggested exercise
---

# Parametric Vase

Creating a parametric vase is a great way to start learning key visual programming concepts needed to use Dynamo.&#x20;

This workflow will teach you how to:

* Use number sliders to control variables in your design.
* Create and modify geometry elements using nodes
* &#x20;Visualize design results in real-time.

![](<../../.gitbook/assets/vase1 (3).gif>)

## Designing Our Vase

Before jumping into dynamo let's conceptually design our vase.&#x20;

Let's say we are going to design a clay vase that takes into account manufacturing practices used by ceramists. Ceramists usually use a pottery wheel to fabricate cylindrical vases. Then, by applying pressure on various heights of the vase they can alter the shape of the vase and create varied designs.   &#x20;

We would use a similar methodology to define our vase. We will create 4 circles at different heights and radii and we will then create a surface by lofting those circles.

![](../../.gitbook/assets/vase2.png)



## Basic steps

### Creating Circles of Different Radii

In order to create a circle, we will use the **Circle.ByCentrePointRadius** node. This node has a default center point and radius.&#x20;

![](<../../.gitbook/assets/vase3 (1).png>)

We will create a slider to manipulate the radius and leave the center point to its default value. Let's also change the slider's maximum to 15 to make it easier to control.&#x20;

Now, let's copy these nodes 4 times as these circles will define our surface.

![](<../../.gitbook/assets/vase4 (1).png>)

> 1. Circles are created by a center point and a radius

### Moving Circles Through the Vase Height

We are missing a key parameter to our vase, its height. In order to control the height, we create another number slider. We then use a **Geometry.Translate** node to place circles at the desired height. Since we want to distribute our circles through the vase we use code blocks to multiply the height parameter by a factor.

![](<../../.gitbook/assets/vase5 (1).png>)

> 2\. Circles are translated (moved) by a variable in the z axis.

### Creating the Surface

In order to create a surface using the **Surface.ByLoft** node we need to combine all of our translated circles into a list. We use the **List.Create** to combine all of our circles into a single list, and then finally output this list to the **Surface.ByLoft** node to view results.

Let's also turn off the preview in other nodes to only display the Surface.ByLoft display.

![](<../../.gitbook/assets/vase6 (1).png>)

> 3\. A surface is created by lofting the translated circles.&#x20;

## Results

We can now use the **Number Sliders** we defined in our script to create different vase designs.&#x20;

![](../../.gitbook/assets/vase7.png)
