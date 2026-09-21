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
