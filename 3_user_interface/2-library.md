# Library

The Library contains all of the loaded Nodes, including the ten default categories Nodes that come with the installation as well as any additionally loaded Custom Nodes or Packages. The Nodes in the Library are organized hierarchically within libraries, categories, and, where appropriate, subcategories.

![](images/3-2/library-libraryUI.jpg)

* Basic Nodes: Comes with default installation.
* Custom Nodes: Store your frequently used routines or special graph as Custom Nodes. You can also share your Custom Nodes with the community
* Nodes from the Package Manager: Collection of published Custom Nodes.

We will go through the [hierarchy of Nodes](3-3\_dynamo\_libraries.md#library-hierarchy-for-categories) categories, show how you can [search quickly from the library](3-3\_dynamo\_libraries.md#quick-search-in-library) and learn about some of the [frequently used Nodes](3-3\_dynamo\_libraries.md#frequently-used-nodes) among them.

### Library Hierarchy for Categories

Browsing through these categories is the fastest way to understand the hierarchy of what we can add to our Workspace and the best way to discover new Nodes you haven't used before.

Browse the Library by clicking through the menus to expand each category and its subcategory

{% hint style="info" %}
Geometry are great menus to begin exploring as they contain the largest quantity of Nodes.
{% endhint %}

![](images/3-2/library-modifiedandresizelibrarycategories.jpg)

> 1. Library
> 2. Category
> 3. Subcategory
> 4. Node

These further categorize the Nodes among same subcategory based on whether the Nodes **Create** data, execute an **Action**, or **Query** data.

* ![](images/3-2/userinterface-create.jpg) **Create**: Create or construct a geometry from scratch. E.g a circle.
* ![](images/3-2/userinterface-action.jpg) **Action**: Perform an action on an object. E.g scaling a circle.
* ![](images/3-2/userinterface-query.jpg) **Query**: Get a property of an object that already exists. E.g get the radius of a circle.

Hover your mouse over a Node to reveal more detailed information beyond its name and icon. This offers us a quick way to understand what the Node does, what it will require for inputs, and what it will give as an output.

![](images/3-2/userinterface-nodedescription.jpg)

> 1. Description - plain language description of the Node
> 2. Icon - larger version of the icon in the Library Menu
> 3. Input(s) - name, data type, and data structure
> 4. Output(s) - data type and structure

### Quick search in Library

If you know with relative specificity which Node you want to add to your Workspace, type in the **Search** field to look up all matching Nodes.

Choose by clicking on the Node you wish to add or hit Enter to add highlighted nodes to the center of the Workspace.

![](images/3-2/userinterface-search.jpg)

#### Search by hierarchy

Beyond using keywords to try to find Nodes, we can type the hierarchy separated with a period in the Search Field or with Code Blocks (which use the _Dynamo textual language_).

The hierarchy of each library is reflected in the Name of Nodes added to the Workspace.

Typing in different portions of the Node's place in the Library hierarchy in the `library.category.nodeName` format returns different results

* `library.category.nodeName`

![](images/3-2/library-searchbyhierarchygeometrypointbycoordinates(1).jpg)

* `category.nodeName`

![](images/3-2/library-searchbyhierarchy2pointbycoordinates.jpg)

* `nodeName` or `keyword`

![](images/3-2/library-searchbyhierarchy3bycoordinates.jpg)

Typically the Name of the Node in the Workspace will be rendered in the `category.nodeName` format, with some notable exceptions particularly in the Input and View Categories.

Beware of similarly named Nodes and note the category difference:

* Nodes from most libraries will include the category format

![](images/3-2/library-nodecategorydifferences1.jpg)

* `Point.ByCoordinates` and `UV.ByCoordinates` have the same Name but come from different categories

![](images/3-2/library-nodecategorydifferences2.jpg)

* Notable exceptions include Built-in Functions, Core.Input, Core.View, and Operators

![](images/3-2/library-nodecategorydifferences3.jpg)

### Frequently Used Nodes

With hundreds of Nodes included in the basic installation of Dynamo, which ones are essential for developing our Visual Programs? Let's focus on those that let us define our program's parameters (**Input**), see the results of a Node's action (**Watch**), and define inputs or functionality by way of a shortcut (**Code Block**).

#### Input Nodes

Input Nodes are the primary means for the User of our Visual Program - be that yourself or someone else - to interface with the key parameters. Here are some available from the Core Library:

| Node           |                                                | Node           |                                                |
| -------------- | ---------------------------------------------- | -------------- | ---------------------------------------------- |
| Boolean        | ![](images/3-2/library-boolean.jpg)        | Number         | ![](images/3-2/library-number.jpg)         |
| String         | ![](images/3-2/library-string.jpg)         | Number Slider  | ![](images/3-2/library-numberslider.jpg)  |
| Directory Path | ![](images/3-2/library-directorypath.jpg) | Integer Slider | ![](images/3-2/library-integerslider.jpg) |
| File Path      | ![](images/3-2/library-filepath.jpg)      |                |                                                |

#### Watch & Watch3D

The Watch Nodes are essential to managing the data that is flowing through your Visual Program. You can view the result of a Node through the **Node data preview** by hovering your mouse over the node.

![](images/3-2/library-nodepreview.jpg)

It will be useful to keep it revealed in a **Watch** Node

![](images/3-2/library-watchnode.jpg)

Or see the geometry results through a **Watch3D** Node.

![](images/3-2/library-watch3dnode.gif)

Both of these are found in the View Category in the Core Library.

{% hint style="info" %}
Tip: Occasionally the 3D Preview can be distracting when your Visual Program contains a lot of Nodes. Consider unchecking the Showing Background Preview option in the Settings Menu and using a Watch3D Node to preview your geometry.
{% endhint %}

#### Code Block

Code Block Nodes can be used to define a block of code with lines separated by semi-colons. This can be as simple as `X/Y`.

We can also use Code Blocks as a shortcut to defining a Number Input or call to another Node's functionality. The syntax to do so follows the Naming Convention of the Dynamo textual language, [DesignScript](../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md).

Here is a simple demonstration (with instructions) for using Code Block in your script.

![](images/3-2/library-codeblockdemo.gif)

1. Double-click to create a Code Block Node
2. Type `Circle.ByCenterPointRadius(x,y);`
3. Click on Workspace to clear the selection should add `x` and `y` inputs automatically.
4. Create a Point.ByCoordinates Node and a Number Slider then connect them to the inputs of the Code Block.
5. The result of the executing the Visual Program is shown as the circle in the 3D Preview
