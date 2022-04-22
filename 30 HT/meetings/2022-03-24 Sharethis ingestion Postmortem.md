---
tags: [meeting, postMortem]
---

Date: 2022-03-24

## Summary
The TL;DR
## Timeline
- Les màquines no tiraven quan Sharethis va deixar anar moltes dades de cop.
- Vam pujar de màquina.
- Ingesters no permeten parallelitzar en horitzontal.
- La velocitat d'ingestió va augmentar molt.
## Root cause
- Incapacitat per ingestar de manera escalable
- Increase de volum amb Sharethis
- Processament de TCF en events de països GDPR.
## Resolution and recovery
- Vam escalar en diferents configuracions: aixecant tres instàncies amb una llista de països diferent cadascuna. Instàncies més petites
## Corrective and preventive measures
Actions to avoid repetitions of this type of incident
What could we have done different while dealing with the incident

- Passar management of TCF strings to Enricher