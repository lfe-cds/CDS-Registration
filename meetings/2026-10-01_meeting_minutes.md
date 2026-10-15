# CDS WG1 (Registration) Meeting 2026-10-01

Recording: https://zoom.us/rec/share/uA8TEbvL823EsJIhP9axKHXL9NYaJWSjghRIpIXIcl_Wjii-I6ggaHsp7gizegHe.Y0UvSg5IB1UKmsRv

## Agenda
* Welcome
* Debrief from LF Energy Summit and feedback
    * Talks:
        * https://lfenergysummiteu2026.sched.com/event/2PMbB/a-tour-of-lf-energys-connected-data-specifications-cds-daniel-roesler-drx-gateway-inc
        * https://lfenergysummiteu2026.sched.com/event/2PMZg/a-demo-of-lf-energys-cds-protocols-on-drx-gateway-daniel-roesler-drx-gateway-inc
    * Slides: https://hosted-files.sched.co/lfenergysummiteu2026/09/LF%20Energy%20Summit%20-%20CDS%20Tour.pdf
    * Video: https://www.youtube.com/watch?v=iH5X6K3jfiQ
* Take a look at the EU's Dataspaces
    * https://internationaldataspaces.org/offers/reference-architecture/
* Update issues with takeaways, changes from LF Energy Summit

## Attendees
* Daniel Roesler (Maintainer)
* Don Coffin (GBA)
* Blade Chapman (Apple)

## Minutes
* Welcome and agenda overview
* Overview of talks at LF Energy Summit
* Data Spaces project review
    * added Issue #26 to figure out compatibility with data spaces
* Review registration with national eID systems (common in EU) for registering clients.
    * Possible pathway: OAuth's software_statements method (https://www.rfc-editor.org/info/rfc7591/#section-2.3)
    * In CDS:
        * `registration_field` types (https://cds-registration.lfenergy.org/specs/cds-wg1-02/dev#registration-field-types)
            * `sso_verification`, etc.
* Like the idea of additional specs for compatibility with other specs (data spaces, eIDs, etc.)


## Closing Discussion
* Consensus to commit this to repo? Yes

