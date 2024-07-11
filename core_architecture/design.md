## ESGF Core Architecture for Publication & Replication
Document outlines plans for next generation architecture for publication and replication in ESGF.

**This document is scoped to the capabilities in ESGF required for AR7 Fasttrack towards publication and management of index.** Specifically, this includes:

- Publication of new data
- Updates to published data
- Retraction of any published data
- Supporting data replication and publication with the federation

## Definitions
- Federation operated indices/ESGF (primary) index
  - At least two replicas, and can support additional replicas
  - Complete metadata for all data that will be available via ESGF
  - Agreed SLA for federated index operations
  - Projects: CMIP (more TBD)
- Tier 1 sites
  - ESGF-SC approved major international data centers around the world with long-term commitments to the ESGF
  - Publish data and make the data available in their system
  - Replicate data within the federation, and make that copy available to the federation. This is done by updating metadata in the ESGF index
- Data nodes
  - They publish data and make the data available in their system
  - They do not replicate data from other parts of the federation, and they do not republish into the ESGF index
- Broader community
  - Can discover and access data
  - Can host index of (sub)set of metadata

## Architecture Overview 
The diagram shows the components in the architecture towards providing scalable publication and management of metadata.

![An Architecture Diagram of the ESGF core which will consist of microservices that work together with a Federated Event Stream to replicate data and Publish, Retract, and Update metadata records for ESGF.](./diagrams/architecture_design.png "Architecture Diagram")

## Components
- Identity and Access management (IAM)
  - ESGF will use federated authentication, and OAuth and OIDC
    standards. Globus Auth and EGI CheckIn will be the two trusted
    authentication brokers in the federation.
  - Users who choose to use resources in the federation that trusts
    Globus Auth, will login via Auth and store policy in Globus (Groups)
    or resource. Similarly, EGI CheckIn and groups that EGI provides
    will be used by another set of users.
  - [IAM for publication](iam_for_publication.md)
- ESGF East/West Ingest Service
  - The Ingest Service allows authorized users in the federation to publish and manage metadata in the federation managed indices. The service is an authorized producer to the ESGF event stream service, and publishes an event to the stream.
  - [Ingest Service](ingest_service.md)
- Publication actions/events
  - A set of well defined publication actions will be supported for managing the metadata in the indices. Each action will also be governed by policy that determines who can perform that action in the federation.
  - [Publication Actions](publication_actions.md)
- Federation wide event stream to manage distributed index and replication (ESGF Event Service)
  - Authorized set of writers: the two Ingest Services
  - Consumers: two search services, and other authorized applications in the ESGF ecosystem
  - [Event Service](event_service.md)
- Publication event consumers for federation wide metadata index (East/West consumer)
  - This step reads events from the stream and depending on event type performs operations. The type of events is defined in [Publication Actions](publication_actions.md)
  - [Publicaton Events Consumer](publication_event_consumer.md)
- Other event consumers
  - Support other authorized consumers to process events from the stream
  - AR7 target does not include supporting consumers other than the two for managing the federation wide search index
  - In the next phase, other ESGF supported services will be added as consumers (E.g. Errata service etc). If any are added they will us native authentication for the event stream service
- Publisher
  - Support authentication via EGI tokens or Globus Auth
  - Update to push to the Ingest service
  - Most of the QC check and validation should happen here, and not be push off to upstream services

## High level flows
### Publication flow
- Pre-requisites
  - Data is already moved to a data node and the publisher information has pointers to the data
- Authorized user runs a publication client (s/w on a machine, Globus flow etc) and chooses a authentication provider to use - Globus (ESGF West) or EGI CheckIn (ESGF East)
- Once the publisher steps for extraction and validation is completed, it invokes the East/West Ingest Service
  - Authorization checks are done to ensure the user can publish
  - Validated request results in an event published on the event stream
  - Authentication to eventing service is done using pre-registered credentials
- East AND West Consumer service processes events off the stream and updates the respective index

### Replication flow
- Tier 1 sites that are allowed to replicate for the federation are:
  - Setup up as authorized consumers of the event stream
  - Setup to update a published entry to add location of replica
- Replication tool consumes events from the stream
- If the event meets criteria determined by the site to choose data that they want to replicate, a replication process is initiated (via Globus flow or other client s/w):
  - Transfer data referred to in the metadata
  - Once data transfer is complete, publish a “replica added” event to the message queue
  - East AND West Consumer service processes event, and updates the respective index to add location of the replica.

## Other ESGF consumers 
The following ESGF services can be set of consumers of event stream, so they can process the events in the context of the capability they deliver. But details of how the events are processed is outside the scope of this document.

## Errata Service
Notes from comments
- (Andrew Robinson) Add this, and a message was all I was thinking from the core arch side, maybe a small CV "reason-code" field too, with things like "Impact partial dataset", "Impact total dataset", "Mistaken publication", "Test publication" etc.
- The errata service (a separate service) can then plumb into this information and greatly simplify the process i.e. not a separate interface to enter errata information. By making these extra meta-data fields required (for updates, new version, retraction etc. events), we can ensure the errata service is used and thus we greatly improve the Data Governance status of the project. After all, "optional" =\> "not used".
- Given the small input effort, and significant improvements to the system, I would suggest that it could be an easy win stretch goal once the MVP is done.

## Open questions
- (Rachana) Other than CMIP, what projects are in scope? (Ben - do you also mean specific older CMIPs like CMIP6, CORDEX; the other MIPS eg PMIP. Input4mips, forcing data? ).
- (Zach) Is Tier 1 still an appropriate name for sites that publish replicas into federation wide index. (Ben - yes)
- (Zach) Do we need public consumers that need not be explicitly authorized in the federation? (Ben - not sure what the question is. We don’t need them, but is the question if we should authorize them?)
