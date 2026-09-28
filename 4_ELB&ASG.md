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

- 
