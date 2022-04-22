# Data flow


# Legend

Governing rule: a data job can only appear in a single domain.

```mermaid
flowchart TD
  e((External Resource))
  d(Batch Job)
  a[(API)]

  subgraph Data Storage
    k>Kafka topic]
  end

  subgraph Data Storage
    s[/S3 Bucket/]
  end

  k --> ss{Service}
  ss --> s
  a --> d
  s --> d
  d --> e
```

# Web browsing events ingestion

```mermaid
  flowchart TD
  subgraph Kafka 1
    inAf>ingested-af]
    inSt>ingested-st]
    inTt>ingested-tt]
  end

  subgraph Kafka 2
    enAf>enriched-v2-af]
    enSt>enriched-v2-st]
    enTt>enriched-v2-tt]
  end

  subgraph S3 1
    ttEv[/af-33across/]
    s3St[/af-sharethis/]
  end

  subgraph S3 2
    s3EnEv[/af-reservoir/enriched-out/geo/af,st,tt/]
    s3Xa[/af-xandr/]
  end
  adTr{Ad Tracker} --> inAf

  stS3((Sharethis S3)) --> raIn{Raw Ingester}
  raIn --> s3St
  s3St --> kaEvIn

  kaEvIn{Kafka Event Ingester}
  kaEvIn --> inSt

  tt((33Across)) --> ttEv
  ttEv --> kaEvIn
  kaEvIn --> inTt

  xa((Xandr)) --> s3Xa

  inAf --> en{Enricher}
  inSt --> en
  inTt --> en

  en --> enAf
  en --> enSt
  en --> enTt

  enAf --> se{Secor}
  enSt --> se
  enTt --> se

  se --> s3EnEv
```

# Cookie mapping

Cookie mapping is the procedure that allows us to build an identity graph capable of syncing cookie IDs from different providers (ourselves, DSPs, third-party data providers).

The responsibility of this domain is to make mapping data available. Mapping datasets are vital as they allow the reconstruction of a user's browsing history across different data providers.

Results datasets:

- `s3://af-mrdata/mapping/userIdsMapperIncremental/`, in parquet. This is loaded "as is" into the AWS Glue Metastore.
- Redshift computing mapping tables: they are partitioned by partner and region under `map_*`.

Problem:

- The `userIdsMapper` job has two responsibilities that we could separate by creating a new Reporting Enablement job:
  - Compute the connected components in the identity graph.
  - To enable reporting by uploading results to Redshift Computing and the Metastore.


```mermaid
flowchart TD
  subgraph Kafka 1
    im>ingested-mapping]
  end

  subgraph Kafka 2
    em>enriched-v2-mapper]
  end

  subgraph S3 1
  s3MapRaw[/af-reservoir/enriched-out/geo/mapper/]
  end

  subgraph S3 2
  s3MapOut[/af-mrdata/mapping/userIdsMapperIncremental/]
  end

  subgraph Redshift Computing
    rcMa[map_*]
  end

  subgraph Metastore
    meMa[mapping_region]
  end

  adTr{Pixel Server} --> ms
  shCa((Sharethis)) --> ms
  ttCa((33Across)) --> ms
  ms --> im
  im --> en{Enricher}
  en --> em
  ms{mapper-service}
  em --> se{Secor}
  se --> s3MapRaw

  s3MapRaw --> uim(userIdsMapper-region)
  uim --> s3MapOut
  uim --> rcMa
  s3MapOut -.-> meMa
```

# Segment building

Segment building is the Data Domain that is responsible for producing data that can be pushed to our DSP(s) for the purpose of enabling the orchestration of online marketing campaigns.

The Data Products built by the jobs in this domain may be classified as follows:

- __User segments__. These are lists of cookie IDs.
- __Contextual segments__. These are lists of URLs or site domains.

Open problems:

- `performantDomains` should not read from the AWS Glue Metastore
- `categorySegments` should not be computed on Redshift Computing


