---
layout: base
nav: specs
title: CDSC-WG1 - 02 Client Registration - Overview
meta_description: Linux Foundation Energy - Carbon Data Specification Consortium (CDSC) - Connectivity Working Group (WG1) - Specifications - cdsc-wg1-02 - Client Registration - Overview
---
[Home]({{ "/" | relative_url }}) / [Specifications]({{ "/specs" | relative_url }}) / [`cdsc-wg1-02` - Client Registration]({{ "/specs/cdsc-wg1-02" | relative_url }}) / Overview

# Client Registration - Overview

This is a summary overview of [`cdsc-wg1-02`]({{ "/specs/cdsc-wg1-02" | relative_url }}), which describes how companies and technical users ("Clients") can register, onboard, and perform authenticated actions with utilities and other centralized entities ("Servers").
This specification builds upon [`cdsc-wg1-01`]({{ "/specs/cdsc-wg1-01" | relative_url }}), which allows Clients to discover a Server's metadata and capabilities. This specification adds a capability to the Server's metadata that specifies to a Client how to register with that Server.

For example, this specification describes how an energy efficiency app can register with a utility so that the app can request access from a utility customer's to see their last year of energy usage.

## Background <a id="background" href="#background" class="permalink">🔗</a>

Utilities and other central entities offer many different ways of providing authenticated access to information, including how to obtain customer authorization when customers need to authorize access to their utility data.
For [use case]({{ "/use-cases" | relative_url }}) companies needing to obtain customer data, an efficient means of registering, onboarding, and configuring authenticated access with these utilities and other central entities is needed.

## OAuth Capability <a id="oauth-capability" href="#oauth-capability" class="permalink">🔗</a>

```
==Request==
GET /.well-known/carbon-data-spec.json HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "cds_metadata_version": "v1",
    ...

    "capabilities": [
        "oauth",
        ...
    ],
    ...

    "oauth_metadata": "https://demoutility.com/.well-known/oauth-authorization-server",
}
```

## OAuth Metadata Endpoint <a id="oauth-metadata" href="#oauth-metadata" class="permalink">🔗</a>

