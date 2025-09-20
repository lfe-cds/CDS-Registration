# CDS WG1 (Registration) Meeting 2025-07-10

Recording: https://zoom.us/rec/share/cubAL4GCce5H5m-7-hmGJWLgcT6szEizdN8nfVXBHzykMwp1zCiWfhTgX8FJ01Pa.DMQVc_SMTNth2ZB2

## Agenda
* Welcome
* Final comments for Use Cases pull request and approval for merging
    * https://github.com/lfe-cds/CDS-Registration/pull/6
* Discussion of request to merge CDS-WG1-01 (DRAFT)
    * https://github.com/lfe-cds/CDS-Registration/pull/4
* Discussion of updates to CDS-WG1-02 (new message type of `grant_request`)

## Attendees
* Daniel Roesler (UtilityAPI) (Maintainer)
* Eloi Ferrer (Flexidao)
* Don Coffin (GBA)
* Jordan Hughes (Apple)

## Minutes
* Welcome
* Pull #6 review (use cases)
    * [Daniel] Went through overview of the 
    * [Jordan] sensible to me
    * [Don] no comments
    * [Daniel] Appears we have consensus with participants, so merging!
* Pull #4 (CDS-WG1-01, Server Metadata)
    * [Daniel] Overview of the spec
    * [Jordan] Should we specify the GeoJSON coordinate system? Maybe message saying we intend usage for WGS 84.
    * [Daniel] Final call for comments due by 2025-08-07
* Pull #5 (client registration)
    * [Daniel] Overview of new `grant_request` Message type and `grants_requested` field.
    * TODO: add requirement that when completed, Servers MUST create a response Message with `related_uri` to Client for the grants (i.e. `grants_list` related_type)
    * [Eloi] Intended to be used for one time or many?
    * TODO: add typical use case descriptions for when this should be used vs. the normal OAuth flow or client_credentials grant.
    * TODO: fill out the remaining TODOs, then mark for final comments

## Closing Discussion
* Consensus to commit this to repo? Yes

