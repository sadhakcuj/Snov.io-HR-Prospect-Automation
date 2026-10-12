# Snov.io-HR-Prospect-Automation
A step-by-step n8n automation integrating the Snov.io API to search for HR professionals in Hyderabad.

# Snov.io HR Prospect Automation Using n8n

## 1. Project Overview

This project uses n8n to integrate with the Snov.io API and search for HR and Talent Acquisition professionals located in Hyderabad, Telangana, India.

The workflow uses HTTP requests to authenticate with Snov.io, search for prospects, and retrieve the search results.

## 2. Node 1: Snov - Get Access Token

### Purpose

This node authenticates the application with Snov.io and obtains an access token. Subsequent nodes use this token to authenticate their API requests.

### Configuration

| Setting | Value |
|---|---|
| Node Type | HTTP Request |
| HTTP Method | POST |
| URL | `https://api.snov.io/v1/oauth/access_token` |
| Authentication | None |
| Send Body | Enabled |
| Body Content Type | Form URL Encoded |

### Body Parameters

The request contains three parameters:

**1. `grant_type`**

Value: `client_credentials`

Specifies the OAuth 2.0 authentication flow. This flow allows an application to request an access token using its own credentials.

**2. `client_id`**

Value: My Snov.io API User ID.

Identifies the registered API client requesting access. It does not independently prove that the application is authorised.

**3. `client_secret`**

Value: My Snov.io API Secret.

A confidential credential associated with the API client. It helps Snov.io authenticate the application and must be kept private.

### Why POST?

POST is used because the Snov.io access-token endpoint expects authentication parameters to be submitted in a POST request.

The server processes the request and returns a response. A separate GET request is not required to receive the access token.

### Why Form URL Encoded?

This setting tells n8n how to format the request body. The parameters are sent as form-encoded data, as expected by the token endpoint.

### Authentication: None

The HTTP Request node's built-in authentication setting is set to None because the credentials are supplied manually in the request body.

Snov.io still validates these credentials. None does not mean that the API request is unauthenticated.

### How the Node Works

1. n8n sends a POST request to the Snov.io access-token endpoint.
2. The request includes the grant type, client ID, and client secret.
3. Snov.io validates the application credentials.
4. If authentication succeeds, Snov.io returns an access token.
5. n8n makes the response available to subsequent nodes.

### Output

The response includes an `access_token` field and may include an `expires_in` field indicating the token's validity duration in seconds.

The token is a separate credential returned by Snov.io; it is not the same as the client ID or client secret.

### Passing the Token to Another Node

The token can be accessed using this n8n expression:

```javascript
{{ $node["Snov - Get Access Token"].json["access_token"] }}
```

This expression retrieves the access token from Node 1's JSON output.

### Key Learnings

- HTTP methods and API endpoints.
- OAuth 2.0 Client Credentials flow.
- Request bodies and form URL encoding.
- Application identification versus authentication.
- API responses and access tokens.
- Passing data between n8n nodes.
