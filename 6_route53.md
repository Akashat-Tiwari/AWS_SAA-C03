# DNS

## what's it?
- DNS is Domain Name System which translates the human friendly hostnames to the IP addresses, eg google => 8.8.8.8 
- DNS uses hierarchical naming structure:

      -       .com
      -     example.com
      -   www.example.com
      -   api.www.example.com
### DNS terminologies
- domain registrar: amazon route 53, GoDaddy
- DNS records: A, AAAA, CName, NS
- zone file: contains DNS records
- name server: resolves DNS queries(authoritative or non-authoritative)
- Top Level domain: .com, .us, .in, .org
- second level domain: example.com

<img width="650" height="350" alt="image" src="https://github.com/user-attachments/assets/74e0cf89-1029-4f59-a9f5-a25e9e8e9245" />

### Very important thing: visualize it 
<img width="980" height="545" alt="WhatsApp Image 2026-10-25 at 16 11 03" src="https://github.com/user-attachments/assets/78e32c54-c51c-40dd-9077-d42ecc5366db" />

# Amazon Route 53 
## *the only AWS service which provides 100% availability SLA ie Service Level Agreement*, there are over 200 fully featured services and total 475 (distinct programmable AWS services)

- A highly available, scalable, fully managed and authoritative DNS (authoritative => the customer can update the DNS records)
- it's also a Domain registrar, health checks can be performed

## Route53-records 
- how you want to route traffic for a domain
- Route53 supports the record types: A, AAAA, CNAME, NS, any few more advanced record types
   
### each record contains:
- sub-domain name ie example.com
- record type: A, or AAAA
- value eg., 12.34.56.78
- routing policy: how route53 responds to queries
- **TTL: amount of time the record cached at DNS resolvers.**

### record types in detail

- A: maps hostname to ipv4
- AAAA: maps hostname to ipv6
- CNAME: maps a hostname to another hostname

      - the target is a domain name which must have an A or AAAA record
      - can't create CNAME record for the top node of DNS namespace (zone apex) eg., you can't create for example.com, but you can create for www.example.com 
- NS: name servers for the hosted zone

### Route53-hosted zones:
- HZs is a container for records that define how to route traffic to a domain and its subdomain
- public HZs: contains records that specify how to route traffic on the internet (public domain names) eg., application1.mypublicdomain.com
- private HZs: contains records that specify how to route traffic within one or more VPCs (private domain names) eg., application1.company.internal
- you pay $0.50 per month per hosted zone

<img width="880" height="508" alt="WhatsApp Image 2026-10-25 at 18 06 45" src="https://github.com/user-attachments/assets/1dae3c55-1048-4393-8844-2c7026c2c12a" />

## Route53 - records TTL (Time To Live)

- high TTL: eg., 24 hrs: less traffic on Route53 DNS, possibly outdated records
- low TTL: eg., 60 sec: more traffic on Route 53 DNS ($$$), records are outdated for less time, easy to change records
- **Except for Alias records, TTL is mandatory for each DNS record.**

<img width="900" height="700" alt="WhatsApp Image 2026-10-27 at 19 24 49" src="https://github.com/user-attachments/assets/fff1d5da-41d4-4940-afca-1f43a3bb858a" />

## CNAME vs Alias

- root domain refers to the domain apex/zone apex (google.com), while a non-root domain refers to a subdomain (www.google.com, mail.google.com)
- AWS resources(LB, cloudFront) expose an AWS hostname: (lb1-123.us.east.elb.amazonaws.com and you want myapp.mydomain.com)
- CNAME:

      - points a hostname to any other hostname(app.mydomain.com -> blabla.anything.com)
      - **only for non root domain (something.mydomain.com)**

- Alias: free of charge, native health checkups

      - points a hostname to an AWS resource (app.mydomain.com -> blablaamazonaws.com)
      - **works for both root and non root domain (mydomain.com)**
      - automatically recognizes changes in the resource's IP Address
      - you can't set the TTL (only type of record in which its not mandatory)
      - Alias record is always of type of A/AAAA for AWS resources (ipv4/ipv6)
  
