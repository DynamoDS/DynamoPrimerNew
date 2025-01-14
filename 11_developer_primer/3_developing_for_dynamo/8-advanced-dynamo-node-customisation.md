# Advanced Dynamo Node Customisation

With a foundation knowledge of ZeroTouch already established. this section delves into the advantages of customising Dynamo nodes to enhance both functionality and user experience. By adding features such as warning messages, informational messages, and custom icons, you can create nodes that are more intuitive, informative, and visually engaging. These customisations not only help users understand potential issues or optimise their workflows but also make your nodes stand out as professional and user-friendly tools.

Customising nodes is an excellent way to ensure your solutions are clear, reliable, and tailored to meet specific project needs.

## Generating Custom Warning Messages Using OnLogWarningMessage <a href="#generating-custom-warning-messages-using-onlogwarningmessage" id="generating-custom-warning-messages-using-onlogwarningmessage"></a>

In Dynamo, the `OnLogWarningMessage` method provides a way to log warning messages directly to Dynamo's console. This is a powerful feature, especially for Zero Touch nodes, as it allows developers to alert users when there are issues with inputs or parameters that could lead to unexpected behavior. This guide will teach you how to implement `OnLogWarningMessage` in any Zero Touch node.

### Implementation Steps for `OnLogWarningMessage` <a href="#implementation-step-for-onlogwarningmessage" id="implementation-step-for-onlogwarningmessage"></a>

#### Step 1: Import the Required Namespace <a href="#import-the-required-namespace" id="import-the-required-namespace"></a>

`OnLogWarningMessage` is part of the `DynamoServices` namespace, so begin by adding this to your project file.

```
using DynamoServices;
```

#### Step 2: Identify When to Log Warnings <a href="#identify-when-to-log-warnings" id="identify-when-to-log-warnings"></a>

Before adding a warning message, consider the logic in your method:

* What conditions might cause incorrect or unexpected results?
* Are there specific input values or parameters that the method requires to function correctly?

Examples of conditions to check:

* **Out-of-range values** (e.g., `if (inputValue < 0)`).
* **Null or empty collections** (e.g., `if (list == null || list.Count == 0)`).
* **Data type mismatches** (e.g., if a file type is unsupported).

#### Step 3: Use `OnLogWarningMessage` to Log the Warning <a href="#use-onlogwarningmessage-to-log-the-warning" id="use-onlogwarningmessage-to-log-the-warning"></a>

Place `OnLogWarningMessage` calls where you detect conditions that could cause issues. When the condition is met, log a warning message that provides clear guidance to the user.

### Syntax for `OnLogWarningMessage` <a href="#syntax-for-onlogwarningmessage" id="syntax-for-onlogwarningmessage"></a>

```
LogWarningMessageEvents.OnLogWarningMessage("Your warning message here.");
```

### Example Implementations of `OnLogWarningMessage` <a href="#example-implementations-of-onlogwarningmessage" id="example-implementations-of-onlogwarningmessage"></a>

To demonstrate `OnLogWarningMessage` in action, here are different scenarios you might encounter when building a Zero Touch node.

#### Example 1: Validating Numeric Inputs <a href="#example-1-validating-numeric-inputs" id="example-1-validating-numeric-inputs"></a>

In this example, we'll build upon the custom node created in the previous "**Zero-Touch Case Study - Grid Node";** A Method named `RectangularGrid` that generates a grid of rectangles based on `xCount` and `yCount` inputs. We will walk through testing If an input is invalid, and then using `OnLogWarningMessage` to log a warning and stop processing.

![OnLogWarningMessage Example 1](images/onlogwarningmessage-example-1.png)

##### Using `OnLogWarningMessage` for Input Validation <a href="#using-onlogwarningmessage-for-input-validation" id="using-onlogwarningmessage-for-input-validation"></a>

When generating a grid based on `xCount` and `yCount`. You want to ensure both values are positive integers before proceeding.

```
public static List<Rectangle> CreateGrid(int xCount, int yCount)
{
    // Check if xCount and yCount are positive
    if (xCount <= 0 || yCount <= 0)
    {
        LogWarningMessageEvents.OnLogWarningMessage("Grid count values must be positive integers.");
        return new List<Rectangle>();  // Return an empty list if inputs are invalid
    }
    // Proceed with grid creation...
}
```

In this example:

