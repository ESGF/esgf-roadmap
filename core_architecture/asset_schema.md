# Asset Schema

The asset schema for a STAC item should accommodate multiple ways of getting the data as well as multiple locations while still adhering to the [STAC Asset Specification](https://github.com/radiantearth/stac-spec/blob/master/commons/assets.md#asset-object).

## Issues/Questions

One problem is that the STAC specification requires an `href` to be at the top level, so there is a sort of `master` record at the top with `alternates` below that. 

* What if the top-level (master) replication/item gets marked for deletion? What becomes the new master item?
* Assign top-level items at random?

## Example 

Please note the example below is using "dummy" data. This is to present a possible structure for the assets dictionary. Proposal is to break out (at the top level) a globus key with links to the Globus collection in the Globus Web Application. `data` items below that are links to the individual netcdf files via https. 
```
"assets": {
  "globus": {
    "href": "https://app.globus.org/file-manager/?origin_id=8896f38e-68d1-4708-bce4-b1b3a3405809&origin_path=/css03_data/CMIP6/PAMIP/NCC/NorESM2-LM/pdSST-futAntSIC/r30i1p1f1/Emon/cSoil/gn/v20191108",
    "type": "text/html",
    "description": "Globus Web App Link",
    "roles": ["data"],
    "alternate:name": "LLNL",
    "alternate": {
      "ORNL": {
        "href": "https://app.globus.org/file-manager/?origin_id=8896f38e-68d1-4708-bce4-b1b3a3405809&origin_path=/css03_data/CMIP6/PAMIP/NCC/NorESM2-LM/pdSST-futAntSIC/r30i1p1f1/Emon/cSoil/gn/v20191108",
        "type": "text/html",
        "description": "Globus Web App Link",
        "roles": ["data"],
        "alternate:name": "ORNL"
      },
      "ALCF": {
        "href": "https://app.globus.org/file-manager/?origin_id=8896f38e-68d1-4708-bce4-b1b3a3405809&origin_path=/css03_data/CMIP6/PAMIP/NCC/NorESM2-LM/pdSST-futAntSIC/r30i1p1f1/Emon/cSoil/gn/v20191108",
        "type": "text/html",
        "description": "Globus Web App Link",
        "roles": ["data"],
        "alternate:name": "ALCF"
      }
    }
  },
  "data001": {
    "href": "https://g-52ba3.fd635.8443.data.globus.org/css03_data/CMIP6/PAMIP/NCC/NorESM2-LM/pdSST-futAntSIC/r30i1p1f1/Emon/cSoil/gn/v20191108/cSoil_Emon_NorESM2-LM_pdSST-futAntSIC_r30i1p1f1_gn_200006-200105.nc",
    "type": "netcdf",
    "description": "HTTPS Link",
    "roles": ["data"],
    "alternate:name": "LLNL",
    "alternate": {
      "ORNL": {
        "href": "https://g-52ba3.fd635.8443.data.globus.org/css03_data/CMIP6/PAMIP/NCC/NorESM2-LM/pdSST-futAntSIC/r30i1p1f1/Emon/cSoil/gn/v20191108/cSoil_Emon_NorESM2-LM_pdSST-futAntSIC_r30i1p1f1_gn_200006-200105.nc",
        "type": "netcdf",
        "description": "HTTPS Link",
        "roles": ["data"],
        "alternate:name": "HTTPS"
      },
      "ALCF": {
        "href": "https://g-52ba3.fd635.8443.data.globus.org/css03_data/CMIP6/PAMIP/NCC/NorESM2-LM/pdSST-futAntSIC/r30i1p1f1/Emon/cSoil/gn/v20191108/cSoil_Emon_NorESM2-LM_pdSST-futAntSIC_r30i1p1f1_gn_200006-200105.nc",
        "type": "netcdf",
        "description": "HTTPS Link",
        "roles": ["data"],
        "alternate:name": "HTTPS"
      }
    }
  },
  "data0002": {
    "href": "https://g-52ba3.fd635.8443.data.globus.org/css03_data/CMIP6/PAMIP/NCC/NorESM2-LM/pdSST-futAntSIC/r30i1p1f1/Emon/cSoil/gn/v20191108/cSoil_Emon_NorESM2-LM_pdSST-futAntSIC_r30i1p1f1_gn_200006-200105.nc",
    "type": "netcdf",
    "description": "HTTPS Link",
    "roles": ["data"],
    "alternate:name": "LLNL",
    "alternate": {
      "ORNL": {
        "href": "https://g-52ba3.fd635.8443.data.globus.org/css03_data/CMIP6/PAMIP/NCC/NorESM2-LM/pdSST-futAntSIC/r30i1p1f1/Emon/cSoil/gn/v20191108/cSoil_Emon_NorESM2-LM_pdSST-futAntSIC_r30i1p1f1_gn_200006-200105.nc",
        "type": "netcdf",
        "description": "HTTPS Link",
        "roles": ["data"],
        "alternate:name": "HTTPS"
      },
      "ALCF": {
        "href": "https://g-52ba3.fd635.8443.data.globus.org/css03_data/CMIP6/PAMIP/NCC/NorESM2-LM/pdSST-futAntSIC/r30i1p1f1/Emon/cSoil/gn/v20191108/cSoil_Emon_NorESM2-LM_pdSST-futAntSIC_r30i1p1f1_gn_200006-200105.nc",
        "type": "netcdf",
        "description": "HTTPS Link",
        "roles": ["data"],
        "alternate:name": "HTTPS"
      }
    }
  }
}
```