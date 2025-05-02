# Volar - A New Task Tool
Volar is a task management tool for your programming needs. The idea is to provide a flexible way for
* Persisting & organizing context 
* Human & AI alignment and collaboration
* Enable an async workflow where humans don't need to wait on AI as much

In the demo I will walk you through how to leverage Volar to go from: one-line idea -> detailed plan -> sub tasks (if needed) -> implementation by AI.

# Demo
https://youtu.be/mrdtMFBqyWM

# Download
Download the latest extension package: https://github.com/linshu123/volar_docs/blob/main/volar-0.0.1.vsix

# Usage
### Task Management
The creation and curation of tasks should be self-evident. You can of course also ask AI to make changes or organize tasks, such as "find all the tasks that are describing a bug and add [Bug] prefix to their title". 

### How AI helps with Task Work
There are 3 key features for asking AI to work on a task: "Flesh out", "Execute", and "Breakdown".

**Flesh out:** With just a few words in the task title, you can use "Flesh out" to get AI to help you expand on that task. AI will help you investigate the codebase and add plans and implementation suggestions to the task. Then you can review that plan. If it all makes sense, you can use **Execute** to actually implement the task.

**Execute:** This will ask AI to go straight into implementation mode. Ideally, there is already some plans and details in the task that you are confident about. AI will follow those plans and make the code changes.

**Breakdown:** If you have a complex task that you feel AI won't be able to accomplish in one go, you can use this option to break down the task into multiple smaller tasks. You can recursively do this until you feel each task is small enough for AI to one-shot it.

### Agent vs MCP
To use AI to work on tasks, you have two ways: the built-in agent, or another agent client through MCP.

**Built-in agent**: First you need to add the API keys for the model you want to use in the VS Code settings. Search for "Volar" in settings and you will see the places to add those API keys. Then, just toggle on "Agent" in the task view. Then click any of the "Flesh out"/"Execute"/"Breakdown" action and the agent will start working on it. If you can't see the chat window, try make the sidebar bigger, and check for the "chat" icon on the right side of the model name.

**Through MCP**: You can use AI agent in Cursor, Windsurf, Cline, etc to perform "Flesh out"/"Execute"/"Breakdown" actions. First make sure the "Agent" toggle is off. Then use the dropdown menu on those actions to copy the prompt for those tasks, and paste it into the chat interface of Cursor/Windsurf/Cline to allow their agent interact with Volar through MCP. **You need to make sure there is only one Cursor/Windsurf/Cline window running for MCP to work successfully.** See section "Known Issues" for more details. Follow the steps below to set up MCP.


# MCP Configs
You can use any client that supports MCP Server-Sent Events (SSE) Transport.

The server runs on `http://localhost:3001/sse` by default. You can change that port number in your VS Code settings. Just remember to update the corresponding port number in your client MCP config. Also remember to refresh your VS Code window for the updated port number to take effect.

The example configs below will use the default port `3001`.

## MCP setup for Cursor
Go to Cursor MCP config, which is a json file. Add the config for Volar with the url. Here is what the entire file looks like if you have only Volar MCP server.
```
{
  "mcpServers": {
    "VolarTaskServer": {
      "url": "http://localhost:3001/sse",
      "enabled": true
    }
  }
}
```

## MCP setup for Windsurf
Go to Windsurf MCP config. Hit "View raw config". Add the config for Volar. If you have only one MCP server that is Volar, this is what the file should look like.
```
{
  "mcpServers": {
    "VolarTaskServer": {
      "serverUrl": "http://localhost:3001/sse"
    }
  }
}
```

## MCP setup for Cline
Go to Cline MCP set up tab, use the following values:

* Server Name: `VolarTaskServer`
* Server URL: `http://localhost:3001/sse`

# Known Issues
Only one AI client (a single Cursor/VS Code window) can be used at a time. If you have 2+ windows, the MCP connection fails in weird ways for all processes that try to access Volar. 

If you run into connection issues (cannot connect, timeout, etc):
* Check your client (e.g. Cursor) MCP config
* Check you have only one client instance running (e.g. one Cursor window)
* Check no other processes are using the same port (default is `localhost:3001`)

