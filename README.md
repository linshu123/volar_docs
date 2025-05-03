# Volar - A New Task Tool
Volar is a task management tool for your programming needs. The idea is to provide a flexible way for
* Persisting & organizing context 
* Human & AI alignment and collaboration
* Enable an async workflow where humans don't need to wait on AI as much

# Demo
In the demo I will walk you through how to leverage Volar to go from: one-line idea -> detailed plan -> sub tasks (if needed) -> implementation by AI.
(UI have changed a lot since the recording of the demo.)
https://youtu.be/mrdtMFBqyWM

# Download
Download the latest version from extension store: https://marketplace.visualstudio.com/items?itemName=VolarTools.volar-ai

# Usage
## Task View & Task Management
The creation and curation of tasks are straightforward.
![Screenshot](https://github.com/linshu123/volar_docs/blob/main/resources/task_view_usage.png)

Click on the "pencil" emoji to update an task. You can add simple context in task description to help AI work better. Both the floating "Done" button and the "Save" emoji will save the changes.
![Screenshot](https://github.com/linshu123/volar_docs/blob/main/resources/task_edit_usage.png)

You can of course also ask AI to make changes or organize tasks, such as "find all the tasks that are describing a bug and add [Bug] prefix to their title". This will be explained below.

## Take Actions on Task

### What are task actions?
There are 3 key features for asking AI to work on a task: "Flesh out", "Execute", and "Breakdown".
![Screenshot](https://github.com/linshu123/volar_docs/blob/main/resources/task_action_buttons_zoomed_in.png)

**Flesh out:** With just a few words in the task title, you can use "Flesh out" to get AI to help you expand on that task. AI will help you investigate the codebase and add plans and implementation suggestions to the task. Then you can review that plan. If it all makes sense, you can use **Execute** to actually implement the task.

**Execute:** This will ask AI to go straight into implementation mode. Ideally, there is already some plans and details in the task that you are confident about. AI will follow those plans and make the code changes.

**Breakdown:** If you have a complex task that you feel AI won't be able to accomplish in one go, you can use this option to break down the task into multiple smaller tasks. You can recursively do this until you feel each task is small enough for AI to one-shot it.

We also have a "commit" button. It's for quickly commit all changes with the task title as the message. (This may be too aggressive. We may remove it later.)
![Screenshot](https://github.com/linshu123/volar_docs/blob/main/resources/commit_button_zoomed_in.png)

## Agent Mode & Non-agent Mode (MCP)
To use AI to work on tasks, you have two ways: using the built-in agent, or using another agent through MCP.

#### Agent Mode
Agent mode requires bringing your own key for modle providers. We current support the top coding models from popular providers including OpenAI, Anthropic, Deepseek and OpenRouter. 

First you need to add the API keys for the model you want to use in the VS Code settings. Search for "Volar" in settings and you will see the places to add those API keys. Then, turn on "Agent" mode in the task view. 

Turn on Agent toggle, cilck the "Play" emoji to see the dropdown menu. Click on any of those actions and a chat window will open up with the agent.
![Screenshot](https://github.com/linshu123/volar_docs/blob/main/resources/task_action_usage_agent.png)

After the agent has finished their work in the chat window, they will update the task content, like description or status. You can continue the conversation if you like.
![Screenshot](https://github.com/linshu123/volar_docs/blob/main/resources/agent_usage.png)

#### Non-agent Mode (MCP)
If you already pay for services like Cursor or Windsurf, you can use those AI services to perform task actions. The idea of using MCP is to set up a connection (instructions below) so agents in other apps can access and update the tasks automatically. However, currently it is not possible to call those agents from outside. (MCP is a "one way" connection for the agent call outside, but not for the outside to call the agent.) So in order to get those agents to work on the tasks, we need to manually give them the instructions. The way we do this in Volar is to let you copy a prompt for the desired task action, and paste it into Cursor/Windsurf/etc chat for their agent to do the work. 

Turn off Agent toggle, click the "Play" emoji to see the dropdown menu. Clicking on any action to copy the prompt. Paste it into Cursor/Windsurf/etc chat window.
![Screenshot](https://github.com/linshu123/volar_docs/blob/main/resources/task_action_usage_non_agent.png)

When that agent does their work, they can access and update tasks through MCP. Shown in the screenshot is a full execution plan worked out by Cursor agent.
![Screenshot](https://github.com/linshu123/volar_docs/blob/main/resources/external_agent_usage.png)

## How to set up MCP for non-agent mode
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

