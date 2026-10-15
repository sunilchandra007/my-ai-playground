# OAuth Authorization Code Flow

## 1. `/authorize` - Get Authorization Code

```text
GET /authorize
      ├─ Identify client
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

## 2. `/token` - Exchange Code for Access Token

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

**Authentication** → Who are you?  
**Consent** → Do you allow this client to act on your behalf?  
**Authorization** → What access is ultimately granted?
