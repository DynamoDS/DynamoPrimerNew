
# What is Dynamo MCP server

**MCP** stands for Model Context Protocol. An **MCP** is effectively code that is built into a program that gives AI models an interface to interact with a piece of software.  

Autodesk Revit will come with an in-built MCP server from 2027 onwards

The Dynamo MCP server is currently only available to those who are on the alpha testing server.

https://blog.bimsmith.com/Revit-2027-What-the-Built-In-MCP-Server-Actually-Does-in-Practice

An MCP is the connecting piece that allows an AI model to talk directly to a live Revit model instead of working blind from whatever you happen to paste into a chat window. For Dynamo users specifically, this means letting the AI read the actual model or script, and having it write or fix the graph for you.

MCP is an open standard that lets an AI model connect to external tools and data sources through a common protocol, rather than needing custom-built integrations for every piece of software. In the Revit/Dynamo world, that means an MCP server acts as a translator sitting between the model and the Revit API (or the Dynamo engine): the model sends a request, the server turns it into an actual API call inside your open Revit session, and the result comes back as structured data the model can reason about.

The practical effect is that the model stops guessing. Instead of you copying wall dimensions or parameter values into the chat by hand, the model can query the live model directly — element IDs, geometry, parameters, project info — and use that real data to write, debug, or adapt a Dynamo graph on the spot.

# How do you use it

New Revit releases will ship with a native MCP server that exposes the live model to MCP-compatible clients, including desktop AI apps and IDE extensions. It's aimed at general model interrogation — floor inventories, egress analysis, code compliance checks — but it also works well for feeding real element data into a Dynamo Python or DesignScript node, since the model can pull the exact parameters a script needs before touching Dynamo at all.

Projects like the open-source revit-mcp server (paired with a Revit add-in/plugin) and Python-based servers built on pyRevit have been around longer and offer a wider, more customizable toolset. Some expose pre-built tools for common tasks; others let the model generate and execute Revit API code directly, which is powerful but has a lower success rate on complex requests.

The Dynamo team has publicly signaled work on bringing MCP-based, agentic capabilities natively into Dynamo itself, aiming to let the tool reason about and modify graphs more intelligently rather than relying entirely on an external client. This is still maturing, so for now most real-world Dynamo+AI workflows go through a Revit-side MCP server rather than Dynamo directly.