<img width="300" height="580" alt="WhatsApp Image 2026-10-31 at 17 28 21" src="https://github.com/user-attachments/assets/3b9e741a-25cb-457e-9427-b04bf837a068" />

- Alias records TARGETS:

      - Elastic LB, cloudFront distributions, S3 websites (not bucket), API gateway, Route53 record (same Hosted zone)
      - elastic beanstalk environments, VPC Interface endpoints, global accelerator
      - *you cannot set an Alias record for an EC2 DNS name*

## Health Checks
- http health checks are only for public resources
- health checks are used for *Automated DNS Failover*

      - 1. HC that monitor an endpoint (application, server, other AWS resource)
      - 2. HC that monitor other health checks (calculated HCs)
      - 3. HC that monitor CloudWatch alarms(full control) eg., alarms on RDS, custom metrices ... (helpful for private resources)
  
- health checks are integrated with CW metrics

 ### HCs- Monitor an Endpoint
 - about 15 global health checkers will check the endpoint health

       - healthy/unhealthy threshold: 3 (by default)
       - interval - 30s (fast health checks- 10s but more cost)
       - supported protocol: http, https, TCP
       - if > 18% of health checkers report the endpoint is healthy, route53 consider it healthy else unhealthy
- health checks pass only when the endpoint responds with the 2xx and 3xx status codes
- health checks can be setup to pass/fail based on the text in the first 5120 bytes of the response
- *configure you router/firewall or "security groups" to allow incoming requests from route53 health checkers*
<img width="600" height="700" alt="WhatsApp Image 2026-11-01 at 18 52 40" src="https://github.com/user-attachments/assets/2cc0dc70-7d3e-4a52-af2c-a8d7a921b34b" />

### HCs- Calculated Health checks
- combine the result of multiple HCs into single health check, can use AND, OR, or NOT
- can monitor upto 255 child HCs
- specify how many of the child health checks need to pass to make the parent health check pass
- usage: perform maintenance to your website without causing all HCs to fail
<img width="600" height="800" alt="WhatsApp Image 2026-11-01 at 18 52 40" src="https://github.com/user-attachments/assets/ef575399-2f6d-4a64-8876-33bcbce4b0c7" />

### HCs- Private Hosted Zones
- Route53 health checkers are outside the VPC
- they can't access private endpoints(private VPC or on-premises resource)
- you can create the cloudWatch metric and associate an cloudWatch alarm, then create a health check that checks the alarm itself


## Route53- Routing Policies
- word 'routing' doesn't mean 'routing the traffic' or load balancer routing
- it means how Route53 responds to DNS queries
- Route53 supports these routing policies: simple, weighted, latency based, failover, geo-location, geo-proximity, multi-value answer 

### Simple
- typically, route traffic to a single resource
- can specify multiple values in the same record, *if mutiple values are returned, a random one is chosen by the client*
- when Alias is enabled, specify only one AWS resource
- can't be associated with health checks
<img width="700" height="780" alt="WhatsApp Image 2026-10-31 at 21 31 09" src="https://github.com/user-attachments/assets/82a3c24b-ba1d-4bba-aabf-01a5a2f9f46d" />

### weighted
- control the % of the request that go to each specific resource
- assign each record a relative weight: traffic(%) = weight for a specific record/ sum of all the weights for all records
- weight don't need to sum up to 100
- DNS record must have the same name and type
- can be assiciated with health checks
- assign a weight of 0 to a record to stop sending traffic to a resource,
- if all the records have weight of 0, then all records will be returned equally
<img width="700" height="800" alt="WhatsApp Image 2026-10-31 at 22 02 57" src="https://github.com/user-attachments/assets/e2a7d367-1c39-4809-b343-a9c44e62e329" />

### latency based
- redirect to the resource that has the least latency close to us
- latency is based/measured on traffic between users and AWS regions
- can be associated with Health checks(capable of failovers)

### failover (Active-Passive)
- <img width="650" height="500" alt="WhatsApp Image 2026-11-07 at 00 09 01" src="https://github.com/user-attachments/assets/657ed9b7-855f-478a-868e-59e13d1be5c8" />

