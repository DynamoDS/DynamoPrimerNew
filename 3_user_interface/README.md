# User Interface

### User Interface Overview

The User Interface (UI) for Dynamo is organized into five main regions. We will briefly cover the overview here and further explain the Workspace and Library in the following sections.

![](images/userinterface-ui.jpg)

> 1. Menus
> 2. Toolbar
> 3. Library
> 4. Workspace
> 5. Execution bar

### Menus

![](../.gitbook/assets/userinterface-menu\(1\).jpg)

Here are Menus for basic functionality of the Dynamo application. Like most Windows software, the first two menus related to managing files, operations for selection and content editing. The remaining menus are more specific to Dynamo.

#### Dynamo Menus

General info and settings can be found on the **Dynamo** drop down menu.

![](images/userinterface-dynamomenu.jpg)

> 1. About - Find out the Dynamo version installed on your machine.
> 2. Agreement to Collect Usability Data - This allows you to opt-in or out for sharing your user data to improve Dynamo.
> 3. Preferences - Includes settings such as define the application's decimal point precision and geometry render quality.
> 4. Exit Dynamo

#### Help

If you're stuck, check out the **Help** Menu. You may access one of the Dynamo reference websites through your internet browser.

![](images/userinterface-helpmenu.jpg)

> 1. Getting Started - A brief introduction to using Dynamo.
> 2. Interactive Guides -
> 3. Samples - Reference example files.
> 4. Dynamo Dictionary - Resource with documentation on all nodes.
> 5. Dynamo Website - View the Dynamo Project on GitHub.
> 6. Dynamo Project Wiki - Visit the wiki for learning about development using the Dynamo API, supporting libraries and tools.
> 7. Display Start Page - Return to the Dynamo start page when within a document.
> 8. Report A Bug - Open an Issue on GitHub.

### Toolbar

Dynamo's Toolbar contains a series of buttons for quick access to working with files as well as Undo \[Ctrl + Z] and Redo \[Ctrl + Y] commands. On the far right is another button that will export a snapshot of the workspace, which is extremely useful for documentation and sharing.

* ![](images/userinterface-newfile.jpg) New - Create a new .dyn file
* ![](<images/userinterface-open(1) (1) (1).jpg>) Open - Open an existing .dyn (workspace) or .dyf (custom node) file
* ![](images/userinterface-save.jpg) Save/Save As - Save your active .dyn or .dyf file
* ![](images/userinterface-undo.jpg) Undo - Undo your last action
* ![](images/userinterface-redo.jpg) Redo - Redo the next action
* ![](images/userinterface-screenshot.jpg) Export Workspace as Image - Export the visible workspace as a PNG file

### Library

The Dynamo Library is a collection of functional libraries, each Library containing Nodes grouped by Category. It consists basic libraries which are added during default installation of Dynamo, as we continue to introduce its usage, we will demonstrate how to extend the base functionality with Custom Nodes and additional Packages. The [2-library.md](2-library.md "mention") section will cover a more detailed guidance on using it.

![](images/userinterface-library.jpg)

### Workspace

The Workspace is where we compose our visual programs, you may also change its Preview setting to view the 3D geometries from here. Refer [1-workspace.md](1-workspace.md "mention") for more details.

![](images/userinterface-workspace.gif)

### Execution Bar

Run your Dynamo script from here. Click the dropdown icon on the Execution button to change between the different modes.

![](images/userinterface-executionbar.gif)

* Automatic: Runs your script automatically. Changes are updated in realtime.
* Manual: Script only runs when the 'Run' button is clicked. Useful for making changes to a complicated and 'heavy' script.
* Periodic: This option is grayed out by default. Only available when the _DateTime.Now_ node is used. You can set the graph to run automatically at a specified interval.

![](images/userinterface-executionbarDateTimenode.jpg)
