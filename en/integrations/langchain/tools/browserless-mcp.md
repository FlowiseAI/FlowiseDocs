---
description: MCP Server for Browserless - scrape pages, take screenshots, generate PDFs, and more
---

# Browserless MCP

The **Browserless MCP** node connects your Flowise agents to [Browserless](https://browserless.io) through the Model Context Protocol (MCP). It enables browser automation capabilities such as web scraping, taking screenshots, generating PDFs, and more — all without managing headless browser infrastructure yourself.

---

## 1. Prerequisites

Before using the Browserless MCP node, you need:

- **A Browserless account**: sign up for free at [browserless.io](https://browserless.io).
- **An API Token**: obtain your token from the Browserless dashboard.

---

## 2. Setting Up Credentials

1. In Flowise, navigate to **Credentials** from the sidebar.
2. Click **Add Credential** and search for **Browserless API**.
3. Fill in the following field:

| Field | Description |
| :---- | :---- |
| **API Token** | Your Browserless API token from the dashboard |

4. Click **Save**.

---

## 3. Adding the Browserless MCP Node

1. Open a chatflow in the Flowise canvas.
2. Add an **Agent** node (e.g., Tool Agent).
3. From the **Tools (MCP)** category, drag the **Browserless MCP** node onto the canvas.
4. Connect the Browserless MCP node's output to the Agent node's **Tools** input.
5. Configure the node (see next section).

---

## 4. Node Configuration

| Parameter | Type | Required | Description |
| :---- | :---- | :---- | :---- |
| **Connect Credential** | Credential selector | Yes | Select the Browserless API credential you created in Step 2. |
| **Available Actions** | Multi-select (async) | Yes | After connecting your credential, click the **refresh** icon to load the list of available actions from the Browserless MCP server. Select the specific actions you want to expose to your agent. |

---

## 5. Selecting Actions

Once you provide a valid credential, click the refresh icon next to **Available Actions**. The node will connect to the Browserless MCP server and retrieve all available tools.

Each action is listed with:

- **Name:** the tool identifier
- **Description:** what the tool does

Select only the actions your agent needs. Fewer tools help the LLM make better decisions and reduce token usage.

---

## 6. Available Capabilities

The Browserless MCP server provides a range of browser automation tools, including:

- **Web scraping** — extract content from web pages
- **Screenshots** — capture page screenshots
- **PDF generation** — generate PDFs from web pages
- **Browser automation** — interact with pages programmatically

The full list of available actions is dynamically loaded from the MCP server when you refresh the **Available Actions** dropdown.

---

## 7. External References

| Resource | Link |
| :---- | :---- |
| Browserless MCP Server Docs | [docs.browserless.io/mcp/browserless-mcp-server](https://docs.browserless.io/mcp/browserless-mcp-server) |
| Browserless Documentation | [docs.browserless.io](https://docs.browserless.io) |
| Browserless Website | [browserless.io](https://browserless.io) |
