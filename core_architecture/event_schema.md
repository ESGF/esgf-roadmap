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

## Example Dataset/Item
```
payload.item = {
    "type": "Feature",
    "stac_version": "1.0.0",
    "stac_extensions": [],
    "id": "CMIP6.VolMIP.NASA-GISS.GISS-E2-1-G.volc-pinatubo-full.r6i10p1f1.Amon.zg.gn.v20190903",
    "collection": "CMIP6",
    "geometry": {
    "type": "Polygon",
    "coordinates": [
        [
            [1.25, -89.0],
            [358.75, -89.0],
            [358.75, 89.0],
            [1.25, 89.0],
            [1.25, -89.0]
        ]
    ]
    },
    "bbox": [1.25, -89.0, 358.75, 89.0],
    "properties": {
    "datetime": null,
    "start_datetime": "8121-01-16T12:00:00Z",
    "end_datetime": "8123-12-16T12:00:00Z",
    "version": "20190903",
    "access": ["HTTPServer", "Globus"],
    "activity_id": ["VolMIP"],
    "cf_standard_name": "geopotential_height",
    "citation_url": "http://cera-www.dkrz.de/WDCC/meta/CMIP6/CMIP6.VolMIP.NASA-GISS.GISS-E2-1-G.volc-pinatubo-full.r6i10p1f1.Amon.zg.gn.v20190903.json",
    "data_spec_version": null,
    "experiment_id": "volc-pinatubo-full",
    "experiment_title": "Pinatubo experiment",
    "frequency": "mon",
    "further_info_url": "https://furtherinfo.es-doc.org/CMIP6.NASA-GISS.GISS-E2-1-G.volc-pinatubo-full.none.r6i10p1f1",
    "grid": "atmospheric grid: 144x90, ocean grid: 288x180",
    "grid_label": "gn",
    "institution_id": "NASA-GISS",
    "mip_era": "CMIP6",
    "model_cohort": "Registered",
    "nominal_resolution": "250 km",
    "pid": "hdl:21.14100/b47a9c82-1b79-3c25-b619-6d7ac92568b8",
    "product": "model-output",
    "project": "CMIP6",
    "realm": ["atmos"],
    "source_id": "GISS-E2-1-G",
    "source_type": ["AOGCM"],
    "sub_experiment_id": "none",
    "table_id": "Amon",
    "title": "CMIP6.VolMIP.NASA-GISS.GISS-E2-1-G.volc-pinatubo-full.r6i10p1f1.Amon.zg.gn",
    "variable": "zg",
    "variable_id": "zg",
    "variable_long_name": "Geopotential Height",
    "variable_units": "m",
    "variant_label": "r6i10p1f1",
    "retracted": null
    },
    "links": [
        {
            "rel": "self",
            "type": "application/geo+json",
            "href": "https://api.stac.esgf-west.org/collections/CMIP6/items/CMIP6.VolMIP.NASA-GISS.GISS-E2-1-G.volc-pinatubo-full.r6i10p1f1.Amon.zg.gn.v20190903"
        },
        { 
            "rel": "parent", 
            "type": "application/json", 
            "href": "https://api.stac.esgf-west.org/collections/CMIP6" 
        },
        { 
            "rel": "collection", 
            "type": "application/json", 
            "href": "https://api.stac.esgf-west.org/collections/CMIP6" 
        },
        { 
            "rel": "root", 
            "type": "application/json", 
            "href": "https://api.stac.esgf-west.org/" 
        }
    ],
    "assets": {
        "globus": {
            "href": "https://app.globus.org/file-manager?origin_id=8896f38e-68d1-4708-bce4-b1b3a3405809&origin_path=/css03_data/CMIP6/VolMIP/NASA-GISS/GISS-E2-1-G/volc-pinatubo-full/r6i10p1f1/Amon/zg/gn/v20190903/",
            "description": "Globus Web App Link",
            "type": "text/html",
            "roles": ["data"],
            "alternate:name": "eagle.alcf.anl.gov",
            "name": "globus"
        },
        "data0000": {
            "href": "https://g-52ba3.fd635.8443.data.globus.org/css03_data/CMIP6/VolMIP/NASA-GISS/GISS-E2-1-G/volc-pinatubo-full/r6i10p1f1/Amon/zg/gn/v20190903/zg_Amon_GISS-E2-1-G_volc-pinatubo-full_r6i10p1f1_gn_812101-812312.nc",
            "description": "HTTPServer Link",
            "type": "application/netcdf",
            "roles": ["data"],
            "alternate:name": "eagle.alcf.anl.gov",
            "name": "data0000"
        }
    }
}
```

## Example events
### Publish
ESGF Record with STAC Payload
```
{
    "metadata": {
        "uuid": "3a8a2536-323b-4259-a218-6002a3491d25",
        "auth": {
            "auth_policy_id": "ESGF-Publish-00012",   # We need registered auth policies?
            "client_id": "CEDA-transaction-client",
            "requester_data": {
              "auth_service": AUTH_SERVICE,       # e.g., "auth.globus.org"    
              "sub": OAUTH_SUB_VALUE,             # e.g., "b16b12b6-d274-11e5-8e41-5fea585a1aa2"
              "user_id": USER_ID,                 # e.g., "7fd9ab20-f6c5-4902-a7ac-b40bc4d8ad7b"
              "identity_provider": OAUTH_IDENTITY_PROVIDER_ID,   # e.g., "0dcf5063-bffd-40f7-b403-24f97e32fa47"
              "identity_provider_display_name": OAUTH_IDENTITY_PROVIDER_NAME   # e.g., "University of Chicago"
            }
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
            "item": <payload.item>
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
            "method": "PUT",
            "collection_id": "cmip6",
            "item_id": "CMIP6.VolMIP.NASA-GISS.GISS-E2-1-G.volc-pinatubo-full.r6i10p1f1.Amon.zg.gn.v20190903",
            "item": <payload.item>
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
            "method": "PATCH",
            "collection_id": "cmip6",
            "item_id": "CMIP6.VolMIP.NASA-GISS.GISS-E2-1-G.volc-pinatubo-full.r6i10p1f1.Amon.zg.gn.v20190903",
            "item": <partial payload.item>
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
            "method": "PATCH",
            "collection_id": "cmip6",
            "item_id": "CMIP6.VolMIP.NASA-GISS.GISS-E2-1-G.volc-pinatubo-full.r6i10p1f1.Amon.zg.gn.v20190903",
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
            "item_id": "CMIP6.VolMIP.NASA-GISS.GISS-E2-1-G.volc-pinatubo-full.r6i10p1f1.Amon.zg.gn.v20190903",
        }
    }
}
```