```
==Request==
GET /.well-known/oauth-authorization-server HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    # OAuth Metadata (inheritied from OAuth specs)
    "issuer": "https://demoutility.com",
    "service_documentation": "https://demoutility.com/docs/oauth",
    "op_policy_uri": "https://demoutility.com/legal/oauth-policy",
    "op_tos_uri": "https://demoutility.com/legal/oauth-terms",
    "registration_endpoint": "https://demoutility.com/oauth/register",
    "authorization_endpoint": "https://demoutility.com/oauth/authorize",
    "token_endpoint": "https://demoutility.com/oauth/token",
    "revocation_endpoint": "https://demoutility.com/oauth/token/revoke",
    "introspection_endpoint": "https://demoutility.com/oauth/token/info",
    "pushed_authorization_request_endpoint": "https://demoutility.com/oauth/par",
    "code_challenge_methods_supported": [
        "S256"
    ],
    "response_types_supported": [
        "code"
    ],
    "grant_types_supported": [
        "client_credentials",
        "authorization_code",
        "refresh_token"
    ],
    "token_endpoint_auth_methods_supported": [
        "client_secret_basic"
    ],
    "scopes_supported": [
        "client_admin",
        "grant_admin",
        "server_provided_files",
        ...
    ],
    "authorization_details_types_supported": [
        "client_admin",
        "grant_admin",
        "server_provided_files",
        ... (same as "scopes_supported")
    ]

    # CDS Extension (added by this specification)
    "cds_oauth_version": "v1",
    "cds_human_registration": "https://demoutility.com/clients/register",
    "cds_test_accounts": "https://demoutility.com/docs/testing",
    "cds_clients_api": "https://demoutility.com/api/clients",
    "cds_messages_api": "https://demoutility.com/api/messages",
    "cds_credentials_api": "https://demoutility.com/api/credentials",
    "cds_grants_api": "https://demoutility.com/api/grants",
    "cds_server_provided_files_api": "https://demoutility.com/api/server-provided-files"
    "cds_scope_descriptions": {

        "client_admin": {
            "id": "client_admin",
            "name": "Client Admin",
            "description": "This scope grants administrative access to the Client management APIs.",
            "documentation": "https://demoutility.com/docs/oauth/scopes#client_admin",
            "registration_requirements": [],
            "response_types_supported": [],
            "grant_types_supported": ["client_credentials"],
            "token_endpoint_auth_methods_supported": ["client_secret_basic"],
            "code_challenge_methods_supported": [],
            "coverages_supported": [],
            "authorization_details_fields": []
        },

        "grant_admin": {
            "id": "grant_admin",
            "name": "Grant Admin",
            "description": "This scope grants administrative access to previously created Grants.",
            "documentation": "https://demoutility.com/docs/oauth/scopes#grant_admin",
            "registration_requirements": [],
            "response_types_supported": [],
            "grant_types_supported": ["client_credentials"],
            "token_endpoint_auth_methods_supported": ["client_secret_basic"],
            "code_challenge_methods_supported": [],
            "coverages_supported": [],
            "authorization_details_fields": [
                {
                    "id": "client_id",
                    "name": "Client object identifier",
                    "description": "The Client object identifier for which the Grant is issued.",
                    "documentation": "https://demoutility.com/docs/oauth/scopes#grant_admin-client_id",
                    "format": "string",
                    "is_required": true,
                },
                {
                    "id": "grant_id",
                    "name": "Grant identifier",
                    "description": "The Grant identifier for which the returned access_token will be given access.",
                    "documentation": "https://demoutility.com/docs/oauth/scopes#grant_admin-grant_id",
                    "format": "string",
                    "is_required": true,
                }
            ]
        },

        "server_provided_files": {
            "id": "server_provided_files",
            "name": "Server-Provided Files",
            "description": "This scope grants access to specific files that the Server wants make available to the Client.",
            "documentation": "https://demoutility.com/docs/oauth/scopes#grant_admin",
            "registration_requirements": [
                "company_name"
            ],
            "response_types_supported": [],
            "grant_types_supported": ["client_credentials"],
            "token_endpoint_auth_methods_supported": [],
            "code_challenge_methods_supported": [],
            "coverages_supported": [],
            "authorization_details_fields": [
                {
                    "id": "file_id",
                    "name": "File identifier",
                    "description": "A file provided by the Server that may be accessed by the Client as part of the Grant.",
                    "documentation": "https://demoutility.com/docs/oauth/scopes#server_provided_files-file_id",
                    "format": "string",
                    "is_required": true,
                }
            ]
        },
        ...
    },
    "cds_registration_fields": {
        "company_name": {
            "id": "company_name",
            "type": "registration_field",
            "field_name": "cds_company_name",
            "description": "The company name to be receiving files.",
            "documentation": "https://demoutility.com/docs/oauth/registration#company_name",
            "format": "string",
            "max_length": 1024
        },
        ...
    },
}
```

## Dynamic Client Registration <a id="client-registration" href="#client-registration" class="permalink">🔗</a>

```
==Request==
POST /oauth/register HTTP/1.1
Content-Type: application/json
Host: demoutility.com

{
    # Normal OAuth registration fields
    "client_name": "Example EV Company",
    "client_uri": "https://example.com",
    "logo_uri": "https://example.com/logo.png",
    "tos_uri": "https://example.com/terms",
    "policy_uri": "https://example.com/privacy",
    "contacts": [
        "mailto:aaa@bbb.com",
        "tel:+15554443333",
    ],
    "scope": "client_admin grant_admin server_provided_files",

    # CDS Extension - additional registration fields
    "cds_company_name": "Example Company"
}


==Response==
HTTP/1.1 201 Created
Content-Type: application/json;charset=UTF-8

{
    # Client Metadata (inheritied from OAuth specs)
    "client_id": "aaaa11111122222",
    "client_id_issued_at": 1740669836,
    "client_name": "aaaa11111122222",
    "contacts": [
        "mailto:aaa@bbb.com",
        "tel:+15554443333",
    ],
    "logo_uri": "https://example.com/logo.png",
    "tos_uri": "https://example.com/terms",
    "policy_uri": "https://example.com/privacy",
    "contacts": [
        "mailto:aaa@bbb.com",
        "tel:+15554443333",
    ],
    "response_types": [],
    "grant_types": [
        "client_credentials"
    ],
    "token_endpoint_auth_method": "client_secret_basic",
    "redirect_uris": [],
    "scope": "client_admin",
    "authorization_details_types": [
        "client_admin"
    ],

    "client_secret": "!09546ut8ijo6.g90ujimrtegobf89uhjnwefs0vifojkpsdlfd.",

    # CDS Extension (added by this specification)
    "cds_created": "2025-01-01T00:00:00Z",
    "cds_modified": "2025-01-01T00:00:00Z",
    "cds_client_uri": "https://demoutility.com/oauth/clients/aaaa11111122222",
    "cds_status": "production",
    "cds_status_options": [
        "production"
    ]
    "cds_server_metadata": "https://demoutility.com/oauth/clients/aaaa11111122222/carbon-data-spec.json",
}
```