* **Condition**: If either `xCount` or `yCount` is less than or equal to zero.
* **Message**: `"Grid count values must be positive integers."`

This will show the warning in Dynamo if a user enters zero or negative values, helping them understand the expected input. 

Now we know what this looks like, we can implement it into the Grids example node:

```
using Autodesk.DesignScript.Geometry;
using DynamoServices;

namespace CustomNodes
{
    public class Grids
    {
        // The empty private constructor.
        // This will not be imported into Dynamo.
        private Grids() { }

        /// <summary>
        /// This method creates a rectangular grid from an X and Y count.
        /// </summary>
        /// <param name="xCount">Number of grid cells in the X direction</param>
        /// <param name="yCount">Number of grid cells in the Y direction</param>
        /// <returns>A list of rectangles</returns>
        /// <search>grid, rectangle</search>
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10)
        {
            // Check for valid input values
            if (xCount <= 0 || yCount <= 0)
            {
                // Log a warning message if the input values are invalid
                LogWarningMessageEvents.OnLogWarningMessage("Grid count values must be positive integers.");
                return new List<Rectangle>(); // Return an empty list if inputs are invalid
            }

            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                    Point cPt = rect.Center();
                }
            }

            return pList;
        }
    }
}
```

##### Example 2: Checking for Null or Empty Collections <a href="#example-2-checking-for-null-or-empty-collections" id="example-2-checking-for-null-or-empty-collections"></a>

If your method requires a list of points but a user passes an empty or null list, you can use `OnLogWarningMessage` to inform them about the issue.

![OnLogWarningMessage Example 2](images/onlogwarningmessage-example-2.png)

```
public static Polygon CreatePolygonFromPoints(List<Point> points)
{
    if (points == null || points.Count < 3)
    {
        LogWarningMessageEvents.OnLogWarningMessage("Point list cannot be null or have fewer than three points.");
        return null;  // Return null if the input list is invalid
    }
    // Proceed with polygon creation...
}
```

In this example:

* **Condition**: If the `points` list is null or contains fewer than three points.
* **Message**: `"Point list cannot be null or have fewer than three points."`

This warns users that they need to pass a valid list with at least three points to form a polygon.

---

##### Example 3: Verifying File Type Compatibility <a href="#example-3-verifying-file-type-compatibility" id="example-3-verifying-file-type-compatibility"></a>

For a node that processes file paths, you may want to ensure that only certain file types are allowed. If an unsupported file type is detected, log a warning.

![OnLogWarningMessage Example 3](images/onlogwarningmessage-example-3.png)

```
public static void ProcessFile(string filePath)
{
    if (!filePath.EndsWith(".csv"))
    {
        LogWarningMessageEvents.OnLogWarningMessage("Only CSV files are supported.");
        return;
    }
    // Proceed with file processing...
}
```

In this example:

* **Condition**: If the file path doesn't end with ".csv".
* **Message**: `"Only CSV files are supported."`

This warns users to ensure they are passing a CSV file, helping prevent issues related to incompatible file formats.

## Adding Informational Messages with `OnLogInfoMessage` <a href="#adding-informational-messages-with-onloginfomessage" id="adding-informational-messages-with-onloginfomessage"></a>

In Dynamo, `OnLogInfoMessage` from the `DynamoServices` namespace lets developers log informational messages directly to Dynamo's console. This is helpful for confirming successful operations, communicating progress, or providing additional insights about node actions. This guide will teach you how to add `OnLogInfoMessage` in any Zero Touch node to enhance feedback and improve user experience.

### Implementation Steps for `OnLogInfoMessage` <a href="#implementation-steps-for-onloginfomessage" id="implementation-steps-for-onloginfomessage"></a>
#### Step 1: Import the Required Namespace <a href="#step-1-import-the-required-namespace" id="step-1-import-the-required-namespace"></a>

`OnLogInfoMessage` is part of the `DynamoServices` namespace, so begin by adding this to your project file.

#### Step 2: Identify When to Log Information <a href="#step-2-identify-when-to-log-information" id="step-2-identify-when-to-log-information"></a>

Before adding an info message, think about the purpose of your method:

* What information would be useful to confirm after an action is completed?
* Are there key steps or milestones within the method that users might want to know about?

Examples of useful confirmations:

