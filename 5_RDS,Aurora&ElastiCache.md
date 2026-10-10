*Must know: In AWS there's a network cost when data goes from one AZ to another.*

# AWS RDS
- Relational Database Service: managed(by AWS) DB service that use SQL (Structured Query Language) as a query language
- types of engine that are supported by AWS RDS: PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server, IBM DB2, Aurora (AWS proprietary Database)

## Advantage over using RDS vs deploying DB on EC2
- RDS is managed service:

      - automated provisioning, OS patching
      - continuous backups and restore to specific timestamp
      - monitoring dashboards
      - read replicas for improved read performance
      - multi AZ setup for DR (disaster recovery )
      - scaling capabilities(vertical & horizontal)
      - storage backed by EBS
   
- but you can't SSH into your instances

## RDS- storage auto scaling

- increase storage on your RDS DB instance dynamically, when RDS detects you are running out of free DB storage, it scales automatically
- you have to set *Max Storage Threshold* (max limit for DB storage)
- automatically modify storage if:

      - free storage is less than 10% of allocated storage
      - low-storage lasts at least for 5 mins
      - 6 hrs have passed since last modification

- useful for applications with unpredictable workloads
- supports all RDS DB engines

## RDS read replicas Vs RDS Multi AZ

<img width="1200" height="1000" alt="WhatsApp Image 2026-10-06 at 23 55 52" src="https://github.com/user-attachments/assets/1d5a1d89-22ad-47df-af7c-9529183f870c" />

### RDS read replicas-use case

- you have a production DB on normal load, you want to run a reporting application to run some analytics
- you create read replicas to run the new workload, (the production application is unaffected)
- read replicas are used for *SELECT only not for INSERT, UPDATE, DELETE*

<img width="800" height="800" alt="WhatsApp Image 2026-10-07 at 00 18 50" src="https://github.com/user-attachments/assets/944e2a94-945d-4a41-92c2-502cd54624a2" />

### RDS read replicas- network cost

- for RDS read replicas within the same region, you don't have to pay that fee
<img width="800" height="410" alt="WhatsApp Image 2026-10-07 at 00 28 57" src="https://github.com/user-attachments/assets/3f992fcd-e54b-4e83-bebb-0fb3c1acee05" />

### RDS multi AZ (Disaster Recovery)

- SYNC replication, one DNS name - automatic app failover to standby
- failover in case of loss of AZ, loss of network , instance or storage failure
- increase availability, no manual intervention in apps, not used for scaling (additional reads)
- *NOTE: you can set up your read replica as a Multi AZ if you want to*

<img width="750" height="700" alt="WhatsApp Image 2026-10-07 at 00 43 57" src="https://github.com/user-attachments/assets/bea46273-95a6-4763-98d3-a7d6073faf00" />

### RDS- from single AZ to multi AZ

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/265072fe-3743-4283-98f6-3cef30d24665" />

- zero downtime operation(no need to stop the DB), just *modify and enable multi AZ* for the DB
- internally:

      - a snapshot is taken, then a new DB is restored from the snapshot in a new AZ
      - synchronization is established b/w the two DBs


### In AWS terms, think of *Sqlectron as a GUI client for a database* (use to connect to the remote DB), not as an AWS service itself.

## RDS custom

- RDS custom is only for two database types, its for Oracle and Microsoft SQL Server
- access to OS and underlying database 
- custom: configure settings, install patches, enable native features, access EC2 instance using SSH or SSM session manager
- RDS Vs RDS Custom:
 
      - RDS: entire DB and the OS is managed by AWS
      - RDS custom: full admin access to the underlying DB and the OS

 ## Amazon Aurora(important from exam POV)

 - Aurora is a proprietary technology from AWS
 - PostgreSQL and MySQL are both supported as Aurora DB (means drivers will work as if Aurora is MySQL or PostgreSQL DB)
 - Aurora is *AWS Cloud Optimised* and claims 5x/3x performance improvement over MySQL/PostgreSQL on RDS
 - Aurora storage automatically grows in increments of 10GB, upto 256 TB
 - aurora can have upto 15 read replicas and the replicatoion is faster
 - failover in Aurora is instantaneous.
 - Aurora costs more than RDS(20% more) - but is way more efficient
 - *Aurora DB capacity is measured in ACU (Aurora Capacity Units), 1 ACU provides 2GB of memory and corresponding compute and networking*

 ### Aurora HA & read scaling

