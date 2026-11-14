# Overview
- The progression of a solution's Architect mindset through many sample case studies:
- WhatIsTheTime.com, MyClothes.com, MyWordPress.com, Instantiating applications quickly, Beanstalk

# Stateless Web App: WhatIsTheTime.com
- no database required, small application, and downtime is acceptable
- full vertical and horizontal scaling

## simple
- a client asked the public EC2 instance(t2/t3 micro), what's the time? it returns: 18:09
- maybe the server needs to stop/restart, so an Elastic IP is attached

## scaling the web app
### vertical scaling
- more no. of clients are using the web app, so we use a bigger instance instead of smaller (t2.micro -> m5.large)
- change the type of instance, Elastic IP will remain the same but server will experience a downtime due to upgradation
- this works but its not a great option (downtime)

### horizontal scaling
- tons of client are there, using the web app ,so we need to scale to a great extent
- take multiple EC2 instances(m5.large), but there are multiple elastic IP and clients need to remember them
- but there is only 5 Elastic IP per region per account, so it can't scale much
- we use Route53 and remove Elastic IP
<img width="600" height="300" alt="WhatsApp Image 2026-11-14 at 18 30 04" src="https://github.com/user-attachments/assets/939bb6eb-90ca-4064-9cd2-65508ae5c2c8" />
- **if we need to add/remove an instance**: as server(IP) is chosen randomly for multi value record having simple routing policy,
- so fewer clients that were using the server which is going to removed, will face a downtime (equal to TTL), as the Route53 return the same IP for TTL time

### horizontal scaling further with a LB
- private EC2 instances(ELB facing) in an AZ, and an ELB in same AZ (public facing)
- can't use a A record at Route53 (ofc), so we use alias record (ELB -> AWS resource) 
- **clients -> route53 -> elb(health checks) -> EC2 instances** : no downtime as ELB use health checks and only healthy instances are passed
- adding/removing instances manually is pretty hectic as you have to register it on ELB

### horizontal scaling with an ASG
- scale in or scale out based on the demand
- managed automatically
<img width="700" height="400" alt="WhatsApp Image 2026-11-14 at 19 00 52" src="https://github.com/user-attachments/assets/e20cf1aa-24cd-49a6-83bf-df39a649631a" />

### Disaster management(in case our AZ in down), making our app Multi-AZ
- app is multi-AZ, highly available, and resilient to failure
- <img width="900" height="500" alt="WhatsApp Image 2026-11-14 at 19 12 16" src="https://github.com/user-attachments/assets/9a8f3d31-1858-4cb4-9adf-554156f3636c" />

### cost saving when possible
- min 2 AZs -> let's reserve capacity: we know that two instances must be running throughout the year, so reserve them and reduce cost for ASG
- as ASG not have to go below reserved capacity instances

# MyClothes.com
