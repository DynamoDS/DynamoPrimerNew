# What is Dynamo MCP server

**MCP** stands for Model Context Protocol. It is an open standard that lets AI models connect to software through a common protocol. An MCP server exposes a program's tools and data so an AI client can read live context, take actions, and return structured results.

**DynamoMCP** is Dynamo's built-in MCP server. It connects Autodesk Assistant to your active Dynamo session — your graph, nodes, wires, and workspace state — so AI help is grounded in what you are actually working on.

For Dynamo users, that means Assistant can do more than answer general questions. It can inspect your open graph, help troubleshoot node issues, and update the canvas based on real session context instead of manual copy and paste.

DynamoMCP is available in Dynamo 4.2.0 and later.

### How to Use DynamoMCP

You use DynamoMCP through Autodesk Assistant — there is no separate setup or standalone interface. Assistant is the entry point for asking questions, getting help with your graph, and letting the AI work with live Dynamo context.

Where you open Assistant depends on where you are working:

- **Dynamo Sandbox** — Dynamo MCP tools are available in Autodesk Assistant inside Dynamo.
- **Revit** — Dynamo MCP tools are available in Autodesk Assistant inside Revit.

In both cases, you stay in your current session and use the same Assistant experience to reach DynamoMCP.
### What DynamoMCP can do

DynamoMCP gives Assistant a set of **tools**. Each tool does one job in Dynamo, such as reading the canvas, placing a node, or running the graph. You never call these tools yourself. You describe what you want in plain language, and Assistant picks the tools it needs.

Knowing what the tools are is still useful. It tells you what Assistant is able to do, and it helps you phrase requests it can act on.

{% hint style="info" %}
Tool names that start with `get_` only read information from your session. Every other tool can change your graph.
{% endhint %}

#### Read the current graph

These tools let Assistant see what you are working on before it answers or makes a change.

| Tool | What it does |
| --- | --- |
| `get_workspace_info` | Lists what is on the canvas: nodes, wires, groups, notes, and workspace details. |
| `get_selection` | Reports the nodes, wires, and groups you have selected. |
| `get_node_connections` | Shows what a node is wired to, port by port. |
| `get_node_property` | Reports node settings such as preview, freeze, pinned preview, and labels. |
| `get_node_lacing` | Reports the lacing setting of a node. |
| `get_installation_info` | Reports your Dynamo version, the host you are running in, and a link to the release notes. |

*Example prompt: "What is on my canvas right now?"*

#### Find the right node

Dynamo has thousands of nodes, including nodes from installed packages. These tools help Assistant choose the correct one instead of guessing.

| Tool | What it does |
| --- | --- |
| `get_nodes_info` | Searches the node library, including package nodes, and returns the exact node to place. |
| `get_node_help` | Returns the written help for a node, and the path to its example graph when one exists. |
| `get_type_schema` | Returns the expected data format for a Dynamo type, such as Point, Number, or Color. |

*Example prompt: "Which node do I use to get the area of a surface?"*

#### Build and edit the graph

These tools do the work on the canvas.

| Tool | What it does |
| --- | --- |
| `create_nodes` | Places one or more nodes on the canvas, with optional starting values. |
| `set_node_value` | Sets the value of input nodes such as Number, String, Code Block, and dropdowns. |
| `connect_nodes` | Wires an output port to an input port. |
| `disconnect_nodes` | Removes wires between two nodes. |
| `delete_node` | Removes a node from the canvas. |
| `rename_node` | Renames input and Watch nodes so their purpose is clear. Function nodes keep their original names. |
| `select_nodes` | Selects nodes on the canvas, so you can see which ones Assistant means. |
| `set_node_property` | Turns node settings on or off: preview, freeze, pinned preview, labels, and input or output. |
| `set_node_lacing` | Sets lacing to Auto, Shortest, Longest, or Cross Product. |
| `set_node_port_list_at_level` | Sets the list level on an input port. |

*Example prompt: "Add a number slider and wire it into the radius input of my circle."*

#### Organize and document the graph

A working graph is not always a readable one. These tools handle layout, grouping, colors, and notes.

| Tool | What it does |
| --- | --- |
| `create_group` | Puts a group around a set of nodes. |
| `create_groups` | Creates several groups in one step. |
| `ungroup_by_guid` | Removes a group. The nodes inside it stay on the canvas. |
| `ungroup_by_title` | Removes groups by their title. The nodes inside them stay on the canvas. |
| `get_available_group_styles` | Lists the group styles you can use, including custom styles from your preferences. |
| `assign_group_styles` | Applies group styles, such as Inputs, Actions, Outputs, and Review. |
| `get_notes` | Lists the notes in the graph. |
| `create_notes` | Adds notes that explain parts of the graph. |
| `update_notes` | Edits the text of a note, or moves it. |
| `delete_notes` | Removes notes. |
| `set_note_state` | Pins a note to a node so the two move together, or unpins it. |
| `auto_layout_workspace` | Rearranges the nodes using the built-in Dynamo auto layout. |
| `set_workspace_description` | Writes a description of the graph into the file's properties. |

*Example prompt: "Group and color-code this graph by inputs, actions, and outputs, then add short notes."*

#### Run the graph and check results

| Tool | What it does |
| --- | --- |
| `run_workspace` | Runs the current graph. In automatic mode the graph already re-runs on every change, so this step is skipped. |
| `get_workspace_warnings` | Lists the nodes with warnings or errors, the message text, and suggested next steps. The graph must run first. |
| `get_node_output_values_shapes` | Summarizes the output values of one node, or of every node in the graph. |
| `get_node_output_value` | Reads the full output of a single node, one page at a time, for results that are too large to summarize. |

*Example prompt: "Run the graph and tell me why the List.GetItemAtIndex node is showing a warning."*

#### Work with graph files

Assistant can also work with graphs on disk, not only the one you have open.

| Tool | What it does |
| --- | --- |
| `new_workspace` | Opens a new, empty graph. If the current graph has unsaved changes, you are asked to save or discard them first. |
| `save_workspace` | Saves the current graph. Without a location, it saves to your Dynamo folder in Documents. |
| `edit_graph_from_path` | Opens a saved graph for editing, without running it. |
| `insert_graph_from_path` | Merges a saved graph into your current canvas. This is how node example graphs are brought in. |
| `run_graph_from_path` | Runs a saved graph, with optional input values that you supply. |
| `run_graph_from_json` | Runs a graph supplied as Dynamo JSON, with no file on disk. |
| `get_graph_info_from_path` | Reads the inputs, outputs, and metadata of a saved graph without opening it. |
| `get_graph_info_from_json` | Reads the same details from a graph supplied as Dynamo JSON. |
| `get_graphs_info_from_path` | Scans a folder of graphs and reports what each one contains. |
| `get_available_graphs_folders` | Lists the folders Dynamo knows about that may contain graphs. |

*Example prompt: "Look through my Dynamo graphs folder and tell me which graphs place walls."*

{% hint style="warning" %}
Assistant can change your graph. Save your work before you ask for a large edit, and review the result before you save over an existing file.
{% endhint %}