## Client Endpoint <a id="client-endpoint" href="#client-endpoint" class="permalink">🔗</a>

```
==Request==
GET /api/clients HTTP/1.1
Host: demoutility.com
Authorization: Bearer <access_token>


==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "clients": [
        {
            "client_id": "aaaa11111122222",
            "client_id_issued_at": 1740669836,
            "client_name": "aaaa11111122222",
            "contacts": [
                "mailto:aaa@bbb.com",
                "tel:+15554443333",
            ],
            "logo_uri": "https://example.com/logo.png",
            "tos_uri": "https://example.com/terms",
            "policy_uri": "https://example.com/privacy",
            "contacts": [
                "mailto:aaa@bbb.com",
                "tel:+15554443333",
            ],
            "response_types": [],
            "grant_types": [
                "client_credentials"
            ],
            "token_endpoint_auth_method": "client_secret_basic",
            "redirect_uris": [],
            "scope": "client_admin",
            "authorization_details_types": [
                "client_admin"
            ],
            "cds_created": "2025-01-01T00:00:00Z",
            "cds_modified": "2025-01-01T00:00:00Z",
            "cds_client_uri": "https://demoutility.com/oauth/clients/aaaa11111122222",
            "cds_status": "production",
            "cds_status_options": [
                "production"
            ],
            "cds_server_metadata": "https://demoutility.com/oauth/clients/aaaa11111122222/carbon-data-spec.json",
        },
        {
            "client_id": "aaaa333333444444",
            "client_id_issued_at": 1740669836,
            "client_name": "aaaa333333444444",
            "contacts": [],
            "response_types": [],
            "grant_types": [
                "client_credentials"
            ],
            "token_endpoint_auth_method": "client_secret_basic",
            "redirect_uris": [],
            "scope": "grant_admin",
            "authorization_details_types": [
                "grant_admin"
            ],
            "cds_created": "2025-01-01T00:00:00Z",
            "cds_modified": "2025-01-01T00:00:00Z",
            "cds_client_uri": "https://demoutility.com/oauth/clients/aaaa333333444444",
            "cds_status": "production",
            "cds_status_options": [
                "production"
            ],
            "cds_server_metadata": "https://demoutility.com/oauth/clients/aaaa333333444444/carbon-data-spec.json",
        },
        {
            "client_id": "aaaa4444444555555",
            "client_id_issued_at": 1740669836,
            "client_name": "aaaa4444444555555",
            "contacts": [],
            "response_types": [],
            "grant_types": [
                "client_credentials"
            ],
            "token_endpoint_auth_method": null,
            "redirect_uris": [],
            "scope": "server_provided_files",
            "authorization_details_types": [
                "server_provided_files"
            ],
            "cds_created": "2025-01-01T00:00:00Z",
            "cds_modified": "2025-01-01T00:00:00Z",
            "cds_client_uri": "https://demoutility.com/oauth/clients/aaaa4444444555555",
            "cds_status": "production",
            "cds_status_options": [
                "production"
            ],
            "cds_server_metadata": "https://demoutility.com/oauth/clients/aaaa4444444555555/carbon-data-spec.json",
        },
    ],
    "next": null,
    "previous": null
}
```

