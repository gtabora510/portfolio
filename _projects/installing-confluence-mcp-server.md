---
layout: page
title: Installing and Configuring the Confluence MCP Server for GitHub Copilot
description: Learn how to install and configure the Confluence MCP server to enhance GitHub Copilot's capabilities.
importance: 6
category: installation
---

## Overview

Confluence Model Context Protocol (MCP) is an MCP server that exposes a curated set of Confluence operations to GitHub Copilot so that developers can query and update Confluence without leaving their IDE.

### Business Challenges Addressed

- Developers spend time switching between Confluence, IDEs and documentation to gather relevant information, leading to inefficiencies and lost coding time.
- Confluence's existing APIs and automation rules are designed for point-to-point integrations, making it difficult to support the dynamic, context-rich workflows required by modern AI solutions.
- Without an MCP service, Confluence remains a siloed tool, unable to provide governed, real-time context to AI agents for advanced triage, planning, and productivity enhancements.

### Benefits of Confluence MCP

- Fewer context switches; faster task completion.
- Consistent governance via the MCP Registry ("Registry only").
- One integration pattern (MCP) that can later extend to Confluence, ServiceNow, Bitbucket and more.

### Workflow

From a high level, the Confluence MCP integration with GitHub Copilot works as follows:

1. Developer asks Copilot: "Get Page Contents for &lt;398876350&gt;" in the IDE.
2. Copilot's MCP client routes the request to the approved Confluence MCP server.
3. Confluence MCP calls Confluence's APIs, applies org policies, and returns structured data to Copilot.
4. Copilot displays the result in chat or uses it in follow-on actions—no browser required.

## Installation and Configuration

### Prerequisites

| Component                         | Supported Version/Status | Action                                                                          | Notes                                                                                          |
| --------------------------------- | ------------------------ | ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| GitHub Copilot Onboarding         | Required                 | Complete both the onboarding and installation of the GitHub Copilot IDE plugin. | Follow the guides found in go/copilot: Onboarding to GitHub Copilot and Installing the Plugin. |
| IntelliJ IDEA                     | Idea2025.1 or later      | Upgrade to idea2025.1+ to use the MCP registry features.                        | See go/devshelltools > IDEA for latest version.                                                |
| GitHub Copilot plugin for IDEA    | v1.9.1 or later          | Ensure you have the correct version of the Copilot plugin.                      | Required for MCP Registry.                                                                     |
| Visual Studio Code (VS Code)      | 1.100.3 or later         | Upgrade to v1.100.3+ to use the MCP registry features.                          | See go/devshelltools > VS Code for latest version.                                             |
| GitHub Copilot plugin for VS Code | v0.33.6 or later         | Ensure you have the correct version of the Copilot plugin.                      | Required for MCP Registry                                                                      |

### Installing the Confluence MCP Server for IntelliJ IDEA

1. Ensure you have completed and understood all the Prerequisites.
2. Complete all steps in Configuring Your IDE Proxy for Copilot.
3. Open a new Copilot Chat window, click on the **Settings** icon, then select **MCP Registry** from the drop-down.
4. Locate `110128-jet-confluence-mcp-server` in the list of MCP servers and click **Install**.
5. Return to the chat window and enter Agent mode.
6. Type an example query such as "What are the Confluence pages I recently worked on?" to initiate authentication. A pop-up displays: the MCP Server Definition `jira.mcp.prod.aws.jpmchase.net/mcp` wants to authenticate to `jira.mcp.prod.aws.jpmchase.net`.

   > **Note on Token Expiry:** IntelliJ may display this pop-up again after the token expires (approximately one hour). If this occurs, repeat the previous two steps to re-authenticate.

7. Click **OK** to proceed. An "Authentication Successful" browser window displays.

### Installing the Confluence MCP Server for VS Code

1. Ensure you have completed and understood all the Prerequisites.
2. Complete all steps in Configuring Your IDE Proxy for Copilot.
3. Close any open Visual Studio Code instances, then relaunch a new instance.
4. Configure your IDE for use with the Confluence MCP Server:
   1. Navigate to **File** > **Preferences** > **Settings**, search for "MCP".
   2. Locate the **Chat > MCP: Access** setting. _If the drop-down is enabled_, set it to **registry**.
5. Install the plugin:
   1. Open the **Extensions Marketplace** and type "@mcp" in the search bar. Alternatively, you can navigate to the **MCP SERVERS** section directly.
   2. Select **Enable MCP Servers Marketplace**.
   3. Click **Install** under **Jet Confluence MCP Server**.
   4. Click **Allow** when "The MCP Server Definition '110128-jet-confluence-mcp-server' wants to authenticate to `ideq2.jpmorganchase.com`" prompt appears.