```mermaid
flowchart LR

  plApi[(platform)]

  subgraph S3 Read
  s3EnEv[/af-reservoir/enriched-out/geo/af,st,tt/]
  s3MapRaw[/af-reservoir/enriched-out/geo/mapper/]
  s3MapOut[/af-mrdata/mapping/userIdsMapperIncremental/]
  end

  subgraph metastore
  meXa[xandr_*]
  end

  subgraph Redshift Computing
  rcSh[sharethis_*]
  rc33[33across_*]
  rcMa[map_*]
  rcDoCa[domain_categories_region]
  end

  subgraph S3 Write
  s3SeSy[/af-mrdata/segment_syncs/]
  end


  meXa --> peDom(performantDomains)
  plApi --> peDom
  peDom --> email

  s3SeSy --> dspSy{dsp-sync}
  dspSy --> dsp

  plApi --> urAuSe(urlAudienceSegments-region)
  s3MapRaw --> urAuSe
  s3MapOut --> urAuSe
  s3EnEv --> urAuSe
  urAuSe --> s3SeSy

  plApi --> caSe(categorySegments-region)
  rcSh --> caSe
  rc33 --> caSe
  rcDoCa --> caSe
  rcMa --> caSe
  caSe --> s3SeSy

  plApi --> loSe(lookalikeSegments-region)
  s3MapRaw --> loSe
  s3EnEv --> loSe
  loSe --> s3SeSy

  dsp((DSP))
  email((email))
```

# Domain classification

This is a domain containing a single Data Product. Its responsibility is to serve a _model_ that enables relating a website domain to its IAB category.

Open problems:

- Add final result to the AWS Glue Metastore reliably

```mermaid
flowchart TD
  subgraph S3 Read
  s3EnEv[/af-reservoir/enriched-out/geo/st,tt/]
  end
  
  subgraph S3 Write
  s3DoCl[/af-mrdata/domain-clusters/]
  end

  subgraph Redshift Computing
  rcDoClMa[domain_clusters_mapping_iab_region]
  rcDoClLa[domain_clusters_labels_iab_region]
  rcDoCaPa[domain_categories_partner]
  rcDoCaWe[domain_categories_webshrinker]
  end

  subgraph Redshift Computing Write
  rcDoCa[domain_categories_region]
  end

  subgraph Redshift Reporting
  rrDoCa[domain_categories_region]
  end

  s3EnEv --> doCl(domain-clusters-region)
  doCl --> s3DoCl
  doCl --> rcDoClLa
  doCl --> rcDoClMa

  rcDoClMa --> doCa(domainIABcategory)
  rcDoClLa --> doCa
  rcDoCaWe --> doCa
  rcDoCaPa --> doCa
  doCa --> rcDoCa
  doCa --> rrDoCa

```

# Reporting enablement

Data products in this domain have the responsibility of loading data into our Data Warehouse/Lake in formats which:

- May be manipulated at speed
- Are respectful of Data Protection regulations


```mermaid
flowchart LR

  plApi[(platform)]

  subgraph S3
  s3EnEv[/af-reservoir/enriched-out/geo/af,st,tt/]
  s3XaSt[/af-xandr/]
  s3SeSy[/af-mrdata/segment_syncs/]
  s3MapDsp[/af-mrdata/mapping/partner-affectv/dsp/]
%%  segmentsUploader

  end

  subgraph metastore
  meEv[events]
  mePl[platform_*]
  meXa[xandr_*]
  meSe[segments_*]
  end

  subgraph redshift-computing
  rcSi[simon_*]
  rcSh[sharethis_*]
  rc33[33across_*]
  rcPl[platform_*]
  rcAfSe[affectv_segments_*]
  rcMaDs[map_dsp_*]
  end

  subgraph redshift-reporting
  rrPl(platform_*)
  end

  s3EnEv --> evPa(eventsPartitioner)
  evPa --> meEv

  plApi --> plTa(platformTables)
  plTa --> mePl
  plTa --> rrPl
  plTa --> rcPl


  s3XaSt --> xaLl(xandrLogLevelLoader)
  xaLl --> meXa

  s3EnEv --> paUp(partnerUploader-*)
  paUp --> rcSh
  paUp --> rc33
  paUp --> rcSi

  s3SeSy --> seUp(segmentsUploader)
  seUp --> rcAfSe
  seUp --> meSe

  s3MapDsp --> maDsUp(mapDspUploader)
  maDsUp --> rcMaDs


%%  monitoringJobs-advertisersDelivery
%%  monitoringJobs-audiencesDelivery
%%  monitoringJobs-pixelRisersFallers
%%  monitoringJobs-platformUpdates*
%%  runrateReporting
%%  thirdpartyAdserversReporting
```



