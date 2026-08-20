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
 
 ### EC2 instance types :
     
- AWS has the following naming convention :-

      - m5.2xlarge
           - m : instance class
           - 5 : generation
           - 2xlarge : size within the instance class

### general purpose
-  t2.micro/t3.micro : general purpose EC2 instance, that provides a balance between compute, memory, networking
-  used for web servers and code repositories as they require similar proportions of resources 

### compute optimised : 
- great for compute intense tasks that requires high performance processors :

      - batch processing workloads
      - high performance web servers
      - scientific modelling and machine learning
      - high performance computing (HPC)
      - dedicated gaming servers

### memory optimised : 
- fast performance for workloads that process large data sets in memory

        - 
