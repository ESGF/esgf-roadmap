# Ingest Service

**The scope of this document is capabilities needed for AR7 FastTrack
timeline.**

The Ingest Service allows authorized users in the federation to publish and manage metadata in the federation managed indices. The service is an authorized producer to the ESGF event stream service, and publishes an event to the ESGF event service.

When a request is received, the ingest service:

- Authenticates the requestor
- Checks if the user is authorized to perform the type of publication action being requested
- Publishes an event to the ESGF event stream service for any valid requests

## Publication actions

The publication actions that need to be supported, along with the policy that should be enforced for such requests, is documented in [Publication Actions](publication_actions.md)

## Ingest service API

API, error handling etc.

## Authentication

## Authorization

## Payload format

This section describes the payload of the event published to the stream.

Initial work on a PoC for a kafka based event system has been defined [here](https://github.com/esgf2-us/event-stream-service). Please see the kafka branch and the [producer.py](https://github.com/esgf2-us/event-stream-service/blob/kafka/event-producer/producer.py) file for more on events. In my example, there is a simple AWS MSK Kafka cluster running with one broker with events streaming to S3 (json) via an AWS Firehose.

Steps to reproduce the event system:

1.  Create a [serverless AWS MSK cluster](https://docs.aws.amazon.com/msk/latest/developerguide/serverless.html)
    a.  Create an S3 bucket and connect it the cluster via Firehose
2.  Make sure a [client machine](https://docs.aws.amazon.com/msk/latest/developerguide/create-serverless-cluster-client.html) is also running
3.  Clone the kafka branch from the above event-stream-service repo. Make sure to change the broker url to your cluster.
4.  Grab some CMIP6 data and put it somewhere the script can read it and
    run generate_events.py  
    ```
    python generate_events.py --path \
    CMIP6/AerChemMIP/AS-RCEC/TaiESM1/hist-piNTCF/r1i1p1f1
    ```
    a.  If you’re [running a consumer](https://docs.aws.amazon.com/msk/latest/developerguide/msk-serverless-produce-consume.html), you will also see the events stream to stdout.

Here are two sample payloads:

- ~~https://github.com/esgf2-us/event-stream-service/blob/main/event-producer/globus-publish-payload.json~~
- https://github.com/esgf2-us/event-stream-service/blob/main/event-producer/stac-publish-payload.json
- Event type field
  - Should be set to one of the type of actions defined in [Publication Actions](https://docs.google.com/document/d/1l26fepBtM95N6pEFHt8BoYINebg5XJXbCq1JfsFTJvc/edit)
- Identity of the user publishing (janedoe@insitution.org)
- Authorization server that asserted the identity (Globus, EGI)
- Timestamp
- Metadata as needed for supporting STAC
- Host urls for delivery types
  - HTTP/S
  - OpenDAP
  - Globus Collection
- Project-specific, examples:
  - WCRP projects
    - CMIP6Plus
    - CMIP7
    - CORDEX
  - E3SM
- CV version used to validate the record
  - This will be used for a downstream check

## Publishing an event to stream

- Will use the native authentication for the event stream service that will be used. This does not preclude adding a layer in front of the service with different security/protocol in the future

## CEDA publishing

- Our current STAC is generated using the [STAC
  Generator](https://github.com/cedadev/stac-generator)
  - It has a range of plugins to generate the records
    - Inputs
      - Define where to collect messages from
    - Outputs
      - Define where to send the generated records
      - We use the [STAC FastAPI output plugin](https://github.com/cedadev/stac-generator/blob/master/stac_generator/plugins/outputs/stac_fastapi.py) which expects STAC formatted records
        - [STAC Item Transaction extension](https://github.com/stac-api-extensions/transaction)
          - [Example item](https://api.stac.ceda.ac.uk/collections/cmip6/items/CMIP6.ScenarioMIP.UA.MCM-UA-1-0.ssp245.r1i1p1f2.Amon.psl.gn.v20190731) payload (an item record)
        - [STAC Collection Transaction extension](https://github.com/stac-api-extensions/collection-transaction)
          - [Example collection](https://api.stac.ceda.ac.uk/collections/cmip6) payload (a collection record)
  - For each message
    - The message’s path is used to select the relevant “Recipe”
    - The recipe defines a list of extraction methods that extract/augment metadata
    - The extraction methods are run in sequence
    - The produced record is passed to any output plugins
- There is an example of its use in the repo [here](https://github.com/cedadev/stac-generator/tree/master/example).

### STAC Generator configuration

- [This cmip6 recipe](https://github.com/cedadev/stac-recipes/blob/master/recipes/item/cmip6.yaml)
- [This STAC FastAPI output plugin](https://github.com/cedadev/stac-generator/blob/master/stac_generator/plugins/outputs/stac_fastapi.py)
- [This text file input plugin](https://github.com/cedadev/stac-generator/blob/master/stac_generator/plugins/inputs/text_file.py)

Example text file contents:
```
{"uri": "/badc/cmip6/data/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/clt/gn/v20191108"}
{"uri": "/badc/cmip6/data/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/hurs/gn/v20191108"}
{"uri": "/badc/cmip6/data/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/huss/gn/v20191108"}
{"uri": "/badc/cmip6/data/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/pr/gn/v20191108"}
{"uri": "/badc/cmip6/data/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/prsn/gn/v20191108"}
{"uri": "/badc/cmip6/data/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/psl/gn/v20191108"}
{"uri": "/badc/cmip6/data/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/rlds/gn/v20191108"}
{"uri": "/badc/cmip6/data/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/rlus/gn/v20191108"}
{"uri": "/badc/cmip6/data/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/rsds/gn/v20191108"}
{"uri": "/badc/cmip6/data/CMIP6/CMIP/CSIRO-ARCCSS/ACCESS-CM2/historical/r1i1p1f1/Amon/rsus/gn/v20191108"}
…
```

### Notes

- I thought we would have a universal payload (not STAC/Globus specific)
- The consumers would then read the message and map this into the desired format
- For CEDA I imagined this is where the STAC Generator would fit

## Use cases/ideas for timelines beyond AR7 Fasttrack 

## Open questions

