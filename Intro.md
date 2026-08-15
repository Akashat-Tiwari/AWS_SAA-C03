# AWS Intro :

- A region is a cluster of DATA centers like us-east-2(U.S east,Ohio), Asia Pacific (ap-south-1) Mumbai, and ap-south-2 hyderabad
- Each region has many availability zones (3 to 6), usually 3
- AWS services may or may not be global (ie region specific or global)
- In cloud computing, metadata means “data about data.” It describes or provides information about a cloud resource without being the actual content/data stored in it.
-      Example: AWS S3

          - Suppose you upload: photo.jpg

          - The actual file is the data.

          - Metadata can include: file name: photo.jpg
                                 File size: 5 MB
                                 Content type: image/jpeg
                                 Last modified: date/time
                                 Storage class: STANDARD
                                 Owner: user/account
                                 Permissions/access information
                                 Encryption status
-          - Data = the actual photo
             Metadata = information describing the photo

- In total AWS provides 475 services (8 AUG. 2026)
- Audit means systematically checking and reviewing something to make sure it is correct, secure, compliant, and following the rules.
- In simple words, Audit = Check everything and verify what happened.

## IAM : 

- a group can only contain user but cant contain another group
- two users can be in different group
- we can have two different accounts logged in on the same browser by using "ADD sessions" option under "Turn On Multi-Session Support"

- 
 <img width="950" height="500" alt="IAM Policies Structure" src="https://github.com/user-attachments/assets/27de1303-1ae9-4570-9995-9b4aab6e03d5" />

### IAM - Password Policy : 

- strong password
- In AWS, you can set up a password policy
- Multi Factor Authentication : MFA

       - virtual, key , hardware 

### How can users Access AWS ? 

- Three options :

        - AWS Management console (protected by password + MFA)
        - AWS CLI (protected by access keys)
        - AWS SDK(s/w development kit) (protected by access keys)

- user manages their own access keys(generated through AWS console)
- DON'T share your ACCESS KEYS they are private to you

### AWS CloudShell : can be used on specific regions only, just like a CLI or terminal built inside the console


### IAM roles for services : 

- some AWS service will need to perform actions on your behalf
- to do so, we assign permissions to AWS services or AWS entities with IAM roles
- common roles : EC2 instance roles, Lambda function roles, and roles for cloudFormation 

### IAM security Tools :

- IAM credentials report(account-level)
- IAM Access Advisor(user-level)

### IAM guidelines & Best Practices : 

- Dont use the root account except for the AWS account setup
- one physical user = one aws user
- assign users to groups and assign permissions to groups
- create a strong password policy
- use and enforce the use of MFA
- create and use roles for giving permissions to AWS services
- use access keys for programmatic access(CLI/SDK)
- Audit permissions of your account using IAM credentials report and IAM Access Advisor
- never share IAM users and Access keys

### IAM section summary : 

- users : mapped to a physical user, has a password for AWS console
- groups : contains users only
- policies : JSON document that outlines permissions for users or groups
- roles : for AWS entities or AWS services like EC2 instance
- security : MFA + password policy
- AWS CLI : manage your AWS services using the command line
- AWS SDK : manage your AWS services using a programming language
- Access keys : access AWS using the CLI or SDK
- Audit : IAM credential reports and IAM Access Advisor
