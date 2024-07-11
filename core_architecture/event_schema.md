# Event Schema

## metadata
- User/Client ID
- Authorization service
- How the record was generated (publisher version)
- Creation time
- Kafka schema version

## Data
- Type of payload (STAC/SOLR)
- Data version (future proofing for if STAC isn't used)
- Payload (method, parameters, record)

## Example events
### Publish
ESGF Record with STAC Payload
```
{
    "metadata": {
        "auth": {
            "client_id": "<client_id>",
            "server": "esgf-login.ceda.ac.uk"
        },
        "publisher": {
            "package": "esgf_publisher",
            "version": "1.1.1"
        },
        "time": "2024-07-04T14:17:35Z",
        "schema_version": "1.0.0",
    }, 
    "data": {
        "type": "STAC",
        "version": "1.0.0",
        "payload": {
            "method": "POST",
            "collection_id": "cmip6",
            "item": <STAC_record>
        }
    }
}
```



### Update
#### Replacement
```
{
    "metadata": {
        "auth": {
            "client_id": "<client_id>",
            "server": "esgf-login.ceda.ac.uk"
        },
        "publisher": {
            "package": "esgf_publisher",
            "version": "1.1.1"
        },
        "time": "2024-07-04T14:17:35Z",
        "schema_version": "1.0.0",
    }, 
    "data": {
        "type": "STAC",
        "version": "1.0.0",
        "payload": {
            "method": "POST",
            "collection_id": "cmip6",
            "item_id": "CMIP6.ScenarioMIP.UA.MCM-UA-1-0.ssp245.r1i1p1f2.Amon.psl.gn.v20190731",
            "item": <STAC_record>
        }
    }
}
```
#### Partial update
```
{
    "metadata": {
        "auth": {
            "client_id": "<client_id>",
            "server": "esgf-login.ceda.ac.uk"
        },
        "publisher": {
            "package": "esgf_publisher",
            "version": "1.1.1"
        },
        "time": "2024-07-04T14:17:35Z",
        "schema_version": "1.0.0",
    }, 
    "data": {
        "type": "STAC",
        "version": "1.0.0",
        "payload": {
            "method": "PUT",
            "collection_id": "cmip6",
            "item_id": "CMIP6.ScenarioMIP.UA.MCM-UA-1-0.ssp245.r1i1p1f2.Amon.psl.gn.v20190731",
            "item": <partial_STAC_record>
        }
    }
}
```


### Revoke
#### Soft Delete
```
{
    "metadata": {
        "auth": {
            "client_id": "<client_id>",
            "server": "esgf-login.ceda.ac.uk"
        },
        "publisher": {
            "package": "esgf_publisher",
            "version": "1.1.1"
        },
        "time": "2024-07-04T14:17:35Z",
        "schema_version": "1.0.0",
    }, 
    "data": {
        "type": "STAC",
        "version": "",
        "payload": {
            "method": "PUT",
            "collection_id": "cmip6",
            "item_id": "CMIP6.ScenarioMIP.UA.MCM-UA-1-0.ssp245.r1i1p1f2.Amon.psl.gn.v20190731",
            "item": {"hidden": true}
        }
    }
}
```
#### Hard Delete
```
{
    "metadata": {
        "auth": {
            "client_id": "<client_id>",
            "server": "esgf-login.ceda.ac.uk"
        },
        "publisher": {
            "package": "esgf_publisher",
            "version": "1.1.1"
        },
        "time": "2024-07-04T14:17:35Z",
        "schema_version": "1.0.0",
    }, 
    "data": {
        "type": "STAC",
        "version": "",
        "payload": {
            "method": "DELETE",
            "collection_id": "cmip6",
            "item_id": "CMIP6.ScenarioMIP.UA.MCM-UA-1-0.ssp245.r1i1p1f2.Amon.psl.gn.v20190731",
        }
    }
}
```