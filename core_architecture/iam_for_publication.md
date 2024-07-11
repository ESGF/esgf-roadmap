# IAM for publication
**Rachana Ananthakrishnan, Phil Kershaw**
**The scope of this document is capabilities needed for AR7 FastTrack timeline.**

Data publication in ESGF requires users to be authenticated and authorized. This document describes the core IAM services that will be used.

## Authentication

- Federation authentication will be used and ESGF will not run any identity providers.
- OAuth and OIDC standards will be used.
- Globus Auth and EGI CheckIn will be the two trusted authentication brokers in the federation, allowing authentication via the identity providers they support
- The identity username or ePPN be used to identity for entity: janedoe@insititution.org
- Authentication context will include information on the authentication broker that was used to establish the user identity.

## Authorization policy management

- Groups will be used to manage authorization policies
- Globus Groups or EGI CheckIn groups can be used, depending on the authentication broker that will be trusted.
- Groups in these two services will NOT be synchronized.
- Authorisation policies themselves have been implemented in ESGF using OPA.

## Secure services in ESGF

Any service in the federation that needs authentication and/or authorization, can use Globus Auth or EGI checkin as the authorization broker. Such a service can trust one or both of the brokers, and use that to validate the tokens.

Groups are not automatically synced between the two services. So authorization policy in the service needs to have information on the group service to use (Globus or EGI), and the group(s) information to enforce the policy. It is expected that if Globus Auth is used as the authentication broker, then Globus Groups will be used to manage the membership. A example policy in the service would be that a user who is member of Globus group “Project1 Publisher”, represented by the UUID of the group, can publish new data sets to Project 1.

## Client applications in ESGF

Clients ideally support use of both services, but may choose to support one or the other authentication broker. The user of a client application is expected to choose the authentication broker they want to use, and the client can redirect the user to the broker to complete the authentication step and obtain tokens.

## Documentation 

### EGI CheckIn for services

### EGI CheckIn for applications

### Globus Auth for services

### Globus Auth for applications

## Use cases/ideas for timelines beyond AR7 Fasttrack 

## Open questions