- 6 copies of your data across 3 AZs

      - 4/6 copies are needed for writes, 3/6 copies are needed for reads
      - self healing with peer-to-peer replication
      - storage is stripped across 100s of volumes 
 
- one aurora instance takes write (Master) => one at a time
- automated failover for master in less than 30s (one of the read replica will become master)
- master + upto 15 aurora read replicas for reads
- supports cross region replication

- Important diagram
<img width="800" height="750" alt="WhatsApp Image 2026-10-09 at 12 08 29" src="https://github.com/user-attachments/assets/c2631401-0006-461b-8a2a-004f5f8e6b97" />

### Aurora DB cluster

- *exam pov*: Reader endpoint: connects cilent to one of the read replicas for read, load balancing at the connection level not at the statement level

<img width="900" height="773" alt="WhatsApp Image 2026-10-09 at 12 20 34" src="https://github.com/user-attachments/assets/1032d3d3-473e-4ad2-9a22-c3128a12c37e" />

### Features of Aurora

- automatic failover, backup & recovery, security, industry compliance, automated patching with zero downtime, advanced monitoring, routine monitoring
- backtrack: restore data at any point of time without using backups

## Aurora: Advance Concepts

### Aurora replicas- auto scaling
<img width="850" height="700" alt="WhatsApp Image 2026-10-09 at 21 54 53" src="https://github.com/user-attachments/assets/a3ebff05-1b37-43b0-8db8-8765b29fe08d" />

### custom endpoints: the reader endpoint is generally not used after defining the custom endpoints
<img width="850" height="700" alt="WhatsApp Image 2026-10-09 at 22 13 08" src="https://github.com/user-attachments/assets/476accba-f5ae-497f-9da9-069e622d980c" />

### aurora serverless:

      - automated DB instantiation & auto scaling based on actual usage
      - good for infrequent, intermittent or unpredictable workloads
      - pay per sec, can be more cost effective
<img width="700" height="650" alt="image" src="https://github.com/user-attachments/assets/680e060b-215e-4924-8363-44a1fe4bf8be" />

### Global Aurora: 

