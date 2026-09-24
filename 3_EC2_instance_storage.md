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

## EBS vol types 

- 
