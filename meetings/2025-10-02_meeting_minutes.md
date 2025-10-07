# CDS WG1 (Registration) Meeting 2025-10-02

Recording: https://zoom.us/rec/share/2y4xUY1XTrWm7P-5orRF6PFNE5nWqTwLKY4JbvgjNWbiiJUtkS5Ccfi71FaZQq7e.PV4yONAX0fgPOCdS

## Agenda
* Welcome
* Discussion of updates to CDS-WG1-02

## Attendees
* Daniel Roesler (Maintainer)
* Don Coffin (GBA)
* Eloi Ferrer (Flexidao)
* Soazig Kaam (Google)

## Minutes
* [Daniel] Giving updates on the transition of Maintainer accounts (Complete)
    * Mainter's branch link: https://daniel-roesler.github.io/CDS-Registration/
    * Demo link: https://demo.cdsgateway.com/
* Pull request for CDS-WG1-02:
    * Will be closing the current pull request (#5)
    * Will be opening a new pull request for new Maintainer branch account
    * New branch for new pull request: https://github.com/daniel-roesler/CDS-Registration/tree/issue_3_client_registration
* Overview of changes made since transition:
    * Added clarification on how Servers must manage `grant_request` Messages
        * [Eloi] Why would a Server reject?
        * [Daniel] Maybe because a Server reviews the request and finds some value insufficient or incorrect.
    * Added "*_id" parameters to listing APIs
    * Message attachments
        * [Soazig] Currently a recommendation, worried about people ignoring the recommendation.
        * [Daniel] Definitely open to ideas on how to enforce limitations on Message attachments.
        * [Daniel] Perhaps if we strongly defined what "relatively small unstructured files" means, we could then say "Clients and Servers MUST only attach ..."
* [TODO] Create another example in the Overview showing how to use public clients
* Timeline for WG
    * October: new pull request and call for merging
    * November: any last edits and adding surrounding docs
    * November 20th: target merge date
    * December: updates to Demo for complete compliance with merged spec
    * End of year: have full drafts merged and fully working demo

## Closing Discussion
* Consensus to commit this to repo? Yes

