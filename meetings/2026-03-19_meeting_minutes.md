# CDS WG1 (Registration) Meeting 2026-03-19

Recording: https://zoom.us/rec/share/3Fe2dcrYKsy4HqO1_XiXFjZ556vZNzr_3MFk3gl76c8-sRFStQBGLfUIVASbXirk.MUQu6d0-vqrGawif

## Agenda
* Welcome
* Review: adding `choice_list` and `string_list` as format options to registration and authorization details fields
    * Issue: https://github.com/lfe-cds/CDS-Registration/issues/15
    * Pull Request: https://github.com/lfe-cds/CDS-Registration/pull/16

## Attendees
* Daniel Roesler (Maintainer)
* Don Coffin (GBA)

## Minutes
* Pull Request #16 review

```
# metadata
GET /.well-known/oauth-authorization-server
{
    ...
    "cds_scope_descriptions": {
        "contract_list": {
            ...
            "id": "contract_list",
            ...
            "authorization_details_fields_supported": [
                {
                    "id": "service_types",
                    ...
                    "format": "choice_list_or_null",
                    "is_required": false,
                    "default": null,
                    "choices": [
                        {
                            "id": "electric",
                            "name": "Electric service",
                            "description": "These are electric services to a building or location...",
                            "documentation": "https://..."
                        },
                        {
                            "id": "gas",
                            "name": "Natural gas service",
                            "description": "These are gas services to a building or location...",
                            "documentation": "https://..."
                        },
                        ...
                    ]
                }

            ]
            ...
        },
        ...
    },
    ...
}

# authorization request
GET /oauth/authorize
    ?response_type=code
    &client_id=111111111111
    &scope=contract_list
    &authorization_details=[
        {
            "type": "contract_list",
            "service_types": ["electric"],
        }
    ]
```

* Pull Request #17 review
    * [Don] Recommend leaving the website redirects and removing the spec markdowns.

* For next time:
    * start working on OpenAPI spec (https://github.com/lfe-cds/CDS-Registration/issues/12)


## Closing Discussion
* Consensus to commit this to repo? Yes/No