## Client Messages Endpoint <a id="client-messages-endpoint" href="#client-messages-endpoint" class="permalink">🔗</a>
```
==Request==
GET /api/messages HTTP/1.1
Host: demoutility.com
Authorization: Bearer <access_token>


==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "outstanding": [
        {
            "uri": "https://demoutility.com/api/clients/aaaaaaaaaa-01/updates/5555555555555",
            "previous_uri": null,
            "type": "update_request",
            "read": true,
            "creator": null,
            "created": "2025-01-01T00:00:00Z",
            "modified": "2025-01-01T00:00:00Z",
            "status": "pending",
            "name": "",
            "description": "",
            "updates_requested": [
                {
                    "name": "client_name",
                    "previous_value": "My Example Client",
                    "new_value": "My New Name",
                },
            ],
        },
    ],
    "outstanding_next": null,
    "outstanding_previous": null,
    "unread": [
        {
            "uri": "https://demoutility.com/api/clients/aaaaaaaaaa-01/updates/66666666666666",
            "previous_uri": "https://demoutility.com/api/clients/aaaaaaaaaa-01/updates/444444444444444",
            "type": "notification",
            "read": false,
            "creator": null,
            "created": "2025-01-01T00:00:00Z",
            "modified": "2025-01-01T00:00:00Z",
            "related_uri": "https://demoutility.com/blog/post/123123",
            "name": "Upcoming Maintenance Reminder",
            "description": "As a reminder, on 2024-01-01 from 12:00 UTC to 14:00 UTC, we will be down for scheduled maintenance",
        },
    ],
    "unread_next": null,
    "unread_previous": null,
    "read": [
        {
            "uri": "https://demoutility.com/api/clients/aaaaaaaaaa-01/updates/444444444444444",
            "previous_uri": null,
            "type": "notification",
            "read": true,
            "creator": null,
            "created": "2025-01-01T00:00:00Z",
            "modified": "2025-01-01T00:00:00Z",
            "related_uri": "https://demoutility.com/blog/post/123123",
            "name": "Upcoming Maintenance",
            "description": "On 2024-01-01 from 12:00 UTC to 14:00 UTC, we will be down for scheduled maintenance",
        },
    ],
    "read_next": null,
    "read_previous": null,
}
```

## Credentials Endpoint <a id="credentials-endpoint" href="#credentials-endpoint" class="permalink">🔗</a>
```
==Request==
GET /api/credentials HTTP/1.1
Host: demoutility.com
Authorization: Bearer <access_token>


==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "credentials": [
        {
            "credential_id": "555555555",
            "uri": "https://demoutility.com/api/credentials/555555555",
            "client_id": "aaaa11111122222",
            "created": "2025-01-01T00:00:00Z",
            "modified": "2025-01-01T00:00:00Z",
            "type": "client_secret",
            "client_secret": "!09546ut8ijo6.g90ujimrtegobf89uhjnwefs0vifojkpsdlfd.",
            "client_secret_expires_at": null,
        },
        {
            "credential_id": "666666666666",
            "uri": "https://demoutility.com/api/credentials/666666666666",
            "client_id": "aaaa333333444444",
            "created": "2025-01-01T00:00:00Z",
            "modified": "2025-01-01T00:00:00Z",
            "type": "client_secret",
            "client_secret": "09u7!yjhhkmlkriud#w8yeuhnjmjg$rvu9uy8h4un4gt58934uri",
            "client_secret_expires_at": null,
        },
    ],
    "next": null,
    "previous": null,
}
```

## Grants Endpoint <a id="grants-endpoint" href="#grants-endpoint" class="permalink">🔗</a>
```
==Request==
GET /api/grants HTTP/1.1
Host: demoutility.com
Authorization: Bearer <access_token>


==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "grants": [
        {
            "grant_id": "8888888888",
            "uri": "https://demoutility.com/api/grants/8888888888",
            "replacing": [],
            "replaced_by": [],
            "parent": null,
            "children": [],
            "created": "2025-01-01T00:00:00Z",
            "modified": "2025-01-01T00:00:00Z",
            "not_before": null,
            "not_after": null,
            "eta": null,
            "expires": null,
            "status": "active",
            "client_id": "aaaa4444444555555",
            "cds_client_uri": "https://demoutility.com/api/clients/aaaa4444444555555",
            "scope": "server_provided_files",
            "authorization_details": [
                {
                    "type": "server_provided_files",
                    "file_id": "9999999999999999"
                }
            ],
            "receipt_confirmations": [],
            "enabled_scope": "server_provided_files",
            "enabled_authorization_details": [
                {
                    "type": "server_provided_files",
                    "file_id": "9999999999999999"
                }
            ],
            "sub_authorization_scopes": [],
        },
    ],
    "next": "https://demoutility.com/api/clients/11111111/grants?after=aaaaaaaaa",
    "previous": null,
}
```

