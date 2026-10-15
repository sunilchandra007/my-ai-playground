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

/authorize is the human-facing part; 

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

/token is normally the back-channel protocol exchange.

```http
POST /token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&code=ABC123
&client_id=https://client.example.com/oauth/client.json
&redirect_uri=http://127.0.0.1:6274/oauth/callback
&code_verifier=VERY_LONG_RANDOM_VALUE
&resource=https://mcp.example.com
```

```text
POST /token

├── Validate authorization code
├── Validate client
├── Validate PKCE code_verifier
└── Issue access token
```

---

## 4. PKCE Binding

During `/authorize`, the client sends a derived challenge:

```text
code_challenge = BASE64URL(SHA256(code_verifier))
```

During `/token`, it sends the original verifier:

```text
code_verifier = VERY_LONG_RANDOM_VALUE
```

The Authorization Server verifies that they correspond.

```text
/authorize

code_challenge
      ▲
      │ derived from
      │
code_verifier

/token
```

This binds the authorization-code exchange to the client instance that generated the verifier.

```text
CLIENT                                      AUTHORIZATION SERVER
  │                                                  │
  │  Generate:                                       │
  │  code_verifier = XYZ...                          │
  │                                                  │
  │  code_challenge =                                │
  │    BASE64URL(SHA256(XYZ...))                     │
  │                                                  │
  │  GET /authorize                                  │
  │  code_challenge=...                              │
  │  code_challenge_method=S256                      │
  ├─────────────────────────────────────────────────►│
  │                                                  │
  │                                  Associate with: │
  │                                  code = CODE123  │
  │                                  challenge = ... │
  │                                  method = S256   │
  │                                                  │
  │  callback?code=CODE123                           │
  │◄─────────────────────────────────────────────────┤
  │                                                  │
  │  POST /token                                     │
  │  code=CODE123                                    │
  │  code_verifier=XYZ...                            │
  ├─────────────────────────────────────────────────►│
  │                                                  │
  │                                  Lookup CODE123  │
  │                                        │         │
  │                                        ▼         │
  │                                  stored challenge│
  │                                        │         │
  │                                  Calculate:      │
  │                                  BASE64URL(      │
  │                                    SHA256(XYZ...)│
  │                                  )     │         │
  │                                        ▼         │
  │                                     COMPARE      │
  │                                        │         │
  │                                     ✓ MATCH      │
  │                                        │         │
  │                               Issue access token │
  │                                                  │
  │  access_token                                    │
  │◄─────────────────────────────────────────────────┤
  │                                                  │
```
---

## 5. CIMD + Confidential Client

A CIMD document can also describe how a confidential client authenticates to the token endpoint.

For example:

```json
{
  "client_id": "https://client.example.com/oauth/client.json",
  "client_name": "My MCP Client",
  "redirect_uris": [
    "https://client.example.com/oauth/callback"
  ],
  "token_endpoint_auth_method": "private_key_jwt",
  "jwks_uri": "https://client.example.com/.well-known/jwks.json"
}
```

Conceptually:

```text
CIMD URL
   │
   ├── Who is this client?
   │
   └── How should it authenticate?
             │
             ▼
       private_key_jwt
             │
             ├── Client signs assertion with private key
             │
             └── Authorization Server verifies using published key
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
