---
layout: base
nav: specs
title: CDS-WG1 - 02 Client Registration - Overview
meta_description: Linux Foundation Energy - Connected Data Specification (CDS) - Registration Working Group (WG1) - Specifications - cds-wg1-02 - Client Registration - Overview
---
[Home]({{ "/" | relative_url }}) / [Specifications]({{ "/specs" | relative_url }}) / [`cds-wg1-02` - Client Registration]({{ "/specs/cds-wg1-02" | relative_url }}) / Overview

# Client Registration - Overview

This is a summary overview of the [`cds-wg1-02`]({{ "/specs/cds-wg1-02" | relative_url }}) specification, which defines how utilities and other central grid entities ("Servers") can provide secure, managed connectivity with external entities ("Clients").
External entities could be anyone, including enterprise customers, vendors, energy service providers, tech companies, and more.
The goal of this specification is to define a standardized way for utilities and other central grid entities to offer secure, streamlined, and automated connections to the external organizations with which they are working.

This specification defines protocols for registration, communication, secrets management, and secure data exchange, which makes it an ideal "off-the-shelf" solution for utilities and other central entities to use to initially establish secure connections to external entities.
Then, once a secure connection is established, other protocols (e.g. demand response signals, energy data exchange, etc.) can use that secure connection as needed for the specific use case.

For example, a utility could deploy a Server that implements this specification where demand response providers can register, submit documents, be reviewed, approved, and get issued access tokens in a standardized and scalable manner.

Additionally, this specification can be used and referenced by other specifications as the means by which their protocols can initially setup a secure connection.

## Background <a id="background" href="#background" class="permalink">🔗</a>

Utilities and other central grid entities are increasingly needing to digitally interact with their vendors, service providers, customers, and other external organizations.
Many of these digital interactions (communication, data exchange, control systems, etc.) are sensitive and require secure connections to work.
However, initially setting up those secure connections in many cases is currently done using ad-hoc and manual processes that do not scale.

Therefore, to enable scaling the number of external secure connections needed by utilities and other central grid entities, an automated process for registering, onboarding, and maintaining these secure connections must be standardized.
This specification's goal is to define that standard.

## Technical Summary <a id="technical-summary" href="#technical-summary" class="permalink">🔗</a>