## Server-Provided Files Endpoint <a id="server-provided-files-endpoint" href="#server-provided-files-endpoint" class="permalink">🔗</a>
```
==Request==
GET /api/server-provided-files HTTP/1.1
Host: demoutility.com
Authorization: Bearer <access_token>


==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "files": [
        {
            "file_id": "9999999999999999",
            "created": "2025-01-01T00:00:00Z",
            "modified": "2025-01-01T00:00:00Z",
            "mime_type": "application/yaml",
            "size": 9999,
            "name": "setup_config.yaml",
            "description": "Configuration settings for connecting to our proprietary API.",
            "download_uri": "https://demoutility.com/api/server-provided-files/9999999999999999"
        },
    ],
    "next": null,
    "previous": null,
}
```

## Examples (using CURL and jq) <a id="examples" href="#examples" class="permalink">🔗</a>

#### Example: Registering a Client <a id="example-registering" href="#example-registering" class="permalink">🔗</a>
<details>
<summary>Show example</summary>
<p>
How to register with a CDS Server for a variety of scopes.
</p>
<pre class="highlight">
<code>
# Start with your CDS Server Metadata's well-known URL
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/carbon-data-spec.json"

# Get the OAuth Authorization Server Metadata URL from the CDS Server Metadata
OAUTH_METADATA_URL=$(curl -v "$CDS_METADATA_URL" | jq -r ".oauth_metadata")

# See the OAuth Authorization Server Metadata

curl -v "$OAUTH_METADATA_URL" | jq "."
{
    ...
    "cds_registration_fields": {...},   # Reference for any required registration fields
    ...
    "cds_scope_descriptions": {...},    # Descriptions of scopes supported, along with any registration requirements
    ...
    "registration_endpoint": "...",     # Where to register via API
    ...
    "scopes_supported": [...],          # The list of supported scopes for this CDS Server
    ...                                 # (e.g. "client_admin grant_admin server_provided_files customer_data")
}
# (determine which scopes and registration requirements you want for registration)
SCOPES_TO_REGISTER="client_admin grant_admin server_provided_files customer_data"

# Submit a OAuth Dynamic Client Registration
# (add any additional registration fields to the submitted json)
REGISTRATION_ENDPOINT=$(curl -v "$OAUTH_METADATA_URL" | jq -r ".registration_endpoint")
curl -v \
    --data-raw "{
        \"client_name\": \"Example Energy Audit Services\",
        \"client_uri\": \"https://example.com/\",
        \"scope\": \"$SCOPES_TO_REGISTER\"
    }" \
    "$REGISTRATION_ENDPOINT" \
    | jq "."
{
    ...
    "client_id": "aaaaaaaaaa",
    ...
    "client_secret": "bbbbbbbbbbbbbbb",
    ...
    "scope": "client_admin",  # The Server creates multiple Client objects based on your registration
    ...                       # and only returns the main client_admin Client object as a response.
                              # Use the Clients API to manage the other Client objects that were created.
}

# You have now registered! Save the client_id and client_secret for use in other examples
CLIENT_ID="aaaaaaaaaa"
CLIENT_SECRET="bbbbbbbbbbbbbbb"
</code>
</pre>
</details>


#### Example: Loading the list of Client objects <a id="example-load-clients" href="#example-load-clients" class="permalink">🔗</a>
<details>
<summary>Show example</summary>
<p>
After registering, you can load the list of Client objects created from your registration request.
</p>
<pre class="highlight">
<code>
# Start with the CDS Server Metadata's well-known URL, as well as your client_id and client_secret from the initial registration
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/carbon-data-spec.json"
CLIENT_ID="aaaaaaaaaa"
CLIENT_SECRET="bbbbbbbbbbbbbbb"

