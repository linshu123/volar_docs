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
The server runs on http://localhost:3001/sse. You can use any client that supports MCP SSE with Volar. I have only tested with Cursor, and here's the config you can copy/paste into it.
## Cursor
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

# Known Issues
Only one AI client (a single Cursor/VS Code window) can be used at a time. If you have 2+ windows, the MCP connection fails in weird ways in all instances. 

If you run into connection issues (cannot connect, timeout, etc):
* Check your client (e.g. Cursor) MCP config
* Check you have only one client instance running (e.g. one Cursor window)
* Check no other processes are using port localhost:3001.

# Contact
If you have ideas, suggestions, feedback, please reach me at linshu123@gmail.com