# Reporting

This domain is responsible for producing aggregated data in a fashion which may be consumed by:

- __PowerBI__ and other dashboards
- Our internal APIs
- Sent emails

Problems:
- `rocCurve` should not write to RDS.

```mermaid
flowchart LR

  plApi[(platform)]

  subgraph metastore
    meEv[events]
    mePl[platform_*]
    meMa[mapping_region]
    meXa[xandr_*]
    meDo[domain_categories_region]
    meXaAdMe[xandr_advertiser_metadata]
  end

  subgraph Redshift Computing
    rcSi[simon_*]
    rcSh[sharethis_*]
    rc33[33across_*]
    rcPl[platform_*]
    rcMa[map_*]
    rcAfSe[affectv_segments_*]
    rcOvRe[overlap_report_*]
    rcAuSi[audience_sizes]
    rcScore[score]
    rcRoc[roc]
  end

  subgraph Redshift Reporting
    rrIn[insights_v2_*]
    rrPl[platform_*]
    rrReTr[recurrent_analysis_trends]
    rrAuIn[audience_insights_*]
    rrAttr[attribution]
    rrDspXa[dsp_xandr_advertiser_frequency]
    rrPiMa[pixels_and_mapped_users_region_*]
    rrOvRe[overlap_report_region]
    rrAuSi[audience_sizes]
    rrDsAp[dsp_apn_*]
  end

  apiAudience[(audience)]
  apiDelivery[(delivery)]

  subgraph S3 Write
  s3Pocs[/af-pocs/]
  end

  subgraph S3 Read
  s3AfTmpLook[/af-tmp/lookalike-segments/]
  end

  xaAp((Xandr API))

  meEv --> inv2(insights)
  meMa --> inv2
  meDo --> inv2
  inv2 --> rrIn

  rcSh --> reKw(recurrentKeywords)
  rc33 --> reKw
  plApi --> reKw
  reKw --> rrReTr
  
  meEv --> auIn(audienceInsights)
  meMa --> auIn
  auIn --> rrAuIn

  rcSi --> attr(attributor)
  rcPl --> attr
  attr --> rrAttr

  meXa --> reXa(reportingXandrLogLevel)
  reXa --> rrDspXa

  rcSi --> piRe(pixelsReporting-region)
  rcMa --> piRe
  piRe --> rrPiMa

  rcAfSe --> ovRe(overlapReport)
  rcMa --> ovRe
  rcPl --> ovRe
  rcSi --> ovRe
  ovRe --> rcOvRe
  ovRe --> rrOvRe
  ovRe --> apiAudience
  ovRe --> apiDelivery
  ovRe --> rcAuSi
  ovRe --> rrAuSi

  rcOvRe --> daGr(dataGraphsJob)
  plApi --> daGr
  rcPl --> daGr
  daGr --> s3Pocs

  s3AfTmpLook --> roCu(rocCurve)
  roCu --> rcScore
  roCu --> rcRoc
  roCu --> apiAudience

  xaAp --> apRe(appnexusReporting) 
  apRe --> rrDsAp

  xaAp --> reXaBu(reportingXandrBuysideService)
  reXaBu --> meXaAdMe

  rcSh --> toUr(topUrls)
  rc33 --> toUr

```

# Maintenance

Jobs here are responsible for deleting old data from Database tables, to ensure that storage is kept at reasonable usage

```mermaid
flowchart TD
  subgraph Redshift Computing
    clNoMoTa(cleanupNonMonthlyTables)
  end

  subgraph Redshift Reporting
    clOlDaReTa(cleanupOldDataInReportingTables)
  end

```
