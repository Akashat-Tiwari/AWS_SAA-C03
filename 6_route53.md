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
- 

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

### failover
- 
 
