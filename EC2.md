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