The [`cds-wg1-02`]({{ "/specs/cds-wg1-02" | relative_url }}) specification is fundamentally an extension of the [OAuth](https://oauth.net) specifications, which have been used to great success in many other industries for secure connectivity.
In addition to building on OAuth's protocols, the specification also defines additional protocols for Servers to manage Client registrations, messaging, credentials, and secure file transfers.

First, the specification defines how a utility or other central entity (called a "Server") can [publish metadata]({{ "/specs/cds-wg1-02" | relative_url }}#auth-server-metadata) about how an external entity (called a "Client") can register with the server.
The process for publishing Server metadata builds on both CDS's [`cds-wg1-01`]({{ "/specs/cds-wg1-02" | relative_url }}) ("Server Metadata") and OAuth's [RFC 8414](https://www.rfc-editor.org/rfc/rfc8414) ("Authorization Server Metadata").
It also defines how Servers can describe the functionality they offer (called "Scope Descriptions") and disclose any requirements or steps for Client registration and onboarding (called "Registration Requirements").

Next, the specification defines protocols for [Client registration]({{ "/specs/cds-wg1-02" | relative_url }}#client-registration-process) and [Client management]({{ "/specs/cds-wg1-02" | relative_url }}#clients-api).
Client registration is based on OAuth's [RFC 7591](https://www.rfc-editor.org/rfc/rfc7591) ("Dynamic Client Registration Protocol").
The Client management protocol also defines how Servers can allow Client administrators to manage and use multiple Client Objects via the [Client Admin Scope]({{ "/specs/cds-wg1-02" | relative_url }}#scopes-client-admin) and [Grant Admin Scope]({{ "/specs/cds-wg1-02" | relative_url }}#scopes-grant-admin).
These protocols allow for Servers to automate and scale the management of potentially high numbers of connected Clients for a diverse set of use cases.

Next, the specification defines a protocol for [messaging]({{ "/specs/cds-wg1-02" | relative_url }}#messages-api) between Servers and Clients.
This allows for Servers to securely communicate with their connected Clients in a standardized manner, as opposed to using insure and unstructured methods (e.g. e-mail) or proprietary methods (e.g. a third-party hosted vendor solution).

Next, the specification defines a protocol for [credential handling]({{ "/specs/cds-wg1-02" | relative_url }}#credentials-api), not only intended for managing and rotating Client secrets that can be used for the various APIs defined in the specification, but also potentially for managing credentials for other protocols that use or reference this specification.
This allows for Servers to securely issue and manage access tokens, secrets, and other highly sensitive credentials for their connected Clients.
Clients are also able to revoke and re-issue credentials, allowing for seamless and automated secret token rotation.

Next, the specification defines a protocol for [managing access grants]({{ "/specs/cds-wg1-02" | relative_url }}#grants-api), so that Clients can see what authorizations or access they've been granted.
This allows Servers to provide visibility of access to Clients, so that Clients may organize and self-manage the various grants they've been given in an automated manner.

Finally, the specification defines a protocol for securely sharing [server-provided files]({{ "/specs/cds-wg1-02" | relative_url }}#server-provided-files-api), so that Servers can securely share arbitrary use-case specific files with Clients.
This protocol is intended to be used for ad-hoc or one-time files (e.g. sharing a certificate during a Client onboarding process).
For structured datasets or regularly scheduled transfers of bulk data, Servers are recommended to use other protocols (that could reference this specification for establishing connectivity) that are more fit for purpose, since the included secure file sharing protocol does not have complex bulk data features, such as versioning, folder structures, deltas, etc.

Overall, the combination of protocols defined in this specification provides an "off-the-shelf" technical solution that can be freely referenced and implemented by utilities and other central entities to meet their needs for establishing and managing secure connectivity with external entities.

## API Overview <a id="api-overview" href="#api-overview" class="permalink">🔗</a>

Below are the web Application Programming Interface ("API") endpoints defined in [`cds-wg1-02`]({{ "/specs/cds-wg1-02" | relative_url }}).

**NOTE:** The below URLs are examples only, since each Server sets their own URLs for the specification's required endpoints and includes them in their [Metadata object]({{ "/specs/cds-wg1-02" | relative_url }}#auth-server-metadata-format).

#### Metadata APIs <a id="api-metadata" href="#api-metadata" class="permalink">🔗</a>

<span class="badge bg-success">GET</span>
[/.well-known/cds-server-metadata.json]({{ "/specs/cds-wg1-02" | relative_url }}#auth-server-metadata-url)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-cds-server-metadata)] -
The CDS Server Metadata endpoint (from [`cds-wg1-01`]({{ "/specs/cds-wg1-01" | relative_url }})) that is extended to include the `oauth` capability.

<span class="badge bg-success">GET</span>
[/.well-known/oauth-authorization-server]({{ "/specs/cds-wg1-02" | relative_url }}#auth-server-metadata-format)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-auth-server-metadata)] -
The OAuth Authorization Server Metadata endpoint that is extended to include various CDS data fields.

#### Client APIs <a id="api-clients" href="#api-clients" class="permalink">🔗</a>

<span class="badge bg-warning text-dark">POST</span>
[/oauth/client-registration]({{ "/specs/cds-wg1-02" | relative_url }}#registration-request)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-client-registration)] -
Registration endpoint for Clients based on OAuth's Dynamic Client Registration Protocol.

<span class="badge bg-success">GET</span>
[/cds-api/v1/clients]({{ "/specs/cds-wg1-02" | relative_url }}#clients-list)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-clients-list)] -
Lists [Client Objects]({{ "/specs/cds-wg1-02" | relative_url }}#client-format) that have been registered.

<span class="badge bg-success">GET</span>
[/cds-api/v1/clients/*&lt;clientId&gt;*]({{ "/specs/cds-wg1-02" | relative_url }}#clients-get)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-client-get)] -
Returns an individual [Client Object]({{ "/specs/cds-wg1-02" | relative_url }}#client-format).

<span class="badge bg-warning text-dark">PUT</span>
[/cds-api/v1/clients/*&lt;clientId&gt;*]({{ "/specs/cds-wg1-02" | relative_url }}#clients-modify)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-client-modify)] -
Modifies an individual [Client Object]({{ "/specs/cds-wg1-02" | relative_url }}#client-format).

#### Messages APIs <a id="api-messages" href="#api-messages" class="permalink">🔗</a>

<span class="badge bg-success">GET</span>
[/cds-api/v1/messages]({{ "/specs/cds-wg1-02" | relative_url }}#messages-list)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-messages-list)] -
Lists [Messages]({{ "/specs/cds-wg1-02" | relative_url }}#message-format) for a Client.

<span class="badge bg-warning text-dark">POST</span>
[/cds-api/v1/messages]({{ "/specs/cds-wg1-02" | relative_url }}#messages-create)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-message-create)] -
Creates a new [Message]({{ "/specs/cds-wg1-02" | relative_url }}#message-format) from the Client to the Server.

<span class="badge bg-success">GET</span>
[/cds-api/v1/messages/*&lt;messageId&gt;*]({{ "/specs/cds-wg1-02" | relative_url }}#messages-get)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-message-get)] -
Returns an individual [Message]({{ "/specs/cds-wg1-02" | relative_url }}#message-format) object.

<span class="badge bg-warning text-dark">PATCH</span>
[/cds-api/v1/messages/*&lt;messageId&gt;*]({{ "/specs/cds-wg1-02" | relative_url }}#messages-modify)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-message-modify)] -
Modifies an individual [Message]({{ "/specs/cds-wg1-02" | relative_url }}#message-format) object.

#### Credentials APIs <a id="api-credentials" href="#api-credentials" class="permalink">🔗</a>

<span class="badge bg-success">GET</span>
[/cds-api/v1/credentials]({{ "/specs/cds-wg1-02" | relative_url }}#credentials-list)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-credentials-list)] -
Lists [Credentials]({{ "/specs/cds-wg1-02" | relative_url }}#credentials-format) for a Client.

<span class="badge bg-warning text-dark">POST</span>
[/cds-api/v1/credentials]({{ "/specs/cds-wg1-02" | relative_url }}#credentials-create)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-credentials-create)] -
Creates a new [Credential]({{ "/specs/cds-wg1-02" | relative_url }}#credentials-format) object.

<span class="badge bg-success">GET</span>
[/cds-api/v1/credentials/*&lt;credentialId&gt;*]({{ "/specs/cds-wg1-02" | relative_url }}#credentials-get)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-credentials-get)] -
Returns an individual [Credential]({{ "/specs/cds-wg1-02" | relative_url }}#credentials-format) object.

<span class="badge bg-warning text-dark">PATCH</span>
[/cds-api/v1/credentials/*&lt;credentialId&gt;*]({{ "/specs/cds-wg1-02" | relative_url }}#credentials-modify)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-credentials-modify)] -
Modifies an individual [Credential]({{ "/specs/cds-wg1-02" | relative_url }}#credentials-format) object.

#### Grants APIs <a id="api-grants" href="#api-grants" class="permalink">🔗</a>

<span class="badge bg-success">GET</span>
[/cds-api/v1/grants]({{ "/specs/cds-wg1-02" | relative_url }}#grants-list)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-grants-list)] -
Lists [Grants]({{ "/specs/cds-wg1-02" | relative_url }}#grant-format) for a Client.

<span class="badge bg-success">GET</span>
[/cds-api/v1/grants/*&lt;grantId&gt;*]({{ "/specs/cds-wg1-02" | relative_url }}#grants-get)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-grants-get)] -
Returns an individual [Grant]({{ "/specs/cds-wg1-02" | relative_url }}#grant-format) object.

<span class="badge bg-warning text-dark">PATCH</span>
[/cds-api/v1/grants/*&lt;grantId&gt;*]({{ "/specs/cds-wg1-02" | relative_url }}#grants-modify)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-grants-modify)] -
Modifies an individual [Grant]({{ "/specs/cds-wg1-02" | relative_url }}#grant-format) object.

#### Server-Provided Files APIs <a id="api-server-provided-files" href="#api-server-provided-files" class="permalink">🔗</a>

<span class="badge bg-success">GET</span>
[/cds-api/v1/server-provided-files]({{ "/specs/cds-wg1-02" | relative_url }}#server-provided-files-list)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-server-provided-files-list)] -
Lists [Server-Provided Files]({{ "/specs/cds-wg1-02" | relative_url }}#server-provided-files-format) for a Client.

<span class="badge bg-success">GET</span>
[/cds-api/v1/server-provided-files/*&lt;fileId&gt;*]({{ "/specs/cds-wg1-02" | relative_url }}#server-provided-files-get)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-server-provided-files-get)] -
Returns an individual [Server-Provided File]({{ "/specs/cds-wg1-02" | relative_url }}#server-provided-files-format) object (metadata and file details for the Server-Provided File).

<span class="badge bg-success">GET</span>
[/cds-api/v1/server-provided-files/*&lt;fileId&gt;*/download]({{ "/specs/cds-wg1-02" | relative_url }}#server-provided-files-download)
[[example]({{ "/specs/cds-wg1-02" | relative_url }}#example-server-provided-files-download)] -
Returns the raw data for an individual [Server-Provided File]({{ "/specs/cds-wg1-02" | relative_url }}#server-provided-files-format) object.

## Examples <a id="examples" href="#examples" class="permalink">🔗</a>

Here are some examples of how to programmatically interact with a Server's APIs using `curl` and `jq` from the command line.

#### Example: Registering a Client <a id="example-registering" href="#example-registering" class="permalink">🔗</a>

How to register with a CDS Server for a variety of scopes.

<details style="margin-bottom:2em;">
<summary>Show example</summary>
<pre class="highlight">
<code>
# Start with your CDS Server Metadata's well-known URL
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/cds-server-metadata.json"

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
    ...                                 # (e.g. "cds_client_admin cds_grant_admin cds_server_provided_files_01 customer_data")
}
# (determine which scopes and registration requirements you want for registration)
SCOPES_TO_REGISTER="cds_client_admin cds_grant_admin cds_server_provided_files_01 customer_data"

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
    "scope": "cds_client_admin",   # The Server creates multiple Client Objects based on your registration
    ...                            # and only returns the main cds_client_admin Client Object as a response.
                                   # Use the Client Objects API to manage the other Client Objects that were created.
}

# You have now registered! Save the client_id and client_secret for use in other examples
CLIENT_ID="aaaaaaaaaa"
CLIENT_SECRET="bbbbbbbbbbbbbbb"
</code>
</pre>
</details>


#### Example: Loading the list of Client Objects <a id="example-load-clients" href="#example-load-clients" class="permalink">🔗</a>

After registering, you can load the list of Client Objects created from your registration request.

<details style="margin-bottom:2em;">
<summary>Show example</summary>
<pre class="highlight">
<code>
# Start with the CDS Server Metadata's well-known URL, as well as your client_id and client_secret from the initial registration
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/cds-server-metadata.json"
CLIENT_ID="aaaaaaaaaa"
CLIENT_SECRET="bbbbbbbbbbbbbbb"

# Get the OAuth Token and Client Objects API endpoints from the OAuth Authorization Server Metadata
OAUTH_METADATA_URL=$(curl "$CDS_METADATA_URL" | jq -r ".oauth_metadata")
OAUTH_METADATA_OBJECT=$(curl "$OAUTH_METADATA_URL" | jq ".")
TOKEN_ENDPOINT=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".token_endpoint")
CDS_CLIENTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_clients_api")

# Obtain a cds_client_admin access_token
curl -v \
    -u "$CLIENT_ID:$CLIENT_SECRET" \
    -d "grant_type=client_credentials" \
    -d "scope=cds_client_admin" \
    "$TOKEN_ENDPOINT" \
    | jq "."
{
    ...
    "access_token": "cccccccccccccc",
    ...
}
# (extract the access token for use in the Client Objects API)
CLIENT_ADMIN_ACCESS_TOKEN="cccccccccccccc"

# Get your list of Client Objects
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
            "scope": "cds_client_admin",
            ...
        },
        {
            ...
            "cds_client_uri": "...",
            ...
            "client_id": "aaaaaaaaaa-1",
            ...
            "scope": "cds_grant_admin",
            ...
        },
        {
            ...
            "cds_client_uri": "...",
            ...
            "client_id": "aaaaaaaaaa-2",
            ...
            "scope": "cds_server_provided_files_01",
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


#### Example: Adding a Redirect URI to a Client Object <a id="example-modify-redirect-uris" href="#example-modify-redirect-uris" class="permalink">🔗</a>

For Client Objects that require user authorization (e.g. `"response_type": ["code"]`), you can set your own list of `redirect_uris` by modifying the relevant Client Object.

<details style="margin-bottom:2em;">
<summary>Show example</summary>
<pre class="highlight">
<code>
# Start with the CDS Server Metadata's well-known URL
# and CLIENT_ADMIN_ACCESS_TOKEN from "Loading the list of Client Objects" example
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/cds-server-metadata.json"
CLIENT_ADMIN_ACCESS_TOKEN="cccccccccccccc"

# Get the Client Objects API endpoint from the OAuth Authorization Server Metadata
OAUTH_METADATA_URL=$(curl "$CDS_METADATA_URL" | jq -r ".oauth_metadata")
OAUTH_METADATA_OBJECT=$(curl "$OAUTH_METADATA_URL" | jq ".")
CDS_CLIENTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_clients_api")

# Get the Client Object that needs to be updated
CUSTOMER_DATA_CLIENT_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CLIENTS_API" \
    | jq ".clients | .[] | select(.scope==\"customer_data\")")

# Add your own redirect_uri to the current redirect_uris
CUSTOM_REDIRECT_URI="https://example.com/my-redirect"
DEFAULT_REDIRECT_URI=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".redirect_uris | .[0]")
UPDATED_CUSTOMER_DATA_CLIENT_OBJECT=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" \
    | sed "s|\"$DEFAULT_REDIRECT_URI\"|\"$DEFAULT_REDIRECT_URI\", \"$CUSTOM_REDIRECT_URI\"|g")

# Modify the Client Object
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

For Client Objects that require user authorization (e.g. `"response_type": ["code"]`), use the `authorization_endpoint` to obtain an authorization code.

This example assumes you've already added your custom `redirect_uri` to the relevant Client Object's `redirect_uris`.

<details style="margin-bottom:2em;">
<summary>Show example</summary>
<pre class="highlight">
<code>
# Start with the CDS Server Metadata's well-known URL
# and CLIENT_ADMIN_ACCESS_TOKEN from "Loading the list of Client Objects" example
# and CUSTOM_REDIRECT_URI from "Adding a Redirect URI to a Client Object" example
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/cds-server-metadata.json"
CLIENT_ADMIN_ACCESS_TOKEN="cccccccccccccc"
CUSTOM_REDIRECT_URI="https://example.com/my-redirect"

# Get the Authorization, Token, Client Objects API, and Credentials API endpoints from the OAuth Authorization Server Metadata
OAUTH_METADATA_URL=$(curl "$CDS_METADATA_URL" | jq -r ".oauth_metadata")
OAUTH_METADATA_OBJECT=$(curl "$OAUTH_METADATA_URL" | jq ".")
CDS_CLIENTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_clients_api")
CDS_CREDENTIALS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_credentials_api")

# Get the Client details to be used for getting authorization
CUSTOMER_DATA_SCOPE="customer_data"
CUSTOMER_DATA_CLIENT_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CLIENTS_API" \
    | jq -r ".clients | .[] | select(.scope==\"$CUSTOMER_DATA_SCOPE\")")
CUSTOMER_DATA_CLIENT_ID=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".client_id")
CUSTOMER_DATA_CDS_METADATA_URL=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".cds_server_metadata")
CUSTOMER_DATA_OAUTH_METADATA_URL=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CUSTOMER_DATA_CDS_METADATA_URL" \
    | jq -r ".oauth_metadata")
CUSTOMER_DATA_OAUTH_METADATA_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CUSTOMER_DATA_OAUTH_METADATA_URL" \
    | jq ".")
CUSTOMER_DATA_AUTHORIZATION_ENDPOINT=$(echo "$CUSTOMER_DATA_OAUTH_METADATA_OBJECT" | jq -r ".authorization_endpoint")
CUSTOMER_DATA_TOKEN_ENDPOINT=$(echo "$CUSTOMER_DATA_OAUTH_METADATA_OBJECT" | jq -r ".token_endpoint")
CUSTOMER_DATA_ACCOUNTS_ENDPOINT=$(echo "$CUSTOMER_DATA_OAUTH_METADATA_OBJECT" | jq -r ".cds_customerdata_accounts_api")

# Get the client_secret for the Client Object that will be used for the authorization
CUSTOMER_DATA_CLIENT_SECRET=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CREDENTIALS_API?client_ids=$CUSTOMER_DATA_CLIENT_ID" \
    | jq -r ".credentials | .[0] | .client_secret")

# Build the Authorization Request URL
AUTHORIZATION_REQUEST_URL="$CUSTOMER_DATA_AUTHORIZATION_ENDPOINT\
?response_type=code\
&client_id=$CUSTOMER_DATA_CLIENT_ID\
&scope=$CUSTOMER_DATA_SCOPE\
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
    "$CUSTOMER_DATA_TOKEN_ENDPOINT" \
    | jq "."
{
    ...
    "access_token": "eeeeeeeeeeeee",
    ...
}

# (extract the access token for use in the Customer Data API)
CUSTOMER_DATA_ACCESS_TOKEN="eeeeeeeeeeeee"

# make a request to a Customer Data API endpoint
curl -v \
    -H "Authorization: Bearer $CUSTOMER_DATA_ACCESS_TOKEN" \
    "$CUSTOMER_DATA_ACCOUNTS_ENDPOINT" \
    | jq "."
{
    "accounts": [...],
    "next": null,
    "previous": null
}
</code>
</pre>
</details>


#### Example: Using the Grant Admin scope to load data for another Grant <a id="example-grant-admin" href="#example-grant-admin" class="permalink">🔗</a>

You can use the `cds_grant_admin` Client Object to generate an `access_token` that can access data for another Client Object's Grants.

This is useful for gaining access to data and functionality when Servers create Grants on their side (e.g. the Server-Provided Files API) or when user authorization requests use the Server's default `redirect_uri` (thus you never get a redirect back with a `code`).

<details style="margin-bottom:2em;">
<summary>Show example</summary>
<pre class="highlight">
<code>
# Start with the CDS Server Metadata's well-known URL
# and CLIENT_ADMIN_ACCESS_TOKEN from "Loading the list of Client Objects" example
# and CUSTOM_REDIRECT_URI from "Adding a Redirect URI to a Client Object" example
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/cds-server-metadata.json"
CLIENT_ADMIN_ACCESS_TOKEN="cccccccccccccc"
CUSTOM_REDIRECT_URI="https://example.com/my-redirect"

# Get the Authorization, Token, Client Objects API, and Credentials API endpoints from the OAuth Authorization Server Metadata
OAUTH_METADATA_URL=$(curl "$CDS_METADATA_URL" | jq -r ".oauth_metadata")
OAUTH_METADATA_OBJECT=$(curl "$OAUTH_METADATA_URL" | jq ".")
CDS_CLIENTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_clients_api")
CDS_CREDENTIALS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_credentials_api")
CDS_GRANTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_grants_api")

# Get the Client details to be used for getting authorization
CUSTOMER_DATA_SCOPE="customer_data"
CUSTOMER_DATA_CLIENT_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CLIENTS_API" \
    | jq -r ".clients | .[] | select(.scope==\"$CUSTOMER_DATA_SCOPE\")")
CUSTOMER_DATA_CLIENT_ID=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".client_id")
DEFAULT_REDIRECT_URI=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".redirect_uris | .[] | select(. != \"$CUSTOM_REDIRECT_URI\")")
CUSTOMER_DATA_CDS_METADATA_URL=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".cds_server_metadata")
CUSTOMER_DATA_OAUTH_METADATA_URL=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CUSTOMER_DATA_CDS_METADATA_URL" \
    | jq -r ".oauth_metadata")
CUSTOMER_DATA_OAUTH_METADATA_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CUSTOMER_DATA_OAUTH_METADATA_URL" \
    | jq ".")
CUSTOMER_DATA_AUTHORIZATION_ENDPOINT=$(echo "$CUSTOMER_DATA_OAUTH_METADATA_OBJECT" | jq -r ".authorization_endpoint")
CUSTOMER_DATA_ACCOUNTS_ENDPOINT=$(echo "$CUSTOMER_DATA_OAUTH_METADATA_OBJECT" | jq -r ".cds_customerdata_accounts_api")

# Build the Authorization Request URL
AUTHORIZATION_REQUEST_URL="$CUSTOMER_DATA_AUTHORIZATION_ENDPOINT\
?response_type=code\
&client_id=$CUSTOMER_DATA_CLIENT_ID\
&scope=$CUSTOMER_DATA_SCOPE\
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

# Get the cds_grant_admin Client details
GRANT_ADMIN_SCOPE="cds_grant_admin"
GRANT_ADMIN_CLIENT_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CLIENTS_API" \
    | jq -r ".clients | .[] | select(.scope==\"$GRANT_ADMIN_SCOPE\")")
GRANT_ADMIN_CLIENT_ID=$(echo "$GRANT_ADMIN_CLIENT_OBJECT" | jq -r ".client_id")
GRANT_ADMIN_CDS_METADATA_URL=$(echo "$GRANT_ADMIN_CLIENT_OBJECT" | jq -r ".cds_server_metadata")
GRANT_ADMIN_OAUTH_METADATA_URL=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$GRANT_ADMIN_CDS_METADATA_URL" \
    | jq -r ".oauth_metadata")
GRANT_ADMIN_TOKEN_ENDPOINT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$GRANT_ADMIN_OAUTH_METADATA_URL" \
    |jq -r ".token_endpoint")

# Get the client_secret grant_admin Client Object
GRANT_ADMIN_CLIENT_SECRET=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CREDENTIALS_API?client_ids=$GRANT_ADMIN_CLIENT_ID" \
    | jq -r ".credentials | .[0] | .client_secret")

# Create an access_token for the grant using the cds_grant_admin
curl -v \
    -u "$GRANT_ADMIN_CLIENT_ID:$GRANT_ADMIN_CLIENT_SECRET" \
    -d "grant_type=client_credentials" \
    -d "authorization_details=[{
        \"type\": \"$GRANT_ADMIN_SCOPE\",
        \"client_id\": \"$CUSTOMER_DATA_CLIENT_ID\",
        \"grant_id\": \"$GRANT_ID\"
    }]" \
    "$GRANT_ADMIN_TOKEN_ENDPOINT" \
    | jq "."
{
    ...
    "access_token": "gggggggggggggggg",
    ...
}

# (extract the access token for use in the Customer Data API)
GRANT_ADMIN_ACCESS_TOKEN="gggggggggggggggg"

# make a request to a Customer Data API endpoint
curl -v \
    -H "Authorization: Bearer $GRANT_ADMIN_ACCESS_TOKEN" \
    "$CUSTOMER_DATA_ACCOUNTS_ENDPOINT" \
    | jq "."
{
    "accounts": [...],
    "next": null,
    "previous": null
}
</code>
</pre>
</details>


#### Example: Providing a JWK in authorization_details for encrypted responses <a id="example-auth-details-jwk" href="#example-auth-details-jwk" class="permalink">🔗</a>

When supported, you can provide a JWK public key in the `authorization_details` of authorization requests, which triggers encrypting API responses for access granted to be encrypted with that public key.

This is useful for enabling device-level encryption of authorized data, where a device can provide a public key, the user of that device can authorize access, and the data can be encrypted with that device's individual public key.

<details style="margin-bottom:2em;">
<summary>Show example</summary>
<pre class="highlight">
<code>
# Start with the CDS Server Metadata's well-known URL
# and CLIENT_ADMIN_ACCESS_TOKEN from "Loading the list of Client Objects" example
# and CUSTOM_REDIRECT_URI from "Adding a Redirect URI to a Client Object" example
CDS_METADATA_URL="https://cdsserver123.example.com/.well-known/cds-server-metadata.json"
CLIENT_ADMIN_ACCESS_TOKEN="cccccccccccccc"
CUSTOM_REDIRECT_URI="https://example.com/my-redirect"

# Generate elliptical curve JWK (below is an example, DO NOT USE THIS, GENERATE YOUR OWN)
JWK_PRIVATE_KEY='{
    "kty": "EC",
    "crv": "P-256",
    "x": "gI0GAILBdu7T53akrFmMyGcsF3n5dO7MmwNBHKW5SV0",
    "y": "SLW_xSffzlPWrHEVI30DHM_4egVwt3NQqeUD7nMFpps",
    "d": "0_NxaRPUMQoAJt50Gz8YiTr8gRTwyEaCumd-MToTmIo"
}'
JWK_PUBLIC_KEY='{
    "kty": "EC",
    "crv": "P-256",
    "x": "gI0GAILBdu7T53akrFmMyGcsF3n5dO7MmwNBHKW5SV0",
    "y": "SLW_xSffzlPWrHEVI30DHM_4egVwt3NQqeUD7nMFpps"
}'

# Get the Clients and Credentials API endpoints from the OAuth Authorization Server Metadata
OAUTH_METADATA_URL=$(curl "$CDS_METADATA_URL" | jq -r ".oauth_metadata")
OAUTH_METADATA_OBJECT=$(curl "$OAUTH_METADATA_URL" | jq ".")
CDS_CLIENTS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_clients_api")
CDS_CREDENTIALS_API=$(echo "$OAUTH_METADATA_OBJECT" | jq -r ".cds_credentials_api")

# Get the customer data Client details to be used for getting authorization
CUSTOMER_DATA_SCOPE="customer_data"
CUSTOMER_DATA_CLIENT_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CLIENTS_API" \
    | jq -r ".clients | .[] | select(.scope==\"$CUSTOMER_DATA_SCOPE\")")
CUSTOMER_DATA_CLIENT_ID=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".client_id")
CUSTOMER_DATA_CDS_METADATA_URL=$(echo "$CUSTOMER_DATA_CLIENT_OBJECT" | jq -r ".cds_server_metadata")
CUSTOMER_DATA_OAUTH_METADATA_URL=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CUSTOMER_DATA_CDS_METADATA_URL" \
    | jq -r ".oauth_metadata")
CUSTOMER_DATA_OAUTH_METADATA_OBJECT=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CUSTOMER_DATA_OAUTH_METADATA_URL" \
    | jq ".")
CUSTOMER_DATA_AUTHORIZATION_ENDPOINT=$(echo "$CUSTOMER_DATA_OAUTH_METADATA_OBJECT" | jq -r ".authorization_endpoint")
CUSTOMER_DATA_TOKEN_ENDPOINT=$(echo "$CUSTOMER_DATA_OAUTH_METADATA_OBJECT" | jq -r ".token_endpoint")
CUSTOMER_DATA_PAR_ENDPOINT=$(echo "$CUSTOMER_DATA_OAUTH_METADATA_OBJECT" | jq -r ".pushed_authorization_request_endpoint")
CUSTOMER_DATA_ACCOUNTS_ENDPOINT=$(echo "$CUSTOMER_DATA_OAUTH_METADATA_OBJECT" | jq -r ".cds_customerdata_accounts_api")

# Get the client_secret for the Client Object that will be used for the authorization
CUSTOMER_DATA_CLIENT_SECRET=$(curl -v \
    -H "Authorization: Bearer $CLIENT_ADMIN_ACCESS_TOKEN" \
    "$CDS_CREDENTIALS_API?client_ids=$CUSTOMER_DATA_CLIENT_ID" \
    | jq -r ".credentials | .[0] | .client_secret")

# Create a Pushed-Authorization Request request_uri with the JWK in authorization_details
PAR_REQUEST_URI=$(curl -v \
    -u "$CUSTOMER_DATA_CLIENT_ID:$CUSTOMER_DATA_CLIENT_SECRET" \
    -d "response_type=code" \
    -d "authorization_details=[{\"type\": \"$CUSTOMER_DATA_SCOPE\", \"jwk\": $JWK_PUBLIC_KEY}]" \
    -d "redirect_uri=$CUSTOM_REDIRECT_URI" \
    -d "state=mystatehere" \
    "$CUSTOMER_DATA_PAR_ENDPOINT" \
    | jq -r ".request_uri")

# Build the Authorization Request URL
AUTHORIZATION_REQUEST_URL="$CUSTOMER_DATA_AUTHORIZATION_ENDPOINT\
?client_id=$CUSTOMER_DATA_CLIENT_ID\
&request_uri=$PAR_REQUEST_URI"

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
    "$CUSTOMER_DATA_TOKEN_ENDPOINT" \
    | jq "."
{
    ...
    "access_token": "eeeeeeeeeeeee",
    ...
}

# (extract the access token for use in the Customer Data API)
CUSTOMER_DATA_ACCESS_TOKEN="eeeeeeeeeeeee"

# Make a request to get the data from the APIs
ENCRYPTED_HEADERS_AND_BODY=$(curl -v --include \
    -H "Authorization: Bearer $CUSTOMER_DATA_ACCESS_TOKEN" \
    "$CUSTOMER_DATA_ACCOUNTS_ENDPOINT")

# Example decrypting data using python's cryptography library
```
    import json
    from struct import pack
    from base64 import urlsafe_b64decode
    from cryptography.hazmat.primitives import hashes
    from cryptography.hazmat.primitives.kdf import concatkdf
    from cryptography.hazmat.primitives.asymmetric import ec
    from cryptography.hazmat.primitives.ciphers import aead

    # the encrypted response from the server
    RESPONSE_HEADERS = {
        "X-CDS-Header-JWE-Protected-Header": (
            "eyJhbGciOiAiRUNESC1FUyIsICJlbmMiOiAiQTI1NkdDTSIsICJlcGsiOiB7ImNydiI6ICJQLTI1NiIs"
            "ICJrdHkiOiAiRUMiLCAieCI6ICJIYlB1c0xxYktPbWFyQ3F1LUZYLWFiZ0FEbG10Y0xvYWRLaU41Yi1F"
            "U05ZIiwgInkiOiAiOHFCRE9KamMtdWZPOWl6UU1QMmw5cjhBREhfdUlUM29JMmpQaUozUXRJRSJ9fQ"
        ),
        "X-CDS-Header-JWE-Initialization-Vector": "7yhXitQOQAXeSUUp",
        "X-CDS-Header-JWE-Ciphertext": "GHyB5gkubIRA1OJYdxRGMXianidlLP0aBeYUAFZqooXeOlL4ckxUMR2HZQW5NZ2A8acJi3s",
        "X-CDS-Header-JWE-Authentication-Tag": "3fxjpNO4vgbgYsedo6srVA",
        "X-CDS-Body-JWE-Protected-Header": (
            "eyJhbGciOiAiRUNESC1FUyIsICJlbmMiOiAiQTI1NkdDTSIsICJlcGsiOiB7ImNydiI6ICJQLTI1NiIs"
            "ICJrdHkiOiAiRUMiLCAieCI6ICJGWDVjTnJjMkZoUjlMa0dfeTUwR0FKbzJFalZCc3FSQ2dGVjNRcWZj"
            "dm5vIiwgInkiOiAiTXRySWZuYnJHUnlnVWd6RVJlTnl4SENXa3ByaHUzUWo5TnRJdkpMVlFDVSJ9fQ"
        ),
        "X-CDS-Body-JWE-Initialization-Vector": "50q7kpETagvHqEBI",
    }
    RESPONSE_BODY = (
        b"G\x9aN\xcb\x88\x1f|@\xaa\x9du\x9a\xc6\x82x\xe8\x93\xb7S\xdeq\x1b"
        b"\x9d\xd3\xa1\x8b\xb3\xa9\x96n.'\xa8D\x86~\xfa\x00g\xd0'Y\x9b\x85"
        b"\xf2\r_\xab\xd4\x1e\xd9[\xfc\x03\x1d\xb50z\xfb\xa4 \xfeH\xd0"
    )

    # previously generated EC private key
    JWK_PRIVATE_KEY = {
        "kty": "EC",
        "crv": "P-256",
        "x": "gI0GAILBdu7T53akrFmMyGcsF3n5dO7MmwNBHKW5SV0",
        "y": "SLW_xSffzlPWrHEVI30DHM_4egVwt3NQqeUD7nMFpps",
        "d": "0_NxaRPUMQoAJt50Gz8YiTr8gRTwyEaCumd-MToTmIo"
    }

    # compose python private key from JWK_PRIVATE_KEY
    x = int.from_bytes(urlsafe_b64decode((JWK_PRIVATE_KEY["x"] + "===").encode()), byteorder="big")
    y = int.from_bytes(urlsafe_b64decode((JWK_PRIVATE_KEY["y"] + "===").encode()), byteorder="big")
    d = int.from_bytes(urlsafe_b64decode((JWK_PRIVATE_KEY["d"] + "===").encode()), byteorder="big")
    priv_key = ec.EllipticCurvePrivateNumbers(d, ec.EllipticCurvePublicNumbers(x, y, ec.SECP256R1())).private_key()

    # decode the public keys the response's encrypted headers
    headers_aad = RESPONSE_HEADERS["X-CDS-Header-JWE-Protected-Header"].encode()
    headers_jwe = json.loads(urlsafe_b64decode(headers_aad + b"==="))
    assert headers_jwe["alg"] == "ECDH-ES"
    assert headers_jwe["enc"] == "A256GCM"
    headers_x = int.from_bytes(urlsafe_b64decode((headers_jwe["epk"]["x"] + "===").encode()), byteorder="big")
    headers_y = int.from_bytes(urlsafe_b64decode((headers_jwe["epk"]["y"] + "===").encode()), byteorder="big")
    headers_pubkey = ec.EllipticCurvePublicNumbers(headers_x, headers_y, ec.SECP256R1()).public_key()

    # decode the public keys the response's encrypted body
    body_aad = RESPONSE_HEADERS["X-CDS-Body-JWE-Protected-Header"].encode()
    body_jwe = json.loads(urlsafe_b64decode(body_aad + b"==="))
    assert body_jwe["alg"] == "ECDH-ES"
    assert body_jwe["enc"] == "A256GCM"
    body_x = int.from_bytes(urlsafe_b64decode((body_jwe["epk"]["x"] + "===").encode()), byteorder="big")
    body_y = int.from_bytes(urlsafe_b64decode((body_jwe["epk"]["y"] + "===").encode()), byteorder="big")
    body_pubkey = ec.EllipticCurvePublicNumbers(body_x, body_y, ec.SECP256R1()).public_key()

    # calculate the JWE ephemeral AES-GCM key for the encrypted headers
    headers_shared_key = priv_key.exchange(ec.ECDH(), headers_pubkey)
    headers_algid = headers_jwe["enc"].encode()
    headers_apu = urlsafe_b64decode((headers_jwe["apu"] + "===").encode()) if "apu" in headers_jwe else b""
    headers_apv = urlsafe_b64decode((headers_jwe["apv"] + "===").encode()) if "apv" in headers_jwe else b""
    headers_other_info = b"".join([
        pack("!I", len(headers_algid)), headers_algid,
        pack("!I", len(headers_apu)), headers_apu,
        pack("!I", len(headers_apv)), headers_apv,
        pack("!I", 256),
    ])
    headers_ckdf = concatkdf.ConcatKDFHash(hashes.SHA256(), 256 // 8, headers_other_info)
    headers_aes_key = headers_ckdf.derive(headers_shared_key)

    # calculate the JWE ephemeral AES-GCM key for the encrypted body
    body_shared_key = priv_key.exchange(ec.ECDH(), body_pubkey)
    body_algid = body_jwe["enc"].encode()
    body_apu = urlsafe_b64decode((body_jwe["apu"] + "===").encode()) if "apu" in body_jwe else b""
    body_apv = urlsafe_b64decode((body_jwe["apv"] + "===").encode()) if "apv" in body_jwe else b""
    body_other_info = b"".join([
        pack("!I", len(body_algid)), body_algid,
        pack("!I", len(body_apu)), body_apu,
        pack("!I", len(body_apv)), body_apv,
        pack("!I", 256),
    ])
    body_ckdf = concatkdf.ConcatKDFHash(hashes.SHA256(), 256 // 8, body_other_info)
    body_aes_key = body_ckdf.derive(body_shared_key)

    # decrypt the headers (NOTE: need to concatinate ciphertext and authentication tag for data to decrypt)
    headers_iv = urlsafe_b64decode((RESPONSE_HEADERS["X-CDS-Header-JWE-Initialization-Vector"] + "===").encode())
    headers_ciphertext = urlsafe_b64decode((RESPONSE_HEADERS["X-CDS-Header-JWE-Ciphertext"] + "===").encode())
    headers_tag = urlsafe_b64decode((RESPONSE_HEADERS["X-CDS-Header-JWE-Authentication-Tag"] + "===").encode())
    headers_cleartext = aead.AESGCM(headers_aes_key).decrypt(headers_iv, headers_ciphertext + headers_tag, headers_aad)

    # decrypt the body (NOTE: authentication tag is already appended to the end of the response)
    body_iv = urlsafe_b64decode((RESPONSE_HEADERS["X-CDS-Body-JWE-Initialization-Vector"] + "===").encode())
    body_cleartext = aead.AESGCM(body_aes_key).decrypt(body_iv, RESPONSE_BODY, body_aad)

    # print the results
    # (encrypted headers = 'Content-Type: application/json\nX-Log-ID: 123456abcdef')
    # (encrypted body = '{"accounts":[], "next": null, "previous": null}')
    print("Headers:")
    for header_row in headers_cleartext.decode().split("\n"):
        print(header_row)
    print("Body:")
    print(body_cleartext)
```
</code>
</pre>
</details>
---

# Other Drafts <a id="other-drafts" href="#other-drafts" class="permalink">🔗</a>

* Maintainer's draft: [[Website](https://daniel-roesler.github.io/CDS-Registration/specs/cds-wg1-02/overview)] [[Code](https://github.com/daniel-roesler/CDS-Registration/blob/main/website/specs/cds-wg1-02/overview.md)]

