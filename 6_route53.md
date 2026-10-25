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
- TTL: amount of time the record cached at DNS resolvers

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

## 
