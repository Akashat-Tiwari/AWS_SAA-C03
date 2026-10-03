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

## Cross-Zone Load Balancing

<img width="950" height="700" alt="WhatsApp Image 2026-10-03 at 17 47 20" src="https://github.com/user-attachments/assets/4c1e4ffc-c35d-4f19-ae4c-73e0a1fdad56" />

- CLB: disabled by dafault, No charges for inter AZ data if enabled
- ALB: enabled by default(can be disabled at TG level), No charges for inter AZ data
- NLB & GWLB: disabled by default, you pay charges for inter AZ data if enabled

## SSL/TLS (x.509 in LB )

- An SSL cert allows traffic between your clients and your LB to be encrypted in transit(in-flight encryption)
- SSL: secure sockets layer and TLS: Transport Layer Security, new version of SSL (TLS is used nowadays, but SSL is for understanding)
- public SSL certs are issued by Certificate Authorities (CA), eg. comodo, DigiCert, GoDaddy, Globalsign, etc
- SSL certs have an expiration data (you set), and must me renewed
- you can manage certificates using ACM ie AWS Certificate Manager
- clients can use SNI to specify the hostname they reach

- SNI (Server Name Indication):

      - SNI solves the problem of loading multiple SSL certificates onto one Web Server (to serve multiple websites)
      - *it's a newer protocol and requires the client to indicate the hostname of the target server in the initial SSL handshake*
      - only works for ALB & NLB (newer gen), cloudFront and not work for CLB (old gen)

<img width="700" height="650" alt="WhatsApp Image 2026-10-03 at 19 08 55" src="https://github.com/user-attachments/assets/3d2db8be-ee4a-4b9e-af6f-4630e68d1344" />

- CLB: support only one SSL certificate, must use multiple CLB for multiple hostname with multiple SSL certs
- ALB & NLB: supports multiple listeners with multiple SSL certificates, uses SNI to make it work
- ALB & NLB => SSL hands on: ADD Listeners-> ....-> import cert from ACM-> thats it

## Connection Draining(CLB)/de-registration delay(ALB,NLB)

- deregistration is the process of removing a target (such as an EC2 instance, IP address, or Lambda function) from a target group.
- *Once a target is deregistered, the load balancer immediately stops routing new traffic to it*
- time to complete "in-flights requests" while the instance is de-registering or unhealthy
- between 1-3600s(default=> 300s), can be disabled(set value to 0), set low value if your requests are short and high if the they are long (good value is 30s)

## Auto Scaling Group(ASG)

- In real life, the load on the websites & application can change, to adjust the server strength accordingly, ASG are used
- the goal of ASG is to:

      - scale out(add)/scale in(remove) EC2 instances to match the increased/decreased load

- automatically register new instances to a load balancer
- re-create an EC2 instance in case a previous one is terminated (unhealthly)
- ASG are free (only pay for instances)

<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/ecb1cd49-30ef-421b-bf6e-85aaf07e3745" />

<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/453dc045-6ca4-415b-8b08-ad0925ad4ee4" />

- ASG Attributes:

      - a launch template: below image
      - min size/ max size/ initial capacity
      - scaling policies: it is possible to scale an ASG based on CloudWatch Alarms, an alarm monitors a metric(such as avg CPU, or a custom metric){computed for overall ASG}, so based on these alarms we can create scale-in/scale-out policies

<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/44852ea8-32f7-42aa-bd1b-3f545c39c9aa" />

<img width="888" height="240" alt="image" src="https://github.com/user-attachments/assets/d6f8c7cc-d3c5-4dd7-a379-399d2be1f23c" />

##        
