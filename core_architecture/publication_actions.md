# Publication Actions
The document outlines the various publication actions that will be supported for the AR7 timeline. It defines for each action, the policy that governs who can perform the action, and the outcome of the action. 

## Publish new data set

This action is to publish a new dataset into the federation, such that it is available via discovery in the federation wide search indices.
### Policy
- Policy is set up per project per institution
- Each institution is authorized to publish to a project, but the metadata must have the field TBD set to the institution’s string
  - Policy enforcement would need to be CV-aware for institution?
- One implementation approach.
  - Set up one or more group per institution, and set a delegate as manager of group to manage membership
  - Store as a group field or name of group the institution name
  - Check the user is member of one of these groups, and the metadata field in the payload matches the institution

### Processing steps

- Publisher client 
  - pushes record(s)
    - Payload granularity TBD
- Publishing API
  - Quick format check
    - This check uses the latest version of the CVs
    - 202 if success (with a handle for the request status)
    - 400 if format is bad
    - Other appropriate status codes, eg 500, 401/3
  - Asynchronous metadata validation
    - Should clients poll API for success/fail?
    - Register email for notification messages
      - Limit granularity of these or could be overwhelming
    - Affix the CV version used to validate for downstream consistency check
  - Upon success API enqueues message
- Message in Event stream Queue
- Index nodes
  - Dequeue message(s)
  - Check payload for quality, consistency etc. (Needs a flow for the case where the payload is rejected because fails checks)
    - Use the CV version referenced in the upstream check
    - If the payload fails, this seems like an anomalous "consistency" problem
      - alert the DevOps team to investigate (email, Slack notifications, etc.),
      - alert the publisher of record as well
  - Check record(s) do not already exist - if this is the case what is the behaviour?
  - Ingest payload into index
- Replica nodes
  - Dequeue message
  - Check payload for quality, consistency etc. (Needs a flow for the case where the payload is rejected because fails checks)
  - Check record(s) do not already exist - if this is the case what is the behaviour?
    - Check file checksums?
      - If they match, then there is no action needed, this was a redundant re-publication perhaps
      - If they don’t match, then replace the files (see below on data transfer)
  - Check dataset id against replication policy, if in policy
    - If not in policy, build a local catalog of data not replicated for future use?
  - Select transfer method based on site hosting data
  - Initiate dataset transfer for replication
    - At this point do we run a quality check on data?
  - Push message that replica is in place
- Index nodes
  - Receive replica message
  - Update file records with inclusion of replica location information
	


 

### Retract a publication
#### Policy
#### Processing steps
- Publisher client
  - Checks for dataset existence
    - Existing behavior, return nonzero errorlevel if not
  - Push retraction
    - 202 if correct
    - Do we want to allow for the option to purge data or have still available?
    - The current understanding is that retracted data may be removed from the accessible location but the provider may still keep on alternate storage
- Publisher API
  - Enqueue message
- Message in queue
- Index nodes 
  - Dequeue message
  - Flag all records dataset/file as retracted
- Replica nodes
  - Dequeue message
  - Check if in collection
  - Recommended to remove from storage or access control 
    - Async process for that?
  - Push "replica removed" message?
		
**Notes/questions:**
- Metadata should be marked as retracted, and not deleted. 
  - S
- Search options
  - Not return retracted metadata
  - Not return data access information for retracted metadata
  - Flag to return full record - 

### Replica added

Change to replica policy allows for new replicas to be added
#### Policy
#### Processing steps
- Replica node
  - Determine additional datasets to replicate
    - Use local catalog copy
    - Interrogate index
    - Esgpull?
  - Initiate data transfers
  - Upon completion enqueue message that replica is available at site
    - Payload of dataset, files + urls
  - Message in queue
  - Index node
    - Dequeue message
    - Dataset must be complete: all files as original
    - Update records to include additional site location for replica / access methods


**Notes/questions:**

- Can only update the "Location" field to add or update URLs. Should this further be controlled such that a user from a site A can only add URLs relevant to that site?
  - It would make sense to have such AC rules

### Replica removed

Due to storage constraints or changes in direction site needs to purge replicas
#### Policy
#### Processing steps

- Replica Site
  - Push message to remove replica record
- Index node
  - Dequeue message 
  - Check correct site
  - Update records to remove replica entry

### Update published data set

Data provider wishes to publish a new version of a previously published dataset,  The old version is no longer "latest" 

#### Policy
#### Processing steps
- Publisher client
  - Push record 
  - Must pass checks, see above, same semantics as 
  - new publish
  - Note this is simplified
- Index Node
  - Dequeue message
    - Consistency check via CV version
  - "Deprecate" prior version
    - Update record for that version
    - Submit replica deprecation notification
  - Publish new version
    - Submit new version notification
  - Replica site
    - Receive deprecation notice
    - Act on notice if within policy
      - Migrate to lower tier storage
      - Purge old data
    - Receive new data message (same as above….)

## Open questions
- Should a "hard delete" action be supported? If so, who should be allowed to do that?
  - Only site administrators, or a smaller set of users should be able to hard delete.
  - Deleted records are not returned in routine queries, only administrator-level queries, to avoid confusion, while retracted records are available, can we do this with the search APIs?
- Republication of the same version due to operator error?
  - This happens a bit in practice.  In the interim, the data could be deleted.
  - Example publisher misconfiguration leads to something not right
  - It may not be reasonable to require a new version be minted due to an error if the data, intrinsic metadata does not change (i.e no checksum changes needed).
  - Maybe some kind of administrative exemption needs to be granted to overwrite a record?
