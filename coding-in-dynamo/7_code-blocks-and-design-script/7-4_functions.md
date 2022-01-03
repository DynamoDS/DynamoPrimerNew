# Functions

Functions can be created in a code block and recalled elsewhere in a Dynamo definition. This creates another layer of control in a parametric file, and can be viewed as a text-based version of a custom node. In this case, the "parent" code block is readily accessible and can be located anywhere on the graph. No wires needed!

## Parent

The first line has the key word “def”, then the function name, then the names of inputs in parentheses. Braces define the body of the function. Return a value with “return =”. Code Blocks that define a function do not have input or output ports because they are called from other Code Blocks. ![Parents](../../.gitbook/assets/21.png)

```
/*This is a multi-line comment,
which continues for
multiple lines*/
def FunctionName(in1,in2)
{
//This is a comment
sum = in1+in2;
return sum;
};
```

## Children

Call the function with another Code Block in the same file by giving the name and the same number of arguments. It works just like the out-of-the-box nodes in your library.

![Children](../../.gitbook/assets/20.png)

```
FunctionName(in1,in2);
```

## Exercise

> Download the example file that accompanies this exercise (Right click and "Save Link As..."). A full list of example files can be found in the Appendix. [Functions\_SphereByZ.dyn](https://github.com/h-iL/ForkedDynamoPrimerReorganized/blob/main/07\_Code-Block/datasets/7-4/Functions\_SphereByZ.dyn)

In this exercise, we will make a generic definition that will create spheres from an input list of points. The radius of these spheres are driven by the Z property of each point.

![Exercise](../../.gitbook/assets/11.jpg)

> Let's begin with a number range of ten values spanning from 0 to 100. Plug these into a _Point.ByCoordinates_ nodes to create a diagonal line.

![Exercise](<../../.gitbook/assets/10 (2).jpg>)

> 1. Create a _code block_ and introduce our definition by using the line of code:

```
def sphereByZ(inputPt){
};
```

The _inputPt_ is the name we've given to represent the points that will drive the function. As of now, the function isn't doing anything, but we'll build up this function in the steps to come.

![Exercise](<../../.gitbook/assets/09 (4).jpg>)

> 1. Adding to the _code block_ function, we place a comment and a _sphereRadius_ variable which queries the _Z_ position of each point. Remember, _inputPt.Z_ does not need parenetheses as a method. This is a _query_ of an existing element's properties, so no inputs are necessary:

```
def sphereByZ(inputPt,radiusRatio)
{
//get Z Value, use it to drive radius of sphere
sphereRadius=inputPt.Z;
};
```

![Exercise](../../.gitbook/assets/08.jpg)

> 1. Now, let's recall the function we've created in another _code block_. If we double-click on the canvas to create a new _code block_, and type in _sphereB_, we notice that Dynamo suggest the _sphereByZ_ function that we've defined. Your function has been added to the intellisense library! Pretty cool.

![Exercise](<../../.gitbook/assets/07 (4).jpg>)

> 1. Now we call the function and create a variable called _Pt_ to plug in the points created in the earlier steps:

```
sphereByZ(Pt)
```

1. We notice from the output that we have all null values. Why is this? When we defined the function, we are calculating the _sphereRadius_ variable, but we did not define what the function should _return_ as an _output_. We can fix this in the next step.

![Exercise](<../../.gitbook/assets/06 (6).jpg>)

> 1. An important step, we need to define the output of the function by adding the line \`\`\`return = sphereRadius;

````
2. Now we see that the output of the *code block* gives us the Z coordinates of each point.


![Exercise](images/7-4/Exercise/05.jpg)
> Let's create actual spheres now by editing the *Parent* function.
1. We first define a sphere with the line of code:
```sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);
````

1. Next, we change the return value to be the _sphere_ instead of the _sphereRadius_: \`\`\`return = sphere;

````
![Exercise](images/7-4/Exercise/04.jpg)
>1. To temper the size of these spheres, let's update the *sphereRadius* value by adding a divider: ```sphereRadius = inputPt.Z/20;
```.  Now we can see the separate spheres and start to make sense of the relationship between radius and Z value.

![Exercise](images/7-4/Exercise/03.jpg)
>1. On the *Point.ByCoordinates* node, by changing the lacing from *Shortest List* to *Cross Product*, we create a grid of points.  The *sphereByZ* function is still in full effect, so the points all create spheres with radii based on Z values.

![Exercise](images/7-4/Exercise/02.jpg)
>1. And just to test the waters, we plug the original list of numbers into the X input for *Point.ByCoordinates*.  We now have a cube of spheres.
2. Note: if this takes a long time to calculate on your computer, try to change *#10* to something like *#5*.

![Exercise](images/7-4/Exercise/01.jpg)
> 1. Remember, the *sphereByZ* function we've created is a generic function, so we can recall the helix from an earlier lesson and apply the function to it.

![Exercise](images/7-4/Exercise/20.jpg)
> One final step: let's drive the radius ratio with a user defined parameter.  To do this, we need to create a new input for the function and also replace the *20* divider with a parameter.
1. Update the *sphereByZ* definition to:
````

def sphereByZ(inputPt,radiusRatio) { //get Z Value, use it to drive radius of sphere sphereRadius=inputPt.Z/radiusRatio; //Define Sphere Geometry sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius); //Define output for function return sphere; };`2. Update the children code blocks by adding a *ratio* variable to the input:`sphereByZ(Pt,ratio);

```
Plug a slider into the newly created code block input and vary the size of the radii based on the radius ratio.
```
