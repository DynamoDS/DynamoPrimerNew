
# Connecting a desktop AI to an MCP

There are several AI desktop apps available and most likely each one will have a slightly different workflow for connecting to the MCPs. For some application specific walkthroughs, the following links are of use:

[Connecting to MCP with Claude Desktop](https://support.claude.com/en/articles/10949351-getting-started-with-local-mcp-servers-on-claude-desktop)

[Connecting to MCP with OpenAI Desktop](https://developers.openai.com/api/docs/guides/tools-connectors-mcp)

[Connecting to MCP with Cursor](https://cursor.com/automate)


Point the desktop app at it. The desktop app will read a configuration file (something like: model_desktop_config.json) that lists available MCP servers under an mcpServers key, with the command needed to launch the server process. Some newer setups use an installer script that writes this config for you; others require pasting in a JSON block by hand, something like:

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

Restart the desktop app. Once it picks up the new server, it should be available to connect with.
Open Revit with a project loaded. The MCP server needs a live Revit session to talk to — most of these connections run entirely over localhost, so nothing about your model leaves your machine.
Test the connection. Ask the model something simple first, like "what tools do you have available for Revit?" or "list the levels in this project." If it comes back with real data from your open file, you're connected.

### MCP With Dynamo

Once the connection is live, the useful pattern for Dynamo work looks like this:

Select an element in Revit, then ask the model to adjust an open Dynamo script to match it — for example, resizing a parametric surface to fit a selected wall's actual width, height, and base constraints. the model reads the live parameters through the MCP server and edits the script's driving values accordingly, rather than you doing it by hand.
Ask for a graph from scratch, describing the outcome you want (e.g., "generate a Dynamo Python script that places a family instance at every intersection of these two curve grids"). the model can use model context to ground the script in your actual project rather than generic placeholder values.
Debug an existing graph by having the model inspect both the script and the model state it's operating on, which tends to surface mismatches — wrong units, unset origins, missing parameters — much faster than manual troubleshooting.


### Practical Limitations


Session limits are real. Large models and long multi-query sessions can burn through context quickly. Breaking work into focused sessions — one floor, one discipline, one task — keeps things responsive.
Direct code execution is powerful but not bulletproof. Servers that let the model generate and run Revit API code on the fly are flexible, but success rates drop on complex, multi-step requests. Simpler, well-scoped asks work best.
Everything stays local. These connections run over 127.0.0.1 between the model, the MCP server, and Revit — your model data doesn't get uploaded anywhere as part of the connection itself.

The bigger shift

The interesting part isn't that any single task gets faster, though most do. It's that this changes when you can ask certain questions. Code compliance checks, egress analysis, or parametric adjustments that used to wait until a design was nearly finished can now happen at every iteration, because the AI is reading the live model instead of a stale export. For Dynamo users in particular, that means less time spent wiring nodes for one-off adjustments and more time spent on the graphs that are actually worth building as reusable tools.

If you're new to this, start small: get one MCP server connected, open a simple existing graph, and ask the model to explain or tweak it based on something selected in the model. Once that loop feels natural, the more ambitious workflows — generating graphs from scratch, cross-checking architectural and structural models, or automating documentation — are a straightforward extension of the same connection.