* **Completion messages** (e.g., when a grid or model is fully created).
* **Details of processed data** (e.g., "10 items processed successfully").
* **Execution summaries** (e.g., parameters used in the process).

#### Step 3: Use `OnLogInfoMessage` to Log Informational Messages <a href="#step-3-use-onloginfomessage-to-log-informational-message" id="step-3-use-onloginfomessage-to-log-informational-message"></a>

Place `OnLogInfoMessage` calls at meaningful points in your method. When a key step or completion occurs, log an informational message to update the user on what happened.

### Syntax for `OnLogInfoMessage` <a href="#syntax-for-onloginfomessage" id="syntax-for-onloginfomessage"></a>

```
LogWarningMessageEvents.OnLogInfoMessage("Your info message here.");
```

### Example Implementations of `OnLogInfoMessage` <a href="#example-implementations-of-onloginfomessage" id="example-implementations-of-onloginfomessage"></a>

Here are different scenarios to demonstrate using `OnLogInfoMessage` in your Zero Touch nodes.

#### Example 1: Validating Numeric Inputs <a href="#example-1-validating-numeric-inputs" id="example-1-validating-numeric-inputs"></a>

In this example, we'll build upon the custom node created in the previous "**Zero-Touch Case Study - Grid Node";** A Method named `RectangularGrid` that generates a grid of rectangles based on `xCount` and `yCount` inputs. We will walk through testing If an input is invalid, and then using `OnLogInfoMessage` to provide info after the node has completed its run.

![OnLogInfoMessage Example 1](images/onloginfomessage-example-1.png)

###### Using `OnLogInfoMessage` for Input Validation <a href="#using-onloginfomessage-for-unput-validation" id="using-onloginfomessage-for-unput-validation"></a>

When generating a grid based on `xCount` and `yCount`. After generating the grid, you want to confirm its creation by logging an informational message with the grid's dimensions.

```
public static List<Rectangle> CreateGrid(int xCount, int yCount)
{
    var pList = new List<Rectangle>();
    // Grid creation code here...

    // Confirm successful grid creation
    LogWarningMessageEvents.OnLogInfoMessage($"Successfully created a grid with dimensions {xCount}x{yCount}.");

    return pList;
}
```

In this example:

* **Condition**: The grid creation process has completed.
* **Message**: `"Successfully created a grid with dimensions {xCount}x{yCount}."`

This message will inform users that the grid was created as specified, helping them confirm that the node worked as expected.

Now we know what this looks like, we can implement it into the Grids example node:

```
using Autodesk.DesignScript.Geometry;
using DynamoServices;

namespace CustomNodes
{
    public class Grids
    {
        // The empty private constructor.
        // This will not be imported into Dynamo.
        private Grids() { }

        /// <summary>
        /// This method creates a rectangular grid from an X and Y count.
        /// </summary>
        /// <param name="xCount">Number of grid cells in the X direction</param>
        /// <param name="yCount">Number of grid cells in the Y direction</param>
        /// <returns>A list of rectangles</returns>
        /// <search>grid, rectangle</search>
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                    Point cPt = rect.Center();
                }
            }

            // Log an info message indicating the grid was successfully created
            LogWarningMessageEvents.OnLogInfoMessage($"Successfully created a grid with dimensions {xCount}x{yCount}.");

            return pList;
        }
    }
}
```

#### Example 2: Providing Data Count Information <a href="#example-2-providing-data-count-information" id="example-2-providing-data-count-information"></a>

If you're creating a node that processes a list of points, you might want to log how many points were processed successfully. This can be useful for large datasets.

![OnLogInfoMessage Example 2](images/onloginfomessage-example-2.png)

```
public static List<Point> ProcessPoints(List<Point> points)
{
    var processedPoints = new List<Point>();
    foreach (var point in points)
    {
        // Process each point...
        processedPoints.Add(point);
    }

    // Log info about the count of processed points
    LogWarningMessageEvents.OnLogInfoMessage($"{processedPoints.Count} points were processed successfully.");

    return processedPoints;
}
```

In this example:

* **Condition**: After the loop completes, showing the count of processed items.
* **Message**: `"6 points were processed successfully."`

This message will help users understand the result of the processing and confirm that all points were processed.


#### Example 3: Summarizing Parameters Used <a href="#example-3-summarizing-parameters-used" id="example-3-summarizing-parameters-used"></a>

