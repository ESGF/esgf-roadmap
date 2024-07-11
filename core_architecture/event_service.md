# Event Service
**Rachana Ananthakrishnan, Steve Turoscy**

Federation wide event stream to manage distributed index and replication, with potential to extend to use for other use cases. For AR7 timeline, the use case will be scoped to data publication.

## Architecture of service

## Publishing events 

- Registering as a producer
- Publishing events

## Registering a consumer

## Operational Details

## Open questions

- How long should the event stream be persisted?
  - To be determined by
    - SLA for consumers operated by the federation
    - Support required for external consumers
  - Suggested: 90 days
  - It was agreed that if a new full replica needs to be stood up, then it will use one of two federation operated indices, and then start to consume events from the stream.
  - Zach: _"Depending on the tech selected, there are options that can persist the actions "forever" cheaply. Blockchain for example can reconstruct the chain in reverse back to origin. This could be useful for disaster recovery/standing up new nodes/services"_
- SLAs for the index
  - Uptime
  - RTO
  - Ingest API rate limit
  - Query API..
  - ?
- SLAs for event stream
  - Uptime
  - RTO
  - Publish rate limit
  - …
- SLAs for ingest service
  - Uptime
  - RTO
  - API rate limit
  - …
