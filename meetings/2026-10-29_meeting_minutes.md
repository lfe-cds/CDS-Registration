# CDS WG1 (Registration) Meeting 2026-10-29

Recording: https://zoom.us/rec/share/ijIELPOhIDyVcPaxbQqIxPfNbYqKZ_Rhm4HR0v9jxnvL9kcYzuCbw-btgSbOgT8Q.yoW2WL7uboFzvUMX

## Agenda
* Welcome
* Revisit open issues and pull requests
* Invite code flow for registration
    * Going to be used for: https://caribbean.uenergyhub.org/hackathon-ii

## Attendees
* Daniel Roesler (Maintainer)
* Don Coffin (GBA)
* Dakshil Shah (Apple)
* Blade Chapman (Apple)

## Minutes
* Welcome
* Overview of open issues
* Overview of Canada and Trinidad and Tobago hackathon
* entity_number (github.com/lfe-cds/CDS-Registration/issues/21)


```
# https://cds-customerdata.lfenergy.org/specs/cds-wg3-01/dev#account-format

{
    "accounts": [
        {
            "cds_account_id": "111111111111",  # primary key for Account Obj
            ...
            "account_number": "{utility_acct_number}",
            "account_entity": {
                "id": "...", # defined by CDS server (e.g. primary key for Entity entry in db)
                "types": ["distributor"],
                "entity_number": "WEB_ALLOCATION_11111333",
                "entity_name": "Utility ABC",
                ...
            },
            ...
        },
        {
            "cds_account_id": "111111111111",  # primary key for Account Obj
            ...
            "account_number": "{cca_acct_number}",
            "account_entity": {
                "id": "...", # defined by CDS server (e.g. primary key for Entity entry in db)
                "types": ["supplier"],
                "entity_number": "WEB_ALLOCATION_1384848",
                "entity_name": "CCA XYZ",
                ...
            },
            ...
        },
    ]
}


# https://cds-registration.lfenergy.org/specs/cds-wg1-01/dev#coverage-endpoint

{
    "coverage_entries": [
        {
            "id": "33333393939dfiei",
            ...
            "entity_name": "Utility ABC",
            "entity_number": "WEB_ALLOCATION_11111333",
            ...
            "geojson_resource": "https://...",
        },
        {
            "id": "334o9lklekdefiei",
            ...
            "entity_name": "Utility ABC",
            "entity_number": "WEB_ALLOCATION_11111333",
            ...
            "geojson_resource": "https://...",
        },
        {
            "id": "330-=4949o3o3",
            ...
            "entity_name": "CCA XYZ",
            "entity_number": "WEB_ALLOCATION_1384848",
            ...
            "geojson_resource": "https://...",
        },
    ],
}
```


* If we were to add an Entity object to CDS-WG1-01 Server Metadata spec


```
GET /cds/entities

{
    "entities": [
        {
            "id": "...", # defined by CDS server (e.g. primary key for Entity entry in db)
            "types": ["distributor"],
            "entity_number": "WEB_ALLOCATION_11111333",
            "entity_name": "Utility ABC",
            ...
        },
        {
            "id": "...", # defined by CDS server (e.g. primary key for Entity entry in db)
            "types": ["supplier"],
            "entity_number": "WEB_ALLOCATION_1384848",
            "entity_name": "CCA XYZ",
            ...
        },
        ...
    ],
    ...
}

GET /cds/coverages
{
    "coverage_entries": [
        {
            ...
            "entity_id": "...",
            ...
        },
        ...
    ]
}
```

* Invite code flow for hackathon

```
# Invite code ABC123 --> Account + Usage Segments


0. GET /.well-known/oauth-authorization-server
{
    "cds_registration_fields": {
        "aaaaaaaaaaa": {
            "id": "aaaaaaaaaaa",
            "type": "registration_field",
            "field_name": "invite_code",
            "format": "string"
        }
    }
    "cds_scope_descriptions": {
        "hackathon_scope": {
            ...
            "type": "cds_query_usage",
            "registration_requirements": ["aaaaaaaaaaa"],
            ...
        }
    }
}

1. POST /oauth/register
{
    "contacts": ["mailto:studentteamA@example.com"],
    "invite_code": "ABC123",
}
Returns: {
    "client_id": "...",
    "client_secret": "...",
    ...
}


2. GET /cds/grants (with client admin access_token)
{
    "grants": [
        {
            "grant_id": "33333939393",
            "status": "pending",
            "scope": "hackathon_scope",
            ...
        },
    ],
}

3. Verify email via Messages API

4. GET /cds/grants (with client admin access_token)
{
    "grants": [
        {
            "grant_id": "33333939393",
            "status": "active",
            "scope": "hackathon_scope",
            ...
        },
    ],
}

5. GET /cds/usage-segments  (with customerdata access_token)
{
    "usage_segments": [
        {
            ... school's usage data
        },
    ],
}
```



## Closing Discussion
* Consensus to commit this to repo? Yes