In some cases, it's useful to confirm the input parameters a node used to complete an action. For example, if your node exports data to a file, logging the file name and path can reassure users that the correct file was used.

![OnLogInfoMessage Example 3](images/onloginfomessage-example-3.png)

```
public static void ExportData(string filePath, List<string> data)
{
    // Code to write data to the specified file path...

    // Log the file path used for export
    LogWarningMessageEvents.OnLogInfoMessage($"Data exported successfully to {filePath}.");

}
```

In this example:

* **Condition**: The export process completes successfully.
* **Message**: `"Data exported successfully to {filePath}."`

This message confirms to users that the export worked and shows the exact file path, helping avoid confusion over file locations.

## Creating and Adding Custom Documentation to Nodes

### Custom node documentation
Historically, there has been limitations in Dynamo for how package authors could provide documentation for their nodes. Custom Node authors have been restricted to only allowing a short description that displays in the tooltip of the node, or to ship your package with heavily annotated sample graphs.

![Node Tooltip Description](images/customnodedocumentation-overloads.png)

### A new way
Dynamo now offers an improved system for package authors to provide better and more explanatory documentation for your custom nodes. This new approach utilises the user-friendly Markdown language for text authoring and the Documentation Browser view extension to display the Markdown in Dynamo. Using Markdown gives package authors a wide array of new possibilities when documenting their custom nodes. 

#### What is Markdown?
Markdown is a lightweight markup language that you can use to format plaintext text documents. Since Markdown was created in 2004 it's popularity has only increased and is now one of the most popular markup languages in the world.

