- Business conditions: partition by country/region.

- Processing in batches without lookback.


Arquitectura:

- AWS Lambda monitoritza `s3://af-mediagrid` i envia els noms de fitxer a una cua.
- Un sistema de cua emmagatzema els fitxers. Candidats: Amazon SQS, RabbitMQ, AWS Elastic Cache.


- Spark job notebook: every 1h, load files to process into S3, partition by region, write into parquet.