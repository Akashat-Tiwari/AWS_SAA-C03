# High Availability and Scalability

- scalability means that an application can handle greater laods by adapting

      vertical and horizontal scalability(=elasticity) 
      
- scalability is linked but different to HA 

### vertical scalability

- it means incr the size of the instance(h/w) eg. t2.micro-> t2.large
- use case: for non distributed systems such as database, RDS, elastiCache
- you can't vertically scale a system infinitely, as there is hardware limits
- incr the instances is scale out, decr is scale in 

### horizontal scalability(elasticity)

- it means incr the number of instances 
- use case: distributed systems, web apps, modern apps
- easy due to AWS EC2 like features

## High Availability

- it means running your application/system in atleast 2 data centers(==AZ)
- the goal of HA is that the application could survive the data center loss

### *smallest EC2 instance in AWS: t2.nano => 0.5GB RAM & 1 vCPU, largest is u-t12tb1.metal => 12.3TB RAM & 448 vCPU* 


## ELB overview

## load balancers
- load balancers(managed by AWS): are servers that forward traffic to multiple servers
- expose a single point of access (DNS) to your application
- provide SSL(secure TCP) termination (HTTPS) for your website
- separate public traffic from private traffic
- uses regular health checks for instances(done on a port and a route) => protocol = HTTP, port = 4567,endpoint = /health
- if the response is not 200 OK, then the instance is unhealthy

## Types of load Balancers (4 kinds)
- Classic LB (V1-old gen)-2009-CLB : HTTP, HTTPS, TCP, SSL
- Application LB (v2-new-gen)-2016-ALB :  HTTP, HTTPS, WebSockets
- Network LB (v2-new-gen)-2017-NLB : TCP, TLS(secure TCP), UDP
- Gateway LB (v2-new-gen)-2020-GWLB : operates at layer3 (network layer)- IP protocol

- LB security groups: the EC2 instance is only allowing the traffic if the traffic originates from the LB

  <img width="700" height="600" alt="WhatsApp Image 2026-09-28 at 23 41 43" src="https://github.com/user-attachments/assets/261ce805-5bfb-41e7-8fec-ac54f93dd3da" />

## ALB

- layer7(HTTP)ie application layer LB, load balancing to multiple applications on the same machine, support for HTTP/2 and websocket
- routing tables to different target groups:

      - routing based on path in url (eg. example.com/users), hostname in url (eg. one.example.com) and query string, headers (eg. example.com/users?id=123&order=false)

- ALB are used for microservices and container-based applications (eg. Amazon ECS, docker)

<img width="700" height="9=600" alt="WhatsApp Image 2026-09-30 at 00 12 36" src="https://github.com/user-attachments/assets/1ecd193d-eb46-4845-802e-c70dd39d8fee" />

- Target groups:

      - EC2 instances (can be managed by an ASG)-HTTP
      - ECS task (managed by ECS itself) -HTTP
      - Lambda functions
      - IP address (private only) routing
      - ALB can route to multiple target groups

<img width="700" height="600" alt="WhatsApp Image 2026-09-30 at 00 23 04" src="https://github.com/user-attachments/assets/c8d317f3-27a6-4169-83d1-d7dff2c76051" />

- good to know:

<img width="700" height="600" alt="WhatsApp Image 2026-09-30 at 00 30 10" src="https://github.com/user-attachments/assets/ceced9ae-4aba-471d-9499-fd7407ba28e1" />

<img width="700" height="600" alt="WhatsApp Image 2026-09-30 at 22 43 06" src="https://github.com/user-attachments/assets/03ec7a1e-0a35-4f55-a075-cde9f00f311f" />

## Network Load Balancer
- layer4(TCP & UDP) ie transport layer LB, extremely high performance, ultra-low latency
- handles millions of TCP/UDP traffic request per sec
- NLB has *one static/elastic IP per AZ* , and supports assigning Elastic IP

<img width="700" height="600" alt="WhatsApp Image 2026-10-01 at 11 54 33" src="https://github.com/user-attachments/assets/90c56d8e-f5fa-4b8d-be97-6971a4935683" />


- Target Groups:
 
      - EC2 instances, IP addresses (must be private IPs and hardcoded), ALB
      - *In NLB, health checks support the TCP, HTTP and HTTPS protocols*

<img width="800" height="400" alt="WhatsApp Image 2026-10-01 at 12 02 52" src="https://github.com/user-attachments/assets/dcf5ce3c-3a23-4a37-aafe-50e9bee4949f" />


## Gateway Load Balancer(GWLB)

- layer3 (network layer) - IP packets
- deploy, scale and manage a fleet of 3rd party virtual network appliances in AWS
- use case: firewalls, intrusion detection & prevention systems, deep packet inspection system, payload manipulation
- *uses GENEVE protocol on the port 6081*   => GWLB

<img width="500" height="700" alt="WhatsApp Image 2026-10-01 at 23 18 53" src="https://github.com/user-attachments/assets/1e035a3b-fedf-46c6-9139-ef3032cb0edb" />

- Target groups:

      - EC2 instances, IP addresses (private & harcoded)
      - diagram is exactly same as that of TG of NLB

## Sticky Sessions (session affinity)

- same client is always redirected to the same instance behind the LB
- can work/implement for CLB, ALB, NLB
- client request LB for stickiness using cookie
- the *cookie* used for stickiness has an expiration date you control
- cookie names:
 
      - Application-based cookies:
                  - custom cookie: generated by target application, can't use AWSALB, AWSALBAPP, AWSALBTG(reserved for use by ELB) 
                  - application cookie: generated by LB, cookie name is AWSALBAPP
  
      - Duration-based cookies:
                  - generated by LB, cookie name is AWSALB for ALB and AWSELB for CLB

- to enable stickiness: TG level-> select TG-> actions-> edit attributes-> tun ON stickiness

## 