- Aurora cross region read replicas: useful for disaster recovery
- Aurora global DB (recommended

      - 1 primary region(read/write), upto 10 secondary regions(read only), and upto 16 read replicas per secondary region
      - promoting another region (for Disaster recovery) has an RTO(recovery time objective) of less than 1 min 
- *must for exam: typically, cross-region replication takes less than 1 sec* => hint to use Global Aurora
<img width="500" height="800" alt="WhatsApp Image 2026-10-09 at 22 25 19" src="https://github.com/user-attachments/assets/e7ab8c8d-064e-4a5b-b82e-1bb1f6c184bd" />

### Aurora Machine Learning

- enables you to add ML based predictions to your applications via SQL
- simple, optimized, and secure integration between Aurora and AWS ML services (amazon sageMaker(use with any ML model) & amazon comprehand(for sentiment analysis))
- use cases: fraud/anomalies detection, product recommendations, sentiment analysis
<img width="700" height="800" alt="WhatsApp Image 2026-10-09 at 22 43 32" src="https://github.com/user-attachments/assets/765784ba-44ad-4700-b29c-9408598860fe" />

### Babelfish for Aurora PostgreSQL

- allows Aurora PostgreSQL to understand commands targeted for Microsoft MySQL server (eg. T-SQL)
- therefore, Microsoft MySQL based applications can work on Aurora PostgreSQL
- requires no to little code changes
- the same applications can be used after a migration of your DB(using AWS SCT & DMS)
<img width="800" height="800" alt="WhatsApp Image 2026-10-09 at 22 59 33" src="https://github.com/user-attachments/assets/0e66ec80-9e70-46ba-9a1a-5094bda34634" />

## RDS & Aurora: Backups & Monitoring

### RDS Backups

- automated backups:

      - daily full backup of the database, transaction logs are backed-up every 5 mins
      - 1 to 35 days of retention, set 0 to disable automated backups

- manual DB snapshots:

      - manually triggered by the user, retention of backup for as long as you want

- *TRICK: using DB 2hrs in a month, in a stopped RDS DB, you will still pay for storage, instead you should snapshot and restore to use it again (very less pay)*

### Aurora Backups

- automated backups:

      - 1 to 35 days of retention, cannot be disabled (can be disabled in RDS not here)
      - point-in-time recovery in that timeframe
  
- manual DB snapshots:

      - manually triggered by the user, retention of backup for as long as you want

### RDS & Aurora Restore Options

- restoring a RDS/Aurora backup or a snapshot creates a new database
- restoring MySQL RDS DB from S3

      - create a backup of your on-premises(concern) DB
      - store it on the Amazon S3 (object storage)
      - restore the backup file onto a new RDS instance running MySQL
  
- restoring MySQL Aurora cluster from S3

      - create a backup of your on-premises(concern) DB *using Percona XtraBackup*
      - store it on the Amazon S3 (object storage)
      - restore the backup file onto a new Aurora cluster running MySQL

### Aurora DB Cloning

- Create a new Aurora DB cluster from an existing one
- *useful to create a "staging" DB from a "production" DB without impacting the production DB*
- faster than snapshot & restore, very fast & cost effective
- uses *copy-on-write* protocol:
 
      - initially, the new DB cluster uses the same data vol as the original DB cluster (fast & efficient as no copying is needed)
      - when updates are made to the new DB cluster, then additional storage is allocated

## RDS & Aurora SECURITY

- At-rest encryption:

      - DB master & replicas encryption using AWS KMS
      - if the master is not encrypted, the read replicas cannot be encrypted
      - to encrypt an un-encrypted DB, go through a DB snapshot & restore as encrypted

- In-flight encryption: TLS-ready by default, use the AWS TLS root certificates client-side
- IAM authentication: IAM roles to connect to your DB (instead of classic pw & username)
- security groups: control n/w access to your RDS/Aurora DB
- *No SSH available except on RDS custom*
- Audit logs can be enabled and sent to cloudWatch logs for longer retention

## Amazon RDS proxy

- Amazon RDS Proxy is a fully managed, serverless, highly available(multi AZ) database proxy service that sits as an intermediary layer between your application and your Amazon Relational Database Service (RDS) or Aurora databases.
- Connection Pooling: Opening a new database connection for every request consumes significant CPU and memory. RDS Proxy establishes and maintains a pool of connections to the database, sharing and reusing them across multiple application requests. This prevents the database from becoming overwhelmed under heavy load.
- *improving DB efficiency by reducing the stress on DB resources (eg. CPU, RAM) and minimize open connections(and timeouts)*
- *reduced RDS & aurora failover time by upto 66% (If your primary database instance crashes, RDS Proxy automatically detects the failure and reroutes traffic to a standby replica. Because the proxy holds the application's connections open during this process).*
- *enforce IAM authentication for DB, and securely store credentials in AWS secrets manager*
- *RDS proxy is never publicly accessible (must be accessed from VPC)*

<img width="700" height="900" alt="WhatsApp Image 2026-10-11 at 01 09 29" src="https://github.com/user-attachments/assets/57b71a54-d57f-4042-b86d-c9399987d0eb" />

## Amazon ElastiCache

##
