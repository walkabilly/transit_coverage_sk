---
title: "transit_coverage_sk"
date: "2023-08-21"
output:
  html_document:
    keep_md: true
---



## Developing Saskatoon or Saskatchewan Transit Coverage 

We are using the [Spatial Access Measures](https://www150.statcan.gc.ca/n1/pub/27-26-0001/272600012023001-eng.htm) developed by Statistics Canada. From the website: 

> There are seven types of amenity categories within the Spatial Access Measures: primary and secondary educational facilities (EFs), postsecondary educational facilities (PSEFs), health care facilities (HFs), places of employment (EMPs), grocery stores (GSs), cultural and arts facilities (CAFs), and sports and recreational facilities (SRFs). For each amenity, there are four variants based on the transportation mode: access via public transit during peak hours, access via public transit during off-peak hours, access via cycling and access via walking.

## Data 


```r
transit_peak <- read_csv("acs_public_transit_peak.csv", name_repair = "universal")
```

```
## New names:
## Rows: 498547 Columns: 16
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (12): CSDNAME, CMANAME, PRNAME, acs_idx_hf, acs_idx_emp, acs_idx_srf, ac... dbl
## (4): DBUID, CSDUID, CMAUID, PRUID
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `acs_lvl_gs-1` -> `acs_lvl_gs.1`
## • `acs_lvl_gs-3` -> `acs_lvl_gs.3`
## • `acs_lvl_gs-5` -> `acs_lvl_gs.5`
```

```r
transit_peak$acs_idx_hf <- as.numeric(transit_peak$acs_idx_hf)
```

```
## Warning: NAs introduced by coercion
```

```r
transit_peak$acs_idx_emp <- as.numeric(transit_peak$acs_idx_emp)
```

```
## Warning: NAs introduced by coercion
```

```r
transit_peak$acs_idx_srf <- as.numeric(transit_peak$acs_idx_srf)
```

```
## Warning: NAs introduced by coercion
```

```r
transit_peak$acs_idx_psef <- as.numeric(transit_peak$acs_idx_psef)
```

```
## Warning: NAs introduced by coercion
```

```r
transit_peak$acs_idx_ef <- as.numeric(transit_peak$acs_idx_ef)
```

```
## Warning: NAs introduced by coercion
```

```r
transit_peak$acs_lvl_gs.1 <- as.numeric(transit_peak$acs_lvl_gs.1)
```

```
## Warning: NAs introduced by coercion
```

```r
transit_peak$acs_lvl_gs.3 <- as.numeric(transit_peak$acs_lvl_gs.3)
```

```
## Warning: NAs introduced by coercion
```

```r
transit_peak$acs_lvl_gs.5 <- as.numeric(transit_peak$acs_lvl_gs.5)
```

```
## Warning: NAs introduced by coercion
```

```r
transit_peak <- transit_peak %>%
                        group_by(CMANAME) %>%
                          mutate(n = n())
                  

transit_peak <- filter(transit_peak, n >= 1000)
```

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

## Creating average values per city


```r
summary(transit_peak$acs_idx_hf)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##   0.000   0.000   0.000   0.041   0.032   1.000    7371
```

```r
transit_peak_city <- transit_peak %>%
                        group_by(CMANAME) %>%
                          summarize(acs_idx_hf = mean(acs_idx_hf), na.rm = TRUE,
                                    
                          )

transit_peak_city <- arrange(transit_peak_city, acs_idx_hf)

kable(transit_peak_city, "markdown")
```



|CMANAME                                                | acs_idx_hf|na.rm |
|:------------------------------------------------------|----------:|:-----|
|Drummondville                                          |  0.0000000|TRUE  |
|Granby                                                 |  0.0000013|TRUE  |
|Kawartha Lakes                                         |  0.0003881|TRUE  |
|Norfolk                                                |  0.0005156|TRUE  |
|Chatham-Kent                                           |  0.0012115|TRUE  |
|Lethbridge                                             |  0.0014741|TRUE  |
|Kamloops                                               |  0.0019205|TRUE  |
|Trois-Rivi<e8>res                                      |  0.0020839|TRUE  |
|Kelowna                                                |  0.0022781|TRUE  |
|Chilliwack                                             |  0.0032876|TRUE  |
|North Bay                                              |  0.0033774|TRUE  |
|Saint John                                             |  0.0034685|TRUE  |
|Charlottetown                                          |  0.0040867|TRUE  |
|Medicine Hat                                           |  0.0042854|TRUE  |
|Shawinigan                                             |  0.0046457|TRUE  |
|Prince George                                          |  0.0076564|TRUE  |
|Moncton                                                |  0.0083302|TRUE  |
|Greater Sudbury / Grand Sudbury                        |  0.0085180|TRUE  |
|St. John's                                             |  0.0088377|TRUE  |
|Thunder Bay                                            |  0.0099815|TRUE  |
|Barrie                                                 |  0.0101239|TRUE  |
|Saguenay                                               |  0.0104821|TRUE  |
|Nanaimo                                                |  0.0109775|TRUE  |
|Abbotsford - Mission                                   |  0.0119171|TRUE  |
|Sherbrooke                                             |  0.0149823|TRUE  |
|Red Deer                                               |  0.0154487|TRUE  |
|Regina                                                 |  0.0190658|TRUE  |
|Guelph                                                 |  0.0193810|TRUE  |
|Kingston                                               |  0.0215448|TRUE  |
|Oshawa                                                 |  0.0237781|TRUE  |
|Saskatoon                                              |  0.0242003|TRUE  |
|London                                                 |  0.0277668|TRUE  |
|Kitchener - Cambridge - Waterloo                       |  0.0286551|TRUE  |
|Halifax                                                |  0.0353573|TRUE  |
|Ottawa - Gatineau (partie du Qu<e9>bec / Quebec part)  |  0.0358536|TRUE  |
|Qu<e9>bec                                              |  0.0510585|TRUE  |
|Hamilton                                               |  0.0516990|TRUE  |
|Ottawa - Gatineau (Ontario part / partie de l'Ontario) |  0.0580875|TRUE  |
|Winnipeg                                               |  0.0628654|TRUE  |
|Victoria                                               |  0.0671921|TRUE  |
|Montr<e9>al                                            |  0.1361589|TRUE  |
|Toronto                                                |  0.1666977|TRUE  |
|Belleville - Quinte West                               |         NA|TRUE  |
|Brandon                                                |         NA|TRUE  |
|Brantford                                              |         NA|TRUE  |
|Calgary                                                |         NA|TRUE  |
|Cape Breton                                            |         NA|TRUE  |
|Edmonton                                               |         NA|TRUE  |
|Fredericton                                            |         NA|TRUE  |
|Peterborough                                           |         NA|TRUE  |
|Prince Albert                                          |         NA|TRUE  |
|Sarnia                                                 |         NA|TRUE  |
|St. Catharines - Niagara                               |         NA|TRUE  |
|Vancouver                                              |         NA|TRUE  |
|Windsor                                                |         NA|TRUE  |
|NA                                                     |         NA|TRUE  |