#### Getting started with Markdown
It's easy to start making Markdown files - all you need is a simple text editor, like Notepad, and you're good to go. However, there are easier ways to write Markdown than using Notepad. There are several online editors, such as [Dillinger](https://dillinger.io/), that let you see your changes in real-time as you make them. Another popular way of editing Markdown files is using a code editor like [Visual studio code](https://code.visualstudio.com/).

#### What can Markdown do?
Markdown is very flexible and should provide enough functionality to easily create good documentation - this includes; adding media files like images or videos, creating tables with different forms of content, and of course simple text formatting features like making text **bold** or *italic*. All of this and so much more is possible when writing Markdown documents, for more information have a look at this guide which explains the 
[basic Markdown syntax](https://www.Markdownguide.org/basic-syntax/).

### Adding extended documentation to your nodes
Adding documentation to your nodes is easy. Documentation can be added to all flavors of custom nodes, covering:
* Out of the Box Dynamo Nodes
* Custom Nodes (.dyf) - Collections of out of the box and/or other package nodes.
* Custom C# Package Nodes (Also known as Zerotouch. These custom nodes look like the out-of-the-box nodes)
* NodeModel Nodes (Nodes that contain special UI features such as drop downs or selection buttons)
* NodeModel Nodes with Custom UI (Nodes that contain unique UI features such as graphics on the node)

Follow these few steps to get your Markdown files to show inside Dynamo.

#### Opening documentation files in Dynamo
Dynamo uses the Documentation Browser view extension to display nodes documentation. To open a nodes documentation, right click on the node and select help. This will open up the Documentation Browser and display the Markdown associated with that node, if there is any provided.

![Documentation Browser](images/customnodedocumentation-no-documentation-provided.png)

The documentation displayed in the Documentation Browser is made up of two parts. The first is the `Node Info` section, this is auto generated from the information extracted from the node, such as the inputs/outputs, node category, node name/namespace and the nodes short description. Second part show the custom nodes documentation, which is the Markdown file that is provided to document the node.

![Custom Node Documentation](images/customnodedocumentation-custom-node-documentation.png)

#### Package doc folder
To add documentation files to your nodes in Dynamo, create a new folder in your package directory called `/doc`. When your package is loaded, Dynamo will scan this directory and get all of the documentation Markdown files in it.

#### Naming Markdown files
To make sure Dynamo knows which file to open when requested for a specific node, the naming of the Markdown file needs to be in a specific format. Your Markdown file should be named according to the node it documents' namespace. If you are unsure about the namespace of the node, look at the `Node Info` section when you press `Help` on the node and under the node name you will see the full namespace of the selected node. 

This namespace should be the name of your Markdown file for that particular node. For example the namespace of `CustomNodeExample` from above images, is `TestPackage.TestCategory.CustomNodeExample` therefore the Markdown file for this node should be named `TestPackage.TestCategory.CustomNodeExample.md`

In special cases where you have overloads of your nodes (nodes with same name, but different inputs), you will have to add the input names in `()` after the node namespace. For example the built-in node `Geometry.Translate` has multiple overloads. In this case we would name the Markdown files for below nodes as follows:
`Autodesk.DesignScript.Geometry.Geometry.Translate(geometry,direction).md`
`Autodesk.DesignScript.Geometry.Geometry.Translate(geometry,direction,distance).md`

![Overload Nodes](images/customnodedocumentation-overloads.png)

#### Modifying Markdown files while open in Dynamo
To make it easy to modify documentation files, the Documentation Browser implements a File Watcher on the open documentation file. This enables you to make changes to your Markdown file and you will see the changes in Dynamo instantly. 

![Hot Reloading](images/customnodedocumentation-hot-reload.gif)

Adding new documentation files can also be done whilst Dynamo is open. Simply add a new Markdown file to the `/doc` folder, with a name that corresponds to the node it documents.

## Adding Custom Icons to Zero Touch Nodes

### Overview

Custom icons for Zero Touch nodes in Dynamo make your nodes visually distinctive and easier to recognise in the Library. By adding bespoke icons, you can make your nodes stand out from the rest, allowing users to quickly identify them in a list.

This guide will show you how to add icons to your Zero Touch nodes.


### Steps to Add Custom Node Icons

#### Step 1: Set Up Your Project

To begin, create a Visual Studio Class Library (.NET Framework) project for your Zero Touch nodes. If you don't already have a project, refer to the **Getting Started** section for step-by-step instructions on creating one.

![Creating a new Visual Studio Project](images/vs-new-project-1.jpg)

![Configuring a new project in Visual Studio](images/zerotouchicons-configure-new-project.jpg)

Make sure you have at least one functional Zero Touch node, as icons can only be added to existing nodes. For guidance, see the **Zero Touch Case Study - Grid Node**.


#### Step 2: Create Your Icon Images

To create custom icons:

1. **Design Your Icons**: Use an image editor to create simple, visually clear icons for your nodes.
2. **Image Specifications**:
    * **Small Icon**: 32x32 pixels (used in the Library's sidebar and on the node itself).
    * **Large Icon**: 128x128 pixels (used in the node properties when hovering over the node in the library).
3. **File Naming Convention**:
    * The file names must match the format below to associate them with the correct node:
        * **`<ProjectName>.<ClassName>.<MethodName>.Small.png`** (for the small icon).
        * **`<ProjectName>.<ClassName>.<MethodName>.Large.png`** (for the large icon).

**Example**: If your project is `ZeroTouchNodeIcons`, your class is `Grids`, and your method is `RectangularGrid`, the files would be named:

* `ZeroTouchNodeIcons.Grids.RectangularGrid.Small.png`
* `ZeroTouchNodeIcons.Grids.RectangularGrid.Large.png`

> Tip: Stick to a consistent design theme across all your icons for a professional look.


#### Step 3: Add a Resources File to Your Project

To embed your icons into the `.dll`, create a resources file:

1. **Add a New Resources File**:

  * Right-click your project in the **Solution Explorer**.

![Adding a new item](images/zerotouchicons-add-resources-file-1.jpg)

  * Go to **Add > New Item** and select **Resources File**.

![Adding a resources file](images/zerotouchicons-add-resources-file-2.jpg)

  * Name the file `<ProjectName>Images.resx`. For example, `ZeroTouchNodeIconsImages.resx`.

2. **Clear the Custom Tool Property**:
    * Select the resources file in the **Solution Explorer**.
    * In the **Properties** panel, clear the `Custom Tool` field by removing the `ResXFileCodeGenerator` value.

![Cleaning the Custom Tool Property](images/zerotouchicons-custom-tool-property.jpg)

> *NOTE: Failing to clear the "Custom Tool" field will result in Visual Studio converting periods to underscores in your resource names. Please verify before Building that your resource names have periods separating class names rather than underscores.*


#### Step 4: Add Your Images as Resources

1. Open the resources file using the **Managed Resources Editor (Legacy)**:
    * If using Visual Studio 17.11 or later, right-click the resources file, choose **Open With**, and select **Managed Resources Editor (Legacy)**.
    * If using a Visual Studio version previous to 17.11, double click on the resources file to open with the Resources Editor (which in your version of Visual Studio has not been made legacy yet).

![Using Open With...](images/zerotouchicons-open-resource-editor.jpg)

![Opening the resources file with Managed Resources Editor (Legacy)](images/zerotouchicons-managed-resource-editor-legacy.jpg)

2. Add your images:
    * Drag and drop your image files into the editor or use the **Add Existing File** option.

![Adding Existing Files](images/zerotouchicons-add-existing-file.jpg)

3. Update persistence:
    * Select the images from within the Resources Editor (this will not work if you select them from the Solution Explorer), change the **Persistence** property in the **Properties** panel to `Embedded in .resx`. This ensures the images are included in your `.dll`.

![Updating Persistence](images/zerotouchicons-edit-persistence-property.jpg)

#### Step 5: Convert Your Project to SDK-Style

If your project is not already SDK-style (required for embedding resources), convert it:

1. Install the `.NET Upgrade Assistant` extension from Visual Studio's **Extensions > Manage Extensions** menu.

![Manage Extensions](images/zerotouchicons-manage-extensions.jpg)

![Installing the .NET Upgrade Assistant](images/zerotouchicons-net-upgrade-assistant.jpg)

2. Right-click the project in the **Solution Explorer** and select **Upgrade > Convert project to SDK-style**.

![Upgrade the project](images/zerotouchicons-upgrade-project.jpg)

![Convert to SDK-style](images/zerotouchicons-convert-to-sdk-style.jpg)

3. Wait for the conversion to complete.

![Upgrade Complete](images/zerotouchicons-upgrade-complete.jpg)


#### Step 6: Add an After-Build Script to Embed Resources

1. Unload the project:
    * Right-click the project in the **Solution Explorer** and select **Unload Project**.

![Unload the project](images/zerotouchicons-unload-project.jpg)

2. Edit the `.csproj` file:
    * Add the following `<Target>` element between `</ItemGroup>` and `</Project>`:

```
<Target Name="CreateNodeIcons" AfterTargets="PostBuildEvent">
		<!-- Get System.Drawing.dll     -->
		<GetReferenceAssemblyPaths TargetFrameworkMoniker=".NETFramework, Version=v4.8">
			<Output TaskParameter="FullFrameworkReferenceAssemblyPaths" PropertyName="FrameworkAssembliesPath" />
		</GetReferenceAssemblyPaths>
		<!-- Get assembly -->
		<GetAssemblyIdentity AssemblyFiles="$(OutDir)$(TargetName).dll">
			<Output TaskParameter="Assemblies" ItemName="AssemblyInfo" />
		</GetAssemblyIdentity>
		<!-- Generate customization dll -->
		<GenerateResource SdkToolsPath="$(TargetFrameworkSDKToolsDirectory)" UseSourcePath="true" Sources="$(ProjectDir)ZeroTouchNodeIconsImages.resx" OutputResources="$(ProjectDir)ZeroTouchNodeIconsImages.resources" References="$(FrameworkAssembliesPath)System.Drawing.dll" />
		<AL SdkToolsPath="$(TargetFrameworkSDKToolsDirectory)" TargetType="library" EmbedResources="$(ProjectDir)ZeroTouchNodeIconsImages.resources" OutputAssembly="$(OutDir)ZeroTouchNodeIcons.customization.dll" Version="%(AssemblyInfo.Version)" />
	</Target>
```
![Adding the After Build code](images/zerotouchicons-after-build.jpg)
1. Replace all instances of `ZeroTouchNodeIcons` with your project name.
2. Reload the project:
    * Right-click the unloaded project and select **Reload Project**.

![Reload the project](images/zerotouchicons-reload-project.jpg)


#### Step 7: Build and Load Your .dll into Dynamo

1. Build the project:
    * After adding the After-Build script, build your project in Visual Studio.

![Build Solution](images/zerotouchicons-build-solution.jpg)

2. Check for output files:
    * Ensure your `.dll` and the `.customization.dll` are in the `bin` folder.
3. Add the `.dll` to Dynamo:
    * In Dynamo, use the Import Library button to import your .dll into Dynamo.

![Import Library Button](images/zerotouchicons-icon-in-dynamo.jpg)

4. Your custom nodes should now appear with their respective icons.