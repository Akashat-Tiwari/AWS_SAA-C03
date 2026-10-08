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


### In AWS terms, think of *Sqlectron as a GUI client for a database* *(use to connect to the remote DB), not as an AWS service itself.

## RDS custom

- RDS custom is only for two database types, its for Oracle and Microsoft SQL Server
- access to OS and underlying database 
- custom: configure settings, install patches, enable native features, access EC2 instance using SSH or SSM session manager
- RDS Vs RDS Custom:
 
      - RDS: entire DB and the OS is managed by AWS
      - RDS custom: full admin access to the underlying DB and the OS

 ## Amazon Aurora

 - 
