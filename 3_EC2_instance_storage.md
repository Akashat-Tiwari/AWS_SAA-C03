# EBS Overview

- an EBS(Elastic Block Store) volume is a network drive(not a physical drive) you can attach to your instance while they run
- to kept data intact, even after the termination of instance
- for CCP: one EBS volume can only be attach to one EC2 instance, while associate level: "multi-attach" feature for some EBS
- bound to specific AZ (a EBS volume in ap-south-1 cannot be attach to ap-south-2)
- its a network drive ie uses network to communicate the instance, means there might be a bit of latency
- have a pre-defined capacity (size in GBs, and IOPS => input output operations per sec), can increase the capacity over time if required


  <img width="700" height="600" alt="WhatsApp Image 2026-09-21 at 23 52 35" src="https://github.com/user-attachments/assets/d73d699c-64ad-45ec-a189-50bd2df7a75d" />

- "delete on Terminate" attribute : controls the EBS volume behaviour when an EC2 instance terminates

      - by default, the root EBS volume is deleted (attribute enabled)
      - by default, any other attached EBS volume is not deleted (attribute disabled)
      - use case: preserve root volume when instance is terminated

## EBS Snapshots

- backup(snapshot) of the EBS volume at a point of time
- not necessary to detach the volume to do snapshot, but its recommended
- can copy snapshots across AZ or region
- EBS snapshots FEATURES :

      - EBS snapshot Archieve: move snapshot to "archieve tier"(75% cheaper), takes within 24-72 hrs for restoring the archieve
      - recycle Bin for EBS snapshot: to recover the snapshots after accidental deletion, specify the retention period (from 1 day to 1 year)
      - Fast Snapshot Restore(FSR): forcefull initializaton of snapshot to have no latency on the first use,(simply: forcefully restoring the snapshot), very expensive feature

  <img width="400" height="400" alt="WhatsApp Image 2026-09-22 at 23 58 55" src="https://github.com/user-attachments/assets/760e214c-2600-4719-b770-8e303543f976" />

- *EBS volume is specific AZ bound but snapshot is not(EBS vol in AZ1 -> snapshot -> restore it but in different AZ -> EBS vol in AZ2)

## AMI Overview

- Amazon Machine Image : template used to create an EC2 instance.
- When you rent a virtual, empty server (EC2), you need an operating system and software to make it run. The AMI is the pre-packaged bundle that contains all of that software.
- AMI are built for a specific region (can be copied across regions)
- Every time you launch a new EC2 instance, you must select an AMI first

      - a public AMI (provided by AWS) like Amazon Linux 2023
      - your own AMI (make and maintain them by yourself)
      - An AWS marketplace AMI: an AMI someone else made and sold it

- AMI process
<img width="700" height="600" alt="WhatsApp Image 2026-09-23 at 23 00 54" src="https://github.com/user-attachments/assets/80dcb7d7-184b-4fe4-bf59-282201a7347b" />

## EC2 instance store 

- the EC2 Instance Store is the built-in, physical hard drive attached directly to the EC2 server.
- Fixed size based on the instance type while EBS vol: Can increase size or detach and move to a new instance 
- ephemeral storage: Your data will be permanently lost if, You Stop/terminate the instance and the physical hardware fails.
- good only for temporary content/cache/buffer
- Because of the risk of accidental data loss, AWS highly recommends using EBS volumes for almost everything: standard applications, databases

## EBS vol types (6 types) 

- gp2/gp3 : general purpose SSD vol that balances price and performance
- io1/io2 block express : highest performance SSD vol for critical, low-latency, high-throughput workloads
- st1 : low cost HDD vol designed for frequently accessed workloads
- sc2 : lowest cost HDD vol designed for less frequently accessed workloads
- * only gp2/gp3 and io1/io2 block express can be used as boot volumes(root OS is going to run)

### general purpose SSD ie gp2/gp3

- 1GB-16TB
- both are used for cost-effective storage
- in gp3 you can independently set the IOPS and the throughput, whereas for gp2 they are linked together

### provisioned IOPS(PIOPS) SSD ie io1/io2 block express

- 4GB-16TB
- great for databases workloads
- PIOPS (io1/io2) supports EBS multi-attach feature

### Hard Disk Drives(HDD) ie st1/sc1

- 125GB- 16TB
- cannot be a boot vol
- throughput optimised HDD => st1
- cold HDD => sc1(lowest cost)

# boot volume => both gp2/gp3 and io1/io2 included (st1/sc1 not included),  EBS multi-attach => io1/io2 block express

## EBS multi-attach(AZ bounded)

- attach the same EBS vol to multiple EC2 instances in the same AZ
- ** limitation: upto "16" EC2 instances at a time (not more than it)
- use case: higher application availibility in clustured linux application (eg. Teradata)

## EBS encryption 

- encryption & decryption are handled in behind by EC2 and EBS(nothing to do)
- EBS encryption leverages keys from KMS (AES-256)
- Encryption:

      - create an EBS snapshot of the EBS vol
      - encrypt the EBS snapshot [using copy]
      - create new EBS vol from this snapshot(vol will also be encrypted) and you can attach the encrypted vol to the original instance
      - shortcut: EBS vol-> EBS snapshot-> restore/create vol from snapshot(check the encryption block) -> created vol is encrypted

  ## Amazon EFS: Elastic File System

- managed NFS(network file system) that can be mounted on many instances (instances can be in different AZ)
- highly available, scalable, expensive(3x of gp2), pay per use
- 
<img width="700" height="600" alt="WhatsApp Image 2026-09-26 at 18 54 23" src="https://github.com/user-attachments/assets/07e1e92b-9bcd-4aa9-aaee-9787ed9d44e3" />

- use cases: content management, web serving, data sharing, wordpress, uses NFSv4.1 protocol internally
- only compatible with linux based AMI(not windows)
- EFS scales automatically, pay-per-use, no capacity planning in advance!
- EFS performance:

      - performance mode(set at EFS creation time): GP(default): (web server) and Max i/o(big data, media processing)
      - throughput mode:
                        - bursting: provides throughput that scales with the amount of storage for workloads
                        - provisioned: if you estimate the throughput requirement, you configure the throughput and pay for it 
                        - elastic(recommended): regardless of the size of storage , give the required throughput or i/o ie for unpredictable i/o or throughput

### EFS storage classes:
- storage tiers (lifecycle management feature-move file after N days)

      - standard: for frequently accessed files
      - infrequent access (EFS-IA): cost to retrieve files, lower price to store 
      - archive: rarely accessed data(few times each year), 50% cheaper
      - implement *lifecycle policies* to move files between storage tiers

- best practice: performance mode-> GP, throughput mode-> Elastic and and enhanced

## EBS Vs EFS 

- *EBS*

      - one instance(except multi-attach io1/io2)
      - are locked at the AZ level
      - gp2: io incr if the disk size incr, gp3/io1: io incr independently
      - to migrate EBS vol across AZ, take a snapshot. restore the snapshot to another AZ
      -* root EBS vol of instances get terminated by default if the instance gets teminated (you can desable it) *

- *EFS*

      - can be attached with 100s of instances across AZs
      - only for Linux instances (POSIX)
      - EFS has higher price than EBS
      - can leverage storage tiers for cost savings