# Get the OAuth Token and Clients API endpoints from the OAuth Authorization Server Metadata
OAUTH_METADATA_URL=$(curl "$CDS_METADATA_URL" | jq -r ".oauth_metadata")
OAUTH_METADATA_OBJECT=$(curl "$OAUTH_METADATA_URL" | jq ".")
TOKEN_ENDPOINT=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".token_endpoint")
CDS_CLIENTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_clients_api")

# Obtain a client_admin access_token
curl -v \
    -u "$CLIENT_ID:$CLIENT_SECRET" \
    -d "grant_type=client_credentials" \
    -d "scope=client_admin" \
    "$TOKEN_ENDPOINT" \
    | jq "."
{
    ...
    "access_token": "cccccccccccccc",
    ...
}
# (extract the access token for use in the Clients API)
CLIENT_ADMIN_ACCESS_TOKEN="cccccccccccccc"

# Get your list of Client objects
curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CLIENTS_API" \
    | jq "."
{
    "clients": [
        {
            ...
            "cds_client_uri": "...",
            ...
            "client_id": "aaaaaaaaaa",
            ...
            "scope": "client_admin",
            ...
        },
        {
            ...
            "cds_client_uri": "...",
            ...
            "client_id": "aaaaaaaaaa-1",
            ...
            "scope": "grant_admin",
            ...
        },
        {
            ...
            "cds_client_uri": "...",
            ...
            "client_id": "aaaaaaaaaa-2",
            ...
            "scope": "server_provided_files",
            ...
        },
        {
            ...
            "cds_client_uri": "...",
            ...
            "client_id": "aaaaaaaaaa-3",
            ...
            "redirect_uris": ["https://cdsserver123.example.com/receipt"],  # default redirect_uris set by the Server
            ...
            "response_types": ["code"],
            ...
            "scope": "customer_data",
            ...
        }
    ],
    ...
}
</code>
</pre>
</details>


#### Example: Adding a Redirect URI to a Client object <a id="example-modify-redirect-uris" href="#example-modify-redirect-uris" class="permalink">🔗</a>
<details>
<summary>Show example</summary>
<p>
For Client objects that require user authorization (e.g. <code>"response_type": ["code"]</code>), you can set your own list of <code>redirect_uris</code> by modifying the relevant Client object.
</p>
<pre class="highlight">
<code>
# Start with the CDS Server Metadata's well-known URL and a client_admin access_token
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/carbon-data-spec.json"
CLIENT_ADMIN_ACCESS_TOKEN="cccccccccccccc"

# Get the Clients API endpoint from the OAuth Authorization Server Metadata
OAUTH_METADATA_URL=$(curl "$CDS_METADATA_URL" | jq -r ".oauth_metadata")
OAUTH_METADATA_OBJECT=$(curl "$OAUTH_METADATA_URL" | jq ".")
CDS_CLIENTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_clients_api")

# Get the Client object that needs to be updated
CUSTOMER_DATA_CLIENT_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CLIENTS_API" \
    | jq ".clients | .[] | select(.scope==\"customer_data\")")

# Add your own redirect_uri to the current redirect_uris
CUSTOM_REDIRECT_URI="https://example.com/my-redirect"
DEFAULT_REDIRECT_URI=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".redirect_uris | .[0]")
UPDATED_CUSTOMER_DATA_CLIENT_OBJECT=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" \
    | sed "s|\"$DEFAULT_REDIRECT_URI\"|\"$DEFAULT_REDIRECT_URI\", \"$CUSTOM_REDIRECT_URI\"|g")

# Modify the Client object
CUSTOMER_DATA_CDS_CLIENT_URI=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".cds_client_uri")
curl -v -X PUT \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    --data-raw "$UPDATED_CUSTOMER_DATA_CLIENT_OBJECT" \
    "$CUSTOMER_DATA_CDS_CLIENT_URI" \
    | jq "."
{
    ...
    "redirect_uris": [
        "https://example.com/my-redirect",          # Your custom redirect_uri
        "https://cdsserver123.example.com/receipt"  # Server's default redirect_uri
    ],
    ...
}