### geo-location
- this routing is based on the user location, specify location by continent, country, or other(if there's is overlapping more precise location is selected)
- should create a default record (in case there's no match on location)
- use case: websites localization, restrict content distribution, load balancing
- can be associated with health checks
- like French verion for france, german version for germany, and english version as default(other location)

### geo-proximity
- used when you need to shift traffic from one region to other (by using bias)
- route traffic to your resources based on the geographic location of users and resources
- ability to shift more traffic to resources based on the defined **bias**
- to change the size of the geographic region, specify bias values

      - to expand (1-99): more traffic to the resource
      - to shrink (-99 to -1): less traffic to the resource
- resources can be: AWS resources (specify AWS region), NON- AWS resouces (specify the longitude or latitude)
- to use this feature, you must use **advanced** Route53 traffic flow
<img width="780" height="500" alt="WhatsApp Image 2026-11-10 at 00 11 59" src="https://github.com/user-attachments/assets/34b7901c-220c-4081-b7bc-2ab6820e8653" />

### IP based routing
- routing is based on the clients' IP addresses
- you provide a list of CIDRs for your clients and the corresponding endpoints/locations (user-IP-to-endpoint mappings)
- use case: reduce network costs, route end users from a particular ISP to a specific endpoint
<img width="600" height="800" alt="WhatsApp Image 2026-11-10 at 00 34 35" src="https://github.com/user-attachments/assets/d0405711-d5ed-42e5-be46-d944c7e58e04" />

### Multi-Value answer routing
- use when routing traffic to multiple resources, route53 return multiple values/resources
- can be associated with health checks (return values only for healthy resources)
- *upto 8 healthy records are returned for each multi-value query*
- **multi-value is not a substitute for having an ELB**

      - Multi-Value Routing: DNS-level distribution of traffic.
      - ELB: Actual load balancing of application traffic across multiple servers.
- record name(www.example.com 3times), record type(A, A, A), value(192.0.2.2, 192.0.2.1, 192.0.2.3), ttl(60,60,60), setID (web1, web2, web3), health check (a,b,c)

## 3rd party(GoDaddy) domains & Route53

### domain registrar Vs DNS service ie **Domain Registrar != DNS service**

- you buy or register your domain name with a domain registrar (by paying annual charges), like GoDaddy, Amazon Route53, etc
- the domain registrar provides you with a DNS service to manage your DNS record
- but you can use another DNS service to manage your DNS records
- example, purchase the domain from GoDaddy and use Amazon Route53 as DNS service to manage your DNS records
- *DNS delegation* : DNS delegation is the process of telling the internet which nameservers are responsible for resolving your domain. ex: GoDaddy + Route53

      -  You purchase example.com from GoDaddy.
      - You create a Public Hosted Zone for example.com in Route 53.
      - Route 53 provides four authoritative nameservers (NS records).
      - You enter these four nameservers in GoDaddy's domain settings.
      - When someone searches for example.com, DNS resolvers can find the Route 53 nameservers.
  
## Route53 Resolver & Hybrid DNS
### Route53 resolver
- by default, Route53 resolver answers DNS queries for: local DN for EC2 instance, records in private hosted zones, records in public hosted zones

### Hybrid DNS
- Hybrid DNS: Allows "AWS systems (VPC or Route53 resolver)" and "your networks" to resolve each other's internal domain names or DNS queries.
- networks can be: VPC itself (peered VPC), on premises data centers (connected to direct connect or AWS VPN)  

### Route53 Resolver- Endpoints
- Route 53 Resolver Endpoints act as communication points between AWS DNS and external DNS servers, such as DNS servers in an on-premises data centre.
- inbound endpoints: An inbound endpoint allows DNS queries from an external network (on-premises) to enter AWS and be resolved by the Route 53 VPC Resolver.

      - On-premises Client → On-premises DNS Server → Inbound Resolver Endpoint → Route 53 VPC Resolver → AWS Private Hosted Zone → DNS Answer returned.
- outbound endpoint: An outbound endpoint allows DNS queries originating inside an AWS VPC to be forwarded to external DNS servers, such as an on-premises DNS server.

      - EC2 Instance → Route 53 VPC Resolver → Resolver Forwarding Rule → Outbound Resolver Endpoint → On-premises DNS Server → DNS Answer returned.

