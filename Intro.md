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

 
