# Volar - Plan, Review & Build with AI

*Annoyed when AI starts touching unrelated code?*

*Getting nervous when you ask AI to write something maybe a little too complicated?*

*Waiting for AI to finish its code before you know if they understand your request or not?*

*Pulling hair out when AI forgets your instructions?*

*Not sure how much context to add in prompt?*

---

Introducing Volar - tool to collaborate with your AI coder. Get your AI to articulate your idea, break down complex requests, plan the implementation, and finally write your code!

# Features

* Plan from simple ideas: Write a single line on what you want. Let AI flesh out the specifics.
* Break down big items: A feature is too complex? Or too ambiguous? Get AI to break it down into smaller pieces, add more details, and do it recusively until you are confident.
* Execute plan: Execute code changes exactly according to the plan. Nothing less and nothing more.
* Work with any MCP enabled AI agents: Supports Cursor, Windsurf, Claude Code, Cline, etc.

# Usage

## Task View & Task Management
The creation and curation of tasks are straightforward.
![Screenshot](https://raw.githubusercontent.com/linshu123/volar_docs/main/resources/Task%20List%20View.png)

View details of a task. Click on the "pencil" emoji to edit the task. Normally the task description will be filled by AI, but you can add clues, context here. You can also make changes to plans written by AI.
![Screenshot](https://raw.githubusercontent.com/linshu123/volar_docs/main/resources/Task%20Modal%20View.png)

Instead of manually editing tasks, you can also ask any AI agent to update tasks for you.

## Take Actions on Task

### What are task actions?
There are 3 key features for asking AI to work on a task: "Plan", "Execute", and "Breakdown".
![Screenshot](https://raw.githubusercontent.com/linshu123/volar_docs/main/resources/Action%20Dropdown.png)

**Plan:** With just a few words in the task title, you can use "Plan" to get AI to help you expand on it. AI will help you investigate the codebase and plan out the execution steps. If it all makes sense, use **Execute** to actually implement the task.

**Execute:** This tells AI to go make code changes. Ideally you would have reviewed the plans. For simple changes, executing directly may make sense too.

**Breakdown:** For complex tasks, use this option to break them down into smaller pieces. Recursively do this until you feel confident for AI to one-shot the subtasks.

## Claude Code Mode
Volar supports MCP with any AI agent, with instructions to configure down below. But for Claude Code, we supports a more seamless experience.

When Claude Code mode is ON: For task actions, Volar will invoke Claude Code directly in a VS Code terminal, with the prompt already filled in.
![Screenshot](https://raw.githubusercontent.com/linshu123/volar_docs/main/resources/Agent%20-%20Actions.png)

When Claude Code mode is OFF: For task actions, Volar will copy the prompt for you to paste into any agent chat.
![Screenshot](https://raw.githubusercontent.com/linshu123/volar_docs/main/resources/Non-agent%20-%20Actions.png)

## How to set up MCP for Cursor/Windsurf/etc
You can use any client that supports MCP Server-Sent Events (SSE) Transport. 

**IMPORTANT: You need to make sure there is only one Cursor/Windsurf/Cline window running for MCP to work successfully.** (See section "Known Issues" for more details.)

The server runs on `http://localhost:3001/sse` by default. You can change that port number in your VS Code settings. Just remember to use the same port number in your client MCP config. (E.g. if you put port `4001` in Volar MCP config, make sure your Cursor/Windsurf/etc MCP config is also `4001`.) Refresh your VS Code window for the updated port number to take effect.

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

