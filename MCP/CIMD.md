# MCP OAuth Authorization Code Flow with CIMD

## What CIMD Changes

With **Client ID Metadata Documents (CIMD)**, the OAuth `client_id` is an **HTTPS URL** pointing to a JSON document that describes the client.

```text
Traditional OAuth
client_id = abc123

CIMD
client_id = https://client.example.com/oauth/client.json
```

There is no need for the Authorization Server to assign that client ID through a separate registration step. When the Authorization Server encounters the URL-form client ID, it can fetch the metadata document and apply its client-validation policy.

---

## 1. Client Hosts Its CIMD Document

Example URL:

```text
https://www.mcpjam.com/.well-known/oauth/client-metadata.json
```

Example metadata:

```json
{
  "client_id": "https://www.mcpjam.com/.well-known/oauth/client-metadata.json",
  "client_name": "MCPJam",
  "redirect_uris": [
    "http://127.0.0.1:6274/oauth/callback"
  ]
}
```

The `client_id` inside the document must exactly match the URL of the document.

---

## 2. `/authorize` - Get Authorization Code

```http
GET /authorize?
    response_type=code
    &client_id=https://client.example.com/oauth/client.json
    &redirect_uri=http://127.0.0.1:6274/oauth/callback
    &scope=mcp:tools
    &code_challenge=ABC123...
    &code_challenge_method=S256
    &resource=https://mcp.example.com
```


```text
What Auth Server Does with /authorize request
      ├─ Identify client            ← client_id = CIMD URL
      ├─ Fetch / validate CIMD metadata
      ├─ Validate redirect URI
      ├─ Authenticate user          ← HUMAN proves IDENTITY
      ├─ Ask user for consent       ← HUMAN provides CONSENT
      └── Issue authorization code
          │
          ▼
      redirect_uri?code=ABC123
```

### Authentication

> **Who are you?**  
> The human proves their identity.

### Consent

> **Do you allow this client application to act on your behalf?**

Example:

```text
MCPJam wants permission to:

✓ Access MCP tools
✓ Access MCP resources

        [Allow]   [Deny]
```

After approval:

```text
Authorization Server
        │
        ▼
redirect_uri?code=ABC123
```

---

## 3. `/token` - Exchange Code for Access Token

```text
POST /token

├── Validate authorization code
├── Validate client
├── Validate PKCE code_verifier
└── Issue access token
```

---

## Mental Model

```text
/authorize
    │
    ├── Who is the client?
    ├── Who is the user?       ← Authentication
    ├── Does the user agree?   ← Consent
    │
    ▼
Authorization Code
    │
    │ POST /token
    ▼
Access Token
```

## In One Line

**CIMD** → Who is the OAuth client?  
**Authentication** → Who is the human?  
**Consent** → Does the human allow this client to act on their behalf? 
**PKCE** → Is this token exchange bound to the client instance that started the flow? 
**Authorization** → What access is ultimately granted?
