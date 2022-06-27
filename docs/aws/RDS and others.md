# RDS
AWS service for Relational Database Systems

- RDS instances are managed by AWS, you can't SSH into the instances
- Supports Aurora, MySQL, MariaDB, Postgres, Oracle, Microsoft SQL Server
- Multi-AZ makes an exact copy of your database in another AZ that is only standby, changes are automatically synchronized. Automatic fail over if one AZ goes out.
- Read replicas allow running multiple copies (up to 5) of the database cross region and can be combined with Multi-AZ. You can't write to read replicas and is intended to alleviate the workload of your primary database to improve performance. They use Async replication (??). Automated backups should be enabled. Read replica could be promoted to their own database but this breaks replication
-  Automated backups, you choose a retention period from 1 to 35 days, There is no additional cost for backup storage, you define your backup window
-  There are also Manual snapshots, if you delete your primary datase the snapshot will still exist and can be restored
-  When you restore an instance it will create a new database. You just need to delete your old database and point trafiic to new restored database
-  You can turn on encryption at-rest for RDS via KMS
- Migration from Postgres to Aurora depends on the version of the Database
> `12.5` and `9.6.20` versions of PostgresSQL are **not supperted** by aurora. What is supported is listed [here](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraPostgreSQL.Updates.20180305.html).
- For aurora serverless you have to have backup enabled.

## Aurora
- Fully managed PostgreSQL (3x faster) or MySQL (5x faster)
- Automatic scaling and backups
- High availability with 2 copies in 3 AZs and Fault Tolerance
- 1/10 the cost over its competitor with similar performace and availability options
- Up to 15 Aurora replicas
- Can span multiple regions with Aurora Global Database
- Aurora serverless allow to start and stop and scale automatically with low-cost, ideal for new projects and/or infrequent database usage.


## RedShift
DataWarehouse service with OLAP and Columnar store
- Data can be loaded from S3, EMR, DynamoDB, or multiple data sources on remote hosts.   
- Redshift is Columnar Store database which can SQL-like queries and is an OLAP.   
- Redshift can handle petabytes worth of data. Redshift is for Data Warehousing   
- Redshift most common use case is Business Intelligence   
- Redshift can only run in a 1 availability zone (Single-AZ)   
- Reshift can run via a single node or multi-node (clusters)   
- A single node is 160 GB in size   
- A multi-node is comprised of a leader node and multiple compute nodes   
- You are bill per hour for each node (excluding leader node in multi-node)   
- You are not billed for the leader node   
- You can have up to 128 compute nodes   
- Redshift has two kinds of Node Type Dense Compute and Dense Storage   
- Redshift attempts to backup 3 copies of your data, the original, on compute node and on S3   
- Similar data is stored on disk sequentially for faster reads   
- Redshift database can be encrypted via KMS or CloudHSM   
- Backup Retention is default to 1 day and can be increase to maximum of 35 days   
- Reshift can asynchronously back up your snapshot to Another Region delivered to S3   
- Redshift uses Massively Parallel Processing (MPP) to distribute queries and data across all loads   
- ln the case of empty table, when importing Redshift will sample data to create a schema

## DynamoDB
DynamoDB is a fully managed NoSQL key/value and document database.   
- Applications that contain large amounts of data but require predictable read and write performance while scaling is a good fit for DynamoDB   
- DynamoDB scales With whatever read and write capacity you specific per second.   
- DynamoDB can be set to have Eventually Consistent Reads (default) and Strongly Consistent Reads   
- Eventually consistent reads data is returned immediately but data can be inconsistent. Copies of data will be generally consistent in 1 second.   
- Strongly Consistent Reads will wait until data in consistent. Data will never be inconsistent but latency will be higher. Copies of data will be consistent With a guarantee ofl second.   
- DynamoDB stores 3 copies of data on SSD drives across 3 regions.