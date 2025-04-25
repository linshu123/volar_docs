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
Only one AI client (a single Cursor/VS Code window) can be used at a time. If you have 2+ windows, the MCP connection fails in weird ways in all instances. 

If you run into connection issues (cannot connect, timeout, etc):
* Check your client (e.g. Cursor) MCP config
* Check you have only one client instance running (e.g. one Cursor window)
* Check no other processes are using the same port as (default is `localhost:3001`).

# Contact
If you have ideas, suggestions, feedback, please reach me at linshu123@gmail.com

