# Installing IQFluence MCP

IQFluence MCP is a remote server: there is nothing to clone, build or run locally. Connect the client to
`https://mcp.iqfluence.io/mcp` over Streamable HTTP.

## Cline

Add this entry to `cline_mcp_settings.json` (keep any existing servers):

```json
{
  "mcpServers": {
    "iqfluence": {
      "type": "streamableHttp",
      "url": "https://mcp.iqfluence.io/mcp",
      "disabled": false
    }
  }
}
```

## Authentication

- **OAuth (default):** the server answers an unauthenticated request with `401` and OAuth metadata. Let the user
  authorize the server in the browser; they sign in to their IQFluence account and click **Allow access**.
- **API key:** if the user provides a key from **Profile → API keys** on iqfluence.io, add it as a header instead:

```json
"headers": { "Authorization": "Bearer <API key>" }
```

Never ask the user to paste a key into the chat if the client can store it in its settings.

## Check the connection

Call `get_account_balance` (free). It returns the account's MCP credit balance. A user without an IQFluence
account can create one for free at https://iqfluence.io; new accounts get 5 trial credits on the first connection.
