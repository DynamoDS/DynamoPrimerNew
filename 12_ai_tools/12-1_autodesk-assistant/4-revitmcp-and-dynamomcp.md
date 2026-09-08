# RevitMCP and DynamoMCP

Autodesk Assistant gets its abilities from MCP servers. Each server gives Assistant a set of tools, and each tool does one job in the product it belongs to.

When you work in Revit, two of those servers matter: **RevitMCP** and **DynamoMCP**. They are not competing options, and you do not choose between them. They work at different levels, and Assistant uses whichever one fits your request.

### RevitMCP: direct model operations

RevitMCP wraps a curated set of Revit API operations as MCP tools. Each tool maps to a defined action in the model, such as reading elements or querying model data.

This is the shortest path for a task the tool set already covers. One request becomes one call, and the model changes immediately. There is no graph to build and nothing to run afterward.

### DynamoMCP: tools that build the operation

DynamoMCP works one level up. Its tools do not perform modeling actions directly. Instead, they author a Dynamo graph: they place nodes, wire them together, set values, and run the result. See [What is Dynamo MCP server](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md) for the full tool list.

This matters because of what a node is. Every node in the Dynamo library is itself one or more API calls, already written, tested, and documented. Package nodes and Python nodes are available in the same way. So a small set of graph-authoring tools reaches the entire Dynamo library.

The effect is that DynamoMCP's tools produce new capability rather than only using existing capability. Instead of a fixed list of actions, Assistant assembles the action you asked for out of nodes. In practice, MCP tools that write graphs behave like tools that create more tools.

### The difference in practice

Say you ask for door tags on one level.

- Handled by RevitMCP, the work is a direct call, and the tags appear.
- Handled by DynamoMCP, Assistant places the nodes that collect the doors, filter them to the level you named, and create the tags, then runs the graph.

Both routes tag the doors. The second route also leaves you something on the canvas.

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| What a tool does | Performs a defined Revit operation | Builds and runs a Dynamo graph |
| Reach | The operations included in the tool set | Any node in your library, including package and Python nodes |
| What you get back | The change in the model | The change in the model, plus the graph that made it |
| Best for | A single, well-defined task | Logic you want to inspect, adjust, reuse, or share |

### Why the graph is worth having

A graph is a record of the work, not just its result. Once it is on the canvas, you can:

- Read it, and see exactly what was done.
- Change a value and run it again, without asking for the whole task a second time.
- Save it and run it on the next project, or hand it to a colleague.
- Publish it to Dynamo Player so people who do not use Dynamo can run it.

The first request is where the time goes. Every run after that is nearly free.

{% hint style="info" %}
In Dynamo Sandbox, Assistant reaches DynamoMCP only, because there is no Revit model to act on. In Revit, both are available.
{% endhint %}

### Choosing what to ask for

You do not need to name a server in your prompt. Describing the outcome is enough, and Assistant selects the tools.

If you want a graph specifically, say so. Prompts such as *"build me a graph that tags the doors on Level 2"* tell Assistant that the reusable version is the point, not only the tagged doors.
