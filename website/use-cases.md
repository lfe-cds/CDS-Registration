---
layout: base
nav: use_cases
title: Use Cases
meta_description: What are the use cases that the CDS Registration specifications this working group is trying to address?
---
[Home]({{ "/" | relative_url }}) / Use Cases

# Use Cases

These are the use cases that this working group is writing specifications to address.

## Use Case 1: Enabling Initial Configuration for Grid Communication Protocols <a id="initial-configuration" href="#initial-configuration" class="permalink">🔗</a>

As the energy grid evolves and energy transition progresses, many utilities and other central entities are increasingly needing to provide secure, confidential, digital, and automated communications with vendors, external entities, and customers.
These communications can include protocols defined by industry specification (e.g. OpenADR, IEEE 1547, etc.) or using proprietary or custom Application Programming Interface (API) protocols (e.g. Oracle, SAP, Mulesoft, etc.).

These secure, confidential connections frequently depend on initially exchanging configuration details between the utility's server and the end client user's systems.
However, currently these configuration details exchanges (including sharing secrets and credentials with client users) are primarily done manually using unspecified ad-hoc delivery methods (e.g. secure email), which significantly limits the capacity for utilities and other central entities to establish connectivity with external entities.
This initial manual configuration bottleneck effectively caps the adoption of grid programs and communications that rely on external connectivity.

Therefore, to enable and unblock scalable adoption of grid communication specifications and protocols, this working group must write open, freely available general connectivity specifications that provide other specifications, vendors, utilities, and other central entities with protocols that provide automated methods for initially establishing secure connectivity with external third party clients.

#### Needs:

* **Structured Disclosure of Capabilities:**
Because the energy grid is highly fragmented, different utilities and other central entities offer widely varying sets of communication protocols to external entities and customers, and differentiating which utilities and other central entities offer what types of connectivity is a highly time-intensive manual task.
These specifications must prescribe protocols by which external entities and customers may automatically discover and parse which capabilities utilities and other central entities offer.

* **Automated Client Registration:**
Because many grid communication specifications and protocols require establishing a secure, confidential connection between the utility systems ("Servers") and external systems ("Clients"), these specifications must prescribe protocols by which Servers can receive requests by Clients to register and initially exchange connection details (e.g. client credentials, endpoint URLs, etc.).

#### Examples of users in this use case:

* Other specifications and protocols that require secure, confidential connectivity ([CDS Customer Data](https://cds-customerdata.lfenergy.org/), [CDS Power Systems Data](https://cds-powersystemdata.lfenergy.org/), [LF Energy TROLIE](https://lfenergy.org/projects/trolie/), [Super Advanced Meter](https://github.com/super-advanced-meter), [OpenADR](https://www.openadr.org/), [IEEE 1547](https://ieeexplore.ieee.org/document/8332112), etc.)

* Energy programs and markets that require secure communication with external entities ([FERC 2222](https://www.ferc.gov/ferc-order-no-2222-explainer-facilitating-participation-electricity-markets-distributed-energy), [CEC Load Management Standards](https://www.energy.ca.gov/programs-and-topics/topics/load-flexibility/load-management-standards), etc.)

* Utilities that need to provide customers with digital, automated access to their own data ([Building Benchmarking](https://www.energystar.gov/buildings/benchmark), [Carbon Accounting](https://en.wikipedia.org/wiki/Carbon_accounting), [SEC Climate-Related Disclosures](https://www.federalregister.gov/documents/2024/03/28/2024-05137/the-enhancement-and-standardization-of-climate-related-disclosures-for-investors), etc.)

* Utilities and other central entities that need to setup secure connectivity with vendors for internal data transfers (consultants, data analysts, suppliers/EDI, program implementers, etc.)

## Use Case 2: Enabling Configuration Management for Grid Communication Protocols <a id="configuration-management" href="#configuration-management" class="permalink">🔗</a>

In addition to [Use Case #1](#initial-configuration), where utilities and other central entities need to be able to initially securely share confidential configuration details with vendors, external entities, and customer to establish connectivity, they also frequently need to provide a means of managing the connectivity configuration on an ongoing basis (e.g. rotating secrets, sending secure messages, notifying of outages, etc.).

Currently, these configuration management tasks are typically manually performed via ad-hoc emails or non-standardized dashboard interfaces for client users.
These current methods are not scalable and effectively cap the capacity for both central entities and external entities to maintain connectivity with many other entities.

Therefore, to enable and unblock scalable adoption of grid communication specifications and protocols, this working group must write open, freely available general connectivity specifications that provide other specifications, vendors, utilities, and other central entities with protocols that provide automated methods for maintaining their previously established secure connectivity with external third party clients.

#### Needs:

* **Use Case 1 Needs:**
In order to be able to manage client credentials, configuration, and communication, this use case depends on the ability for Servers and Clients to initially establish secure connectivity, the needs of which are described in [Use Case #1](#initial-configuration).

* **Client Configuration Management:**
After initially establishing secure connectivity between Servers and Clients, the configuration details for that Client registration may change (e.g. API endpoints, enabled scopes of access, webhook URLs, etc.).
These specifications must prescribe APIs by which Clients may retrieve configurations details about their registration and connection with Servers, as well as be able to modify field that they are permitted to modify.

* **Client Secrets Management:**
Cybersecurity best practices require that Client API credentials (e.g. `client_secret`, TLS certificates, API tokens, etc.) be rotated periodically.
To enable automated rotation for client secrets and credentials, these specifications must prescribe APIs by which Clients can retrieve their currently configured credentials, as well as adding of newly generated secrets and removing obsolete secrets.

* **Client Messaging Protocols:**
As part of many grid specifications and protocols, Servers and Clients frequently need to send automated and manual messages to each other for notifications and other communications about the status of their systems, alert of system changes, ask questions, or get technical support.
These ad-hoc or "out-of-band" communications have traditionally been via email, which because they are not standardized requires significant resources by both Server and Client to process.
To alleviate this unstandardized communication processing overhead, these specifications must prescribe APIs by which Servers and Clients and send secure messages to each other for transactional and ad-hoc communication.

#### Examples of users in this use case:

* Examples for this use case are the same as [Use Case #1](#initial-configuration).

## Use cases NOT in scope for this working group <a id="not-in-scope" href="#not-in-scope" class="permalink">🔗</a>

* Specifying communication protocols for specific grid communication use cases (e.g. solar curtailment protocols, energy market bidding protocols, customer data access protocols, etc.).

* Aggregation of utility and other central entity capabilities into a directory or hub for external entities to browse (e.g. search engines, directory services, etc.).

* Operating energy marketplaces, grid programs, or other grid services (e.g. ISO energy markets, rebate programs, statewide incentive programs).