# If you want to revert back to just the Server default, submit an empty list as the redirect_uris
REVERTED_CUSTOMER_DATA_CLIENT_OBJECT="..." # (modified to "redirect_uris": [])
curl -v -X PUT \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    --data-raw "$REVERTED_CUSTOMER_DATA_CLIENT_OBJECT" \
    "$CUSTOMER_DATA_CDS_CLIENT_URI" \
    | jq "."
{
    ...
    "redirect_uris": [
        "https://cdsserver123.example.com/receipt"  # Reverted back to just the Server default
    ],
    ...
}
</code>
</pre>
</details>


#### Example: Obtaining a user authorization <a id="example-user-authorization" href="#example-user-authorization" class="permalink">🔗</a>
<details>
<summary>Show example</summary>
<p>
For Client objects that require user authorization (e.g. <code>"response_type": ["code"]</code>), use the <code>authorization_endpoint</code> to obtain an authorization code.
</p>
<p>
This example assumes you've already added your custom <code>redirect_uri</code> to the relevant Client object's <code>redirect_uris</code>.
</p>
<pre class="highlight">
<code>
# Start with the CDS Server Metadata's well-known URL and a client_admin access_token
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/carbon-data-spec.json"
CLIENT_ADMIN_ACCESS_TOKEN="cccccccccccccc"
CUSTOM_REDIRECT_URI="https://example.com/my-redirect"

# Get the Authorization, Token, Clients API, and Credentials API endpoints from the OAuth Authorization Server Metadata
OAUTH_METADATA_URL=$(curl "$CDS_METADATA_URL" | jq -r ".oauth_metadata")
OAUTH_METADATA_OBJECT=$(curl "$OAUTH_METADATA_URL" | jq ".")
AUTHORIZATION_ENDPOINT=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".authorization_endpoint")
TOKEN_ENDPOINT=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".token_endpoint")
CDS_CLIENTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_clients_api")
CDS_CREDENTIALS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_credentials_api")

# Get the Client ID to be used for getting authorization
CUSTOMER_DATA_CLIENT_ID=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CLIENTS_API" \
    | jq -r ".clients | .[] | select(.scope==\"customer_data\") | .client_id")

# Get the client_secret for the Client object that will be used for the authorization
CUSTOMER_DATA_CLIENT_SECRET=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CREDENTIALS_API?client_ids=$CUSTOMER_DATA_CLIENT_ID" \
    | jq -r ".credentials | .[0] | .client_secret")

# Build the Authorization Request URL
AUTHORIZATION_REQUEST_URL="$AUTHORIZATION_ENDPOINT\
?response_type=code\
&client_id=$CUSTOMER_DATA_CLIENT_ID\
&scope=customer_data\
&redirect_uri=$CUSTOM_REDIRECT_URI\
&state=mystatehere"

# (user open's the AUTHORIZATION_REQUEST_URL in a browser and completes the registration)

# (after authorization, the user is redirected back to your custom redirect_uri with and authorization code)
https://example.com/my-redirect?state=mystatehere&code=ddddddddddddd

AUTHORIZATION_CODE="ddddddddddddd"

# Obtain an access_token from the authorization code
curl -v \
    -u "$CUSTOMER_DATA_CLIENT_ID:$CUSTOMER_DATA_CLIENT_SECRET" \
    -d "grant_type=authorization_code" \
    -d "code=$AUTHORIZATION_CODE" \
    -d "redirect_uri=$CUSTOM_REDIRECT_URI" \
    "$TOKEN_ENDPOINT" \
    | jq "."
{
    ...
    "access_token": "eeeeeeeeeeeee",
    ...
}

# (extract the access token for use in the Customer Data API)
CUSTOMER_DATA_ACCESS_TOKEN="eeeeeeeeeeeee"
</code>
</pre>
</details>


