# Publication Events Consumer
**Steve Turoscy**

This document outlines the implementation plan for consumers of the events from the ESGF event stream service. The event types that will be published are documented in [Publication Actions](publication_actions.md). The events in scope update the federation wide search indices. It has been agreed that eventual consistency between the indices is the goal. 

Handling of each event type will be developed as a module to facilitate use of the processing code for multiple deployments of the ingest service. Native authentication for the event stream service will be used to authorize the consumers.

Globus Search has done research into STAC and has created some initial scripts for ingesting STAC items into a Globus Search index https://github.com/globusonline/sc-33660-stac-in-search 

### Open questions

- Decide on tolerance for drift between the two federation wide indices, given the eventual consistency agreement.
- For a new publication event, should we use CEDA’s STAC generator to ingest items into STAC? For information about CEDA's generator implementation, please see [here](https://docs.google.com/document/d/17T7wHPww8BIu-zJ71gPrk4wjYYeWJS2L/edit#heading=h.xbu61f5cchfg).
- Through the research that the Globus Search team has done, they have created some scripts to get STAC items from CEDA and place them in a Globus Search index https://github.com/globusonline/sc-33660-stac-in-search Should we modify and use their scripts for a Publish event?
