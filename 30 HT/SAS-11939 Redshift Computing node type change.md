---
tags: [migration, plan]
---

## Steps outline
- [x] Make snapshot of Redshift Computing
- [x] Restore snapshot
- [x] Test network config by connecting from own device
- [x] Test network config by connecting from airflow alpha
- [x] Test load by running a user ids mapper against it

## Migration Notes

### Snapshot
- 
### New cluster details
-   Cluster identifier: `computing-migration-test`
-   Node type: `ra3.xlplus`
-   Number of nodes: `2`
-   Database Configurations (must match the ones in the production cluster):
    -   Database name: `warehouse`
    -   Port: `5439`
-   Tags: `product: computing-test`

### Network & IAM Roles
Network must match the production cluster.
- VPC: `vpc-060220b63062bbe0b`
-  Subnet: `production-redshiftcomputing-cluster-subnet-group-1`
-  VPC Security groups:
    - `sg-0c30007b7b97393bf`
    - `sg-0b18154c25777eb26`
- Availability zone: `eu-west-a1`
- Enhanced VPC Routing: `enabled`
- Publicly accessible: `enabled`
- Elastic IP: `54.170.249.7`

IAM Roles:
- `redshift_aggregate`
- `RedshiftS3`

Monitoring:
- We need to setup cloud watch alerts for storage usage, as done up until now.
- We need to setup new budget alerts when storage goes above 15% (surpassing 10TB of storage), for budget purposes.

## Story Result

### TL;DR
- The migration can be carried out successfully.
- The only unknown, to be sorted afterwards, is whether we will have enough CPU power to deliver results in a timely fashion; this can be sorted out by adding a third node, manually.

### Verifications
- [ ] Can connect from local device and run queries
- [ ] Airflow (alpha) can connect to the database

### Migration plan
(updated in place)

## Notes during migration
- [x] Setup cloudwatch alarms for the cluster at 15% disk usage.
- [x] Setup cloudwatch alarms for the cluster at 90% disk usage.