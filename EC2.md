# EC2 : 

- elastic compute cloud : infrastructure as a service
- EC2 instances, EBS, ELB, ASG, etc
- which O.S, compute power(CPU cores), RAM, EBS, EFS, network card, firewall rules, security group, bootstrap script


### EC2 user data

- it is possible to bootstrap our instances using an Ec2 user data script
- launching some commands when a machine starts: bootstraping

        - installiing updates, installing softwares, downloading common files from the internet

 - The EC2 user data script runs with the root user

### Launching an EC2 instance running Linux 

- user data script only run once, when the system starts
- if you stop an instance and you start it later, the associated public IP will change but the private IP will remains the same 
 
 ### EC2 instance types : there are 7 different types 
     
- AWS has the following naming convention :-

      - m5.2xlarge
           - m : instance class
           - 5 : generation
           - 2xlarge : size within the instance class

### 1. general purpose
-  t2.micro/t3.micro : general purpose EC2 instance, that provides a balance between compute, memory, networking
-  used for web servers and code repositories as they require similar proportions of resources 

### 2. compute optimised : 
- great for compute intense tasks that requires high performance processors :
- starts with C name like C5, C4, etc
  
      - batch processing workloads
      - high performance web servers
      - scientific modelling and machine learning
      - high performance computing (HPC)
      - dedicated gaming servers

### 3. memory optimised : 
- fast performance for workloads that process large data sets in memory
- starts with R, X, Z

      - applications performing real time processing of big unstructured data
      - In- memory databases optimisation for BI(business intelligence)
      - Distributed web scale cache stores

### 4. storage optimised :
- great for storage intensive tasks that require high, sequential read and write access to large data sets on local storage
    
      - high freq. online transaction processing (OLTP) systems
      - relational and NoSQL databases
      - cache for in memory databases

### observation : x relates to vCPU
      - t2.micro: 0 vCPU, t2.xlarge: 4 vCPU, m5.2xlarge: 8 vCPU

## Security group : 

- are fundamental of the network security in AWS
- how traffic is going into or out of the EC2 instance
- security group only contain "allow" rules
<img width="400" height="500" alt="image" src="https://github.com/user-attachments/assets/4bc8fec6-8373-4092-a6fd-41462493325d" />






- security groups are acting as firewall on EC2 instances

      - they regulate : access to ports, authorised IP ranges (IPv4-IPv6)
                        control of inbound network(from other to instance) and outbound network(from instance to other)

<img width="800" height="500" alt="image" src="https://github.com/user-attachments/assets/6e89342f-d682-4feb-a689-28b81f680f08" />





- security group can be attached to multiple instances
- its good to maintain a separate security group for SSH access
- ***if your application is not accessible ie "time out" then its maybe a security group issue but if its a "connection refused" then its a application error

- all inbound traffic is blocked and outbound traffic is authorised by default

- referencing other security groups:
 <img width="750" height="500" alt="image" src="https://github.com/user-attachments/assets/20033946-576d-4eb5-bcef-4b9b70e5effe" />


### classic ports to know :

- 22 : SSH(secure login): log into a linux instance
- 21 : FTP(upload files into a file share)
- 22 : SFTP(upload files using SSH)
- 23 : Telnet(login)
- 80 : HTTP(access unsecured websites)
- 443 : HTTPS(access secured websites)
- 3389 : RDP(remote desktop protocol): log into a windows instance
  
### SSH overview:

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/e688219c-ed94-4afd-b45a-c0b28482ed0f" />

### SSH using Linux/Mac :

- SSH allows you to control a remote machine, all from the CLI
- it like controling the remote machine if you're inside it , that's all from the CLI

- To SSH from your Linux/WSL terminal into an AWS EC2 instance, you need:

      - A running EC2 instance
      - Its public IPv4 address or public DNS
      - The .pem key pair file used when launching the instance
      - Port 22 (SSH) allowed in the Security Group
      - ssh -i YOUR_KEY.pem USERNAME@PUBLIC_IP , username = ec2-user, ubuntu = ubuntu, i = identity

