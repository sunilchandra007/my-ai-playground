```
# fetch auth code from Auth Server
GET /authorize
      ├─ Identify client
      ├─ Validate redirect URI
      ├─ Authenticate user          ← HUMAN proves IDENTITY
      ├─ Ask user for consent       ← HUMAN provides CONSENT
      |---------------------------------------------------
      |# Hey HUMAN, do you allow client application to act ON YOUR BEHALF ?
      |
      |  MCPJam wants permission to:
      |
      |✓ Access MCP tools
      |✓ Access MCP resources
      |
      |      [Allow]  [Deny]
      |---------------------------------------------------
      └─ Issue auth code
                 │
                 ▼
        callback?code=ABC123

# fetch token from Auth Server
POST /token
      ├─ Validate code
      ├─ Validate client
      ├─ Validate PKCE verifier
      └─ Issue access token
```
