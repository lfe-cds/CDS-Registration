---
layout: base
nav: specs
title: CDS-WG1 - 01 Server Metadata - Overview
meta_description: Linux Foundation Energy - Connected Data Specifications (CDS) - Registration Working Group (WG1) - Specifications - cds-wg1-01 - Server Metadata - Overview
---
[Home]({{ "/" | relative_url }}) / [Specifications]({{ "/specs" | relative_url }}) / [`cds-wg1-01` - Server Metadata]({{ "/specs/cds-wg1-01" | relative_url }}) / Overview

# Server Metadata - Overview

This is a summary overview of [`cds-wg1-01`]({{ "/specs/cds-wg1-01" | relative_url }}), which describes how utility and other central entity servers ("Servers") provide a structured reference of information about them, the functionality they offer, and how other entities ("Clients") can interoperate with them.

**NOTE:** Utility and other central entity servers ("Servers") are not always run by utilities themselves.
They could be other types of energy companies, such as electric retailers or community choice aggregators (CCAs).
They can also be centralized entities such as state-run data warehouses or private infrastructure providers.
Throughout this overview, the term "utility or entity" is used instead of just "utility" to describe the organizations providing access to utility data and functionality.

## Background <a id="background" href="#background" class="permalink">🔗</a>

There are thousands of utilities serving customers across the world, and each has their own way of organizing and structuring data and functionality.
For [use case]({{ "/use-cases" | relative_url }}) companies needing to access data or functionality, an efficient means of discovering, registering, and interoperating with these utilities is needed.

## Metadata Endpoint <a id="metadata-endpoint" href="#metadata-endpoint" class="permalink">🔗</a>

Server metedata is provided under the [well-known url](https://datatracker.ietf.org/doc/html/rfc8615) `"/.well-known/cds-server-metadata.json`.
The response is a [structured object]({{ "/specs/cds-wg1-01" | relative_url }}#metadata-object-format) containing information about the server, what functionality it offers, and how to interoperate with it.

#### Metadata Example <a id="metadata-example" href="#metadata-example" class="permalink">🔗</a>
```
==Request==
GET /.well-known/cds-server-metadata.json HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "cds_metadata_version": "v1",
    "cds_metadata_url": "https://demoutility.com/.well-known/cds-server-metadata.json",

    "created": "2022-01-01T00:00:00.000000Z",
    "updated": "2022-06-01T00:00:00.000000Z",

    "name": "Demo Gas & Electric",
    "description": "A fictional utility that can be used for testing and demonstration purposes.",
    "website": "https://demoutility.com/data-access",
    "documentation": "https://demoutility.com/data-access/docs",
    "support": "https://demoutility.com/contact-us?type=developers",
    "capabilities": [
        "coverage",
        "oauth",
        "ieee1547"
    ],
    "related_metadata": ["https://static.demoutility.com/sister-company-cds.json"],

    "coverage": "https://static.demoutility.com/cds-coverage.json",

    "oauth_metadata": "https://demoutility.com/.well-known/oauth-authorization-server",

    "ieee1547_setup": "https://demoutility.com/smart-inverters/setup"
}
```

## Coverage Endpoint <a id="coverage-endpoint" href="#coverage-endpoint" class="permalink">🔗</a>

This specification also defines a `coverage` capability that adds the the [Coverage Endpoint]({{ "/specs/cds-wg1-01" | relative_url }}#coverage-endpoint) for Servers to offer a way to list the covered geographic territories or logical groupings for which they offer the listed capabilities in a structured format.

#### Coverage Example <a id="coverage-example" href="#coverage-example" class="permalink">🔗</a>
```
==Request==
GET /cds-coverage.json HTTP/1.1
Host: static.demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
    "coverage_entries": [
        {
            "id": "dge_elec_west",

            "created": "2022-06-01T00:00:00.000000Z",
            "updated": "2022-06-01T00:00:00.000000Z",

            "entity_name": "Demo Gas & Electric",
            "entity_abbreviation": "DG&E",

            "country": "US",
            "name": "Western electric service territory",
            "description": "This is the electric service coverage for the western half of the region.",

            "type": "geographic",
            "role": "official",
            "infrastructure_types": [
                "distribution_utility",
            ],
            "commodity_types": [
                "electricity",
            ],
            "capabilities": [
                "oauth",
                "ieee1547"
            ],

            "map_resource": "https://static.example.com/cds-coverage-map.png",
            "map_content_type": "image/png",
            "geojson_resource": "https://static.example.com/cds-coverage-map-geo.json"
        }
    ],
    "next": null,
    "previous": null
}
```

---

# Other Drafts <a id="other-drafts" href="#other-drafts" class="permalink">🔗</a>

* Maintainer's draft: [[Website](https://daniel-utilityapi.github.io/CDS-Registration/specs/cds-wg1-01/overview)] [[Code](https://github.com/daniel-utilityapi/CDS-Registration/blob/main/website/specs/cds-wg1-01/overview.md)]

