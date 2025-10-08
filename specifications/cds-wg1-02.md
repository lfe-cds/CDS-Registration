# CDS-WG1-02 - Client Registration

## Abstract <a id="abstract" href="#abstract" class="permalink">🔗</a>

This specification defines how utilities and other central entities ("Servers") may allow external entities ("Clients") to register, configure, and maintain connectivity with the utility or other central entity. This specification extends OAuth 2.0 [[RFC 6749](#ref-rfc6749)] and its related specifications (Dynamic Client Registration [[RFC 7591](#ref-rfc7591)], Authorization Server Metadata [[RFC 8414](#ref-rfc8414-server-metadata)], and others) to create a robust framework for offering secure, streamlined, and standardized connectivity between central entities, such as utilities and energy grid operators, and external entities, such as building owners and energy service providers.

## Status <a id="status" href="#status" class="permalink">🔗</a>

<b style="color:red">WARNING: This specification is a DRAFT and has not achieved consensus by the working group.</b>

## Copyright <a id="copyright" href="#copyright" class="permalink">🔗</a>

Copyright Joint Development Foundation Projects, LLC, LF Energy Standards and Specifications Series and its contributors ("LFESS").
All rights reserved.
For more information, visit [https://lfess.energy/](https://lfess.energy/).

## Table of Contents <a id="table-of-contents" href="#table-of-contents" class="permalink">🔗</a>

* [1. Introduction](#introduction)  
* [2. Terminology](#terminology)  
* [3. Authorization Server Metadata](#auth-server-metadata)  
    * [3.1. Metadata URL](#auth-server-metadata-url)  
    * [3.2. Metadata Object Format](#auth-server-metadata-format)  
    * [3.3. Scopes Supported](#scopes)  
        * [3.3.1. Client Admin Scope](#scopes-client-admin)  
        * [3.3.2. Grant Admin Scope](#scopes-grant-admin)  
        * [3.3.3. Server-Provided Files Scope](#scopes-server-provided-files)  
    * [3.4. Scope Descriptions Object Format](#scope-descriptions-format)  
    * [3.5. Registration Field Object Format](#registration-field-format)  
    * [3.6. Registration Field Types](#registration-field-types)  
    * [3.7. Registration Field Formats](#registration-field-formats)  
    * [3.8. Authorization Details Field Object Format](#auth-details-fields-format)  
    * [3.9. Authorization Details Field Formats](#auth-details-field-formats)  
    * [3.10. Authorization Details Field Choice Object Format](#auth-details-field-choice-format)  
* [4. Client Registration Process](#client-registration-process)  
    * [4.1. Client Registration Request](#registration-request)  
    * [4.2. Client Registration Response](#registration-response)  
* [5. Clients API](#clients-api)  
    * [5.1. Client Object Format](#client-format)  
    * [5.2. Client Statuses](#client-statuses)  
    * [5.3. Listing Clients](#clients-list)  
    * [5.4. Retrieving Individual Clients](#clients-get)  
    * [5.5. Modifying Clients](#clients-modify)  
* [6. Messages API](#messages-api)  
    * [6.1. Message Object Format](#message-format)  
    * [6.2. Message Types](#message-types)  
    * [6.3. Message Statuses](#message-statuses)  
    * [6.4. Message Related Types](#message-related-types)  
    * [6.5. Client Update Request Object Format](#client-update-request-format)  
    * [6.6. Client Grant Request Object Format](#client-grant-request-format)  
    * [6.7. Message Attachment Object Format](#message-attachment-format)  
    * [6.8. Listing Messages](#messages-list)  
    * [6.9. Creating Messages](#messages-create)  
    * [6.10. Retrieving Individual Messages](#messages-get)
    * [6.11. Modifying Messages](#messages-modify)  
* [7. Credentials API](#credentials-api)  
    * [7.1. Credentials Object Format](#credentials-format)  
    * [7.2. Credentials Types](#credentials-types)  
    * [7.3. Listing Credentials](#credentials-list)  
    * [7.4. Retrieving Individual Credentials](#credentials-get)  
    * [7.5. Creating Credentials](#credentials-create)  
    * [7.6. Modifying Credentials](#credentials-modify)  
* [8. Grants API](#grants-api)  
    * [8.1. Grant Object Format](#grant-format)  
    * [8.2. Grant Statuses](#grant-statuses)  
    * [8.3. Grant Authorization Requests](#grant-authorization-requests)  
    * [8.4. Listing Grants](#grants-list)  
    * [8.5. Retrieving Individual Grants](#grants-get)  
    * [8.6. Modifying Grants](#grants-modify) 
* [9. Server-Provided Files API](#server-provided-files-api)  
    * [9.1. Server-Provided Files Object Format](#server-provided-files-format)  
    * [9.2. Listing Server-Provided Files](#server-provided-files-list)  
    * [9.3. Retrieving Individual Server-Provided Files](#server-provided-files-get)  
* [10. Extensions](#extensions)  
* [11. Examples](#examples)  
    * [11.1. CDS Server Metadata](#example-cds-server-metadata)  
    * [11.2. Authorization Server Metadata](#example-auth-server-metadata)  
    * [11.3. Client Registration Request](#example-client-registration)  
    * [11.4. Client List](#example-client-list)  
    * [11.5. Creating a Message](#example-message-create)  
* [12. Security Considerations](#security)  
    * [12.1. Scopes and Client Management](#scopes-client-management)  
    * [12.2. Restricted Access](#restricted-access)  
    * [12.3. Rate Limiting](#rate-limiting)  
* [13. References](#references)  
* [14. Acknowledgments](#acknowledgments)  
* [15. Authors](#authors)  

## 1. Introduction <a id="introduction" href="#introduction" class="permalink">🔗</a>

This specification was developed as part of the global effort to combat the climate crisis.
Specifically, in order to scalably measure carbon emissions of organizations and calculate the impact of deploying and operating clean energy technologies, companies need an efficient means to register and interact with energy utilities and other similar entities.

There are thousands of utilities serving customers across the world, and each have their own way of providing access to privileged functionality and data.
This specification defines a way for these utilities and other entities ("Servers") to provide a standardized, structured process for external users and companies ("Clients") to register and obtain secure access to privileged functionality and data.
By offering a standardized Client registration and management protocol, utilities and other entities can enable secure interoperability with other external systems providing climate-change fighting services such as carbon tracking, decarbonization measures, grid flexibility capabilities, and energy benchmarking.

This specification does NOT describe what specific privileged functionality or data is offered to Clients as part of their registration.
Instead, relevant stakeholders are encouraged to write [Extensions](#extensions) to this specification that enable various use cases that need access to privileged functionality or data.
For example, this specification may be extended to define a method for Clients to be able to access customer-authorized energy usage data.

## 2. Terminology <a id="terminology" href="#terminology" class="permalink">🔗</a>

<a id="server" href="#server" class="permalink">🔗</a> **"Server"** - The entity hosting the specified endpoints.
A Server can be an energy utility or another type of entity that want to provide access to privileged functionality or data.
These entities can include, but are not limited to, distribution utilities, grid operators, electric retailers, community choice aggregators, government agencies, data warehouses, and private infrastructure providers.

<a id="client" href="#client" class="permalink">🔗</a> **"Client"** - The entity requesting Server's metadata endpoints.
A Client can be any organization or user seeking to register with a Server for a specific scope of access.
These entities can include, but are not limited to, carbon tracking applications, electric vehicle companies, clean energy technology providers, commercial utility customers, grid management applications, and energy efficiency organizations.

<a id="array" href="#array" class="permalink">🔗</a> "Array" - A list of objects or values as defined by `Arrays` in [[RFC 8259 Section 5](#ref-rfc8259-arrays)].

<a id="base64" href="#base64" class="permalink">🔗</a> "Base64" - A method of encoding binary data into a string defined in [[RFC 4648](#ref-rfc4648)].

<a id="boolean" href="#boolean" class="permalink">🔗</a> "boolean" - Either `true` or `false` defined in [[RFC 8259 Section 3](#ref-rfc8259-values)].

<a id="date" href="#date" class="permalink">🔗</a> "date" - A string representing date in the format of `full-date` as defined by [[RFC 3339 Section 5.6](#ref-rfc3339-datetime)] (e.g. "2024-01-01").

<a id="datetime" href="#datetime" class="permalink">🔗</a> "datetime" - A string representing date and time in the format of `date-time` as defined by [[RFC 3339 Section 5.6](#ref-rfc3339-datetime)] (e.g. "2024-01-01T00:00:00Z").

<a id="decimal" href="#decimal" class="permalink">🔗</a> "decimal" - A decimal value as defined by `number` in [[RFC 8259 Section 6](#ref-rfc8259-numbers)].

<a id="get" href="#get" class="permalink">🔗</a> "GET" - A request method defined in [[RFC 9110 Section 9](#ref-rfc9110-methods)].

<a id="https" href="#https" class="permalink">🔗</a> "HTTPS" - A secure web request protocol defined in [[RFC 9110 Section 4.2.2](#ref-rfc9110-https)].

<a id="integer" href="#integer" class="permalink">🔗</a> "integer" - A positive integer value as defined by `int` in [[RFC 8259 Section 6](#ref-rfc8259-numbers)].

<a id="json" href="#json" class="permalink">🔗</a> "JSON" - A data format defined in [[RFC 8259](#ref-rfc8259)].

<a id="jwk-enc" href="#jwk-enc" class="permalink">🔗</a> "JWK Public Encryption Key" - A JSON object representing a public key as defined in [[RFC 7517 Section 4](#ref-rfc7517-pk)] where the `use` field MUST contain the value `enc`.

<a id="map" href="#map" class="permalink">🔗</a> "Map" - A JSON object as defined by `Objects` in [[RFC 8259 Section 4](#ref-rfc8259-objects)].

<a id="mime-type" href="#mime-type" class="permalink">🔗</a> "MIME type" - A string representing a document media type as defined in [[RFC 6838](#ref-rfc6838)] (e.g. "image/png").

<a id="null" href="#null" class="permalink">🔗</a> "null" - The `null` value defined in [[RFC 8259 Section 3](#ref-rfc8259-values)].

<a id="post" href="#post" class="permalink">🔗</a> "POST" - A request method defined in [[RFC 9110 Section 9](#ref-rfc9110-methods)].

<a id="put" href="#put" class="permalink">🔗</a> "PUT" - A request method defined in [[RFC 9110 Section 9](#ref-rfc9110-methods)].

<a id="patch" href="#patch" class="permalink">🔗</a> "PATCH" - A request method defined in [[RFC 5789](#ref-rfc5789)].

<a id="status-code" href="#status-code" class="permalink">🔗</a> "Status Code" - A response status code defined in [[RFC 9110 Section 15](#ref-rfc9110-codes)].

<a id="string" href="#string" class="permalink">🔗</a> "string" - A series of unicode characters as defined in [[RFC 8259 Section 7](#ref-rfc8259-strings)].

<a id="url" href="#url" class="permalink">🔗</a> "URL" - A string representing resource as defined in [[RFC 3986 Section 1.1.3](#ref-rfc3986-url)] (e.g. "https://example.com/page1").

<a id="key-words" href="#key-words" class="permalink">🔗</a> Key Words: "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" are defined in accordance with [[BCP 14](#ref-bcp14)].

## 3. Authorization Server Metadata <a id="auth-server-metadata" href="#auth-server-metadata" class="permalink">🔗</a>

To enable automated Client registration, Servers MUST host an endpoint with structured metadata about how to submit a [registration request](#registration-request), as well as functionality and data capabilities offered by the Server.
The requirements for hosting this metadata endpoint follows OAuth's Authorization Server Metadata [[RFC 8414](#ref-rfc8414-server-metadata)] with several additions for Connected Data Specifications (CDS) functionality described in the following sections.

### 3.1. Metadata URL <a id="auth-server-metadata-url" href="#auth-server-metadata-url" class="permalink">🔗</a>

Following OAuth's Obtaining Authorization Server Metadata [[RFC 8414 Section 3](#ref-rfc8414-server-metadata-req)] process, a Server's Authorization Server Metadata Object is retrieved by making an HTTPS [GET](#get) request to a specified URL.
To ensure discoverability of the URL, Servers MUST implement the CDS Server Metadata specification [[CDS-WG1-01](#ref-cds-wg1-01)] with the following extensions.

Server Capabilities [[CDS-WG1-01 Section 3.4](#ref-cds-wg1-01-server-capabilities)] are extended to require the following values:

* `oauth` - _[string](#string)_ - (REQUIRED) Including Client Registration via OAuth in the list of a Server's available capabilities

Server Metadata Objects [[CDS-WG1-01 Section 3.2](#ref-cds-wg1-01-metadata-object)] are extended to require the following values:

* `oauth_metadata` - _[URL](#url)_ - (REQUIRED) The URL where Clients can request the Server's Authorization Server Metadata Object

In addition to requiring that the URL be included in CDS's Server Metadata Object [[CDS-WG1-01 Section 3.2](#ref-cds-wg1-01-metadata-object)], Servers MAY also configure the URL such that it conforms to the default Well-Known URI format [[RFC 8414 Section 7.3](#ref-rfc8414-server-metadata-wellknown)], which can increase interoperability beyond the above specified CDS-WG1-01 extension.

### 3.2. Metadata Object Format <a id="auth-server-metadata-format" href="#auth-server-metadata-format" class="permalink">🔗</a>

A Server's Authorization Server Metadata Object follows OAuth's Authorization Server Metadata object format [[RFC 8414 Section 2](#ref-rfc8414-server-metadata-obj)] with the following modifications from OPTIONAL or RECOMMENDED to REQUIRED:

* `registration_endpoint` - _[URL](#url)_ - (REQUIRED) OAuth's Dynamic Client Registration [[RFC 7591](#ref-rfc7591)] functionality is required to enable the [Client Registration Process](#client-registration-process).
  For Metadata Objects accessed by means of the `cds_server_metadata` field in a [Client object](#client-format) and subsequently the `oauth_metadata` field, the `registration_endpoint` field for the returned Metadata Object is OPTIONAL.
* `scopes_supported` - _Array[[string](#string)]_ - (REQUIRED) The scopes in this array MUST represent a union of all scope `id` values contained in the `cds_scope_descriptions` objects.
* `service_documentation` - _[URL](#url)_ - (REQUIRED) Developer documentation is required by the Server to help streamline Client integration.
* `op_policy_uri` - _[URL](#url)_ - (REQUIRED) Policies for Clients registering is required.
* `op_tos_uri` - _[URL](#url)_ - (REQUIRED) Terms of use for Clients registering is required.
* `revocation_endpoint` - _[URL](#url)_ - (REQUIRED) OAuth's Token Revocation [[RFC 7009](#ref-rfc7009)] functionality is required.
* `introspection_endpoint` - _[URL](#url)_ - (REQUIRED) OAuth's Token Introspection [[RFC 7662](#ref-rfc7662)] functionality is required.
* `code_challenge_methods_supported` - _Array[[string](#string)]_ - (REQUIRED) The code challenge methods in this array MUST represent a union of all `code_challenge_methods_supported` values contained in the `cds_scope_descriptions` objects.

In addition to the standard set of OAuth Authorization Server Metadata object values [[RFC 8414 Section 2](#ref-rfc8414-server-metadata-obj)], this specification also requires the following OAuth extension capabilities to be included in the metadata object values:

* `authorization_details_types_supported` - _Array[[string](#string)]_ - (REQUIRED) OAuth's Rich Authorization Requests [[RFC 9396](#ref-rfc9396)] functionality is required.
  This array of values MUST be the same as `scopes_supported`.
* `pushed_authorization_request_endpoint` - _[URL](#url)_ - (OPTIONAL) OAuth's Pushed Authorization Requests [[RFC 9126](#ref-rfc9126)] functionality is REQUIRED when `response_types_supported` is not an empty list  (i.e. user authorization is supported for some scopes).

In addition to the above additionally required set of OAuth Authorization Server Metadata object values [[RFC 8414 Section 2](#ref-rfc8414-server-metadata-obj)], this specification clarifies use of the following OAuth standard values:

* `response_types_supported` - _Array[[string](#string)]_ - (REQUIRED) The response types in this array MUST represent a union of all `response_types_supported` values contained in the `cds_scope_descriptions` objects.
* `grant_types_supported` - _Array[[string](#string)]_ - (REQUIRED) The response types in this array MUST represent a union of all `grant_types_supported` values contained in the `cds_scope_descriptions` objects.
* `token_endpoint_auth_methods_supported` - _Array[[string](#string)]_ - (REQUIRED) The response types in this array MUST represent a union of all `token_endpoint_auth_methods_supported` values contained in the `cds_scope_descriptions` object.

In addition to OAuth capabilities included in the metadata object, this specification adds the following Connected Data Specifications (CDS) values:

* `cds_oauth_version` - _[string](#string)_ - (REQUIRED) The version of the CDS-WG1-02 Client Registration specification that the Server has implemented, which for this version of the specification is `v1`
* `cds_human_registration` - _[URL](#url)_ - (REQUIRED) Where Clients who do not have the technical capacity to use the `registration_endpoint` can visit to manually register a Client using a user device
* `cds_clients_api` - _[URL](#url)_ - (REQUIRED) The URL for the [Clients API](#clients-api).
* `cds_messages_api` - _[URL](#url)_ - (REQUIRED) The URL for the [Messages API](#messages-api).
* `cds_credentials_api` - _[URL](#url)_ - (REQUIRED) The URL for the [Credentials API](#credentials-api).
* `cds_grants_api` - _[URL](#url)_ - (REQUIRED) The URL for the [Grants API](#grants-api).
* `cds_server_provided_files_api` - _[URL](#url)_ - (OPTIONAL) The URL for the [Server-Provided Files API](#server-provided-files-api).
  This MUST be included if `scopes_supported` includes the `server_provided_files` scope.
* `cds_test_accounts` - _[URL](#url)_ - (OPTIONAL) Where Clients can find developer documentation on what test account credentials may be used for testing functionality.
  This MUST be included if `response_types_supported` is not an empty list (i.e. user authorization is supported for some scopes).
* `cds_scope_descriptions` - _Map[[ScopeDescription](#scope-descriptions-format)]_ - (REQUIRED) An object providing additional information about what Scope values listed in `scopes_supported` mean.
  This object MUST include a key for each Scope listed in `scopes_supported` with a [Scope Description](#scope-descriptions-format) as that key's value.
* `cds_registration_fields` - _Map[[RegistrationField](#registration-field-format)]_ - (REQUIRED) An object providing additional information about Registration Field that are referenced in [Scope Description's](#scope-descriptions-format) `registration_requirements` list.
  This object MUST include a key for each Registration Field `id` included in the `registration_requirements` lists in the metadata object with a [Registration Field](#registration-field-format) as that key's value.
  If all `registration_requirements` lists are empty, this reference object is an empty object (`{}`).

Scope functionality described by [Scope Description](#scope-descriptions-format) objects included in the `cds_scope_descriptions` object MUST be available for all of the scope's coverage, as described by the Scope Description's `coverages_supported` list, up to any limits specified in the scopes authorization details, as described by the Scope Description's `authorization_details_fields_supported` [Authorization Details](#auth-details-fields-format) object's `limit` values.
For situations where the same scope is available for multiple groups of coverages and limits, multiple Scope Description entries with different `id` values MUST be included.
For example, if a Server offers a scope that grants access to historical customer usage data, offering 24 months of usage history for electricity service contracts and and 12 months of history for natural gas service contracts, the Server MUST include two Scope Descriptions with different `id` values that include only the coverage and limits of that scope's functionality.

Other values not mentioned here but listed in specifications for the Authorization Server Metadata specification or other extensions MAY be included as described in their respective specifications.

### 3.3. Scopes Supported <a id="scopes" href="#scopes" class="permalink">🔗</a>

This section defines scopes that MAY be included in a Server's `scopes_supported` list.

Servers MUST include at least the `client_admin` and `grant_admin` scopes in the Server's `scopes_supported` list.

Extensions MAY define and Servers MAY include additional scopes to the above list of defined scopes, so long as they include [Scope Descriptions](#scope-descriptions-format) for the scopes in the `cds_scope_descriptions`.

Extensions are RECOMMENDED to define additional scopes with their specification name or abbreviation as a prefix to prevent namespace collisions with other scopes defined by other extensions and Servers, such as `examplespec_*`.
Server are RECOMMENDED to include their name or other relevant identifier as a prefix when adding additional scopes custom to their server, such as `examplepowercompany_*`.

Clients MUST ignore any scopes in a Server's `scopes_supported` list for which the Client is not compatible.

#### 3.3.1 Client Admin Scope <a id="scopes-client-admin" href="#scopes-client-admin" class="permalink">🔗</a>

This is the main administrative scope that grants access for the Client to manage their registration using the defined APIs in this specification.

The following are how the [Scope Descriptions](#scope-descriptions-format) fields MUST be configured for this scope:

* `id` is `"client_admin"`.
* `name` is `"Client Admin"`.
* `description` is `"This scope grants administrative access to the Client management APIs."`.
* `documentation` is a _[URL](#url)_ for the Server's documentation on the `client_admin` scope.
  Servers MAY set the URL to an online publicly-available copy of this specification as adequate documentation for this scope.
* `registration_requirements` is `[]`.
* `registration_optional` is `[]`.
* `response_types_supported` is `[]`.
* `grant_types_supported` is `["client_credentials"]`.
* `token_endpoint_auth_methods_supported` is `["client_secret_basic"]`.
* `code_challenge_methods_supported` is `[]`.
* `coverages_supported` is `[]`.
* `authorization_details_fields_supported` is `[]`.

#### 3.3.2 Grant Admin Scope <a id="scopes-grant-admin" href="#scopes-grant-admin" class="permalink">🔗</a>

A scope for administratively gaining access to a specific Grant data and functionality.
Since Servers MAY arbitrarily create Grant objects included in the [Grants API](#grants-api), this scope serves as an additional method of gaining access to previously created Grant scopes by Client administrators.

The following are how the [Scope Descriptions](#scope-descriptions-format) fields MUST be configured for this scope:

* `id` is `"grant_admin"`.
* `name` is `"Grant Admin"`.
* `description` is `"This scope grants administrative access to previously created Grants."`.
* `documentation` is a _[URL](#url)_ for the Server's documentation on the `grant_admin` scope.
  Servers MAY set the URL to an online publicly-available copy of this specification as adequate documentation for this scope.
* `registration_requirements` is `[]`.
* `registration_optional` is `[]`.
* `response_types_supported` is `[]`.
* `grant_types_supported` is `["client_credentials"]`.
* `token_endpoint_auth_methods_supported` is `["client_secret_basic"]`.
* `code_challenge_methods_supported` is `[]`.
* `coverages_supported` is `[]`.
* `authorization_details_fields_supported` is a JSON array with the following two entries of [Authorization Details Field Objects](#auth-details-fields-format).
    * Client ID:
        * `id` is `client_id`.
        * `name` is `"Client object identifier"`.
        * `description` is `"The Client object identifier for which the Grant is issued."`.
        * `documentation` is a _[URL](#url)_ for the Server's documentation on the `client_id` authorization details field.
          Servers MAY set the URL to an online publicly-available copy of this specification as adequate documentation for this authorization details field.
        * `format` is `string`.
        * `is_required` is `true`.
        * `default` is not included.
        * `relative_date_limit` is not included.
        * `absolute_date_limit` is not included.
        * `limit` is not included.
        * `choices` is not included.
    * Grant ID:
        * `id` is `grant_id`.
        * `name` is `"Grant identifier"`.
        * `description` is `"The Grant identifier for which the returned access_token will be given access."`.
        * `documentation` is a _[URL](#url)_ for the Server's documentation on the `grant_id` authorization details field.
          Servers MAY set the URL to an online publicly-available copy of this specification as adequate documentation for this authorization details field.
        * `format` is `string`.
        * `is_required` is `true`.
        * `default` is not included.
        * `relative_date_limit` is not included.
        * `absolute_date_limit` is not included.
        * `limit` is not included.
        * `choices` is not included.

Server MUST only approve submissions of this scope to the token endpoint with a single authorization details entry to the token endpoint, ensuring the access token returned is only valid for one `grant_id`.
Servers MUST only approve submissions of this scope to the token endpoint when the `grant_id` is for a Grant that has a matching `client_id`, has a non-empty `enabled_scope` value (i.e. the Grant is not disabled) and neither `client_admin` or `grant_admin` are part of the `scope`.

#### 3.3.3 Server-Provided Files Scope <a id="scopes-server-provided-files" href="#scopes-server-provided-files" class="permalink">🔗</a>

A scope for Servers to grant Client access to arbitrary files that the Server wishes to securely share with the Client.
Grants with this scope MUST be created by the Server and appear on the [Grants API](#grants-api) and include `authorization_details` entries that list the `file_id` values that are being shared for that Grant.

The following are how the [Scope Descriptions](#scope-descriptions-format) fields MUST be configured for this scope:

* `id` is `"server_provided_files"`.
* `name` is `"Server-Provided Files"`.
* `description` is `"This scope grants access to specific files that the Server wants make available to the Client."`.
* `documentation` is a _[URL](#url)_ for the Server's documentation on the `server_provided_files` scope.
  Servers MAY set the URL to an online publicly-available copy of this specification as adequate documentation for this scope.
* `registration_requirements` is `[]`.
* `registration_optional` is `[]`.
* `response_types_supported` is `[]`.
* `grant_types_supported` is `[]`.
* `token_endpoint_auth_methods_supported` is `[]`.
* `code_challenge_methods_supported` is `[]`.
* `coverages_supported` is `[]`.
* `authorization_details_fields_supported` is a JSON array with a single entry [Authorization Details Field Object](#auth-details-fields-format) with the following fields.
    * `id` is `file_id`.
    * `name` is `"File identifier"`.
    * `description` is `"A file provided by the Server that may be accessed by the Client as part of the Grant."`.
    * `documentation` is a _[URL](#url)_ for the Server's documentation on the `file_id` authorization details field.
      Servers MAY set the URL to an online publicly-available copy of this specification as adequate documentation for this authorization details field.
    * `format` is `string`.
    * `is_required` is `true`.
    * `default` is not included.
    * `relative_date_limit` is not included.
    * `absolute_date_limit` is not included.
    * `limit` is not included.
    * `choices` is not included.

Since Grants with the scope are only created by Servers, Client do not receive an authorization code or access token for that Grant with which they may access the files. Instead, to access the files, Clients MUST submit a `client_credentials` grant with the scope of `grant_admin` and an `authorization_details` containing the `client_id` and `grant_id` for the Grant the Client found via the [Grants API](#grants-api).

### 3.4. Scope Descriptions Object Format <a id="scope-descriptions-format" href="#scope-descriptions-format" class="permalink">🔗</a>

Scope Description objects are formatted as JSON objects and contain named values.
The following values are included in the default list available in scope description objects.

* `id` - _[string](#string)_ - (REQUIRED) The unique identifier of the scope.
  This MUST be the same value as the object key the Metadata Object's `cds_scope_descriptions` object.
* `type` - _[ScopeType](#scope-types)_ - (REQUIRED) The functionality type of the scope.
  This allows Servers to define multiple scope `id` values for the same Scope Type, for situations where Coverages or Authorization Details fields may be different (e.g. the historical customer data available for one territory has smart meter interval data, and for another it only has monthly meter reading data).
* `name` - _[string](#string)_ - (REQUIRED) A human-readable name of the scope.
* `description` - _[string](#string)_ - (REQUIRED) A human-readable description of what access or actions the scope will enable.
* `documentation` - _[URL](#url)_ - (REQUIRED) Where developers can find documentation for the scope.
* `registration_requirements` - _Array[[string](#string)]_ - (REQUIRED) A list of any registration fields that MUST be submitted with [Client Registration Requests](#registration-request) or requirements that must be completed after registration before the scope is available to use in the Server's production environment.
  All values in this array MUST be included in the Metadata Object's `cds_registration_fields` object for a complete description of the requirement.
  If no additional requirements are needed, this value is an empty array (`[]`).
* `registration_optional` - _Array[[string](#string)]_ - (REQUIRED) A list of any registration fields that Clients MAY submit with the [Client Registration Requests](#registration-request).
  If no additional fields are available, this value is an empty array (`[]`).
* `response_types_supported` - _Array[[string](#string)]_ - (REQUIRED) The OAuth `response_type` values that can be used with this scope.
  This value follows the same behavior as OAuth's Authorization Server Metadata object's `response_types_supported` field [[RFC 8414 Section 2](#ref-rfc8414-server-metadata-obj)], except that this value represents the response types supported for this individual scope and not all scopes for the Client.
  If a scope is not available for OAuth authorization requests (e.g. a `client_credentials`-only scope), this value is an empty array (`[]`).
* `grant_types_supported` - _Array[[string](#string)]_ - (REQUIRED) The OAuth grant type values that can be used with this scope.
  This value follows the same behavior as OAuth's Authorization Server Metadata object's `grant_types_supported` field [[RFC 8414 Section 2](#ref-rfc8414-server-metadata-obj)], except that this value represents the grant types supported for this individual scope and not all scopes for the Client.
  This array MUST contain at least one grant type.
* `token_endpoint_auth_methods_supported` - _Array[[string](#string)]_ - (REQUIRED) The OAuth client authentication methods that can be used for granting tokens with this scope.
  This value follows the same behavior as OAuth's Authorization Server Metadata object's `token_endpoint_auth_methods_supported` field [[RFC 8414 Section 2](#ref-rfc8414-server-metadata-obj)], except that this value represents the client authentication methods supported for this individual scope and not all scopes for the Client.
  A value of `"none"` this array indicates that the scope may be used for public application without using a client secret.
* `code_challenge_methods_supported` - _Array[[string](#string)]_ - (REQUIRED) For scope descriptions where `grant_types_supported` includes the value `authorization_code`, OAuth's Proof Key for Code Exchange by OAuth Public Clients [[RFC 7636](#ref-rfc7636)] functionality is required, and Servers MUST offer `S256` and MUST NOT offer `plain` code verifier methods.
  For scope descriptions where  `grant_types_supported` does not include `authorization_code`, no authorization request is performed, so no code challenge is required and this field value is an empty list (`[]`).
* `coverages_supported` - _Array[[string](#string)]_ - (REQUIRED) A list of CDS's Coverage Entry Object [[CDS-WG1-01 Section 4.3](#ref-cds-wg1-01-coverage-entry)] `id` values for which the scope is available by the Server.
  If the scope provides functionality unrelated to a Server's coverage entries (e.g. the `client_admin` scope), this value is an empty list (`[]`).
* `authorization_details_fields_supported` - _Array[[AuthorizationDetailsField](#auth-details-fields-format)]_ - (REQUIRED) A list of fields that MAY be included in OAuth's Rich Authorization Requests [[RFC 9396](#ref-rfc9396)] the authorization details object for this scope `type`.
  If no extra authorization details fields are available, this value is an empty list (`[]`).

### 3.5. Registration Field Object Format <a id="registration-field-format" href="#registration-field-format" class="permalink">🔗</a>

Registration Field objects are formatted as JSON objects and contain named values.
The following values are included in the default list available in registration fields objects.

* `id` - _[string](#string)_ - (REQUIRED) The unique identifier of the registration field.
  This MUST be the same value as the object key the Metadata Object's `cds_registration_fields` object.
* `type` - _[RegistrationFieldType](#registration-field-types)_ - (REQUIRED) What type of Registration Field this entry is.
* `description` - _[string](#string)_ - (REQUIRED) A human-readable description of what should be submitted or expected for this registration field.
* `documentation` - _[URL](#url)_ - (REQUIRED) Where developers can find more information about this registration field.
* `field_name` - _[string](#string)_ - (OPTIONAL) If type is `registration_field`, this is the name of the field to submit in the [Client Registration Request](#registration-request) and MUST start with `cds_`.
* `format` - _[RegistrationFieldFormats](#registration-field-formats)_ - (OPTIONAL) If type is `registration_field`, this is the data format that MUST be used in the value of the field.
* `default` - _various_ - (OPTIONAL) If type is `registration_field` and the field is optional, this is the default value that will be used in lieu of the Client submitting a value themselves.
  Including this `default` value in the object indicates the registration field is optional.
* `max_length` - _[integer](#integer)_ - (OPTIONAL) If format is one of `string`, `string_or_null`, `url`, `url_or_null`, `email`, or `email_or_null`, this is the maximum length of the submitted value, if not `null`.
* `max_size` - _[integer](#integer)_ - (OPTIONAL) If format is one of `image`, `image_or_null`, `pdf`, or `pdf_or_null`, this is the maximum file size of the submitted value before Base64 encoding, if not `null`.
* `amount` - _[decimal](#decimal)_ - (OPTIONAL) If type is `payment_required`, this is the amount in `currency` that will be required to complete registration.
* `currency` - _[string](#string)_ - (OPTIONAL) If type is `payment_required`, this is the monetary currency in [[ISO 4217](#ref-iso4217)] currency code.

### 3.6. Registration Field Types <a id="registration-field-types" href="#registration-field-types" class="permalink">🔗</a>

Registration Field types define which type of action the Client or Server will take for that registration field.
Registration Fields can be either something that MUST or MAY be submitted as part of the [Client Registration Request](#registration-request), depending on the submitted scope's `registration_requirements` and `registration_optional` values, or can be something that a Client must do after registration in order to be full approved for use in the production environment.

The following list of strings are an enumerated set of registration field types that are valid `type` values in the registration field object.

* `registration_field` - This field should be submitted with the [Client Registration Request](#registration-request) as the `field_name`.
  This type of registration field MUST also have `field_name` and `format` values.
  If this field is optional as part of registration, `default` must also be defined in the registration field object.
* `internal_review` - This field indicates that the Server will review the registration internally before approving it for production use.
  Notifications and communication about the status of this internal review will be conveyed using the [Messages API](#messages-api).
* `payment_required` - This field indicates that a setup payment will be required before the Server will approve the Client for production use.
  Notifications and communication about how to pay and confirmation of payment will be conveyed using the [Messages API](#messages-api).
* `email_verification` - This field indicates that the Client must verify their email before the Server will approve the Client for production use.
  Notifications and communication about how to verify the Client's contact email will be conveyed using the [Messages API](#messages-api).

### 3.7. Registration Field Formats <a id="registration-field-formats" href="#registration-field-formats" class="permalink">🔗</a>

Registration Field formats define the data type of submitted values for these fields in [Client Registration Requests](#registration-request).

The following list of strings are an enumerated set of registration field formats that are valid `format` values in the registration field object.

* `string` - If required, a [string](#string) value MUST be submitted.
* `string_or_null` - Same as `string`, only with `null` being an additional possible value.
* `url` - If required, a [URL](#url) value MUST be submitted.
* `url_or_null` - Same as `url`, only with `null` being an additional possible value.
* `email` - If required, a valid email address string MUST be submitted.
* `email_or_null` - Same as `email`, only with `null` being an additional possible value.
* `boolean` - If required, a [boolean](#boolean) value MUST be submitted.
* `boolean_or_null` - Same as `boolean`, only with `null` being an additional possible value.
* `image` - If required, a valid image file formatted as `image/jpeg` or `image/png` encoded as a Base64 string using MUST be submitted.
* `image_or_null` - Same as `image`, only with `null` being an additional possible value.
* `pdf` - If required, a valid pdf file formatted encoded as a Base64 string MUST be submitted.
* `pdf_or_null` - Same as `pdf`, only with `null` being an additional possible value.

### 3.8. Authorization Details Field Object Format <a id="auth-details-fields-format" href="#auth-details-fields-format" class="permalink">🔗</a>

Authorization Details Field objects are formatted as JSON objects and contain named values.
The following values are included in the default list available in authorization details field objects.

* `id` - _[string](#string)_ - (REQUIRED) The unique identifier of the authorization details field.
  This value is used as the key for the field when added to `authorization_details` data fields as part of OAuth's Rich Authorization Requests [[RFC 9396](#ref-rfc9396)].
* `name` - _[string](#string)_ - (REQUIRED) A human-readable name of the authorization details field.
* `description` - _[string](#string)_ - (REQUIRED) A human-readable description of what submitting values for this authorization details field means.
* `documentation` - _[URL](#url)_ - (REQUIRED) Where developers can find more information about this authorization details field.
* `format` - _[AuthorizationDetailsFieldFormats](#auth-details-field-formats)_ - (REQUIRED) The data format that MUST be used in the value of the field when including it as a data field in `authorization_details` objects.
* `is_required` - _[boolean](#boolean)_ - (REQUIRED) Whether the authorization details field is required or not.
  If required, `authorization_details` MUST be submitted in authorization or token requests with this field included.
* `default` - _various_ - (OPTIONAL) If `is_required` is `false`, this is the default value that will be used in lieu of the Client submitting a value themselves.
  This is also the value that will be used if a basic OAuth `scope` string parameter is used instead of an `authorization_details` parameter.
  If `is_required` is `true`, this is not included.
* `relative_date_limit` - _[integer](#integer)_ - (OPTIONAL) If `format` is `relative_or_absolute_date` and the value is limited by the Server, this is REQUIRED and MUST be the largest relative date range that may be submitted.
  Servers MUST validate both submitted absolute dates and relative dates against the relative date limit, where when comparing to a submitted absolute date, the current Server date in the Server's local timezone is used as the relative point of reference.
  If `format` is not `relative_or_absolute_date`, this is not included.
* `absolute_date_limit` - _[integer](#integer)_ - (OPTIONAL) If `format` is `relative_or_absolute_date` the value is limited by the Server, this is REQUIRED and MUST be the furthest away date that may be submitted.
  Servers MUST validate both submitted absolute dates and relative dates against the absolute date limit, where when comparing to a submitted relative date, the current Server date in the Server's local timezone is used as the relative point of reference.
* `limit` - _[integer](#integer)_ - (OPTIONAL) If `format` is one of `int` or `decimal` and the value is limited by the Server, this is REQUIRED and MUST be the largest value that can be the submitted.
  If `format` is `string` and the length of the value is limited by the Server, this is REQUIRED and MUST be the maximum string length that can be the submitted.
* `choices` - _Array[[AuthorizationDetailsFieldChoice](#auth-details-field-choice-format)]_ - (OPTIONAL) If `format` is `choice`, this is REQUIRED and MUST be a list of one or more available choice objects.

### 3.9. Authorization Details Field Formats <a id="auth-details-field-formats" href="#auth-details-field-formats" class="permalink">🔗</a>

Authorization Details Field formats define the data type of submitted values for these fields in `authorization_details` for OAuth's Rich Authorization Requests [[RFC 9396](#ref-rfc9396)].

The following list of strings are an enumerated set of authorization details field formats that are valid `format` values in the [Authorization Details Field objects](#auth-details-fields-format).

* `int` - An [integer](#integer) value.
* `decimal` - A [decimal](#decimal) value, which can have any number of significant units, but MUST NOT be stored or handled as a float value, in order to retain the precision of the value throughout Server and Client processing.
* `string` - A [string](#string) value.
* `string_or_null` - A [string](#string) value or `null`.
* `boolean` - A [boolean](#boolean) value.
* `relative_or_absolute_date` - A relative or absolute date string.
  Relative dates are formatted as defined by `duration` in [[RFC 3339 Appendix A](#ref-rfc3339-duration)].
  For example, `P3Y` represents a relative date range of 3 years.
  Absolute dates are formatted as [date](#date) (`YYYY-MM-DD`).
  For example, `2024-01-02` represents the 2nd of January, 2024.
  Dates are defined as the date from the perspective of the Server's local timezone.
* `choice` - A [string](#string) value from the `id` parameter in one of the listed available `choices` [Authorization Details Field Choice](#auth-details-field-choice-format) objects.
* `jwk_or_null` - A [JWK Public Encryption Key](#jwk-enc) object or `null`.

### 3.10. Authorization Details Field Choice Object Format <a id="auth-details-field-choice-format" href="#auth-details-field-choice-format" class="permalink">🔗</a>

Authorization Details Field Choice objects are formatted as JSON objects and contain named values.
The following values are included in the default list available in Authorization Details Field Choice objects.

* `id` - _[string](#string)_ - (REQUIRED) The unique identifier of the authorization details field choice.
  This is used as the value for the relvant field when that field is included in an object as part of a `authorization_details` list.
* `name` - _[string](#string)_ - (REQUIRED) A human-readable name of the authorization details field choice.
* `description` - _[string](#string)_ - (REQUIRED) A human-readable description of what submitting this value for the authorizations details field means.
* `documentation` - _[URL](#url)_ - (REQUIRED) Where developers can find more information about this authorization details field choice.

## 4. Client Registration Process <a id="client-registration-process" href="#client-registration-process" class="permalink">🔗</a>

OAuth's Dynamic Client Registration [[RFC 7591](#ref-rfc7591)] is used as the basis for initially registering Clients with Servers.
To perform the dynamic registration process, the Client submits a request to the Server's `registration_endpoint` provided in the Server's [Metadata object](#auth-server-metadata-format) as described in [Client Registration Request](#registration-request).
The Server then responds with either an error or a generated Client registration response as described in [Client Registration Response](#registration-response).

### 4.1. Client Registration Request <a id="registration-request" href="#registration-request" class="permalink">🔗</a>

This specification requires Clients and Servers follow the process described in OAuth's Client Registration Request [[RFC 7591 Section 3.1](#ref-rfc7591-client-reg-request)], with the following modifications.

* Clients MAY submit additional named values that are defined as part of scope `registration_requirements` and `registration_optional` arrays in `cds_scope_descriptions` objects, using the registration field reference's `field_name` value as the submitted key in the registration request.
* Servers MUST ignore any submitted `redirect_uris` values.
  Clients MAY later update individual Client object `redirect_uri` values via the [Modifying Clients](#clients-modify) process, when that Client object `response_types` list is non-empty.

### 4.2. Client Registration Response <a id="registration-response" href="#registration-response" class="permalink">🔗</a>

This specification requires Servers follow the process described in OAuth's Client Registration Response [[RFC 7591 Section 3.2](#ref-rfc7591-client-reg-resp)], with the following modifications to OAuth's Client Information Response [[RFC 7591 Section 3.2.1](#ref-rfc7591-client-info-resp)] object.

* The reponse object format MUST be the extended [Client object format](#client-format) defined by the Clients API.
* The `scope` value MUST be `client_admin`, indicating that the Client in the response is only used for `client_admin` access.
* The `redirect_uris` value MUST be an empty list (`[]`).
* The `response_types` value MUST be an empty list (`[]`).
* The `grant_types` value MUST contain only the `client_credentials` grant, indicating that the Client details in the response may only be used for `client_admin` client credentials grant.
* The `token_endpoint_auth_method` MUST be set to `client_secret_basic`.
* The `client_secret` MUST be included, despite it not being included in the Client object format returned by the Clients API, and set to the same value as the `client_secret` in the created [Credential object](#credentials-format) for the Client object in the response.

Upon valid registration, servers MUST create a Client object with the scope defined as `"client_admin"`, even if the `client_admin` scope was not included in the Client Registration Request.
This Client object scoped to `client_admin` is the one returned as the main Client object as the Client Registration Response.

Servers MUST also create a Client object with the scope defined as `"grant_admin"`, though it is not returned as part of the Client Registration response and is only available via the [Clients API](#clients-api).

Servers MUST also create Client objects that are configured for any other scopes for which the Client submitted registration and the submission was accepted by the Server.
Servers MAY combine scopes into individual Client objects if the `response_types`, `grant_types`, and `token_endpoint_auth_method` for the scopes are the same.

For each Client object created that has its `token_endpoint_auth_method` value as something other than `null`, at least one [Credential object](#credentials-format) MUST be created and made available on the [Credentials API](#credentials-api) for the Client to use.

For created Client objects that have a non-empty `response_types` list, Servers MUST create a default `redirect_uri` value and and set it as the single value in the created Client object `redirect_uris` list.
The created `redirect_uri` MUST provide functionality such that if specified as part of authorization request parameters, will redirect the user to a receipt of the authorization when the authorization is submitted successfully, or show an error to the user if the authorization is declined or otherwise errors.
This default `redirect_uri` allows Clients to request user authorization without needing to operate their own redirect endpoint.
Clients MAY choose to update the `redirect_uris` to remove this default or add additional redirect URIs using the [Modifying Clients](#clients-modify) functionality.

Servers MUST NOT create Clients that have both `production` and `sandbox` values in the same `cds_status_options`, so that Client objects cannot be used for both testing and production environments.
For situations where Servers do not initially create a Client with `production` as a `cds_status_option`, such as when the Server has a manual review step, Servers MUST create a Client object with `sandbox` as a `cds_status_option` so that Clients MAY being to test their applications immediately after registration.
Then, later if and when the Server decides to allow the Client access to the production environment, the Server MUST create a new Client object with `production` as a `cds_status_option`.

While Servers MAY support OAuth's Dynamic Client Registration Management Protocol [[RFC 7592](#ref-rfc7592)] by including the `registration_client_uri` and `registration_access_token` values in their registration response, this specification requires that Servers MUST support the [Clients API](#clients-api) by including the `cds_clients_api` value in the [Authorization Server Metadata](#auth-server-metadata-format) as a way for Clients to manage their registrations.
The Clients API is needed beyond the OAuth Dynamic Client Registration Management Protocol because as part of registration, Servers create multiple Clients that group the various registered scopes, and OAuth's Dynamic Client Registration Management Protocol does not support APIs for listing multiple clients or credentials that are created from a single registration request.

## 5. Clients API <a id="clients-api" href="#clients-api" class="permalink">🔗</a>

This specification requires Servers provide a set of Application Programming Interfaces (APIs) allowing Clients to view and edit their Client registrations.
These APIs are authenticated using a Bearer `access_token` obtained by the Client using OAuth 2.0's `client_credentials` grant process [[RFC 6749 Section 4.4](#ref-rfc6749-client-credentials)], where the `client_id` used is for a Client that includes the `client_admin` scope.

### 5.1. Client Object Format <a id="client-format" href="#client-format" class="permalink">🔗</a>

Client objects are formatted as JSON objects and contain named values.
Client objects are required to follow the same object format as the OAuth Dynamic Client Registration Management Protocol's Client Metadata [[RFC 7591 Section 2](#ref-rfc7591-client-metadata)], with the following modifications.

The following fields defined in OAuth's Client Metadata [[RFC 7591 Section 2](#ref-rfc7591-client-metadata)] are REQUIRED to be included with all Client objects:

* `client_id`
* `client_id_issued_at`
* `scope` - If no scope is configured, this value is an empty string (`""`).
* `redirect_uris` - If no redirect URIs are configured, this value is an empty list (`[]`).
* `token_endpoint_auth_method` - If no auth methods are configured, this value is `null`.
* `grant_types` - If no grant types are configured, this value is an empty list (`[]`).
* `response_types` - If no response types are configured, this value is an empty list (`[]`).
* `client_name` - If no value is submitted by the Client, the default value is the `client_id`.
* `contacts` - If no contacts are configured, this value is an empty list (`[]`).

The following fields defined in OAuth's Rich Authorization Requests Metadata [[RFC 9396 Section 10](#ref-rfc9396-rar-metadata)] are REQUIRED to be included with all Client objects:

* `authorization_details_types`

The following fields MUST NOT be included, except when the Client object is returned as a [Client Registration Response](#registration-response):

* `client_secret`

The following fields MUST NOT be included, since it is moved to the [Credentials API](#credentials-api):

* `client_secret_expires_at`

Other fields defined in the OAuth's Client Metadata [[RFC 7591 Section 2](#ref-rfc7591-client-metadata)] and its extensions, such as OAuth's Client Information Response [[RFC 7592 Section 3](#ref-rfc7592-client-metadata)], remain unmodified in their requirement and behavior.

In addition to the fields defined by OAuth's Client Metadata [[RFC 7591 Section 2](#ref-rfc7591-client-metadata)] and its extensions, the following fields are additionally defined:

* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Client object was created.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Client object was last modified.
* `cds_client_uri` - _[URL](#url)_ - (REQUIRED) Where to submit modifications using the Clients API [Modifying Clients](#clients-modify) functionality.
  Servers MUST make this URL unique for each Client object.
* `cds_status` - _[ClientStatus](#client-statuses)_ - (REQUIRED) The current status of availability for this Client.
* `cds_status_options` - _Array[[ClientStatus](#client-statuses)]_ - (REQUIRED) What `cds_status` values to which the Client MAY change the Client when [modifying](#clients-modify) it.
  For Client objects that have the `client_admin` scope, this list MUST NOT contain the `disabled` value, and for all other Client objects this list MUST contain at least the `disabled` value, so that Clients MAY opt to disable Client objects as needed.
* `cds_server_metadata` - _[URL](#url)_ - (REQUIRED) Where the Client can find their registration-specific version of the Servers's CDS Server Metadata [[CDS-WG1-01](#ref-cds-wg1-01)].
  If the Client's registered scopes have different [Scope Descriptions](#scope-descriptions-format) than what is in the publicly available `oauth_metadata` resource, such as more limited `coverages_supported` or different limits in `authorization_details_fields_supported`, then the `cds_server_metadata` MUST be to a resource that has an updated `oauth_metadata` resource with the Client-specific values.
  If the Client's CDS server metadata is no different from the public CDS server metadata, Servers MAY simply link to the public URL.
  If this metadata endpoint requires authentication, Servers MUST authenticate Client requests to this endpoint via Bearer access token obtained using OAuth's `client_credentials` grant with a scope of `client_admin`, and reject unauthenticated requests with a `401 Unauthorized` Status Code.
  Clients know that they must use a Bearer token when Servers return a `401 Unauthorized` Status Code for this endpoint when the Client makes an unauthenticated request to the endpoint.
* `cds_default_scope` - _[string](#string)_ - (OPTIONAL) The default scope string used when no `scope` parameter is provided as part of an authorization request.
  This MUST be included if `response_types` is not an empty list (i.e. authorization requests are enabled).
  For the initial registration response, Servers MAY choose to configure the default scope string the same as the `scope` field or another subset of default scopes.
* `cds_default_redirect_uri` - _[URL](#url)_ - (OPTIONAL) The default redirect_uri string used when no `redirect_uri` parameter is provided as part of an authorization request.
  This MUST be included if `response_types` is not an empty list (i.e. authorization requests are enabled).
  For the initial registration response, Servers MUST set this value to the value in the `redirect_uris` that was required to be created by the Server that will redirect the user by default to an authorization receipt.
* `cds_default_authorization_details` - _Array[[OAuth AuthorizationDetail](#ref-rfc9396-auth-details)]_ - (OPTIONAL) The default authorization details list used when no `authorization_details` parameter is provided as part of an authorization request.
  This MUST be included if `response_types` is not an empty list (i.e. authorization requests are enabled).
  For the initial registration response, Servers MUST set this to an empty list (`[]`).

### 5.2. Client Statuses <a id="client-statuses" href="#client-statuses" class="permalink">🔗</a>

Client object `cds_status` values MUST be one of the following:

* `production` - The Client object MAY be used on the Server's production systems.
  For scopes that require user authorization (e.g. `response_type` values of `code`), this `cds_status` indicates that the Client may request authorization from real users.
* `sandbox` - The Client object MAY be used to request access on the Server's test environment systems.
  For scopes that require user authorization (i.e. `response_type` values of `code`), this `cds_status` indicates that the Client may request authorization from fictional test users, as provided in the Server's `cds_test_accounts` documentation.
* `disabled` - The Client is is disabled.

### 5.3. Listing Clients <a id="clients-list" href="#clients-list" class="permalink">🔗</a>

Clients may request to list Client objects that they have access to by making an HTTPS [GET](#get) request, authenticated with a valid Bearer `access_token` scoped to the `client_admin` scope, to the `cds_clients_api` URL included in the [Authorization Server Metadata](#auth-server-metadata-format).
The Client listing request responses are formatted as JSON objects and contain the following named values.

* `clients` - _Array[[Client](#client-format)]_ - (REQUIRED) A list of Clients to which the requesting `access_token` is scoped to have access.
  If no Clients are accessible, this value is an empty list (`[]`).
  If more than 100 Clients are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Clients.
* `next` - _[URL](#url) or `null`_ - Where to request the next segment of the list of Clients.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url) or `null`_ - Where to request the previous segment of the list of Clients.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the [GET](#get) request, which will filter the list of Client objects to be the intersection of results for each of the URL parameters filters:

* `client_ids` - A space-separated list of `client_id` values for which the Servers MUST filter the Client objects.

Listings of Client objects MUST be ordered in reverse chronological order by `cds_modified` timestamp, where the most recently updated relevant Client MUST be first in each listing.

For Client objects in the listing that are added, removed, or modified by the Server or Client after initial registration, Servers MUST add a message to the [listed Messages](#messages-list) notifiying the Client that a Client object has been added, removed, or modified.
This creates a changelog in the Messages listing for Client object updates.

### 5.4. Retrieving Individual Clients <a id="clients-get" href="#clients-get" class="permalink">🔗</a>

The URL to be used to send [GET](#get) requests for retrieving individual Client objects MUST be the `cds_client_uri` provided in the [Client object](#client-format).
If a Server has optionally implemented OAuth's Dynamic Client Registration Management Protocol [[RFC 7592](#ref-rfc7592)], the value of `cds_client_uri` MUST be the same as `registration_client_uri`, and access tokens issued from either a `client_credentials` grant with the scope `client_admin` or the access token provided as the `registration_access_token` MUST be valid access tokens to interact with the client retrieval endpoint (`cds_client_uri`).

### 5.5. Modifying Clients <a id="clients-modify" href="#clients-modify" class="permalink">🔗</a>

This specification requires that the procedure to modify Clients MUST follow OAuth's Client Update Request [[RFC 7592 Section 2.2](#ref-rfc7592-client-mgmt-updates) section in Dynamic Client Registration Management Protocol [[RFC 7592](#ref-rfc7592)].

The URL to be used to send [PUT](#put) requests for updating clients MUST be the `cds_client_uri` provided in the [Client object](#client-format).
If a Server has optionally implemented OAuth's Dynamic Client Registration Management Protocol [[RFC 7592](#ref-rfc7592)], the value of `cds_client_uri` MUST be the same as `registration_client_uri`, and access tokens issued from either a `client_credentials` grant with the scope `client_admin` or the access token provided as the `registration_access_token` MUST be valid access tokens to interact with the client endpoint.

Servers MUST allow Clients to intially update the following fields:

* `redirect_uris`
* `client_name`
* `client_uri`
* `logo_uri`
* `scope`
* `contacts`
* `tos_uri`
* `policy_uri`
* `cds_status`
* `cds_default_scope`
* `cds_default_redirect_uri`
* `cds_default_authorization_details`

Servers MUST NOT allow clients to update the following fields:

* `client_id` - This value is immutable from when it was assigned upon Client registration.
* `client_id_issued_at` - This value is immutable from when the Client initially registered.
* `client_secret` - This value is NOT included in any Client object response, except for the initial Client registration request.
  Clients MUST use the [Credentials API](#credentials-api) to manage client secrets.
* `client_secret_expires_at` - This value is NOT included in any Client object response.
  Clients MUST use the [Credentials API](#credentials-api) to discover when a client secret will expire.
* `grant_types` - This list is determined by the Server based on the `grant_types_supported` described in the the [Scope Descriptions](#scope-descriptions-format).
* `response_types` - This list is determined by the Server based on the `response_types_supported` described in the the [Scope Descriptions](#scope-descriptions-format).
* `token_endpoint_auth_method` - This is set by the Server based on the `token_endpoint_auth_methods_supported` described in the the [Scope Descriptions](#scope-descriptions-format).
* `cds_created` - This is a URL set by the Server.
* `cds_modified` - This is a URL set by the Server.
* `cds_client_uri` - This is a URL set by the Server.
* `cds_server_metadata` - This is a URL set by the Server.
* `cds_status_options` - This is a list set by the Server.

For other fields, the Server MUST determine the validity of the submitted Client fields and reject with a `400 Bad Response` Status Code if not all submitted fields are either the same as previously set, or invalid values for that field.

If a Client does not include a field that is included in the Client object, this indicates that the Client wishes to reset the value of that field to the Server default.

If a Server needs to asynchronously review and approve changes to any submitted Client object fields that have been submitted by the Client and are different from the current values, for valid update requests the Server MUST respond with a `202 Accepted` Status Code, which indicates that the submission was accepted but not fully saved as completed yet.
Any fields that have not been synchronously updated as part of the request and response MUST remain in the response as their previous values, and the Server MUST add one or more entries to the [listed Messages](#messages-list) for the modified fields that need to be asynchronously reviewed and approved.
Clients MAY then use the [Messages API](#messages-api) to track the asynchronous review of the modification request.
If all submitted fields have been synchronously updated as part of the response, Servers MUST respond with a `200 OK` Status Code.

## 6. Messages API <a id="messages-api" href="#messages-api" class="permalink">🔗</a>

To facilitate automated communication and notificatiosn between Servers and Clients, this specification requires that official communication between Servers and Clients be performed using the Client Messages APIs.
Servers MAY implement other means of communications for exchanging messages and notifications, such as email support, but they MUST also mirror any official communications that impact Client or Grant statuses, settings, or access using the Messages API.

The Messages API endpoints are authenticated using a Bearer `access_token` obtained by the Client using OAuth 2.0's `client_credentials` grant process [[RFC 6749 Section 4.4](#ref-rfc6749-client-credentials)], where the `client_id` used is for a Client that includes the `client_admin` scope.

### 6.1. Message Object Format <a id="message-format" href="#message-format" class="permalink">🔗</a>

Message objects are formatted as JSON objects and contain the following named values:

* `message_id` - _[string](#string)_ - (REQUIRED) The unique identifier for the Message on the Server's system.
* `uri` - _[URL](#url)_ - (REQUIRED) Where to retrieve or modify this specific Message object.
* `previous_uri` - _[URL](#url) or `null`_ - (REQUIRED) Where to find the previous Message to which this Message has been created as a reply.
* `type` - _[ClientMessageType](#message-types)_ - (REQUIRED) The type of Message.
* `read` - _boolean_ - (REQUIRED) Whether the object has been marked as read by a Client.
  When the Server creates a Message, this value MUST be `false` by default, so that the Message appears in the [unread](#messages-list) list.
* `creator` - _[string](#string) or `null`_ - (REQUIRED) If the Server created the Message, this value is `null`.
  If the Client created the Message, this value is the Client's `client_id`.
* `created` - _[datetime](#datetime)_ - (REQUIRED) When the Message was created.
* `modified` - _[datetime](#datetime)_ - (REQUIRED) When the Message was last modified.
* `status` - _[ClientMessageStatus](#message-statuses)_ - (REQUIRED) The current status of the Message.
* `name` - _[string](#string)_ - (REQUIRED) A human-readable name of the Message.
  If the Message `type` is `notification`, `private_message`, or `support_request`, this is the message subject.
* `description` - _[string](#string)_ - (REQUIRED) A human-readable description of the Message.
  If the Message `type` is `notification`, `private_message`, or `support_request`, this is the message body.
* `updates_requested` - _Array[[ClientUpdateRequest](#client-update-request-format)]_ - (OPTIONAL) The list of values that a Client has requested to be updated.
  This field is required for Messages with a `type` value of `field_changes` or `server_request`.
* `grants_requested` - _Array[[ClientGrantRequest](#client-grant-request-format)]_ - (OPTIONAL) The list of grants with their parameters that a Client has requested to be issued by the Server.
  This field is required for Messages with a `type` value of `grant_request`.
* `attachments` - _Array[[MessageAttachment](#message-attachment-format)]_ - (OPTIONAL) A list of file attachments associated with the Message.
  Message attachments are intended to allow Clients and Servers to attach relatively small unstructured files, such as PDF documents, to their messages that are relevant.
  For example, a Client MAY attach a letter of authorization scanned PDF as part their `grant_request` Message.
  Message attachments are not intended to provide a means for repeatedly transferring structured data or large amounts of data.
  It is RECOMMNEDED that Servers prefer to share files via the [Server-Provided Files API](#server-provided-files-api) or other relevant APIs and only use Message attachments when the attachment is relatively small in size and only relevant to the Message.
* `related_uri` - _[URL](#url) or `null`_ - (OPTIONAL) If the Message `type` is `notification` or `private_message`, this value is where the Client can find more information, if available.
  If the Message `type` is `support_request`, this value is a relevant URL for which the Client is requesting technical support.
  If the Message `type` is `field_changes`, this is where the Client can retrieve the object that has been requested to be modified.
  If the Message `type` is `payment_request`, this is where the Client can submit their payment to the Server or if paid, a link to the payment receipt.
  If the Message `type` is `server_request`, this is where the Client can find more information about what information is being requested by the Server.
  If the Message `type` is `grant_request` and `creator` is not `null`, this value is a relevant Client `cds_client_uri` to which the requesting Client is requesting the Grants be assinged to if created by the Server, which MAY be a Client that is managed by the requesting Client (e.g. a "self" grant) or MAY be a completely separate Client with which the requesting Client is working (e.g. a "third-party" grant).
* `related_type` - _[ClientMessageRelatedType](#message-related-types)_ - (OPTIONAL) The type of object or resource linked to by the `related_uri`.
  If `related_uri` is included, this field is REQUIRED.
* `amount` - _[decimal](#decimal)_ - (OPTIONAL) If the Message `type` is `payment_request`, this amount the Client needs to pay to satisfy the payment request.
* `currency` - _[string](#string)_ - (OPTIONAL) If the Message `type` is `payment_request`, this is the monetary currency for the `amount` in [[ISO 4217](#ref-iso4217)] currency code.

### 6.2. Message Types <a id="message-types" href="#message-types" class="permalink">🔗</a>

Message object `type` values MUST be one of the following:

* `notification` - The Server is sending the Client at notification message that is relevant to the Client.
  This message is considered to be not individually tailored to the Client specifically, but instead directed to all relevant Clients.
* `private_message` - The Server or Client is sending a private message to the opposite party.
  This message is considered to be individually tailored and relevant only to that Client or Server.
* `support_request` - The Client is submitting a technical support request to the Server.
* `grant_request` - The Client is submitting a request for the Server to create Grants with the parameters provided in the `grants_requested` list.
* `field_changes` - This Message represents that the Client has submitted changes to field values in other API objects, listed in the `updates_requested` list, but these changes were not synchronously approved by the Server and need to be reviewed asynchronously by the Server.
  While the Client originally triggered this Message to be created by submitting field changes that need to be asynchronously reviewed, the Server is responsible for creating this Message, so the `creator` value is `null`.
* `server_request` - The Server is requesting the Client submit a new Message of type `client_submission` with the fields listed in `updates_requested`.
* `client_submission` - The Client is submitting a response to a `server_request` to update one or more fields of a specific object.
  Both Clients and Servers MAY create these types of Message requests.
* `payment_request` - The Server is requesting payment by the Client to complete a financial payment before the Client may proceed with either registration approval or completing actions listed in the `updates_requested` list.

### 6.3. Message Statuses <a id="message-statuses" href="#message-statuses" class="permalink">🔗</a>

Message object `status` values MUST be one of the following:

* `complete` - For Messages with `type` values of `notification`, `private_message`, or `client_submission`, the `status` MUST be set as `complete`.
  For Messages with `type` values of `support_request`, `grant_request`, `field_changes`, `server_request`, or `payment_request`, this represents the Server has completed and approved or resolved the Client's technical support request, field changes, submission, or payment.
  For Messages with a `type` value of `grant_request` and `status` value of `complete`, Servers MUST set the `related_type` to be `grant_list` and `related_uri` to link to Grants API [listing](#grants-list) with URL parameters that filter to only the relevant Grants.
* `open` - For Messages with `type` values of `server_request` or `payment_request`, this represents that the Client has not yet submitted a response to the Server's submission or payment request.
* `pending` - For Messages with `type` values of `support_request`, `grant_request`, `field_changes`, `server_request`, or `payment_request`, this represents the Server has not yet completed it's review of the Client's technical support request, field changes, submission, or payment.
* `rejected` - For Messages with `type` values of `field_changes`, `server_request`, `grant_request`, or `payment_request`, this represents the Server has completed and rejected the Client's requested field changes, submission, grant, or payment.
  When Servers create a rejected Message, the Server's created Message MUST contain information in the `description` field on why the Message that was created by the Client, typically linked by the `previous_uri` field, was rejected.
* `errored` - For Messages with `type` values of `field_changes`, `server_request`, or `payment_request`, this represents the Server encountered an issue while processing the Client's field changes, submission, or payment.
  The Client is RECOMMENDED to submit a `support_request` Messages with the `related_uri` as the relevant errored Message's `uri`.

### 6.4. Message Related Types <a id="message-related-types" href="#message-related-types" class="permalink">🔗</a>

When included, Message object `related_type` values MUST be one of the following:

* `more_info` - The `related_uri` is to a resource (e.g. regulatory compliance details website page) with additional information for the Client to reference.
* `documentation` - The `related_uri` is to a technical documentation resource (e.g. API documentation website page) for the Client to reference.
* `support` - The `related_uri` is to a technical support resource (e.g. support contact form) for the Client to reference.
* `client_list` - The `related_uri` is to a [Client list](#clients-list), which can include request parameters.
* `client` - The `related_uri` is to a [individual Client](#clients-get).
* `grant_list` - The `related_uri` is to a [Grant list](#grants-list), which can include request parameters.
* `grant` - The `related_uri` is to a [individual Grant](#grants-get).
* `message_list` - The `related_uri` is to a [Message list](#messages-list), which can include request parameters.
* `message` - The `related_uri` is to a [individual Message](#messages-get).
* `credential_list` - The `related_uri` is to a [Credential list](#credentials-list), which can include request parameters.
* `credential` - The `related_uri` is to a [individual Credential](#credentials-get).

For `more_info`, `documentation`, and `support` related types, authentication for requests to these resources is unspecified, where Servers MAY allow unauthenticated requests to the resource (e.g. public webpage) or MAY require user authentication (e.g. user login).

For `client_list`, `client`, `grant_list`, `grant`, `message_list`, `message`, `credential_list`, and `credential` related types, Clients MUST authenticate requests to the resource with a valid Bearer `access_token` scoped to the `client_admin` scope.

### 6.5. Client Update Request Object Format <a id="client-update-request-format" href="#client-update-request-format" class="permalink">🔗</a>

Client Update Request objects are formatted as JSON objects and contain the following named values:

* `field` - _[string](#string)_ - (REQUIRED) The field name of the field that is being requested to be updated.
  For Messages with `type` values of `field_changes`, this is the name of the object's field on the API.
  For Messages with `type` values of `server_request`, this is the Server's identifier for the submission being requested from the client.
* `name` - _[string](#string)_ - (OPTIONAL) For Messages with `type` values of `server_request`, this MUST be a human-readable name of the submission type being requested as the client.
* `description` - _[string](#string)_ - (OPTIONAL) For Messages with `type` values of `server_request`, this MUST be a human-readable description providing the Client with more information about what this submission request item is.
* `submitted_uri` - _[URL](#url)_ - (OPTIONAL) For Messages with `type` values of `client_submission`, this MAY be a URL that the Client is submitting as their response to the Server's `server_request`, when the Server's requested field is for a remote resource, such as an logo or binary file.
* `previous_value` - _various_ - (OPTIONAL) For Messages with `type` values of `field_changes`, this MUST be the value of the field that the is being requested to be changed from.
* `new_value` - _various_ - (OPTIONAL) For Messages with `type` values of `field_changes`, this MUST be the value of the field that the is being requested to be changed to.

### 6.6. Client Grant Request Object Format <a id="client-grant-request-format" href="#client-grant-request-format" class="permalink">🔗</a>

Client Grant Request objects are formatted as JSON objects and contain the following named values:

* `scope` - _[string](#string)_ - (REQUIRED) The OAuth scope string being requested for the Grant.
* `authorization_details` - _Array[[OAuth AuthorizationDetail](#ref-rfc9396-auth-details)]_ - (REQUIRED) An authorization details list as defined by [[RFC 9396 Section 7.1](#ref-rfc9396-auth-details)], to further specify the grant's requested scope.
If the `scope` string is sufficient to define the requested scope being requested by the Client, this value is an empty list (`[]`).

### 6.7. Message Attachment Object Format <a id="message-attachment-format" href="#message-attachment-format" class="permalink">🔗</a>

Message Attachment objects are formatted as JSON objects and contain the following named values:

* `filename` - _[string](#string)_ - (REQUIRED) The attached file's name.
* `mime_type` - _[MIME type](#mime-type)_ - (REQUIRED) The attached file's content type. For example, an attached PDF file would have a `mime_type` of `application/pdf`. Unknown file content types MUST have a `mime_type` value of `application/octet-stream`.
* `data` - _[string](#string)_ - (REQUIRED) A Base64 encoded string of the file's data.

### 6.8. Listing Messages <a id="messages-list" href="#messages-list" class="permalink">🔗</a>

Clients may request to list Message objects that they have access to by making an HTTPS [GET](#get) request, authenticated with a valid Bearer `access_token` scoped to the `client_admin` scope, to the `cds_messages_api` URL included in the [Authorization Server Metadata](#auth-server-metadata-format).
The Message listing request responses are formatted as JSON objects and contain the following named values.

* `outstanding` - _Array[[ClientMessage](#message-format)]_ - (REQUIRED) A list of Messages where the `status` is `open` or `pending`.
* `outstanding_next` - _[URL](#url) or `null`_ - Where to request the next segment of the list of outstanding Messages.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `outstanding_previous` - _[URL](#url) or `null`_ - Where to request the previous segment of the list of outstanding Messages.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.
* `unread` - _Array[[ClientMessage](#message-format)]_ - (REQUIRED) A list of Messages where the `read` is `false`.
* `unread_next` - _[URL](#url) or `null`_ - Where to request the next segment of the list of unread Messages.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `unread_previous` - _[URL](#url) or `null`_ - Where to request the previous segment of the list of unread Messages.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.
* `read` - _Array[[ClientMessage](#message-format)]_ - (REQUIRED) A list of Messages where the `read` is `true`.
* `read_next` - _[URL](#url) or `null`_ - Where to request the next segment of the list of read Messages.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `read_previous` - _[URL](#url) or `null`_ - Where to request the previous segment of the list of read Messages.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the [GET](#get) request, which will filter the list of Messages to be the intersection of results for each of the URL parameters filters:

* `message_ids` - A space-separated list of `message_id` values for which the Servers MUST filter the Messages.

Responses to `outstanding_next`, `outstanding_previous`, `unread_next`, `unread_previous`, `read_next`, or `read_previous` MUST be formatted the same as the initial Message listing, and MUST only include listings for the relevant next segment.
For example, if the Client requests a `unread_next`, the Server's response MUST have `outstanding` and `read` lists be empty lists (`[]`).

Listings of Message objects MUST be ordered in reverse chronological order by `modified` timestamp, where the most recently modified relevant Message MUST be first in each listing.

### 6.9. Creating Messages <a id="messages-create" href="#messages-create" class="permalink">🔗</a>

Clients create new Messages by sending an authenticated HTTPS [POST](#post) request to the `cds_messages_api` endpoint with the body of the request formatted a JSON object.

The fields included in JSON object MUST include the following:

* `previous_uri` - _[URL](#url) or `null`_ - If submitting a Message with a `type` value of `client_submission`, this value MUST be the Message `uri` that this Message is being submitted in response to (i.e. must have a `type` value of `server_request`).
  If submitting a Message with a `type` value of `support_request` or `private_message`, if the Client is responding to a previous Message, this value MUST be the Message `uri` of that Message.
  If submitting a Message with a `type` value of `support_request` or `private_message` that is not responding to another specific Message, this value MUST be `null`.
* `type` - _[ClientMessageType](#message-types)_ - This value MUST be one of `private_message`, `support_request`, `grant_request`, or `client_submission`.
* `name` - _[string](#string)_ - If submitting a Message with a `type` value of `client_submission`, this value MUST be an empty string (`""`).
  If submitting a Message with a `type` value of `support_request` or `private_message`, this value MUST be the subject line of the message.
* `description` - _[string](#string)_ - If submitting a Message with a `type` value of `client_submission`, this value MUST be an empty string (`""`).
  If submitting a Message with a `type` value of `support_request` or `private_message`, this value MUST be the body of the message.

The fields included in JSON object MAY include the following:

* `updates_requested` - _Array[[ClientUpdateRequest](#client-update-request-format)]_ - If submitting a Client Update with a `type` value of `client_submission`, this is required and MUST be a list of [Client Update Request](#client-update-request-format) objects with `field` values matching the `field` values in the corresponding `server_request` Client Update Request objects, and `description` or `submitted_uri` values being the Client's submission response to the Server's request for that `field`.
* `grants_requested` - _Array[[ClientGrantRequest](#client-grant-request-format)]_ - If submitting a Client Update with a `type` value of `grant_request`, this is required and MUST be a list of [Client Grant Request](#client-grant-request-format) objects.
* `related_uri` - _[URL](#url) or `null`_ - If submitting a Message with a `type` value of `support_request`, this value MAY be a URL to the relevant API endpoint for the support request.
  If there is no relevant API endpoint, the Client MUST not include this field.

Servers MUST reject requests with a `400 Bad Request` Status Code when a Client submits an incomplete request, the submitted values are invalid.
Servers MUST reject requests with a `413 Content Too Large` Status Code when a Client submits a Message with attachments that exceed the Server's submission size limit, which MUST be equal to or greater than 10 megabytes.
For valid [POST](#post) requests from Clients, Servers MUST respond with a `201 Created` Status Code with a JSON object of the complete newly created Message object.
When committing Messages created by Clients, Servers MUST populate the following fields in addition to the Client's submitted fields:

* `uri` - _[URL](#url)_ - The endpoint where the Client can retrieve the newly created Message object.
* `read` - _boolean_ - Always set to `true`.
* `creator` - _[string](#string)_ - Always set to the Client's `client_id`.
* `created` - _[datetime](#datetime)_ - Always set to the Server's timestamp for when the Message was created.
* `modified` - _[datetime](#datetime)_ - Always the same as `created`.
* `status` - _[ClientMessageStatus](#message-statuses)_ - For `type` values of `private_message` or `client_submission`, this value MUST be `complete`.
  For `type` values of `support_request` and `grant_request`, this value MUST be `pending`.

When Clients submit Messages with `type` value of `client_submission`, if the Message referenced in the `previous_uri` has a `status` of `open`, then the Server MUST update the `status` of that referenced Message to `pending`, which indicates that the Client has submitted a response for Server review.

When Clients submit Messages with `type` value of `grant_request`, Servers MUST review `grants_requested` values and create a new Message replying to the Client with `type` value of `grant_request`, `previous_uri` value of the Clients grant request Message `uri`, and `status` of `pending` (if the Server has not yet determined whether to create the requested grants), `complete` (if the request is approved and Grants have been created), or `rejected` (if the request is rejected for any reason).

### 6.10. Retrieving Individual Messages <a id="messages-get" href="#messages-get" class="permalink">🔗</a>

The URL to be used to send [GET](#get) requests for retrieving individual Message objects MUST be the Message `uri` provided in the [Message object](#message-format).

### 6.11. Modifying Messages <a id="messages-modify" href="#messages-modify" class="permalink">🔗</a>

Clients may modify fields in a Messages object by sending an authenticated HTTPS [PATCH](#patch) request to the Message `uri` endpoint with the body of the request formatted a JSON object.
The fields included in JSON object are the fields the Client intends to modify with the submitted fields' values.
If a field is not included in the [PATCH](#patch) request, the Server MUST leave the field unmodified from its current value.

The following are fields that MAY be included in the [PATCH](#patch) request, and modification MUST be supported by Servers:

* `read` - Servers MUST accept values of `true` and `false` from the Client.

If a Client wishes to modify a previously submitted Message of type `client_submission` they created, the Client MUST [create a new Message](#messages-create) with the same `previous_uri` value as the Message the Client is wishing to supersede.
Severs MUST process newer Messages created by the Client as overriding the Client's previous submission.

If a Client wishes to amend a previously created Message of type `private_message` or `support_request` they created, the Client MUST [create a new Message](#messages-create) with the `previous_uri` value set as the Message `uri` for which they are wanting to amend.
Severs MUST consider newer Messages created by the Client as amending the Client's previous support request or message.

## 7. Credentials API <a id="credentials-api" href="#credentials-api" class="permalink">🔗</a>

To allow Clients to manage `client_secret` values used for authentication to APIs, this specification requires that Server implement a Credentials API.
Servers MAY implement other means of managing Credentials, such as a web interface, but they MUST also mirror any Credentials managed by other means on this required Credentials API.

The Credentials API endpoints are authenticated using a Bearer `access_token` obtained by the Client using OAuth 2.0's `client_credentials` grant process [[RFC 6749 Section 4.4](#ref-rfc6749-client-credentials)], where the `client_id` used is for a Client that includes the `client_admin` scope.

### 7.1. Credentials Object Format <a id="credentials-format" href="#credentials-format" class="permalink">🔗</a>

Credentials objects are formatted as JSON objects and contain the following named values:

* `credential_id` - _[string](#string)_ - (REQUIRED) The unique identifier for the Credential on the Server's system.
* `uri` - _[URL](#url)_ - (REQUIRED) Where to retrieve or modify this specific Credential object.
* `client_id` - _[string](#string)_ - (REQUIRED) Which Client this Credential belongs to.
* `created` - _[datetime](#datetime)_ - (REQUIRED) When the Credential was created.
* `modified` - _[datetime](#datetime)_ - (REQUIRED) When the Credential was last modified.
* `type` - _[CredentialType](#credentials-types)_ - (REQUIRED) The type of credential.
* `client_secret` - _[string](#string)_ - (OPTIONAL) A sufficiently random value generated by the Server that can be used by the Client to authenticate themselves when obtaining an `access_token` from the Server's OAuth Token Endpoint [[RFC 6749 Section 3.2](#ref-rfc6749-token-endpoint)].
  This field MUST be included if the `type` is `client_secret`.
* `client_secret_expires_at` - _[integer](#integer)_ - (OPTIONAL) The timestamp at which the `client_secret` will expire, as defined in OAuth's Client Information Response [[RFC 7591 Section 3.2.1](#ref-rfc7591-client-info-resp)].
  If the Client object refrenced by the `client_id` has a status of `disabled`, this value MUST be set to the time when the Client object was most recently disabled.
  This field MUST be included if the `type` is `client_secret`.

### 7.2. Credentials Types <a id="credentials-types" href="#credentials-types" class="permalink">🔗</a>

Credential object `type` values MUST be one of the following:

* `client_secret` - The Credential is a shared secret string string in the `client_secret` value on the Credential object.

### 7.3. Listing Credentials <a id="credentials-list" href="#credentials-list" class="permalink">🔗</a>

Clients may request to list Credential objects that they have access to by making an HTTPS [GET](#get) request, authenticated with a valid Bearer `access_token` scoped to the `client_admin` scope, to the `cds_credentials_api` URL included in the [Authorization Server Metadata](#auth-server-metadata-format).
The Credential listing request responses are formatted as JSON objects and contain the following named values.

* `credentials` - _Array[[ScopeCredential](#credentials-format)]_ - (REQUIRED) A list of Credentials to which the requesting `access_token` is scoped to have access.
  If no Credentials are accessible, this value is an empty list (`[]`).
  If more than 100 sCredentials are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Credentials.
* `next` - _[URL](#url) or `null`_ - Where to request the next segment of the list of Credentials.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url) or `null`_ - Where to request the previous segment of the list of Credentials.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the [GET](#get) request, which will filter the list of Credentials to be the intersection of results for each of the URL parameters filters:

* `credential_ids` - A space-separated list of `credential_id` values for which the Servers MUST filter the Credentials.
* `client_ids` - A space-separated list of `client_id` values for which the Servers MUST filter the Credentials.
* `after` - A [datetime](#datetime) for which the Server MUST filter Credentials that were created after or on the datetime.
* `before` - A [datetime](#datetime) for which the Server MUST filter Credentials that were created before or on the datetime.

Listings of Credential objects MUST be ordered in reverse chronological order by `modified` timestamp, where the most recently modified relevant Credential MUST be first in each listing.

For Credential objects in the listing that are added, removed, or modified by the Server or Client after initial registration, Servers MUST add a message to the [listed Messages](#messages-list) notifiying the Client that a Credential object has been added, removed, or modified.
This creates a changelog in the Messages listing for Credential object updates.

### 7.4. Retrieving Individual Credentials <a id="credentials-get" href="#credentials-get" class="permalink">🔗</a>

The URL to be used to send [GET](#get) requests for retrieving individual Credential objects MUST be the Credential `uri` provided in the [Credential object](#credentials-format).

### 7.5. Creating Credentials <a id="credentials-create" href="#credentials-create" class="permalink">🔗</a>

Clients create new Credentials by sending an authenticated HTTPS [POST](#post) request to the `cds_credentials_api` endpoint with the body of the request formatted a JSON object.
The fields included in JSON object MUST include the following:

* `client_id` - _[string](#string)_ - This value is Client object's identifier for which a new Credential object will be created.

Servers MUST reject requests with a `400 Bad Request` Status Code when a Client submits an incomplete request or the submitted values are invalid.
For valid [POST](#post) requests from Clients, Servers MUST respond with a `201 Created` Status Code with an updated JSON object of the created Credential object.

### 7.6. Modifying Credentials <a id="credentials-modify" href="#credentials-modify" class="permalink">🔗</a>

Clients may modify fields in the Credentials API by sending an authenticated HTTPS [PATCH](#patch) request to the Credential `uri` endpoint with the body of the request formatted a JSON object.
The fields included in JSON object are the fields the Client intends to update with the submitted fields' values.
If a field is not included in the [PATCH](#patch) request, the Server MUST leave the field unmodified from its current value.

The following are fields that MAY be included in the [PATCH](#patch) request, and modification MUST be supported by Servers:

* `client_secret_expires_at` - Servers MUST only accept valid timestamp values greater than or equal to the current time and less than or equal to the current value.
  When the current value is `0`, indicating no expiration time, Severs MUST allow Clients to set a value of `0` or any timestamp greater than or equal to the current time.

Servers MUST reject requests with a `400 Bad Request` Status Code when fields are submitted that are not able to be modified by the Client or the submitted values are invalid.
If all submitted fields are valid and have been updated, the Server MUST respond with a `200 OK` Status Code.

If a Client updates the `client_secret_expires_at` of a Credential to be a timestamp equal to or less than the current time, the Server MUST assume the Credential is compromised and synchronously disable the Credential's `client_secret` from being used on the token endpoint and revoke any `access_token` or `refresh_token` values issued by the token endpoint as a result of using the now-disabled Credential.

Servers MAY, based on their own policies or discussion between the Client and Server, modify the `client_secret_expires_at` arbitrarily.

Servers MUST NOT modify `client_secret` values after they are created.
If the `client_secret` is determined to be leaked or compromised, Servers MUST update the `client_secret_expires_at` value to the current time or less, thus expiring the Credential.

## 8. Grants API <a id="grants-api" href="#grants-api" class="permalink">🔗</a>

This specification requires Servers provide an API allowing Clients to view and edit OAuth user authorizations and client credentials grants ("Grants") related to their registration.
These APIs are authenticated using a Bearer `access_token` obtained by the Client using OAuth 2.0's `client_credentials` grant process [[RFC 6749 Section 4.4](#ref-rfc6749-client-credentials)], where the `client_id` used is for a Client that includes the `client_admin` scope.

### 8.1. Grant Object Format <a id="grant-format" href="#grant-format" class="permalink">🔗</a>

Grant objects are formatted as JSON objects and contain the following named values:

* `grant_id` - _[string](#string)_ - (REQUIRED) The unique identifier for the Grant on the Server's system.
* `uri` - _[URL](#url)_ - (REQUIRED) A link to this specific Grant that can be used to [retrieve](#grants-get) or [modify](#grants-modify) the individual Grant.
* `replacing` - _Array[[URL](#url)]_ - (REQUIRED) If this Grant is superseding another Grant, this is the list of Grant `uri` values of the superseded Grants.
  If this grant is not superseding any other Grants, this value is an empty list (`[]`).
* `replaced_by` - _Array[[URL](#url)]_ - (REQUIRED) If this Grant has been superseded by other Grants, this is the list of Grant `uri` values of the superseding Grants.
  If no other Grants have superseded this Grant, this value is an empty list (`[]`).
* `parent` - _[string](#string) or `null`_ - (REQUIRED) If this Grant represents an individual sub-Grant that is part of another Grant that required multiple authorizations, this is the parent Grant's `grant_id` value.
  If this Grant is not a sub-Grant, this value is `null`.
* `children` - _Array[[string](#string)]_ - (REQUIRED) If this Grant represents a parent Grant that has multiple sub-Grants underneath it, this is a list of Grant `grant_id` values for each sub-Grant.
  If this Grant does not have any sub-Grants, this value is an empty list (`[]`).
* `created` - _[datetime](#datetime)_ - (REQUIRED) When the Grant was created.
* `modified` - _[datetime](#datetime)_ - (REQUIRED) When the Grant was most recently modified.
  If the Grant has not been modified since creation, this is the same value as `created`.
* `not_before` - _[datetime](#datetime) or `null`_ - (REQUIRED) When a Grant that is for future access, Grant's `status` will be switched from `future` to another value.
  If the Grant is not for future access and access is available immediately upon creation, this value is `null`.
* `not_after` - _[datetime](#datetime) or `null`_ - (REQUIRED) When a Grant has access that will expire in the future, this value indicates when the Grant's `status` will be switched to `expired`.
  If the Grant does not expire, this value is `null`.
* `eta` - _[datetime](#datetime) or `null`_ - (REQUIRED) When a Grant has a `status` with the value `pending`, this value indicates an estimated time for when the status is expected to be switched to another value.
  If the Grant's `status` is not `pending` this value is `null`.
* `expires` - _[datetime](#datetime) or `null`_ - (REQUIRED) When the Grant will expire and access will be removed by the Server.
  If the Grant is to continue indefinitely, this value is `null`.
* `status` - _[GrantStatus](#grant-status)_ - (REQUIRED) The current [Grant Status](#grant-status) of the Grant.
* `client_id` - _[string](#string)_ - (REQUIRED) Which Client for which this Grant is issued.
* `scope` - _[string](#string)_ - (REQUIRED) The scopes for which this Grant has issued access.
* `authorization_details` - _Array[[OAuth AuthorizationDetail](#ref-rfc9396-auth-details)]_ - (REQUIRED) An authorization details list as defined by [[RFC 9396 Section 7.1](#ref-rfc9396-auth-details)] which contains scopes that are granted in addition to this object's `scope` value.
  If no authorization details scopes are configured in addition to the `scope` string, this value is an empty array (`[]`).
  If the `type` field of an authorization detail object in this array is the same as a value in the `scope` string, the authorization detail object replaces the scope with additional more refined access details for the scope.
* `receipt_confirmations` - _Array[[string](#string)]_ - (REQUIRED) For Grants with scopes that can be obtained via user authorization (`grant_types` contains `authorization_code`), this is a list of receipt confirmation codes that were provided to the end users who authorized the access.
  If no receipt was provided or the Grant did not get issued via `authorization_code` then this value is an empty list (`[]`).
* `enabled_scope` - _[string](#string)_ - (REQUIRED) For Grants where access has been partially granted, but some access is still disabled, this value is the `scope` that has been enabled by the server.
  For Grants where access has been fully granted, this value is the same as the `scope` value.
  For Grants that have had their access removed (e.g. `disabled`), this value is an empty string (`""`).
* `enabled_authorization_details` - _Array[[OAuth AuthorizationDetail](#ref-rfc9396-auth-details)]_ - (REQUIRED) For Grants where access has been partially granted, but some access is still disabled, this value is an authorization details list as defined by [[RFC 9396 Section 7.1](#ref-rfc9396-auth-details)] that has the scopes which have been enabled by the server.
  For Grants where access has been fully granted, this value is the same as the `authorization_details` value.
  For Grants that have had their access removed (e.g. `disabled`), this value is an empty list (`[]`).

### 8.2. Grant Statuses <a id="grant-statuses" href="#grant-statuses" class="permalink">🔗</a>

Grants object `status` values MUST be one of the following:

* `active` - The Grant is active and access for the features and data specified in the `scope` or `authorization_details` is currently enabled.
  For Grants that provide access to data APIs that are being regularly updated, this status also indicates that the Server is currently on schedule to provide the expected ongoing data updates.
* `future` - The Grant will be made in the future, and the Client should check the `not_before` value for when the Grant will have its status switched to `active`.
* `pending` - The Grant has been create and is valid, but the Server is still processing or reviewing the Grant and the status will be changed to another status at approximately the time provided in the Grant's `eta` value.
  While a Grant's status is `pending`, Servers MUST provide access to the granted scope's features or data as the Server processes and the features or data becomes available.
* `partial` - The Grant was submitted originally with the `scope` and `authorization_details` values, but the Server has limited the Client's access a subset of the Grant's `scope`.
  The enabled access is listed in the `enabled_scope` and `enabled_authorization_details` values.
* `needs_authorization` - The Grant has been created, but access is limited to the `enabled_scope` and `enabled_authorization_details` until a user authorizes the listed `scope` and `authorization_details` values using an authorization URL with a `cds_grant_id` parameter of this `grant_id` as described in [Grant Authorization Requests](#grant-authorization-requests).
* `needs_sub_grants` - The Grant has been created, but access is limited to the `enabled_scope` and `enabled_authorization_details` until listed `children` Grant objects' `status` values change to an appropriate value determined by the Server for the Grant scope's use case.
  Servers MUST include in their technical documentation which sub-Grant `status` values are sufficient to meet the critera to address this `needs_sub_grants` status for each scope's use case.
* `disabled` - The Grant has been indefinitely disabled by the Server, and the Client SHOULD NOT expect the Grant's status to be updated in the future.
* `suspended` - The Grant has been temporarily disabled by the Server or user who authorized access, and the Client SHOULD expect the Grant's status to be updated in the future.
* `revoked` - For Grants with scopes that can be obtained via user authorization (`grant_types` contains `authorization_code`), this status means the user has revoked access.
* `closed` - The Grant has been revoked by the Client via the [Modifying Grants](#grants-modify) API.
* `expired` - The Grant has expired and access has been removed.
  The Grant's `not_after` value indicates when the Grant's access was removed and the Grant is considered expired.
* `delayed` - For Grants that provide access to data APIs that are being regularly updated, this status indicates that the data being provided is behind schedule and will be updated when the Server resolves its internal delay issues.
* `stopped` - For Grants that provide access to data APIs that are being regularly updated, this status indicates that the data being provided has stopped being updated either by the Server or the user who originally authorized the access.
  Access to existing data is still available, but the Client should not expect updates to the data being provided.
* `errored` - The Server has encountered an internal error for this Grant, and Client access cannot be confirmed by the Server.

Grant `status` values of `future`, `disabled`, `suspended`, `revoked`, `closed`, and `expired` indicate that access to all features and data that were accessible via this Grant are no longer accessible.

### 8.3. Grant Authorization Requests <a id="grant-authorization-requests" href="#grant-authorization-requests" class="permalink">🔗</a>

When a Grant `status` has a value of `needs_authorization`, the Grant needs a user authorization for the Grant to be approved.
Clients achieved user authorization by following OAuth's Authorization Code Grant process [[RFC 6749 Section 4.1](#ref-rfc6749-code-grant)], with the following modifications.

* In the OAuth Authorization Request [[RFC 6749 Section 4.1.1](#ref-rfc6749-auth-request)], Clients MUST include the URL request parameter `cds_grant_id` with a value of the `grant_id` for the Grant for which the Client is requesting user authorization.
* In the OAuth Authorization Request [[RFC 6749 Section 4.1.1](#ref-rfc6749-auth-request)], Clients MAY include the URL request parameter `prompt` with a value of `login`.
* Clients MAY include `cds_grant_id` and `prompt` in the Pushed Authorization Request process [[RFC 9126](#ref-rfc9126)] to include them in the `request_uri` parameter.
  Servers MUST be able to parse `cds_grant_id` and `prompt` parameters from a received `request_uri` parameter in the authorization request.
* Upon receiving an authorization request, prior to any necessary user authentication, Servers MUST evaluate any provided `cds_grant_id` parameter in relation to the other authorization request parameters and, if the `cds_grant_id` parameter is invalid (e.g. not appropriate for the requested `scope` or `client_id`), MUST reject the authorization request and redirect back to the Client's `redirect_uri` with an `error` parameter value of `cds_grant_id_invalid_value`.
  Servers MUST evaluate other authorization request parameters first, and reject the authorization request with any appropriate errors before evaluating the `cds_grant_id` parameter.
  Servers MUST ignore empty `cds_grant_id` parameters as if it were not included in the authorization request.
  Servers MUST reject authorization requests with more than one non-empty `cds_grant_id` parameter.
* Upon receiving an authorization request, if the Client included a `prompt=login` parameter, Servers MUST require the user to authenticate, even if the user is already authenticated.
* Upon receiving an authorization request, after successfully completing any necessary user authentication, Servers MUST evaluate again any provided `cds_grant_id` value in relation to the other authorization request parameters and the user and, if the `cds_grant_id` parameter is invalid (e.g. not intended for the authenticated user), MUST reject the authorization request and redirect back to the Client's `redirect_uri` with an `error` parameter value of `cds_grant_id_invalid_user`.
* Upon the user authorizing the authorization request, Servers MUST update the Grant with a `grant_id` value matching the `cds_grant_id` parameter to reflect the obtained authorization.
  For example, a Grant could have its `status` value updated from `needs_authorization` to `active` or `pending`.
  Servers MUST NOT wait for the Access Token Request [[RFC 6749 Section 4.1.3](#ref-rfc6749-token-request)] to update the Grant.

### 8.4. Listing Grants <a id="grants-list" href="#grants-list" class="permalink">🔗</a>

Clients may request to list Grant objects that they have access to by making an HTTPS [GET](#get) request, authenticated with a valid Bearer `access_token` scoped to the `client_admin` scope, to the `cds_grants_api` URL included in the [Authorization Server Metadata](#auth-server-metadata-format).
The Grant listing request responses are formatted as JSON objects and contain the following named values.

* `grants` - _Array[[Grant](#grant-format)]_ - (REQUIRED) A list of Grants to which the requesting `access_token` is scoped to have access.
  If no Grants are accessible, this value is an empty list (`[]`).
  If more than 100 Grants are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Grants.
* `next` - _[URL](#url) or `null`_ - Where to request the next segment of the list of Grants.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url) or `null`_ - Where to request the previous segment of the list of Grants.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the [GET](#get) request, which will filter the list of Grants to be the intersection of results for each of the URL parameters filters:

* `grant_ids` - A space-separated list of `grant_id` values for which the Servers MUST filter the Grants.
* `parents` - A space-separated list of `parent` values for which the Servers MUST filter the Grants.
* `statuses` - A space-separated list of `status` values for which the Servers MUST filter the Grants.
* `client_ids` - A space-separated list of `client_id` values for which the Servers MUST filter the Grants.
* `scopes` - A space-separated list of `scope` values for which the Server MUST filter the Grants, where the included `scope` values are values within the Grant `scope` (space separated) or as a `type` value in the `authorization_details` list.
* `receipt_confirmations` - A space-separated list of receipt confirmation codes for which the Server MUST filter the Grants.
* `after` - A [datetime](#datetime) for which the Server MUST filter Grants that were created after or on the datetime.
* `before` - A [datetime](#datetime) for which the Server MUST filter Grants that were created before or on the datetime.

Listings of Grant objects MUST be ordered in reverse chronological order by `modified` timestamp, where the most recently updated relevant Grant MUST be first in each listing.

### 8.5. Retrieving Individual Grants <a id="grants-get" href="#grants-get" class="permalink">🔗</a>

The URL to be used to send [GET](#get) requests for retrieving individual Grant objects MUST be the Grant `uri` provided in the [Grant object](#grant-format).

### 8.6. Modifying Grants <a id="grants-modify" href="#grants-modify" class="permalink">🔗</a>

Clients may modify fields in the Grants API by sending an authenticated HTTPS [PATCH](#patch) request to the Grant `uri` endpoint with the body of the request formatted a JSON object.
The fields included in JSON object are the fields the Client intends to update with the submitted fields' values.
If a field is not included in the [PATCH](#patch) request, the Server MUST leave the field unmodified from its current value.

The following are fields that MAY be included in the [PATCH](#patch) request body, and modification MUST be supported by Servers:

* `status` - Servers MUST only accept a value of `closed` from the Client.
* `scope` - Servers MUST evaluate the updated scope for whether another user authorization is required.
  Servers MUST NOT require another user authorization if the updated scope is a reduction in scope of access (e.g. removal of a scope value from the scope string).
  If a new user authorization or sub-Grants are required, the Server MUST update the `status` to one of `needs_authorization` or `needs_sub_grants` and update the `enabled_scope` to be the value of the current scope for which the Client is allowed access.
* `authorization_details` - Servers MUST evaluate the updated authorization details for whether another user authorization is required.
  Servers MUST NOT require another user authorization if the updated scope is a reduction in scope of access (e.g. removal of a scope value from the scope string).
  If a new user authorization or sub-Grants are required, the Server MUST update the `status` to one of `needs_authorization` or `needs_sub_grants` and update the `enabled_authorization_details` to be the value of the current authorization details for which the Client is allowed access.

Servers MUST reject requests with a `400 Bad Request` response when fields are submitted that are not able to be modified by the Client or the submitted values are invalid.
For valid [PATCH](#patch) requests from Clients, Servers MUST respond with a `200 OK` Status Code with an updated JSON object of the complete current Grant object.

If a Server needs to asynchronously review and approve changes to any submitted Grant object fields that have been submitted by the Client and are different from the current values, for valid modification requests the Server MUST update the Grant's `status` to `pending` in the response.
Additionally, if the `scope` or `authorization_details` has been updated, the Server MUST update the `enabled_scope` and `enabled_authorization_details` to reflect for what the Client currently has access while the Grant is pending.

## 9. Server-Provided Files API <a id="server-provided-files-api" href="#server-provided-files-api" class="permalink">🔗</a>

This specification defines an API by which Servers MAY provide an access to arbitrary files to Clients to download.
These APIs are authenticated using a Bearer `access_token` obtained by the Client using OAuth 2.0's `client_credentials` grant process [[RFC 6749 Section 4.4](#ref-rfc6749-client-credentials)], where the scope of the access token is `grant_admin` with `authorizatin_details` entries listing `grant_id` values that are for Grants that have the `server_provided_files` scope.

This API is intended to provide a convenient way for Servers to provide secure ad-hoc file access to Clients, such as sharing connectivity-related files (e.g. configs, certificates, secret keys, etc.) or manually created bulk files (e.g. initial backfill raw data, analysis reports, etc.).
This API is NOT intended to be used for automated sharing of structured data (e.g. nightly interval extracts) because the API has limited functionality to convey the appropriate metadata for automated file sharing, such as versioning or schemas.
It is RECOMMENDED that automated sharing of structured datasets be performed using another structured API or transfer method more fit for purpose, which MAY be an extension to this specification.

### 9.1. Server-Provided Files Object Format <a id="server-provided-files-format" href="#server-provided-files-format" class="permalink">🔗</a>

Server-Provided File objects are metadata for arbitrary files made accessible by the Server and are formatted as JSON objects containing the following named values:

* `file_id` - _[string](#string)_ - (REQUIRED) The unique identifier for the Server-Provided File on the Server's system.
* `uri` - _[URL](#url)_ - (REQUIRED) A link to this specific Server-Provided File that can be used to [retrieve](#server-provided-files-get) the individual Server-Provided File.
* `created` - _[datetime](#datetime)_ - (REQUIRED) When the Server-Provided File was created.
* `modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server-Provided File was most recently modified.
  If the Server-Provided File has not been modified since creation, this is the same value as `created`.
* `mime_type` - _[MIME type](#mime-type)_ - (REQUIRED) The content type of the file.
* `size` - _[integer](#integer)_ - (REQUIRED) The size of the file in bytes.
* `name` - _[string](#string)_ - (REQUIRED) The file name, including any file extension.
* `description` - _[string](#string)_ - (REQUIRED) A description of the file.
  If no description is provided by the Server, this is an empty string (`""`).
* `download_uri` - _[URL](#url)_ - (REQUIRED) A link to the raw file being made accessible by the Server.
  Servers MUST authenticate this download endpoint using the same authentication requirements as the rest of the [Server-Provided Files API](#server-provided-files-api).

### 9.2. Listing Server-Provided Files <a id="server-provided-files-list" href="#server-provided-files-list" class="permalink">🔗</a>

Clients may request to list Server-Provided File objects that they have access to by making an HTTPS [GET](#get) request, authenticated with a valid Bearer `access_token` scoped to the `client_admin` scope, to the `cds_server_provided_files_api` URL included in the [Authorization Server Metadata](#auth-server-metadata-format).
The Server-Provided File listing request responses are formatted as JSON objects and contain the following named values.

* `files` - _Array[[ServerProvidedFile](#server-provided-files-format)]_ - (REQUIRED) A list of Server-Provided File objects to which the requesting `access_token` is scoped to have access.
  If no Server-Provided File objects are accessible, this value is an empty list (`[]`).
  If more than 100 Server-Provided File objects are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Server-Provided File objects.
* `next` - _[URL](#url) or `null`_ - Where to request the next segment of the list of Server-Provided File objects.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url) or `null`_ - Where to request the previous segment of the list of Server-Provided File objects.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the [GET](#get) request, which will filter the list of Server-Provided Files to be the intersection of results for each of the URL parameters filters:

* `file_ids` - A space-separated list of `file_id` values for which the Servers MUST filter the Server-Provided Files.

Listings of Server-Provided File objects MUST be ordered in reverse chronological order by `modified` timestamp, where the most recently updated relevant Server-Provided File object MUST be first in each listing.

### 9.3. Retrieving Individual Server-Provided Files <a id="server-provided-files-get" href="#server-provided-files-get" class="permalink">🔗</a>

The URL to be used to send [GET](#get) requests for retrieving individual Server-Provided File objects MUST be the Server-Provided File `uri` provided in the [Message object](#message-format).

## 10. Extensions <a id="extensions" href="#extensions" class="permalink">🔗</a>

Other specifications and Servers MAY extend this specification to as necessary to address their use cases.
Other specifications and Servers MUST NOT remove required parts of this specification in their specifications or implementations.

[Scopes Supported](#scopes) MAY be extended to add additional scope values and definitions, according to the requirements listed in [Section 3.3](#scopes).

[Metadata Object](#auth-server-metadata-format), [Scope Descriptions Object](#scope-descriptions-format), [Registration Field Object](#registration-field-format), [Authorization Details Field Object](#auth-details-fields-format), [Authorization Details Field Choice Object](#auth-details-field-choice-format), [Client Object](#client-format), [Client Listing](#clients-list), [Message Object](#message-format), [Client Update Request Object](#client-update-request-format), [Client Grant Request Object](#client-grant-request-format), [Message Attachment Object](#message-attachment-format), [Message Listing](#messages-list), [Credential Object](#credentials-format), [Credential Listing](#credentials-list), [Grant Object](#grant-format), [Grant Listing](#grant-list), [Server-Provided Files Object](#server-provided-files-format), and [Server-Provided Files Listing](#server-provided-files-list) MAY be extended to allow for additional fields to be possible in their objects.
When extending the object format, other specifications or Server documentation MUST reference the relevant section in this specification and denote that they are extending the object to add a new named field.
The additional field MUST be specified with a general description, the field value's format, and whether the field is REQUIRED or OPTIONAL.

The enumerated lists for valid [Registration Field Types](#registration-field-types), [Registration Field Formats](#registration-field-formats), [Authorization Details Field Formats](#auth-details-field-formats), [Client Statuses](#client-statuses), [Message Types](#message-types), [Message Statuses](#message-statuses), [Message Related Types](#message-related-types), [Credentials Types](#credentials-types), and [Grant Statuses](#grant-statuses) MAY be extended to allow for additional strings to be valid.
When extending enumerated list, other specifications or Server documentation MUST reference the relevant section in this specification and denote that they are extending the list to add a new string.
The additional string MUST be specified with a description of what that string means when it is included in the relevant array.

To facilitate forwards compatibility, Clients MUST ignore unknown or undocumented object fields and enumerated strings.
If a Client cannot provide adequate functionality based on too many unknown or undocumented object fields or enumerated strings, the Client SHOULD refer to the Server's technical documentation linked in the `documentation` value of the CDS Server Metadata object [[CDS-WG1-01 Section 3.2(#ref-cds-wg1-01-metadata-object)] or contact the Server's technical support (via the `support` value in the metadata object).

## 11. Examples <a id="examples" href="#examples" class="permalink">🔗</a>

### 11.1. CDS Server Metadata <a id="example-cds-server-metadata" href="#example-cds-server-metadata" class="permalink">🔗</a>

The following is a non-normative example of requesting a CDS Server Metadata object that includes this specification's [`oauth` capability](#auth-server-metadata-url).

```
==Request==
GET /.well-known/cds-server-metadata.json HTTP/1.1
Host: example.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "cds_metadata_version": "v1",
    "cds_metadata_url": "https://example.com/.well-known/cds-server-metadata.json",
    "created": "2022-01-01T00:00:00Z",
    "updated": "2022-06-01T00:00:00Z",
    "name": "Example Data Hub",
    "description": "A fictional regional data hub that offers information about the region's utilities.",
    "website": "https://example.com/data-access",
    "documentation": "https://example.com/docs",
    "support": "https://example.com/developers/contact",
    "capabilities": [
        "coverage",
        "oauth"
    ],
    "coverage": "https://example.com/cds-coverage.json",
    "oauth_metadata": "https://example.com/.well-known/oauth-authorization-server"
}
```

### 11.2. Authorization Server Metadata <a id="example-auth-server-metadata" href="#example-auth-server-metadata" class="permalink">🔗</a>

The following is a non-normative example of requesting the [Authorization Server Metadata object](#auth-server-metadata-format).

```
==Request==
GET /.well-known/oauth-authorization-server HTTP/1.1
Host: example.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "issuer": "https://example.com",
    "service_documentation": "https://example.com/docs/oauth",
    "op_policy_uri": "https://example.com/legal/oauth-policy",
    "op_tos_uri": "https://example.com/legal/oauth-terms",
    "registration_endpoint": "https://example.com/oauth/register",
    "authorization_endpoint": "https://example.com/oauth/authorize",
    "token_endpoint": "https://example.com/oauth/token",
    "revocation_endpoint": "https://example.com/oauth/token/revoke",
    "introspection_endpoint": "https://example.com/oauth/token/info",
    "pushed_authorization_request_endpoint": "https://example.com/oauth/par",
    "code_challenge_methods_supported": ["S256"],
    "response_types_supported": ["code"],
    "grant_types_supported": ["client_credentials", "authorization_code", "refresh_token"],
    "token_endpoint_auth_methods_supported": ["client_secret_basic"],
    "scopes_supported": ["client_admin", "grant_admin", "server_provided_files", "example_custom"],
    "authorization_details_types_supported": ["client_admin", "grant_admin", "server_provided_files", "example_custom"],
    "cds_oauth_version": "v1",
    "cds_human_registration": "https://example.com/clients/register",
    "cds_test_accounts": "https://example.com/docs/testing",
    "cds_clients_api": "https://example.com/api/clients",
    "cds_messages_api": "https://example.com/api/messages",
    "cds_credentials_api": "https://example.com/api/credentials",
    "cds_grants_api": "https://example.com/api/grants",
    "cds_server_provided_files_api": "https://example.com/api/server-provided-files"
    "cds_scope_descriptions": {
        "client_admin": {
            "id": "client_admin",
            "name": "Client Admin",
            "description": "This scope grants administrative access to the Client management APIs.",
            "documentation": "https://example.com/docs/oauth/scopes#client_admin",
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
            "documentation": "https://example.com/docs/oauth/scopes#grant_admin",
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
                    "documentation": "https://example.com/docs/oauth/scopes#grant_admin-client_id",
                    "format": "string",
                    "is_required": true,
                },
                {
                    "id": "grant_id",
                    "name": "Grant identifier",
                    "description": "The Grant identifier for which the returned access_token will be given access.",
                    "documentation": "https://example.com/docs/oauth/scopes#grant_admin-grant_id",
                    "format": "string",
                    "is_required": true,
                }
            ]
        },
        "server_provided_files": {
            "id": "server_provided_files",
            "name": "Server-Provided Files",
            "description": "This scope grants access to specific files that the Server wants make available to the Client.",
            "documentation": "https://example.com/docs/oauth/scopes#grant_admin",
            "registration_requirements": [],
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
                    "documentation": "https://example.com/docs/oauth/scopes#server_provided_files-file_id",
                    "format": "string",
                    "is_required": true,
                }
            ]
        },
        "server_provided_files": {
            "id": "example_custom",
            "name": "Custom Scope",
            "description": "This scope is an example for a Server-defined custom authorization scope.",
            "documentation": "https://example.com/docs/oauth/scopes#example_custom",
            "registration_requirements": ["company_name"],
            "response_types_supported": ["code"],
            "grant_types_supported": ["authorization_code", "refresh_token"],
            "token_endpoint_auth_methods_supported": ["client_secret_basic"],
            "code_challenge_methods_supported": ["S256"],
            "coverages_supported": ["coverage123"],
            "authorization_details_fields": []
        }
    },
    "cds_registration_fields": {
        "company_name": {
            "id": "company_name",
            "type": "registration_field",
            "field_name": "cds_company_name",
            "description": "The company name to display on the authorization request form",
            "documentation": "https://example.com/docs/oauth/registration#company_name",
            "format": "string",
            "max_length": 1024
        },
    },
}
```

### 11.3. Client Registration Request <a id="example-client-registration" href="#example-client-registration" class="permalink">🔗</a>

The following is a non-normative example of a Client submitting a [Client Registration Request](#registration-request).

```
==Request==
POST /oauth/register HTTP/1.1
Host: example.com

{
    "scope": "client_admin grant_admin example_custom",
    "client_name": "My App Name",
    "cds_company_name": "My Company Name"
}


==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "client_id": "aaf026921707f5d5",
    "client_id_issued_at": 2893256800,
    "scope": "client_admin",
    "redirect_uris": [],
    "response_types": [],
    "grant_types": ["client_credentials"],
    "token_endpoint_auth_method": ["client_secret_basic"],
    "client_secret": "Q3VpGy7k6Mj9Yc-F1wtujttAq2HiEel8O1Ie5zEw00AslNsoUU3SMzKPeRPZgqA6dMW3jSvZQ_O0iWpQRa1NaQ",
    "client_name": "My App Name",
    "cds_company_name": "My Company Name",
    "authorization_details_types": ["client_admin"],
    "cds_created": "2022-01-01T00:00:00Z",
    "cds_modified": "2022-01-01T00:00:00Z",
    "cds_client_uri": "https://example.com/api/clients/aaf026921707f5d5",
    "cds_status": "production",
    "cds_status_options": ["production"],
    "cds_server_metadata": "https://example.com/api/clients/aaf026921707f5d5/cds-server-metadata",
}
```

### 11.4. Client List <a id="example-client-list" href="#example-client-list" class="permalink">🔗</a>

The following is a non-normative example of a Client loading their list of Client objects via the [Clients API](#clients-api).

```
==Request==
GET /api/clients HTTP/1.1
Host: example.com
Authorizatin: Bearer vjzia9aP-os_rw-bPvMe--uIniUWdmGmXtHH7XaVbTM_KS8eBYCp7IWyoNDC1KCc7DtkVm8fKYIBaOja_08xEQ

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "clients": [
        {
            "client_id": "aaf026921707f5d5",
            "client_id_issued_at": 2893256800,
            "scope": "client_admin",
            "redirect_uris": [],
            "response_types": [],
            "grant_types": ["client_credentials"],
            "token_endpoint_auth_method": ["client_secret_basic"],
            "client_name": "My App Name",
            "cds_company_name": "My Company Name",
            "authorization_details_types": ["client_admin"],
            "cds_created": "2022-01-01T00:00:00Z",
            "cds_modified": "2022-01-01T00:00:00Z",
            "cds_client_uri": "https://example.com/api/clients/aaf026921707f5d5",
            "cds_status": "production",
            "cds_status_options": ["production"],
            "cds_server_metadata": "https://example.com/api/clients/aaf026921707f5d5/cds-server-metadata",
        },
        {
            "client_id": "22bb40b5b823fa8c",
            "client_id_issued_at": 2893256800,
            "scope": "grant_admin",
            "redirect_uris": [],
            "response_types": [],
            "grant_types": ["client_credentials"],
            "token_endpoint_auth_method": ["client_secret_basic"],
            "authorization_details_types": ["grant_admin"],
            "cds_created": "2022-01-01T00:00:00Z",
            "cds_modified": "2022-01-01T00:00:00Z",
            "cds_client_uri": "https://example.com/api/clients/22bb40b5b823fa8c",
            "cds_status": "production",
            "cds_status_options": ["production"],
            "cds_server_metadata": "https://example.com/api/clients/22bb40b5b823fa8c/cds-server-metadata",
        },
        {
            "client_id": "af653d57fa364da5",
            "client_id_issued_at": 2893256800,
            "scope": "example_custom",
            "redirect_uris": ["https://example.com/oauth/default-redirect"],
            "response_types": ["code"],
            "grant_types": ["client_credentials"],
            "token_endpoint_auth_method": ["client_secret_basic"],
            "authorization_details_types": ["example_custom"],
            "cds_created": "2022-01-01T00:00:00Z",
            "cds_modified": "2022-01-01T00:00:00Z",
            "cds_client_uri": "https://example.com/api/clients/af653d57fa364da5",
            "cds_status": "sandbox",
            "cds_status_options": ["sandbox", "disabled"],
            "cds_server_metadata": "https://example.com/api/clients/af653d57fa364da5/cds-server-metadata",
        }
    ],
    "next": null,
    "previous": null
}
```

### 11.5. Creating a Message <a id="example-message-create" href="#example-message-create" class="permalink">🔗</a>

The following is a non-normative example of a Client creating a Message via the [Messages API](#messages-api).

```
==Request==
POST /api/messages HTTP/1.1
Host: example.com
Authorizatin: Bearer vjzia9aP-os_rw-bPvMe--uIniUWdmGmXtHH7XaVbTM_KS8eBYCp7IWyoNDC1KCc7DtkVm8fKYIBaOja_08xEQ

{
    "type": "private_message",
    "name": "My Subject",
    "description": "Hello World!"
}


==Response==
HTTP/1.1 201 Created
Content-Type: application/json;charset=UTF-8

{
    "message_id": "00d1852055088ae7",
    "uri": "https://example.com/api/messages/00d1852055088ae7",
    "previous_uri": null,
    "type": "private_message",
    "read": true,
    "creator": "aaf026921707f5d5",
    "created": "2022-01-01T00:00:00Z",
    "modified": "2022-01-01T00:00:00Z",
    "status": "complete",
    "name": "My Subject",
    "description": "Hello World!"
}
```

## 12. Security Considerations <a id="security" href="#security" class="permalink">🔗</a>

This specification describes a protocol by which a utility or other central entity (a Server) can allow external entities (Clients) to register and obtain privileged access to the Server's offered capabilities and data.
Because the functionality described in this specification enables access to private Server functionality and data, Servers MUST follow industry cybersecurity best practices when securing their implementations of this specification to prevent unintended or inadvertent access to privileged functionality or data to Clients who are not authorized.
These best practices include requiring [HTTPS](#https) for API endpoints using the latest widely adopted encryption standards, undergoing regular security audits and penetration tests, and internally requiring security-focused process controls and data handling procedures.

### 12.1. Scopes and Client Management <a id="scopes-client-management" href="#scopes-client-management" class="permalink">🔗</a>

Because legal and regulatory requirements are highly diverse across the energy sector, Servers MUST be responsible for only offering scopes allowed by their jurisdictions.

Servers MUST include the appropriate `registration_requirements` values in their [Scope Descriptions](#scope-descriptions-format) for each scope's use case and capabilities to ensure that Clients submit all required disclosures (e.g. contact information) and be appropriately informed about any required steps (e.g. manual review) or payments (e.g. registration fees).
Servers MUST NOT impose overly burdensome registration requirements beyond what is deemed necessary by the Server's jurisdiction requirements for the type of capabilities or data made available by an offered scope.

Severs MUST be responsible for appropriately monitoring and reviewing the use of registered Clients as necessary for their legal and regulatory jurisdictions.

### 12.2. Restricted Access <a id="restricted-access" href="#restricted-access" class="permalink">🔗</a>

For unauthenticated endpoints ([Authorization Server Metadata](#auth-server-metadata), [Client Registration Process](#client-registration-process)), while Servers can add [rate limiting](#rate-limiting) configurations to protect their systems from being overwhelmed with requests, Servers MUST NOT add anti-bot blocking measures (e.g. captchas) that prevent automated requests from other systems.
The functionality described in this specification is intended to be able to be integrated in other platforms to allow those platforms to automate interactions with Servers on their users' behalf.

If [Authorization Server Metadata](#auth-server-metadata) is referenced in a public CDS Server Metadata Endpoint [[CDS-WG1-01 Section 3](#ref-cds-wg1-01-metadata-endpoint)], then the [Authorization Server Metadata](#auth-server-metadata) and [Client Registration Process](#client-registration-process) must also be public.

Servers that wish to restrict access of by-default unauthenticated endpoints to certain Clients MUST configure well established authentication processes for Clients to ensure that only the approved Clients may access the restricted endpoint.
This specification does not describe specifically how Servers will authenticate Clients for by-default unauthenticated endpoints, as these restricted access protocols are context dependent.
For example, if a Server providing a private Client Registration endpoint as part of an existing logged in portal, then they can use that logged in portal's session cookie to authenticate Client requests to the registration endpoint.

For authenticated endpoints ([Clients API](#clients-api), [Messages API](#messages-api), [Credentials API](#credentials-api), [Grants API](#grants-api), [Server-Provided Files API](#server-provided-files-api)), Servers MUST authenticate requests using OAuth's Authorization Request Header Field [[RFC 6750 Section 2.1](#ref-rfc6750-auth-header)] with access tokens obtained using the OAuth 2.0's Issuing an Access Token process [[RFC 6749 Section 5](#ref-rfc6749-access-tokens)].

### 12.3. Rate Limiting <a id="rate-limiting" href="#rate-limiting" class="permalink">🔗</a>

For unauthenticated endpoints ([Authorization Server Metadata](#auth-server-metadata), [Client Registration Process](#client-registration-process)), Servers SHOULD configure rate limiting restrictions so that bots and misconfigured scripts will not flood and overwhelm the endpoints with requests, while still allowing legitimate and low-volume automated requests have access to the endpoints.

For authenticated endpoints ([Clients API](#clients-api), [Messages API](#messages-api), [Credentials API](#credentials-api), [Grants API](#grants-api), [Server-Provided Files API](#server-provided-files-api)), Servers SHOULD configure rate limiting by Client and Credential to ensure that individual Clients do not overwhelm Servers with authenticated API requests.
Additionally, Servers SHOULD configure rate limiting for unauthenticated or failed authentication requests to authenticated API endpoints to prevent brute force attempts to gain access to authenticated APIs.

## 13. References <a id="references" href="#references" class="permalink">🔗</a>

<a id="ref-cds-wg1-01" href="#ref-cds-wg1-01" class="permalink">🔗</a>
`CDS-WG1-01` - "Server Metadata", CDS-WG1-01, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-01/](https://cds-registration.lfenergy.org/specs/cds-wg1-01/)

<a id="ref-cds-wg1-01-metadata-endpoint" href="#ref-cds-wg1-01-metadata-endpoint" class="permalink">🔗</a>
`CDS-WG1-01 Section 3` - Section 3. Metadata Endpoint, "Server Metadata", CDS-WG1-01, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-01/#metadata-endpoint](https://cds-registration.lfenergy.org/specs/cds-wg1-01/#metadata-endpoint)

<a id="ref-cds-wg1-01-metadata-object" href="#ref-cds-wg1-01-metadata-object" class="permalink">🔗</a>
`CDS-WG1-01 Section 3.2` - Section 3.2. Metadata Object Format, "Server Metadata", CDS-WG1-01, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-01/#metadata-object-format](https://cds-registration.lfenergy.org/specs/cds-wg1-01/#metadata-object-format)

<a id="ref-cds-wg1-01-server-capabilities" href="#ref-cds-wg1-01-server-capabilities" class="permalink">🔗</a>
`CDS-WG1-01 Section 3.4` - Section 3.4. Server Capabilities, "Server Metadata", CDS-WG1-01, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-01/#server-capabilities](https://cds-registration.lfenergy.org/specs/cds-wg1-01/#server-capabilities)

<a id="ref-cds-wg1-01-coverage-entry" href="#ref-cds-wg1-01-coverage-entry" class="permalink">🔗</a>
`CDS-WG1-01 Section 4.3` - Section 4.3. Coverage Entry Format, "Server Metadata", CDS-WG1-01, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-01/#coverage-entry-format](https://cds-registration.lfenergy.org/specs/cds-wg1-01/#coverage-entry-format)

<a id="ref-rfc9110-https" href="#ref-rfc9110-https" class="permalink">🔗</a>
`RFC 9110 Section 4.2.2` - Section 4.2.2. https URI Scheme, "HTTP Semantics", RFC 9110, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9110#section-4.2.2](https://www.rfc-editor.org/rfc/rfc9110#section-4.2.2)

<a id="ref-rfc9110-methods" href="#ref-rfc9110-methods" class="permalink">🔗</a>
`RFC 9110 Section 9` - Section 9. Methods, "HTTP Semantics", RFC 9110, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9110#section-9](https://www.rfc-editor.org/rfc/rfc9110#section-9)

<a id="ref-rfc9110-codes" href="#ref-rfc9110-codes" class="permalink">🔗</a>
`RFC 9110 Section 15` - Section 15. Status Codes, "HTTP Semantics", RFC 9110, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9110#section-15](https://www.rfc-editor.org/rfc/rfc9110#section-15)

<a id="ref-rfc5789" href="#ref-rfc5789" class="permalink">🔗</a>
`RFC 5789` - "PATCH Method for HTTP", RFC 5789, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc5789](https://www.rfc-editor.org/rfc/rfc5789)

<a id="ref-rfc4648" href="#ref-rfc4648" class="permalink">🔗</a>
`RFC 4648` - "The Base16, Base32, and Base64 Data Encodings", RFC 4648, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc4648](https://www.rfc-editor.org/rfc/rfc4648)

<a id="ref-rfc8259" href="#ref-rfc8259" class="permalink">🔗</a>
`RFC 8259` - "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259](https://www.rfc-editor.org/rfc/rfc8259)

<a id="ref-rfc8259-values" href="#ref-rfc8259-values" class="permalink">🔗</a>
`RFC 8259 Section 3` - Section 3. Values, "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259#section-3](https://www.rfc-editor.org/rfc/rfc8259#section-3)

<a id="ref-rfc8259-objects" href="#ref-rfc8259-objects" class="permalink">🔗</a>
`RFC 8259 Section 4` - Section 4. Objects, "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259#section-4](https://www.rfc-editor.org/rfc/rfc8259#section-4)

<a id="ref-rfc8259-arrays" href="#ref-rfc8259-arrays" class="permalink">🔗</a>
`RFC 8259 Section 5` - Section 5. Arrays, "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259#section-5](https://www.rfc-editor.org/rfc/rfc8259#section-5)

<a id="ref-rfc8259-numbers" href="#ref-rfc8259-numbers" class="permalink">🔗</a>
`RFC 8259 Section 6` - Section 6. Numbers, "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259#section-6](https://www.rfc-editor.org/rfc/rfc8259#section-6)

<a id="ref-rfc8259-strings" href="#ref-rfc8259-strings" class="permalink">🔗</a>
`RFC 8259 Section 7` - Section 7. Strings, "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259#section-7](https://www.rfc-editor.org/rfc/rfc8259#section-7)

<a id="ref-rfc3339-datetime" href="#ref-rfc3339-datetime" class="permalink">🔗</a>
`RFC 3339 Section 5.6` - Section 5.6. Internet Date/Time Format, "Date and Time on the Internet: Timestamps", RFC 3339, Internet Engineering Task Force (IETF),  
[https://datatracker.ietf.org/doc/html/rfc3339#section-5.6](https://datatracker.ietf.org/doc/html/rfc3339#section-5.6)

<a id="ref-rfc3339-duration" href="#ref-rfc3339-duration" class="permalink">🔗</a>
`RFC 3339 Appendix A` - Appendix A. ISO 8601 Collected ABNF, "Date and Time on the Internet: Timestamps", RFC 3339, Internet Engineering Task Force (IETF),  
[https://datatracker.ietf.org/doc/html/rfc3339#appendix-A](https://datatracker.ietf.org/doc/html/rfc3339#appendix-A)

<a id="ref-rfc3986-url" href="#ref-rfc3986-url" class="permalink">🔗</a>
`RFC 3986 Section 1.1.3` - Section 1.1.3. URI, URL, and URN, "Uniform Resource Identifier (URI): Generic Syntax", RFC 3986, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc3986.html#section-1.1.3](https://www.rfc-editor.org/rfc/rfc3986.html#section-1.1.3)

<a id="ref-rfc7517-pk" href="#ref-rfc7517-pk" class="permalink">🔗</a>
`RFC 7517 Section 4` - Section 4. JSON Web Key (JWK) Format, "JSON Web Key (JWK)", RFC 7517, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7517.html#section-4](https://www.rfc-editor.org/rfc/rfc7517.html#section-4)

<a id="ref-rfc6838" href="#ref-rfc6838" class="permalink">🔗</a>
`RFC 6838` - "Media Type Specifications and Registration Procedures", RFC 6838, Internet Engineering Task Force (IETF),  
[https://datatracker.ietf.org/doc/html/rfc6838](https://datatracker.ietf.org/doc/html/rfc6838)

<a id="ref-bcp14" href="#ref-bcp14" class="permalink">🔗</a>
`BCP 14` - "Best Current Practice 14", Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/info/bcp14](https://www.rfc-editor.org/info/bcp14)

<a id="ref-rfc6749" href="#ref-rfc6749" class="permalink">🔗</a>
`RFC 6749` - "The OAuth 2.0 Authorization Framework", RFC 6749, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6749](https://www.rfc-editor.org/rfc/rfc6749)

<a id="ref-rfc6749-token-endpoint" href="#ref-rfc6749-token-endpoint" class="permalink">🔗</a>
`RFC 6749 Section 3.2` - Section 3.2: Token Endpoint, "The OAuth 2.0 Authorization Framework", RFC 6749, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6749#section-3.2](https://www.rfc-editor.org/rfc/rfc6749#section-3.2)

<a id="ref-rfc6749-code-grant" href="#ref-rfc6749-code-grant" class="permalink">🔗</a>
`RFC 6749 Section 4.1` - Section 4.1. Authorization Code Grant, "The OAuth 2.0 Authorization Framework", RFC 6749, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6749#section-4.1](https://www.rfc-editor.org/rfc/rfc6749#section-4.1)

<a id="ref-rfc6749-auth-request" href="#ref-rfc6749-auth-request" class="permalink">🔗</a>
`RFC 6749 Section 4.1.1` - Section 4.1.1. Authorization Request, "The OAuth 2.0 Authorization Framework", RFC 6749, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6749#section-4.1.1](https://www.rfc-editor.org/rfc/rfc6749#section-4.1.1)

<a id="ref-rfc6749-token-request" href="#ref-rfc6749-token-request" class="permalink">🔗</a>
`RFC 6749 Section 4.1.3` - Section 4.1.3. Access Token Request, "The OAuth 2.0 Authorization Framework", RFC 6749, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6749#section-4.1.3](https://www.rfc-editor.org/rfc/rfc6749#section-4.1.3)

<a id="ref-rfc6749-client-credentials" href="#ref-rfc6749-client-credentials" class="permalink">🔗</a>
`RFC 6749 Section 4.4` - Section 4.4. Client Credentials Grant, "The OAuth 2.0 Authorization Framework", RFC 6749, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6749#section-4.4](https://www.rfc-editor.org/rfc/rfc6749#section-4.4)

<a id="ref-rfc6749-access-tokens" href="#ref-rfc6749-access-tokens" class="permalink">🔗</a>
`RFC 6749 Section 5` - Section 5. Issuing an Access Token, "The OAuth 2.0 Authorization Framework", RFC 6749, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6749#section-5](https://www.rfc-editor.org/rfc/rfc6749#section-5)

<a id="ref-rfc6750-auth-header" href="#ref-rfc6750-auth-header" class="permalink">🔗</a>
`RFC 6750 Section 2.1` - Section 2.1. Authorization Request Header Field, "The OAuth 2.0 Authorization Framework: Bearer Token Usage", RFC 6750, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6750#section-2.1](https://www.rfc-editor.org/rfc/rfc6750#section-2.1)

<a id="ref-rfc7591" href="#ref-rfc7591" class="permalink">🔗</a>
`RFC 7591` - "OAuth 2.0 Dynamic Client Registration Protocol", RFC 7591, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7591](https://www.rfc-editor.org/rfc/rfc7591)

<a id="ref-rfc7591-client-metadata" href="#ref-rfc7591-client-metadata" class="permalink">🔗</a>
`RFC 7591 Section 2` - Section 2. Client Metadata, "OAuth 2.0 Dynamic Client Registration Protocol", RFC 7591, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7591#section-2](https://www.rfc-editor.org/rfc/rfc7591#section-2)

<a id="ref-rfc7591-client-reg-request" href="#ref-rfc7591-client-reg-request" class="permalink">🔗</a>
`RFC 7591 Section 3.1` - Section 3.1. Client Registration Request, "OAuth 2.0 Dynamic Client Registration Protocol", RFC 7591, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7591#section-3.1](https://www.rfc-editor.org/rfc/rfc7591#section-3.1)

<a id="ref-rfc7591-client-reg-resp" href="#ref-rfc7591-client-reg-resp" class="permalink">🔗</a>
`RFC 7591 Section 3.2` - Section 3.2. Responses, "OAuth 2.0 Dynamic Client Registration Protocol", RFC 7591, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7591#section-3.2](https://www.rfc-editor.org/rfc/rfc7591#section-3.2)

<a id="ref-rfc7591-client-info-resp" href="#ref-rfc7591-client-info-resp" class="permalink">🔗</a>
`RFC 7591 Section 3.2.1` - Section 3.2.1. Client Information Response, "OAuth 2.0 Dynamic Client Registration Protocol", RFC 7591, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7591#section-3.2.1](https://www.rfc-editor.org/rfc/rfc7591#section-3.2.1)

<a id="ref-rfc8414-server-metadata" href="#ref-rfc8414-server-metadata" class="permalink">🔗</a>
`RFC 8414` - "OAuth 2.0 Authorization Server Metadata", RFC 8414, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8414](https://www.rfc-editor.org/rfc/rfc8414)

<a id="ref-rfc8414-server-metadata-obj" href="#ref-rfc8414-server-metadata-obj" class="permalink">🔗</a>
`RFC 8414 Section 2` - Section 2. Authorization Server Metadata, "OAuth 2.0 Authorization Server Metadata", RFC 8414, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8414#section-2](https://www.rfc-editor.org/rfc/rfc8414#section-2)

<a id="ref-rfc8414-server-metadata-req" href="#ref-rfc8414-server-metadata-req" class="permalink">🔗</a>
`RFC 8414 Section 3` - Section 3. Obtaining Authorization Server Metadata, "OAuth 2.0 Authorization Server Metadata", RFC 8414, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8414#section-3](https://www.rfc-editor.org/rfc/rfc8414#section-3)

<a id="ref-rfc8414-server-metadata-wellknown" href="#ref-rfc8414-server-metadata-wellknown" class="permalink">🔗</a>
`RFC 8414 Section 7.3` - Section 7.3. Well-Known URI Registry, "OAuth 2.0 Authorization Server Metadata", RFC 8414, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8414#section-7.3](https://www.rfc-editor.org/rfc/rfc8414#section-7.3)

<a id="ref-rfc7009" href="#ref-rfc7009" class="permalink">🔗</a>
`RFC 7009` - "OAuth 2.0 Token Revocation", RFC 7009, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7009](https://www.rfc-editor.org/rfc/rfc7009)

<a id="ref-rfc9396" href="#ref-rfc9396" class="permalink">🔗</a>
`RFC 9396` - "OAuth 2.0 Rich Authorization Requests", RFC 9396, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9396](https://www.rfc-editor.org/rfc/rfc9396)

<a id="ref-rfc9396-auth-details" href="#ref-rfc9396-auth-details" class="permalink">🔗</a>
`RFC 9396 Section 7.1` - Section 7.1. Enriched Authorization Details in Token Response, "OAuth 2.0 Rich Authorization Requests", RFC 9396, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9396#section-7.1](https://www.rfc-editor.org/rfc/rfc9396#section-7.1)

<a id="ref-rfc9396-rar-metadata" href="#ref-rfc9396-rar-metadata" class="permalink">🔗</a>
`RFC 9396 Section 10` - Section 10. Metadata, "OAuth 2.0 Rich Authorization Requests", RFC 9396, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9396#section-10](https://www.rfc-editor.org/rfc/rfc9396#section-10)

<a id="ref-rfc7592" href="#ref-rfc7592" class="permalink">🔗</a>
`RFC 7592` - "OAuth 2.0 Dynamic Client Registration Management Protocol", RFC 7592, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7592](https://www.rfc-editor.org/rfc/rfc7592)

<a id="ref-rfc7592-client-updates" href="#ref-rfc7592-client-updates" class="permalink">🔗</a>
`RFC 7592 Section 2.2` - Section 2.2. Client Update Request, "OAuth 2.0 Dynamic Client Registration Management Protocol", RFC 7592, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7592#section-2.2](https://www.rfc-editor.org/rfc/rfc7592#section-2.2)

<a id="ref-rfc7592-client-metadata" href="#ref-rfc7592-client-metadata" class="permalink">🔗</a>
`RFC 7592 Section 3` - Section 3. Client Information Response, "OAuth 2.0 Dynamic Client Registration Management Protocol", RFC 7592, Internet Engineering Task Force (IETF),
[https://www.rfc-editor.org/rfc/rfc7592#section-3](https://www.rfc-editor.org/rfc/rfc7592#section-3)

<a id="ref-rfc7662" href="#ref-rfc7662" class="permalink">🔗</a>
`RFC 7662` - "OAuth 2.0 Token Introspection", RFC 7662, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7662](https://www.rfc-editor.org/rfc/rfc7662)

<a id="ref-rfc9126" href="#ref-oauth2-par" class="permalink">🔗</a>
`RFC 9126` - "OAuth 2.0 Pushed Authorization Requests", RFC 9126, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9126](https://www.rfc-editor.org/rfc/rfc9126)

<a id="ref-rfc7636" href="#ref-rfc7636" class="permalink">🔗</a>
`RFC 7636` - "Proof Key for Code Exchange by OAuth Public Clients", RFC 7636, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc7636](https://www.rfc-editor.org/rfc/rfc7636)

<a id="ref-iso4217" href="#ref-iso4217" class="permalink">🔗</a>
`ISO 4217` - "Currency Codes", ISO 4217, International Organization for Standardization (ISO),  
[https://www.iso.org/iso-4217-currency-codes.html](https://www.iso.org/iso-4217-currency-codes.html)

## 14. Acknowledgments <a id="acknowledgments" href="#acknowledgments" class="permalink">🔗</a>

The authors would like to thank the late Shuli Goodman, who was the Executive Director of LFEnergy, for her incredible leadership in initially organizing the CDS.

## 15. Authors <a id="authors" href="#authors" class="permalink">🔗</a>

Daniel Roesler (Primary Author, Working Group Maintainer)  
Daniel Roesler LLC  
Email: [daniel@danielroesler.com](mailto:daniel@danielroesler.com)  
URI: [https://danielroesler.com/](https://danielroesler.com/)

Eloi Fábrega Ferrer (Working Group Contributor)  
Flexidao  
Email: [e.ferrer@flexidao.com](mailto:e.ferrer@flexidao.com)  
URI: [https://flexidao.com/](https://flexidao.com/)

Soazig Kaam (Working Group Contributor)  
Google  
Email: [soazig@google.com](mailto:soazig@google.com)  
URI: [https://google.com/](https://google.com/)

Jordan Hughes (Working Group Contributor)  
Apple  
Email: [jhughes22@apple.com](mailto:jhughes22@apple.com)  
URI: [https://apple.com/](https://apple.com/)

Antara Sargam and Po Ki Chui (Reviewers)  
Google

Dionna Amalie Glaze (Reviewer)  
Apple
