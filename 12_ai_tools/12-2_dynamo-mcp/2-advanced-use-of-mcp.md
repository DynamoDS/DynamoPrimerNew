
# What is Dynamo MCP server

**MCP** stands for Model Context Protocol. An **MCP** is effectively code that is built into a program that gives AI models an interface to interact with a piece of software.  

Autodesk Revit will come with an in-built MCP server from 2027 onwards

The Dynamo MCP server is currently only available to those who are on the alpha testing server.

https://blog.bimsmith.com/Revit-2027-What-the-Built-In-MCP-Server-Actually-Does-in-Practice

It's the connecting piece that allows an AI model to talk directly to a live Revit model instead of working blind from whatever you happen to paste into a chat window. For Dynamo users specifically, this opens up a genuinely different way of working: instead of hand-building every node graph, you can describe what you want, let the AI read the actual model or script, and have it write or fix the graph for you.

MCP is an open standard that lets an AI model connect to external tools and data sources through a common protocol, rather than needing custom-built integrations for every piece of software. In the Revit/Dynamo world, that means an MCP server acts as a translator sitting between the model and the Revit API (or the Dynamo engine): the model sends a request, the server turns it into an actual API call inside your open Revit session, and the result comes back as structured data the model can reason about.

The practical effect is that the model stops guessing. Instead of you copying wall dimensions or parameter values into the chat by hand, the model can query the live model directly — element IDs, geometry, parameters, project info — and use that real data to write, debug, or adapt a Dynamo graph on the spot.

The current landscape

There isn't one single official "Dynamo MCP server" yet, but the space has moved fast and there are several solid paths depending on what you need:

Autodesk's built-in MCP server (Revit 2027). Recent Revit releases ship with a native MCP server that exposes the live model to MCP-compatible clients, including the model Desktop and the model Code. It's aimed at general model interrogation — floor inventories, egress analysis, code compliance checks — but it also works well for feeding real element data into a Dynamo Python or DesignScript node, since the model can pull the exact parameters a script needs before touching Dynamo at all.
Community Revit MCP servers. Projects like the open-source revit-mcp server (paired with a Revit add-in/plugin) and Python-based servers built on pyRevit have been around longer and offer a wider, more customizable toolset. Some expose pre-built tools for common tasks; others let the model generate and execute Revit API code directly, which is powerful but has a lower success rate on complex requests.
Dynamo's own agentic workflows initiative. The Dynamo team has publicly signaled work on bringing MCP-based, agentic capabilities natively into Dynamo itself, aiming to let the tool reason about and modify graphs more intelligently rather than relying entirely on an external client. This is still maturing, so for now most real-world Dynamo+AI workflows go through a Revit-side MCP server rather than Dynamo directly.


For most people getting started, the practical entry point is: connect an MCP server to Revit, open your Dynamo graph alongside it, and use the model to bridge the two.

Setting it up

The general shape of the setup is the same across most of these servers:


This is usually a small local application — a Node.js or Python process, sometimes bundled with a pyRevit extension or a dedicated Revit add-in (a .dll in the case of C#-based bridges). Installers typically handle dependency setup (like uv for Python-based servers) automatically now, so this is less painful than it was a year ago.
Point the model Desktop at it. the model Desktop reads a configuration file (the model_desktop_config.json) that lists available MCP servers under an mcpServers key, with the command needed to launch the server process. Some newer setups use an installer script that writes this config for you; others require pasting in a JSON block by hand, something like:

```json
    {
  "mcpServers": {
    "revit-mcp": {
      "command": "node",
      "args": ["<path-to-project>/build/index.js"]
    }
  }
}
```

Restart the model Desktop. Once it picks up the new server, you'll see a small connector or hammer icon appear in the chat interface, confirming the tools are available.
Open Revit with a project loaded. The MCP server needs a live Revit session to talk to — most of these connections run entirely over localhost, so nothing about your model leaves your machine.
Test the connection. Ask the model something simple first, like "what tools do you have available for Revit?" or "list the levels in this project." If it comes back with real data from your open file, you're connected.


Using it with Dynamo specifically

Once the connection is live, the useful pattern for Dynamo work looks like this:


Select an element in Revit, then ask the model to adjust an open Dynamo script to match it — for example, resizing a parametric surface to fit a selected wall's actual width, height, and base constraints. the model reads the live parameters through the MCP server and edits the script's driving values accordingly, rather than you doing it by hand.
Ask for a graph from scratch, describing the outcome you want (e.g., "generate a Dynamo Python script that places a family instance at every intersection of these two curve grids"). the model can use model context to ground the script in your actual project rather than generic placeholder values.
Debug an existing graph by having the model inspect both the script and the model state it's operating on, which tends to surface mismatches — wrong units, unset origins, missing parameters — much faster than manual troubleshooting.


A few practical notes


Session limits are real. Large models and long multi-query sessions can burn through context quickly. Breaking work into focused sessions — one floor, one discipline, one task — keeps things responsive.
Direct code execution is powerful but not bulletproof. Servers that let the model generate and run Revit API code on the fly are flexible, but success rates drop on complex, multi-step requests. Simpler, well-scoped asks work best.
Everything stays local. These connections run over 127.0.0.1 between the model, the MCP server, and Revit — your model data doesn't get uploaded anywhere as part of the connection itself.


The bigger shift

The interesting part isn't that any single task gets faster, though most do. It's that this changes when you can ask certain questions. Code compliance checks, egress analysis, or parametric adjustments that used to wait until a design was nearly finished can now happen at every iteration, because the AI is reading the live model instead of a stale export. For Dynamo users in particular, that means less time spent wiring nodes for one-off adjustments and more time spent on the graphs that are actually worth building as reusable tools.

If you're new to this, start small: get one MCP server connected, open a simple existing graph, and ask the model to explain or tweak it based on something selected in the model. Once that loop feels natural, the more ambitious workflows — generating graphs from scratch, cross-checking architectural and structural models, or automating documentation — are a straightforward extension of the same connection.