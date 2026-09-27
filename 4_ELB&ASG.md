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
- 