## procedure :

### SSH into an AWS EC2 Instance from Linux

- Launch an **EC2 instance** on AWS.
- While launching it, **create or select a Key Pair**.
- When creating a new Key Pair, download the file ending in **`.pem`**.
  - Example: `my-ec2-key.pem`
  - This file is your **private SSH key**.
  - Keep it safe and never share it.

- Make sure your EC2 instance is in the **Running** state.

- Go to:
  - **AWS Console → EC2 → Instances**
  - Select your EC2 instance.

- Copy the instance's **Public IPv4 address**.
  - Example: `13.233.100.50`

- Check which operating system your EC2 instance is using:
  - **Ubuntu** → Username: `ubuntu`
  - **Amazon Linux** → Username: `ec2-user`

- Ensure the EC2 **Security Group** allows SSH:
  - Type: `SSH`
  - Protocol: `TCP`
  - Port: `22`
  - Source: Preferably **My IP**

- Open your **Linux/WSL terminal**.

- Go to the folder containing your `.pem` file:

- Give the .pem file secure permissions:
   - chmod 400 my-ec2-key.pem : owner-read, others-no acces

- ssh -i YOUR_KEY.pem USERNAME@PUBLIC_IP
  
- your terminal prompt will change to something similar to:
   -[ec2-user@ip-172-30-4-258 ~]$

- You are now connected to and controlling your AWS EC2 instance remotely.


## EC2 instance connect: 

- instance-> connect-> press final connect -> AWS CLI (connected)
- instance-> security-> security group-> inbound rules-> remove 22-> not it won't connect
- again add 22-> then it will connect to EC3 instance connect

### to provide aws credentials(permissions/access) to an EC2 instance by IAM roles only 
- instance->actions->security->modify iam roles->choose iam role->save


### EC2 instances purchasing options :
<img width="750" height="500" alt="WhatsApp Image 2026-09-18 at 12 26 52" src="https://github.com/user-attachments/assets/392dbbec-eeff-4b99-b6f1-4a87e03d8f11" />

- EC2 on demand : pay for what you use, linux/windows : billing per sec, after the first min, other O.S : billing per hour

      - highest cost, but no long term commitment
      - recommended for "short-term and un-interrupted workloads", where you can't predict how the application will behave

- EC2 reserved instances : upto 72% discount compared to on demand, reservation period: 1y(+discount)/3y(+++discount)

      - payments options : no upfront(+), partial upfront(++), all upfront(+++) 
      - reserve a specific instance attributes (instance type,region,tenancy,os)
      - reserved instance's scope : regional/zonal (reserve capacity in an AZ)
      - recommended for steady-state usage applications(think database), you can sell/buy in the reserved instance marketplace

- convertible reserved instance : upto 66% discount

      - can change the EC2 instance attributes (instance type, instance family, OS, tenancy, scope)

- EC2 savings plans : upto 72% (same as RIs)

      - usage beyond EC2 savings plans is billed at the on-demand price
      - locked to a specific family and region (eg, M5 in ap-south-1)
      - Flexible across :
                      - instance size(eg, m5.xlarge, m5.2xlarge)
                      - OS
                      - tanancy (host, dedicated, default)

- spot instances : upto 90% discount compared to on-demand, making it the most "cost-efficient" instances in AWS

      - instances that you can "lose" at any point of time if your max price is less than the current spot price
      - useful for workloads that are resilient to failure : batch jobs, data analysis, image processing, any distributed workloads, workloads with a flexible start            and end time
      - not suitable for critical jobs or databases

