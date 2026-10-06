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

## RDS read replicas Vs Multi AZ

- 
