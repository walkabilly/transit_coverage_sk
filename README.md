## Developing Saskatoon or Saskatchewan Transit Coverage 

We are using the [Spatial Access Measures](https://www150.statcan.gc.ca/n1/pub/27-26-0001/272600012023001-eng.htm) developed by Statistics Canada. From the website: 

> There are seven types of amenity categories within the Spatial Access Measures: primary and secondary educational facilities (EFs), postsecondary educational facilities (PSEFs), health care facilities (HFs), places of employment (EMPs), grocery stores (GSs), cultural and arts facilities (CAFs), and sports and recreational facilities (SRFs). For each amenity, there are four variants based on the transportation mode: access via public transit during peak hours, access via public transit during off-peak hours, access via cycling and access via walking.

## Code 

Code is available for [download here](https://github.com/walkabilly/transit_coverage_sk/blob/main/transit_coverage_sk.md)

The dataset fields are as follows

| Field name      | Data type | Description                                                                                                            |
|-----------------|-----------|------------------------------------------------------------------------------------------------------------------------|
| DBUID           | int64     | Uniquely identifies a dissemination block (composed of the 2-digit province/territory unique identifier followed by the 2-digit census division code, the 4-digit dissemination area code and the 3-digit dissemination block code). |
| CSDUID          | int64     | Uniquely identifies a census subdivision (composed of the 2-digit province/territory unique identifier followed by the 2-digit census division code and the 3-digit census subdivision code). |
| CSDNAME         | object    | Census subdivision name.                                                                                              |
| CMAUID          | int64     | Uniquely identifies a census metropolitan area/census agglomeration.                                                  |
| CMANAME         | object    | Census metropolitan area or census agglomeration name.                                                               |
| PRUID           | int64     | Uniquely identifies a province or territory.                                                                         |
| PRCODE          | object    | Province or territory code.                                                                                          |
| acs_idx_hf      | float64   | Normalised value of a dissemination block's access to health care facilities.                                        |
| acs_idx_emp     | float64   | Normalised value of a dissemination block's access to employment.                                                    |
| acs_idx_srf     | float64   | Normalised value of a dissemination block's access to sports and recreation facilities.                              |
| acs_idx_psef    | float64   | Normalised value of a dissemination block's access to post-secondary education facilities.                          |
| acs_idx_ef      | float64   | Normalised value of a dissemination block's access to primary and secondary education facilities.                   |
| acs_idx_caf     | float64   | Normalised value of a dissemination block's access to cultural and arts facilities.                                  |
| acs_lvl_gs-1    | float64   | Time in minutes to reach the closest grocery store.                                                                 |
| acs_lvl_gs-3    | float64   | Time in minutes to reach the 3rd closest grocery store.                                                             |
| acs_lvl_gs-5    | float64   | Time in minutes to reach the 5th closest grocery store.                                                             |