- EC2 dedicated hosts : access to a physical server, with EC2 instance capacity fully dedicated to your use
     
      - allows you to use your existing server-bound software licenses (per-socket, per-core, pe-VM software licenses)
      - purchasing options : on-demand: pay per sec for active dedicated host and reserved-1/3y (no upfront, partial upfront, all  upfront) 
      - the most expensive option
      - useful for s/w that have complicated licensing model(BYOL) or for companies that have strong regulatory or compliance needs

- EC2 dedicated instances : own instance own hardware

      - instances run on h/w that's dedicated to you
      - may share the h/w with other instances in same account but never with different customer
      - no control over instance placement (can move h/w after start/stop)

- EC2 capacity reservations :

      - reserve on demand instance capacity in a specific AZ for any duration
      - no time commitment(create/cancel anytime), no billing discounts 
      - combined with Regional reserved instances and saving plans to benefit from billing discounts
      - you are charged at on-demand rate whether you run instances or not
      - suitable for short term , un-interrupted workloads that needs to be in a specific AZ

### you should know which type of instance is the right one based on given workloads 

 <img width="800" height="660" alt="WhatsApp Image 2026-09-18 at 15 06 58" src="https://github.com/user-attachments/assets/4be4f902-10e3-4de4-a33c-7f8c7ed10d13" />


### Spot instances : 

- define max spot price and get the instance while current spot price < max spot price
- once the current spot price > max spot price : you will get a 2 min grace period to stop or terminate (after saving or retreiving your essentials)
- better for distributed workloads (mentioned in above topic), not suitable for critical jobs
  
### A spot request :

<img width="780" height="660" alt="WhatsApp Image 2026-09-18 at 22 16 02" src="https://github.com/user-attachments/assets/d145b685-b59c-418f-94e7-acc8cbb78b28" />


### spot fleets : set of spot instances + on-demand instances(optional)

- spot fleets allow us to automatically request spot instances with the lowest price
- spot fleet will try to meet the target capacity with price constraint

      - define possible launch pools(instance attributes): instance type(m5.large),OS,tenancy,AZ
      - can have multiple launch pools, so the spot fleet can choose
      - spot fleet stop launching instances when reaching max cost or capacity
  
- strategies to allocate spot instances:

      - lowestPrice: from the pool with the lowest price (cost optimisation,short workload)
      - diversified: distributed across all pools(great for availability, long workloads)
      - capacityOptimised: pool with the optimal capacity for the no of instances
      - priceCapacityOptimised(recommended): pools with the highest capacity available, then select the pool with the lowest price (best choice for most workloads)

# EC2-SAA level

## Private, public vs elastic IP 

### IPV4 :
- 2^32 = 4.3 billion but some are reserved for private networks, loopback, multicasting, experimental (0.6 bilion)
- so total addresses available for ordinary public internet allocation are 3.7 billion

### public IP 
- the machine can be identified on the internet(www) ie publicaly available 
- the IP must be unique across the whole web

### private IP
- the machine can only be identified on the private network
- the IP must be unique across the private network
- but the two different private networks(companies) can have same private IPs
- machines connect to WWW using a NAT + internet gateway

### elastic IP
- when you stop & then start an EC2 instance, it can change it public IP, but if you want a fixed public IP for your instance, you need a Elastic IP
- An elastic IP is public IPV4 and you own it as long as you don't delete it
- you can attach it to one instance at a time (ofc)
- you can only have 5 Elastic IP in your account(can be increased by requesting AWS)
- *** used to mask the failure of an instance by quickly switching the address to another instance in your account
- disadvantages : of using an Elastic IP: try to avoid using it  
   
      - poor architectural decisions
      - instead, use a random public IP and register a DNS name to it
      - also, you can use a Load Balancer and don't use a public IP

### IPs in AWS EC2 

- by default, an EC2 instance comes with

      - a private IP for the internal AWS network
      - p public IP for the WWW

- when we are doing SSH into our EC2 machines we can only use the public IP, not the private IP because we are not in the same network

## EC2 placement groups 

### 