#### Example: Using the Grant Admin scope to load data for another Grant <a id="example-grant-admin" href="#example-grant-admin" class="permalink">🔗</a>
<details>
<summary>Show example</summary>
<p>
You can use the <code>grant_admin</code> Client object to generate an <code>access_token</code> that can access data for another Client object's Grants.
</p>
<p>
This is useful for gaining access to data and functionality when Servers create Grants on their side (e.g. the Server-Provided Files API) or when user authorization requests use the Server's default `redirect_uri</code> (thus you never get a redirect back with a <code>code</code>).
</p>
<pre class="highlight">
<code>
# Start with the CDS Server Metadata's well-known URL and a client_admin access_token
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/carbon-data-spec.json"
CLIENT_ADMIN_ACCESS_TOKEN="cccccccccccccc"
CUSTOM_REDIRECT_URI="https://example.com/my-redirect"

# Get the Authorization, Token, Clients API, and Credentials API endpoints from the OAuth Authorization Server Metadata
OAUTH_METADATA_URL=$(curl "$CDS_METADATA_URL" | jq -r ".oauth_metadata")
OAUTH_METADATA_OBJECT=$(curl "$OAUTH_METADATA_URL" | jq ".")
AUTHORIZATION_ENDPOINT=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".authorization_endpoint")
TOKEN_ENDPOINT=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".token_endpoint")
CDS_CLIENTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_clients_api")
CDS_CREDENTIALS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_credentials_api")
CDS_GRANTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_grants_api")

# Get the Client ID and default redirect_uri to be used for getting authorization
CUSTOMER_DATA_CLIENT_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CLIENTS_API" \
    | jq -r ".clients | .[] | select(.scope==\"customer_data\")")
CUSTOMER_DATA_CLIENT_ID=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".client_id")
DEFAULT_REDIRECT_URI=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".redirect_uris | .[] | select(. != \"$CUSTOM_REDIRECT_URI\")")

# Build the Authorization Request URL
AUTHORIZATION_REQUEST_URL="$AUTHORIZATION_ENDPOINT\
?response_type=code\
&client_id=$CUSTOMER_DATA_CLIENT_ID\
&scope=customer_data\
&redirect_uri=$DEFAULT_REDIRECT_URI\
&state=mystatehere"

# (user open's the AUTHORIZATION_REQUEST_URL in a browser and completes the registration)

# (after authorization, the user is redirected to a Server-hosted receipt page)
https://cdsserver123.example.com/receipts/fffffffffffffffff/

# Get the Grant for the most recent authorization
GRANT_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_GRANTS_API?client_ids=$CUSTOMER_DATA_CLIENT_ID" \
    | jq ".grants | .[0]")
GRANT_ID=$(echo "$GRANT_OBJECT" | jq -r ".grant_id")

# Get the client_id grant_admin Client object
GRANT_ADMIN_CLIENT_ID=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CLIENTS_API" \
    | jq -r ".clients | .[] | select(.scope==\"grant_admin\") | .client_id")

# Get the client_secret grant_admin Client object
GRANT_ADMIN_CLIENT_SECRET=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CREDENTIALS_API?client_ids=$GRANT_ADMIN_CLIENT_ID" \
    | jq -r ".credentials | .[0] | .client_secret")

# Create an access_token for the grant using the grant_admin
curl -v \
    -u "$GRANT_ADMIN_CLIENT_ID:$GRANT_ADMIN_CLIENT_SECRET" \
    -d "grant_type=client_credentials" \
    -d "scope=grant_admin" \
    -d "authorization_details=[{
        \"type\": \"grant_admin\",
        \"client_id\": \"$CUSTOMER_DATA_CLIENT_ID\",
        \"grant_id\": \"$GRANT_ID\"
    }]" \
    "$TOKEN_ENDPOINT" \
    | jq "."
{
    ...
    "access_token": "gggggggggggggggg",
    ...
}

# (extract the access token for use in the Customer Data API)
GRANT_ADMIN_ACCESS_TOKEN="gggggggggggggggg"
</code>
</pre>
</details>


---

# Other Drafts <a id="other-drafts" href="#other-drafts" class="permalink">🔗</a>

* Maintainer's draft: [[Website](https://daniel-utilityapi.github.io/Connectivity/specs/cdsc-wg1-02/overview)] [[Code](https://github.com/daniel-utilityapi/Connectivity/blob/main/website/specs/cdsc-wg1-02/overview.md)]

