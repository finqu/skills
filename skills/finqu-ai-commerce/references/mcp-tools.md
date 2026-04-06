# MCP Tools

Connecting external services to Finqu's AI agent via Model Context Protocol (MCP).

## What Are MCP Tools?

MCP tools extend the Success Agent's capabilities by connecting external services. The agent can call these tools during conversations to fetch data, perform actions, or integrate with third-party systems.

## Authentication Methods

### OAuth

For services requiring user authorization:

1. Configure OAuth client credentials in Finqu admin
2. User authorizes access during setup
3. Agent uses the granted token for API calls

### Bearer Token

For API key-based services:

1. Obtain an API key from the external service
2. Configure the key in Finqu admin
3. Agent includes the key in requests

## Transport Methods

### SSE (Server-Sent Events)

- Persistent connection for streaming responses
- Good for real-time data feeds
- Server pushes events to the client

### Streamable HTTP

- Standard HTTP with streaming support
- Request/response model with streamed body
- More widely compatible with existing infrastructure

## Configuration

To add an MCP tool:

1. Open AI Commerce settings in the store admin
2. Add a new MCP tool
3. Configure:
    - **Name** — Descriptive tool name
    - **Endpoint URL** — The MCP server URL
    - **Authentication** — OAuth or Bearer token
    - **Transport** — SSE or Streamable HTTP
4. Test the connection
5. Assign the tool to relevant Workers

## Full Reference

- [MCP Tools](https://developers.finqu.com/build-with-finqu/ai-commerce/mcp-tools.md.txt)
