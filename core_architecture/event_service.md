# Event Service
**Rachana Ananthakrishnan, Steve Turoscy**

Federation wide event stream to manage distributed index and replication, with potential to extend to use for other use cases. For AR7 timeline, the use case will be scoped to data publication.

## Architecture of service

## Event producer registration
- Event producers MUST register with the ESGF event service and obtain credentials.
- The registration process is NOT self-service. It requires communication with the event service operators.
- For the forseeable future, it is expected that the only registered event producers will be the East and West ESGF Ingest services.

## Publishing events

## Event consumers
- Q: Is registration required?
- Q: What does the subscription interface look like?
- Q: What are expectations regarding event availability?

## Event topics
- Q: How are topics managed?
- Q: Is there a topic lifecycle?
- Q: How can I find the list of available topics?

## Availability, maintenance, and changes

We expect there will occasionally be a need to take the ESGF Event Service offline for maintenance and that it will occasionally be necessary to make changes to the service API and/or behavior. Downtime and changes will have impacts on WCRP modeling centers, ESGF Index services, and other event consumers in the ESGF community. The ESGF Event Service should meet the following expectations for availability and change management.

- All planned downtime and all API changes MUST be announced in advance.
- API behavior changes that affect all clients (example: a change to the available topics) MUST be announced in advance.
- Unplanned downtime lasting longer than 10 minutes SHOULD be announced as soon as the downtime is detected. Another announcement SHOULD follow when it is believed that service has been restored.
- The Event Service MUST provide an email list, to which all registered modeling centers are subscribed, where downtime announcements (both planned and unplanned) and API and API behavior change announcements are made.
- During planned downtimes, the Event Service API SHOULD provide a reasonable error response so clients can, in turn, log and/or display information about the downtime. Instances in which the Event Service cannot accept connections or cannot respond to requests at all SHOULD be extremely rare.
- The Event Service MUST provide a changelog accessible via web browser where all API and API behavior changes are recorded. The log should cover the entire service period of the Event Service.

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
