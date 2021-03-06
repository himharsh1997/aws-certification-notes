# aws-certification-notes
Notes per lecture of aws 


## IAM Policy
  In below code we define policy to give permission to user/group/role
  ```
  const policy  = {
    "Version": "2012-10-17", // Policy language version value
    "Id": "admin-permissions",
    "Statement": [
     {
         "Sid": "1.1",
         "Effect": "Allow",  // Allow or Deny access on resource(s)
         "Principal": {
           "AWS": ["arn:aws:iam:1234567890:root"]  // user/account/role
          },
          "Action": [
              "s3:PutObject",
              "s3:GetObject"
          ],
          "Resource": ["arn:aws:s3:::bucket1/*"]
     }
    ]
   }
  ```

 `Either can either create policy or add already existing policy to user else can also add policy directly to that partifular user as inline policy`

### <u>IAM - Policy Password. </u>
 `In this we use options to make users to either :-`
  - Not change password
  - Change in every 90 days forced
  - Allow user to change password

### <u>Multi Factor Authentication(MFA)</u>
- This is used as some users may have access to your account and can possibly change anything in resources you have, overuser also.
- So as we want to protect our root user and other users
- MFA= password + security device you login
- You can use Virtual MFA by installing apps like Authy, Google authenticator

### <u>MFA device options</u> - 
 - Google authenticator(phone only)
 - Authy(multi-device) 
 - It is one of type of MFA device asked by aws when you want to enable MFA in your root account/other IAM user accounts.
 - You can enable MFA for your AWS account and for individual IAM users you have created under your account. MFA can be also be used to control access to AWS service APIs.
 - Universal 2nd Factor(U2F) Security Key.eg; Yubikey byYubico(3rd party to aws and is hardware key). Has support for root user and other iam users, provide single key for all so no need to keep tract of keys of users.
 - Hardware key Fob MFA device(provided by Gemalto. by 3rd party to aws).

### <u>IAM MFA Hands on</u>
 - Go to 'My Security Credentials' and then goto MFA and click 'Activate MFA'
 - We will get 3 options: Virtual MFA device, U2F security device, Other hardware MFA device.

### <u>AWS Access Key, CLI and SDK</u>
  
  ### <u>How to access AWS, we have 3 options</u>:
   - AWS Management console(protected by password + MFA(optional))
   - AWS Command Line Interface(CLI): protected by access keys(access Key ID,secret access Key)
   - AWS Software Development Kit(SDK) - for code: protected by access keys
  #### Note
  `User will have their access keys and they will manage/protect those keys by themselves. treat access Key ID as username, secret access Key as password.`

### <u>AWS CLI</u>
   - Command line tool which enable you to interact with aws services using commands in command-line shell.
   - eg; aws s3 cp smile.jpeg s3://demo-001/smile.jpeg
   - We get direct access of public api's of aws services.
### <u>AWS SDK</u>
   - Language specific APIs(set of libraries).
   - We can access and manage our aws services resources by program using SDK's APIs.
   - Support: Nodejs, Javascript, PHP, .Net, Ruby, Java, Go, Python, Javascript, C++, etc.
   - Also Mobile SDK(Andriod, IOS, ...).
   - IoT Devices SDKs(Embedded C, Aurdino). 

### <u>AWS CloudShell</u>
   - It is browser-based, pre-authenticated shell(Bash, Powershell, z shell) which you can launch directly from aws management console.
   - Not available in all regions.

### <u>IAM Roles for AWS Services</u>
   - As aws services require to perform actions on behalf of iam user(suppose a lambda function want to invoke other lambda function).
   - So we require to provide permissions to AWS services using IAM Roles.
   - These are roles create to get used by aws services, not by users.
   - Common roles: 
    - EC2 instance roles
    - Lambda function roles
    - Roles for cloudformation

### <u>IAM Security Tools</u>
   - **IAM Credentials report (account-level):** report list all of your account users and status of this various credentials. eg; this report give last time account login, is MFA active, access keys generated or not. Help to get know user for enhance security.
   - **IAM Access Advisor(user-level):** show service permission granted to a user and last time those services are used by those user. This is helpfull to know which permissions are not in used and then we can remove those permission from that user.

### <u>IAM Guides and best practices</u>
   - Don't use root account except for aws account setup.
   - One physical user   = one aws user. as sharing credentials may cause missuse. It's better to create seperate user for them with minimal permissions or policy attached to them.
   - Assign users to groups and assign permissions to groups to manage access at group level.
   - Create strong password policy.
   - User yourself MFA. Can also enforce other iam users of your account to enforce use of MFA. This is for safety of user credentils.
   - Create and use roles for giving permissions to AWS services.
   - User access keys for programmatically(cli/sdk) access of aws services.
   - User IAM credentials report for audit permissions of users. also iam access advisor.
   - Never share IAM user and access keys.
   - group cannot  have group(s), only have users.

### <u>EC2 fundamentals</u>

   **<u>EC2 Basics</u>**
   - Used for infrastructure as a service.
   - Used for rent virtual machines, storing data on virtual drives(EBS), destribute load access machines(ELB), scaling services using auto-scaling group(ASG).
   - Learning ec2 fundamental is learning cloud work.
   - For storage space:
       - Network-attached (EBS & EFS)
       - Hardware (EC2 Instance store)
   - Firewall rules: security groups
   - Bootstrap script(configure at first launch): EC2 user data scripts. That means launch/run commands once when machine start(first time). This this uses root user for doing this.

   **<u>EC2 instance launch</u>**
    - Select Amamzon Machine Image(AMI) or your own AMI created.
    - security group base on your comsumer and security.
    - storage EBS
    - user data or script to run at time of instance launch.
    - stop instance if not in use.

   **<u>EC2 Instance Type</u>**
   - General purpose: Balanced in cpu, memory, network resources. it also has differnet family like m5.2xlarge
   - Compute Optimize(C): Great for compute-intensive tasks that require high performance like
     - batch processing workloads
     - media transcoding
     - high performance web server
     - scientific model and machine learning
     - dedicated gaming servers 
     - Compute Optimized EC2 instances are great for compute-intensive workloads requiring high performance processors, such as batch processing, media transcoding, high performance web servers, high performance computing, scientific modeling & machine learning, and dedicated gaming servers.
   - Memory Optimize: Fast performance for worload with large dataset like
     - High performance relational/non-realtional database
     - Distributed web scale cache stores
     - In-memory databases optimized for BI
   - Storage Optimize: For storage-intensive tasks that require high, sequential read and write access to large data in localstorage like
      - High frequency online transaction processing(OLTP) systems
      - Relational& NoSQL database
      - Distributed file system
   - etc

   **<u>EC2 Instance Naming convention</u>**
   -  m5.2xlarge : 
      - m is instance class
      - 5 is generation(aws improve it over time)
      - 2xlarge is size within instance class(size, memory, cpu)

   **<u>EC2 billing</u>**
     - cover later
     - Spot Instances are good for short workloads and this is the cheapest EC2 Purchasing Option. But, they are less reliable because you can lose your EC2 instance.
     - Reserved Instances are good for long workloads. You can reserve EC2 instances for 1 or 3 years
     - Storage Optimized EC2 instances are great for workloads requiring high, sequential read/write access to large data sets on local storage.
     - Dedicated Hosts are good for companies with strong compliance needs or for software that have complicated licensing models. This is the most expensive EC2 Purchasing Option available.
   **<u>Security Groups</u>**
   - For network security in AWS. Like a firewall.
   - Control traffic in or out of EC2 instances.
   - Can be attach to instance to set inbound and outboud rules
   - They control:
    - Access to ports
    - Authorized ipv4 and ipv6
    - Control inbound(traffic from out to in instance).
    - Control outbound(from instance to other).
   - We can attach single group to multiple instances, also one instance with multiple groups
   - Security group are outside instance, so Ec2 instance won't know what traffic are blocked.
   - Good practice to maintain seperate security group for SSH access.
   - If you gettin connection refused error than it is application error not security groups error.
   - All inbound traffic is blocked by default.
   - All outbound traffic is authorized by default.

   **<u>Reference security group from other security groups</u>**
    - If we have Instance1 with inbound security group 1, security group 2. 
    - Then if Instance2 with security group 2 and Instance3 with securoty group 1 try to call it can do it without timout due to having 
   
   **<u>Ports to know</u>**
   - 22 = SSH(Secure Shell) - log into a linux shell.
   - 21 = FTP(File Transfer Protocol) - upload fiels in file share.
   - 22 = SFTP(Secure File Transfer Protocol) - upload file using SSH.
   - 80 = HTTP - access unsecured websites
   - 443 = HTTPS - access secured websites.
   - 3389 = RDP(Remote Desktop Protocol) - login in window instance.
 
  > **Note**s: 0.0.0.0/0 means from anywhere on ipv4 and ::/0 means from anywhere on ipv6

  ### <u>SSH Summary</u>
  - Protocal to communicate securely with remote server using command execution from terminal only.
  - SSH can we used in macos, linux and window >= 10. For windows < 10 we can use **putty** to use ssh protocol to connecto server(key filename has extension ppk).
  - We can also use ssh connect(from all browser). But only applicable to Linux EC2 instances for now. But make sure inbound rule allowed it.

  ###  <u>Work with SSH connect</u>
  - You will have key file like key_name.pem downloaded at time of ec2 instance created.
  - Only those user with that file can access ec2 using ssh
  - First time when you try to connect with command `ssh ec2-user@x.y.z.m` where x,y,z,m are ipv4 values you will ge  error of permission deny as only user with  key file can access.
  - Next time if you try to connect with command `ssh -i filename ec2-user@x.y.x.m` you will get bad permission 06  (-rw-r--r-) as due to security reason ssh won't allow you to use key file with other having access to read.
  - So run `chmod 0400 filename` to change permission
  - Now try again connect using `ssh -i filename ec2-user@x.y.x.m` you get connected to ec2 instance. Hurrah!!!
  - Make sure you ec2 instance also having ssh utility installed
  - Permission denied (publickey,gssapi-keyex,gssapi-with-mic) error cause due to wrong key file/ wrong username(ec2-user).


  ><u>Note</u>: Never store your ec2 credential in ec2 instance as it may get use by other users which make this as bad practice. So it better to attach IAM role to service as good practice instead of adding aws access keys.

### <u>EC2 instance purchase option or launch type</u>
- **On demand Instances**: short workload, predicatable pricing.
- **Reserved Instance**: Minimum 1 yr which can be use in pattern -
   - Reserved Instances: long workload
   - Convertable reserved instances: long workloads with flexible instance
   - Scheduled reserved instances: eg every thursday between 3 to 6 pm.
- **Spot Instances**: 


### <u>EC2 on demand</u>
- Pay as you use:
 - Linux - billing per second after first minute.
 - other OS: billing per hour.
- No need to pay in advance/upfront for use.
- No long-term commitment.
- recommended for short-term and un-interrupted workloads, where you can't predict how the application will behave.

### <u>EC2 reserved instances</u>
- Upto 75% discount compared to on-demand.
- Reservation period: 1 yr = +discount | 3 yr = +++discount
- Purchasing option: no upfront(pay monthly) | partial upfront = + | All upfront(pay right before) =++discount
- We can reserved a specific instance.
- used for steady-state usage application(eg; database).
- **Convertible reserved instance option**:
  - can change EC2 instance type.
  - upto 52% discount.
- **Scheduled reserved instances**:
  - launch within time window you reserve.
  - Use when you require it for fraction of day/week/month.
  - Need commitment of 1 to 3 years.
- **EC2 Stop instances**:
  - Provide discount upto 90% compared to on-demand.
  - instances that we can lose at any point of time if your max price is less than current spot price.
  - most cost-effecient way in aws.
  - Good for short workloads.
  - usefull for workloads that are resilient/stong enough to failure.
    - batch jobs.
    - data analysis.
    - image processing.
    - any distributed workloads.
    - workloads with flexible start, end time. 
  - Not suitable for critical jobs or databases.
  - Daily life example: Like all people in hotel can bid for empty rooms but highest bidder keeps the room, other kicked out
- **EC2 dedicated hosts**:
  - amazon ec2 dedicated host is a phyical server with EC2 instance capability fully dedicatedto your use.
  - Dedicated host help you to address compliance requirements and reduce costs by allowing you to use your existing server bound licenses.
  - Required 3yr commitment for reservation.
  - More expensive.


>Note: We can reserve instance either for full 1 yr or full 3 yr(not in mid like 2 or more than 3).

## EC2 Instance Storage 

 **<u>EBS Volume</u>**:
 - Elastic Block Storage Volume is used network drive you can attach to your instances while they run.
 - It allow instance to persist data even after they terminate.
 - Can be mount to one instance at a time.(at CCP level).
 - bound to a specific availibility zone, means cannot be attached to instance in other zone if created in different zone.
 - Consider then as 'usb drive/pen drive'.
 - Free tier: 30GB free of general purpose(SSD) type or magnetic per month.
 - Can be detached from one EC2 instance and attached to another one quickly.
 - Taking snapshot of a ebs volume can make it possible to use it in different availibility zone.
 - Has fixed/provisional size in GBs, IOPS. Billed for provisional capacity, can increase capacity over time.
 - When ec2 got create we got option like 'Delete on Termination attribute'(default is for Volume Type 'Root' instance and not for new instances)

 **<u>EBS Snapshot</u>**
  - You can make backup or snapshot of your volume at any point of time.
  - So even if original volume is deleted volume will be available to use in other instances.
  - It is not recommanded to detach volume to take snapshot, but it is recommenede so everything is clean in your ebs volume.
  - Can copy snapshots across AZ or Region for global infrastructure by copy snapshot to other region,availibility zone using aws console or CLI.

## AMI
- Stands for Amazon Machine Image.
- Customization of a EC2 instance:
  - We can add our own software, configuration, operating system.
  - Provide lot of option to choose for OS, things preinstalled.
  - Faster boot/ configuration time because all your software is prepackaged in it.
- Build for specific region but can be copied across multiple region.
- We can launch EC2 instances from:
  - **A Public AMI**: AWS provided.
  - **Your own AMI**: You can make and maintain your own AMI by yourself. Responsibility is your.
  - **AWS Marketplace AMI**: An AMI someone else made and (potentially sells by someone which you can buy).


## AMI process (from a EC2 instance)
- Start a EC2 instnce and customize it using AMI.
- Stop instance(for data integrity)
- Build an AMI - this will create EBS snapshots.
- Launch instances from other AMIs.


## EC2 Instance Store or Local EC2 Instance Store
- EBS volumes are network drives with good but "limited" performance.
- But sometime we need even higher performance due to heavy tasks, mean we require high performance hardware disk.
- If instance reboot data will persist.
- But data will not persist in following cases:
  - Underlying disk drive fails.
  - Instance stops(impheral).
  - instance terminates. 
- Very IOPS. eg i3.large*=35000(write),100k(read) to i3en.metal=1.6million(write),2million(read).

## EBS Volume Type(6 types)
- **GP2/GP3(SSD)**: General Purpose SSD Volume that balances price and performance for variety of workloads.
- **io1/io2(SSD)**: High performance SSD volume for mission-critical low-latency or high-throughput workloads. Predictable, ,consistence, high performance, low latency.
- **st 1(HDD)**: Low cost HDD volume designed for frequently accessed throughput-intensive workloads.
- **sc 1(HDD)**: Lowest cost HDD volumes designed for frequently accessed, thoughput-intensive workloads.

- EBS Volumes are categorized in Size| Throughput|  IOPS(i/o ops per second).
- A boot volume refers to the portion of a hard drive containing the operating system, its supporting file system, and what hard drive contains the operating system.
- Only gp2/gp3 and io 1/io 2 are used as boot volumes.
- **Use cases**:
  - General purpose SSD:
     - Cost effective storage, low-latency.
     - System boot volumes, Virtual desktops, development and test environments.
     - 1GB - 16 TiB.
     - gp3(newer generation of volume):
       - Basline of 3,000 IOPS and throughput of 125MiB/s.
       - Can increase IOPS to 16,000  and throughput to 1000MiB/s.
       - Can independently change IOPS and thoughput.
    - gp2:
      - Small gp2 volumes can burst IOPS to 3000.
      - Size of the volume and IOPS are linked, max IOPS is 16,000.
      - 3IOPS per GB, means 5,334GB we the max iOPS.

## Elastic File System
- Managed file system that can be mounted on any EC2, lambda.
- EFS works with EC2 instances in multi-AZ unlike EBS.
- Highly available, scalable, expensive(3Xgp2), pay per use.
- NFS will be attached to security grouup.
- All EC2 can mount same EFS and share file among them or access same files.
- **Use case**: content management, web serving, data sharing, wordpress.
- Uses NFSv4.1 protocol which is standard way to mount network drive.
- use security group to control access to EFS.
- Compatible with Linux based AMI(not windows), lambda(as they also use linux).
- Can use Encryption  at rest with KMS.
- Use POSIX file system(~Linux) that has standard file system.
- Scale automatically, pay-per-use, no capacity planning.
- Scale:
  -  1000 of concurrent NFS clients, 10GB+ /s thoughput.
  -  File system can grow to petabyte-scale  network file sytem automatically.
- **EFS Scale**:
   - 1000s of concurrent NFS clients, 10GB+/s thoughput.
   - Grow to petabyte-scale network file system, automically.
- **Performance mode**(set at EFS creation time):
   - General purpose(default): latency-sensitive, low thoughput cases(web server, CMS, etc) where files are small and quick to access.
   - Max I/O - higher latency, thoughput, highly parellel (big data, media processing).
- **Throughput mode**:
   - Bursting(1 TB = 50MiB/s + burst of 100MiB/s).
   - Provised: set your thoughput regardless of storage size, ex; 1GiB/s for 1 TB storage.(storage size is small but need high throughput).
- We can mount EFS to EC2 instance but make sure EC2 instance has Type NFS and security group as what that NFS has.
- Eg; Mount EFS in 2 Ec2 instance's any directory and then create file there it will go to EFS and will be visible to both and on write by one reflect to other also.
- After you create an Amazon EFS file system and mount it locally on your EC2 instance, it exposes an empty directory called the file system root.


## EBS VS EFS
- <p style="color:#2979ff">Elastic Block Storage :::</p>
- EBS volume can be attached to on Ec2 instance at a given time and is locked at the Avalibility Zone(AZ) level.
- EBS gp2: IO increases if the disk size increses.
- EBS io 1: IO increases independently.
- EBS colume can be migrate accross AZ by taking snapshot and restore snapshot to another AZ.
- EBS baackups use IO's and shouldn't run them while your application is handling a lot of traffic.
- By default root EBS volumes got terminated(can disable that) when EC2 instance got terminated.
- Pay for provisioned capacity instead of used capacity.
- <p style="color:#2979ff">Elastic File System :::</p>
- Mounting 100s of EC2 instances across AZ but only to linux instances(as it is POSIX file system).
- EFS share website files(wordpress).
- Has more price than EBS but we can do cost saving using EFS-IA.

# Command
- `lsblk` to get storage volumes attached to instance.

### AWS Cognito
- Service to give users an identity so that interact with our application(mobile, web). This not mean IAM user.
- We 2 things:
  - **Cognito User pool(CUP)**: 
    - Signin functionality for app users.
    - Integrate with API Gateway & Application Load Balancer.
    - Used for authentication.
    - Using this your user can signin to your application or federate thought third party identity provider(IdP)
  - **Cognito Identity Pools(Federation Identity)**:
    - Provide AWS credentials to users so that they can access AWS resources directly.
    - Integrate with cognito user pools as an identity provider.
    - Identity pools are for authorization (access control). 
    - You can use identity pools to create unique identities for users and give them access to other AWS services.
    - Generate temporary AWS credentials for unauthenticated users.
  - **Cognito Sync**:
    - Service to synchronize data from device to cognito.
    - It is deprecated and replace by aws appsync.
    - Cognito Vs IAM: Think cognito to user for "hinder of users", "mobile users", "authenticate with SAML". IAM for user who you trust.


### Cognito User Pool(CUP):

- Service to crete serverless database of user for your web & mobile apps.
- It mean your user can signup/signin through username/email:password combination.
- User for password reset.
- Email & phone number verification.
- MFA of user.
- Federated identities: Login through facebook, google, apple, SAML,etc.
- Feature: Block user if their credentials are compromised.
- Login sent back JWT.
- **How it work with API Gateway?**=>
  - User login through CUP and get JWT(cognito token).
  - User sent that token to api gateway for interacting with some api.
  - API gateway interact with CUP to evaluate cognito token.
  - If token is valid mean user belong to pool and is not block then request sent to backend.
- **How it work with Application Load Balancer?**=>
  - We need to have ALB with listeners & Rules.
  - In this setup ALB first authenticate user with CUP,once done we can forward request to backend in target group which could be EC2s, lambda functions.

**Working with CUP**
- Create pool with some name and for example choose default settings.
- Go inside that pool.
- Go to App Clients to create client with client id, client secret(optional) to access this pool from your application.
- In App client setting you can set config of callback URL and identity provider, auth 2.0 settings as it could differnt for diffent client.
- Go to domain to create domain for signin, signup pages which are hosted by aws cognito unique to a region. Prefixed domain names can only contain lower-case letters, numbers, and hyphens
- You must enable at least one identity provider for each app client.
- You also custom domain UI like logo, background color.
- For federation:
  - Go to Identity Provider and you can choose any of the external federated identity provider like google, apple, amazon, OpendIdConnect, SAML.
  - Just type your external IDP client id, secret, authorization scope.
- AWS cognito also provide triggers to invoke our lambda function to either on signup, after sigin, forget password. Bascially to have flow or do things before/after user signin/signup/forgetpassword/MFA.

**Cognito Identity Pool**
- Get identities for users so that can obtain temporary aws credentials.
- This identity pool allow you to login though:
  - Public providers(google, amazon, apple,etc).
  - Allow login to user who are in Cognito User Pool.
  - OpenID Connect Providers & SAML Identity Providers.
  - Developer Authenticated Identities(custom login servers).
  - Cognito identity pools allow for unauthenticated(guest) users.
- Then uer can access AWS services either by sdk or directly though API gateway
  - IAM Policies are applied to the user credentials which are defined by cognito
- Eg;
  - Web & Mobile App will login in Cognito User Pools and he/she will get JWT token.
  - Call Identity pool with cognito token.
  - Identity pool check token with cognito user pools and then then call AWS STS.
  - AWS STS send temporary aws credential to user through which they can access aws services.
- How IAM roles are decided for users?:
  - Can have default role for both authenticated and guest users.
  - Defines rules to choose the role for each user base on their users's ID.
  - Using policy variable to allow to get only specific acccess on any resources.
  - IAM credentials are obtained by Cognito Identity Pools though STS.
  - Role must have "trust" policy of Cognito Identity Pools to work
- For unauthenticated user we need to create role with cretain policy attach to it.
- And we can also define policy variable on aws s3 based on prefix match with user id or identity.
 - Like you can set condition on bucket. eg; 
  ```{
      "Version": "2012-10-07",
      "Statement" : [
        {
          "Sid": "demoUserRestrict",
          "Effect": "Allow",
          "Resource": ["arn:aws:s3::fileStoreBucket"]
          "Condition" : {"StringLike": {"s3:prefix": ["${cognito-identity.amazonaws.com:sub}/*"]}}
        }
      ]
    }
  ```

**AWS STS**
- AWS provides AWS Security Token Service (AWS STS) as a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for users you authenticate (federated users). 
- Available as a global service.
- AWS STS requests go to a single endpoint at https://sts.amazonaws.com

# Scalibity & High Availibity
- Scalibity means application can handle higher loads by adapting.
- Kind of scalibility:
   - Vertical(eg;instance size increase but upto a limit).
   - Horizontal(elasticity.eg; increase number of instances). Implies distributed systems.
- Scalibility is link to availibility but different.
- High availibility goes in hand with horizontal scaling.
- High availibity zone mean your application running in atleast 2 data center(=== avalibility zones).
- Goal of high avalibility is to survive data loss.
- For Ec2 vertical scaling is EC2 from t2.nano - 0.5GB RAM, 1CPU to u-12tbI.metal - 12.3TB of RAM, 448vCUPs.
- Horizontal scaling:
    - scale in(decrese number of instances), scale out(increase number of instances).
    - Auto Scaling group.
    - Load Balancer.
- High availibity: 
    - Run instances for the same application accross multi AZ. 
    - Auto Scaling group multi AZ.
    - Load Balancer multi AZ

# Elastic Load Balancing(ELB):
- Load balancer are servers forward traffic to multiple servers(eg; EC2 instances) downstream.
- Why use:
  - Spread load accross multiple downstream instances.
  - Expose a single point of access(DNS) to your application.
  - Seamlessly handle failures(health check in periodically) of downstream instances.
  - Do healthcheck to your instances.
  - Provide SSL termination(HTTPS encrypted traffic) for your websites.
  - With sticky sessions, a load balancer assigns an identifying attribute to a user, typically by issuing a cookie or by tracking their IP details. Then, according to the tracking ID, a load balancer can start routing all of the requests of this user to a specific server for the duration of the session.
  - High availibity accross zones.
  - Seperate public traffic from private traffic on your cloud.
  - ELB is managed Load Balancer:
      - AWS gaurantees that it will be working.
      - AWS takes care of upgrades, maintaince,high avalibility.
      - AWS will only provide you few configuration knobs.
  - We can also create our own load balancer which will cost less but it will be a lot of effort on our end.
  - It is integrated with many AWS services
     - EC2, EC2 autoscaling groups, amazon ECS.
     - AWS certificate manager(ACM), cloudwatch.
     - Route 53, AWS WAF, AWS Global Accelerator.

  ### Health checks
  - Crucial for load balancers.
  - It enable load balancer to check if instance to which it fowarding traffic is ready to reply requests.
  - Health check is done on a port and route(/health is common).
  - If response is not 200(OK) then the instance is unhealthy.

  ## Type of load balancer in AWS
  AWS has 4 types of load balancers
  - **Classic Load Balancer(v1 - old generation)** - 2009 - CLB
     - Supports HTTP(layer 7), HTTPS(layer 7), TCP(layer 4), SSL(secure TCP).
  - **Application Load Balancer(v2 - new generation)** - 2016 - ALB
    - Supports HTTP, HTTPS, Websocket protocol.
  - **Network Load Balancer(v2 - new generation)** - 2017 - NLB
    - Supports TCP,TLS(secure TCP), UDP.
    - Provides the highest performance and lowest latency if your application needs it.
    - Network Load Balancer has one static IP address per AZ and you can attach an Elastic IP address to it. Application Load Balancers and Classic Load Balancers as a static DNS name.
  - **Gateway Load Balancer** - 2020 - GWLB
    - Operates at network layer(3) - IP Protocol
  - It is recommended to use the newer generation load balancers as they provide more flexibility.
  - Some load balancer can be setup as internal(private) or external(public) ELBs.
  - Your EC2 security group will not same as of loadbalancer but it's port allow will be for HTTP(80) but source will not be a IP range but instead will be security group of load balancer. This will link security group of  EC2 to load balancer which allow traffic from load balancer in EC2.

  # Classic Load Balancer(v1)
  - Supports HTTP(layer 7), HTTPS(layer 7), TCP(layer 4), SSL(secure TCP).
  - Health check are TCP or HTTP based.
  - Fixed hostname XXX.region.elb.amazonaws.com

## Application Load Balancer(V2)
- Layer 7(HTTP) LB.
- Around 400ms latency.
- Used for load balancing to multiple HTTP applications across machines(target machines).
- Used for load balancing to multiple applications on the same machine(ex: containers).
- Support for HTTP/2 and websocket.
- Support redirects(from HTTP and HTTPS).
- Support for routing tables to different target groups:
  - Routing based on path in URL(example.com/users and example/com/chats).
  - Routing based on hostname in URL (one.example.com and other.example.com)
  - Routing based on Query string, headers(example.com/users?id=123&order=1). We can send traffic for mobile, desktop based on query eg; ?Platform=Mobile, ?Platform=Desktop.
- Great fit for microservices & container-based application
  (example: Docker & Amazon ECS).
- Has port mapping feature to redirect to a dynamic port in ECS.
- In comparison, we need multiple classic Load balancer per application. But we require one ALB for all applications. 
- Can route to multpiple target groups.
- Health checks are done at target group level.
- Application servers don't see IP, port of clients directly but need to get send in headers X-Forwared-For, X-Forwared-Port.

When you enable ELB Health Checks, your ELB won't send traffic to unhealthy (crashed) EC2 instances.

## Target groups for ALB
- EC2 instances(can be managed by ASG) - HTTP.
- ECS tasks(managed by ECS itself) - HTTP.
- Lamabda functions - HTTP request is transalated into a JSON event.
- Prvate IPs.

## Network loadBalancer
- Layer 4 of osi model.
- Latency ~100ms
- Forward TCP and UDP traffic to your instances.
- Handle millions of requests per second. Mean very high performance.
- Has one static IP per AZ, and supports assigning Elastic IP(helpfull for whitelisting specific IP). It is different from CLB, ALB who have static hostname but not static IP.
- Used for extreme performance, TCP or UDP traffic.
- Not in free tier.
- Creating target groups for it make sure create them for TCP for traffic forward, health check.

## Sticky Sessions(Session Affinity)
- To implement stickiness so that same client is directed to same instance behind a load balancer.
- This work for CLB & ALB.
- The "cookie" used for stickiness has an expiration date you control.
- Use case: User doesn't lose the session data.
- This approach may cause imbalance in loadbalancer due to stickness of users to a instance.
- Cookies type:
  - **Application-based cookies**:
     - Custom cookies
        - Generated by the target.application itself.
        - Can include any custom attributes required by the application.
        - Cookie name must be specified individually for each target group.
        - Don't use AWSALB, AWSALBAPP, AWSALBTG (reserved for use by ELB).
     - Application cookie
       - Generated by Load balancer itself.
       - Cookie name is AWSALBAPP.
  - **Duration-based Cookie**:
     - Cookie generated by load balancer itself.
     - Duration of cookie to get expired decide by load balancer.
     - Cookie name is AWSALB for ALB, AWSELB for CLB.
- Cookie info is required when working with cloudfront.

## Cross-Zone Load Balancing
- The nodes for your load balancer distribute requests from clients to registered targets. When cross-zone load balancing is enabled, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones. When cross-zone load balancing is disabled, each load balancer node distributes traffic only across the registered targets in its Availability Zone.
- For ALB: 
  - Always on(can't be disabled).
  - No charges for inter AZ data.
- For NLB:
  - Disabled by default.
  - Pay charges(dollar) for inter AZ data if enabled.
- For CLB:
  - Through console => Enabled by default.
  - Through CLI/API => Disabled by default.
  - No charges for inter AZ data if enabled.

### SSL/TLS Basics
 - An SSL certificate allow traffic between your clients and your load balancer to be encrypted in transit(in-flight encryption)
 - SSL refers to Secure Socket Layer, used to encrypt connections.
 - TLS refers to Transport Layer Security, new version of SSL.
 - Nowdays, TLS certificate are mainly used.
 - Public SSL certificates are mainly issed by Certificate Authority(CA).
 - Eg of CA: Commodo, Godaddy, GlobalSign, Digicert, etc.
 - certificate are attach to load balancer to encrypt connection between clients & LB. encrypt throught ssl certificate termination.
 - They have expiry date.
 - User to LB connection is HTTPS and LB to EC2 instance connection is HTTP(in some VPCs).
 - LB uses an X.509 certificate(SSL/TLS server certificate).
 - You can manage certificates using ACM(Amazon cerificate manager).
 - You can upload your own certificates alternatively.
 - At HTTPS listener:
    - You must specify a default cerficate.
    - You can add an optional list of certs to support multiple domains.
    - Clients an use SNI(Server name indication) to specify the hostname they reach.
    - Ability to specify a security policy to support older version of SSL/TLS(lagacy clients). 

## SNI(Server name indication)
- Solves the problem of loading multiple SSL certificates into one web server(to serve multiple websites).
- It's a new protocol, require the client to indicate the hostname of the target server to initiate SSL handshake.
- The server will then find the correct cert., or return default.
- Note: Only works for ALB & NLB(new generation), Cloudfront. Not work with CLB(old generation).
- Eg; You have a **ALB** and behind it we have 2 target groups for www.mycorp.com and domain1.example.com. Here when client want to call www.mycorp.com and it reach ALB then ALB will load cert associate with thaat target group.


## ELB - SSL cerficates
- **CLB(v1)**:
  - Support only one SSL cerficates.
  - Must use multiple CLB for multiple hostname with multiple SSL cerificate.
- **ALB(v2), NLB(v2)**:
  - Support multiple listeners with multiple SSL cerficates.
  - Uses SNI to make it work.

### Connection draining

- Feature draining:
  - In CLB it is called as connection draining.
  - In ALB & NLB it was call as Deregistration Delay.
- Time to complete 'in-flight requests' while the instance is de-registring or unhealthy.
- Stop sending new request to EC2 instance which is de-registering.
- Can set connection draining value betweeen 1-3600 seconds(default:300s).
- Can be disabled(set value to 0).
- Set value based on your request time is short or high.

### Auto-scaling Group
- In real-life load on your website and application can change(increase/decrease).
- With this we can create and get rid of instances very quickly.
- Goal of it is:
   - Scale out(add EC2 instaances) to match an increased load.
   - Scale in(remove EC2 instances) to match an decreased in load.
   - Ensure we have a minimum and a maximum number of machines running.
   - Automatically Register new instances to a load balancer. 
- We have minium EC2 instances running in ASG, Acctual size/desired capacity EC2 instance running currently, maximum of EC2 instance can increase.
- ASG and Load balancer work hand in hand to handle out traffic increase/decrease automatically.
- The Auto Scaling Group can't go over the maximum capacity (you configured) during scale-out events.

### ASG has following attributes:
-  A launch configuration(same like when we launch EC2 instance):
   - AMI+instance Type
   - EC2 user data(script run after EC2 instance launch).
   - EBS volumes.
   - Security groups.
   - SSH Key pair.
- Min Size/ Max Sizze/Initial Capacity.
- Network + Subnets information.
- Load Balancer information.
- Scaling policies.

### Auto Scaling Alarms
 - It is possible to sacle an ASG based on cloudwatch alarms
 - An alarm monitors a metric(average CPU utilisation of all EC2 instance).
 - Metrics are computed for the overall ASG instances.
 - Based on the alarm:
   - We can create scale-out policies(increase the number of instances).  
   - We can create scale-in policies(decrease the number of instances).  

### Auto Scaling new rules
 - It is now possible to define "better" auto scaling rules that are directly managed by EC2.
    - Target Average CPU usage.
    - Number of requests on the ELB per instance.
    - Average network in.
    - Average network out.
 - These rules are easy to setup and make more sense on scaling group.

### Auto Scaling custom metric
 - Can auto scale based on custom metric like number of users connected to a instance.
 - We can send custom metric from our application in EC2 instance using PutMetric API.
 - Create cloudwatch alarms to react to high/low values.
 - Use cloudwatch alarms as a scaling poicy for your ASG

 ### ASG Brain Dump
 - Scaling policies can be on CPU, Network, ... and can even be on custom metrics or based on a schedule(if you know your visitors paattern).
 - ASG use launch configuration or launch templates(newer).
 - To update ASG provide new launch configuration.
 - IAM role attach to ASG got attach to EC2 instance in it.
 - ASG are free. You only pay for underlying resources launched.

 ## ASG Policies
 - Dynamic Policies
 - Predictive Scaling

 ## Dynamic Policies
  - **Target Tracking Scaling**:
    - Most simple and easy to set-up.
    - Example: I want to have avarage CPU usage around 40%.
    - The scaling policy adds or removes capacity as required to keep the metric at, or close to, the specified target value.
  - **Simple/ Step Scaling**:
    - We have to create cloudwatch alarm which is trigger(CPU > 70) then add 2 units.
    - We have to create cloudwatch alarm which is trigger(CPU < 40) then remove 2 units.
    - We have to create cloudwatch alarm which is trigger(CPU > 70) then set to certain units.
  - **Scheduled Acions**:
    - Anticipate a scaling group based on certain pattern.
    - Eg; increase the min capacity to 10 at 5 PM on every Friday.

## Predictive Scaling
   - Continously forecast load and schedule scaling ahead.

## Good Metrics scales on
- CPU utilisation: Average CPU utilisation across your instances.
- RequestCountPerInstance: To make sure number of request per instances is stable.
- Average Network in/out (if you're application is network bound). eg; lot of uploads, downloads happens.
- Custom Metrics.

## Scaling Cooldown
- After a scaling activity happens you are in cooldown period(default 300 seconds).
- During cooldown period ASG will not launch/terminate any instances(for metrics to stabilize).
- Advice: Use a ready-to-use AMI to reduce configuration time in order to be serving request fasters and reduce the cooldown period.


------------------------------------
## AWS RDS <img src="https://aws-course-resources.s3.amazonaws.com/rds.png" alt="Your image title" height="30" width="30"/>
- RDS stands for Relational Database Service.
- It's a managed DB service for DB use SQL as query language.
- It allows you to create databases in the cloud that are managed by AWS.
   - Postgres
   - MySQL
   - MariaDB
   - Oracle
   - Microsoft SQL Server
   - Aurora(AWS Propriety Database)

### Advantage of RDS versus deploying on EC2
- RDS is managed service:
  - Automated provisioning, OS patching.
  - Continious backups and restore to specific timestamp(Point in time restore).
  - Monitoring dashboards.
  - Read replica for improved performance.
  - Multi AZ setup for DR(Disaster Recovery).
  - Maintainance windows for upgrade.
  - Scaling capability(vertical and horizontal) by increasing read replica.
  - Storage backed by EBS(gp2 or io1).
- But you can't SSH into your service.

### RDS Backups
- Backups are automatically anabled by RDS.
- Automated backups:
    - Daily full backup of database(during the maintainance window eg; at night 11 every day).
    - Transaction logs are backup by RDS every 5 minutes.
    - ability to restore to any point in time(from oldest to 5min ago backup).
    - 7 days retention(can be increased to 35 days).
- DB snapshots:
  - Manually triggers by user.
  - Backup retention for as long as you want.

### RDS Storage Auto Scaling
 - <img src="https://aws-course-resources.s3.amazonaws.com/rds-storage-auto-scaling.png" height="260" width="260"/>
 - Helps you increase storage on your RDS DB instance dynamically.
 - When RDS detects you are running out of free storage, it scales automatically.
 - Avoid manual scaling your database storage.
 - You have to set maximum storage threshold(maximum limit for DB storage).
 - It will automatically modify storage if:
   - Free storage is less than 10% of allocated storage.
   - Low-storage last at least 5 minutes.
   - 6 hours has been passed since last modification.
 - Usefull for application with unpredicatable workloads. 


## RDS Read Replica vs Multi AZ

### RDS read replicas for read scalibility
- <img src="https://aws-course-resources.s3.amazonaws.com/rds-read-replica-scalibility.png" height="260" width="290"/>
- Upto 5 Read Replicas.
- Can be within same AZ(availibility zone), cross AZ or Cross region.
- Replication is ASYNC, so reads are eventually consistent.Require low bandwidth. Asynchronous replication products copy the data to the replica after the data is already written to the primary storage. Although the replication process may occur in near-real-time, it is more common for replication to occur on a scheduled basis. For instance, write operations may be transmitted to the replica in batches on a periodic basis (for example, every one minute). In case of a fail-over event, some data loss may occur.
- Replicas can be promoted to their own DB means now they will become primary DB.


### Use case:
   <img src="https://aws-course-resources.s3.amazonaws.com/rds-read-replica-use-case.png" height="400" width="800"/>

## RDS read replicas Network cost
- In AWS there's a network cost when goes from one AZ to another
- If read replica is in same availibility zone then we don't need to pay fee for those replicas.
- But if read replicas are in different region or cross region then there is some cost need to pay.

### RDS multi AZ(Disaster Recovery)

- Syncronous replication.
- There will be a standby DB instance in your region to which every write change in master will get syncronously replicated.
- One DNS name - automatic failover to standup.
- Increase availibility.
- Failover in case of loss of AZ, loss of network, instance or storage failure.
- No manual intervation in apps.
- Not used for scaling.
- Read replica can be setup to multi AZ for disaster recovery.
- To go from single-AZ to multi-AZ there will be:
   - Zero downtime(no need to stop DB).
   - Just click on modify for database.
- Following thing happens internally in failover:
  - Snapshot is taken.
  - A new DB is restored from the snapshot in a new AZ.
  - Syncronization is established between two database.

  ### RDS security - Encryption
  <img src="https://aws-course-resources.s3.amazonaws.com/rds-security-encryption.png" width="700" height="330"/>

  ### RDS Encryption Operations
  - **Encrypting RDS backups**:
    - Snapshots of un-encrypted RDS will be un-encrypted.
    - Snapshots of encrypted RDS databases are encrypted.
    - We can create snapshot of un-encrypted RDS and make it encrypted by modifying the config(enable encryption).
  - **To encrypt and un-encrypted RDS database**: 
    - Create snapshot of un-encrypted datbase.
    - Copy the snapshot and enable encyption for the snapshot.
    - restore the database from un-encrypted database.
    - Migrate application to new database, delete old database.

  ### RDS Security - Network & IAM
  <img src="https://aws-course-resources.s3.amazonaws.com/rds-security-network-iam.png"  width="700" height="330"/>

  ### RDS - IAM Authentication
  -  <img src="https://aws-course-resources.s3.amazonaws.com/rds-iam-authentication.png" width="380" height="320"/> 
  - IAM datbase authentication only works with MySQL and PostgreSQL.
  - You don't need password, just an auth token obtain IAM & RDS API Calls.
  - Auth token has lifetime of 15 minutes. 
  - Benfits:
    - Network in/out must be encrypted using SSL.
    - IAM to centrally manage users instead of DB.
    - Can leverage IAM roles and ec2 instance profile for easy integration.

  ## Amazon Aurora
   - Prorietory technology from AWS(not open sourced).
   - Postgres and MySQL are both supported Aurora DB(means your driverss will work if Aurora was postgres or mysql).
   - Aurora is "AWS cloud optimized" and claims 5x performance improvement over MYSQL, over 3x then performance of postgres on RDS.
   - Aurora storage  automatically grows in increments of 10GB, upto 64TB.
   - Aurora can have 15 read replicas while MySQL has 5, the replication process in faster
   - Failover in Aurora is instantenous as compared to MsSQL RDS. It's HA(High Availibility) native.
   - Aurora costs more than RDS (20% more) - but is more efficient.

  ## Aurora high availibility and read replicas
  - <img src="https://aws-course-resources.s3.amazonaws.com/aurora-high-availibility-read-replicas.png" width="700" height="330"/>
  - 1 master
  - Shared storage volumes
  - Data is automatically replicated across Availability Zones, your data is highly durable with less possibility of data loss.
  -  Restore data at any point of time without using backups. 

  ### Aurora DB Clustor
  - <img src="https://aws-course-resources.s3.amazonaws.com/aurora-clustor.png" width="580" height="340" style="border: 1px solid white"/>
  - Provide Writer Endpoint so failover not effect.
  - Provide Reader Endpoint which will connect our application to replicas and load balance traffic comming to read replicas.

  ### Aurora Security
  - Similar to RDS as both uses same engines.
  - Encryption at rest using KMS.
  - Automatication snapshots, backups and replicas are also encrypted.
  - Encryption in-flight using SSL(same as MySQL or Postgres).
  - Can authenticate with IAM token(same as RDS).
  - You are responsible for protecting instance with security groups.
  - You can't SSH.


-------------------------------------------------------------------------------------------------------------
 
 
## AWS ElasticCache <img src="https://aws-course-resources.s3.amazonaws.com/elastic-cache.jpeg" height="50" width="50" style="margin-left: 30px; margin-top: 30px;border: 1px solid white">  
 - The same way RDS is get managed Relational Database.
 - Elasticcache is to manged Redis or Memchached.
 - Caches are in-memory databases with really high performance, low latency.
 - Helps reduce load off of databases for read intensive workloads.
 - Make your application stateless.
 - AWS will take care of OS maintainance/patching, optimizations, setup, configuration, monitoring, failure recovery and backups.
 - Using this involves heavy application code changes.


### Redis vs Memecache
<img src="https://aws-course-resources.s3.amazonaws.com/redis-vs-memecache.png" width="680" height="340" />

### Caching strategies

- **Lazy loading/ cache aside**: 
   - Cache hit return data to application.
   - Cache miss then call RDS to get data and then store it in cache and returm data. so 3 round trip.
   - Data may become stale.
   - All data is in use.
- **Write Through**: 
   - When RDS update happening then update cache also. 2 calls
   - Always updated/fresh data in cache.
   - Chance of unused data.
- **Cache Eviction and TTL**:
   - <img src="https://aws-course-resources.s3.amazonaws.com/cache-eviction-and-ttl.png" width="680" height="340" >

 ### ElatiCache Replication: Cluster Mode disabled
 - <img src="https://aws-course-resources.s3.amazonaws.com/read-replica-cluster-mode-disabled.png" width="280" height="220" />
 - One primary node, upto 5 replica.
 - Has max 1 shard in a cluster.
 - Primary node is for read/write.
 - Ther other nodes are read only.
 - All node have all the data.
 - This guard against failover or data loss.
 - Multi-AZ enabled by default.
 - Helpfull of read performance. This point is main highlight.

  ### ElatiCache Replication: Cluster Mode enabled.
   - <img src="https://aws-course-resources.s3.amazonaws.com/elasticache-read-replicas-cluster-enabled.png" width="350" height="290" />
  - Data is partitoned across shards(helpfull to scale writes).
  - Each shard will have a primary and upto 5 read replicas(same concept as before).
  - Multi-AZ capability.
  - Upto 500 nodes per cluster.
    - 500 shared with single master.
    - 250 shareds with 1 master and 1 read replica.
    - ...
    - 83 shards with 1 master and 5 read replicas.


  >> Multi-AZ helps when you plan a disaster recovery for an entire AZ going down. If you plan against an entire AWS Region going down, you should use backups and replication across AWS Regions.
  >> Multi-AZ keeps the same connection string regardless of which database is up.


----------------------------------------------------------------------------------------
  ### DNS Terminologies
  - **Domian Registrar**: Amazon Route 53, GoDaddy, etc.
  - **DNS Records**: A, AAAA, CNAME, NS,..
  - **Zone File**: Contains DNS records. Used for match hostname to IP address.
  - **Name Server**: resolve DNS queries(Authoritative or Non-Authoritative).
  - **Top Level Domain(TLD)**: .com, .us, .in, .gov, .org, etc.
  - **Second Level Domain(SLD)**: amazon.com, google.com, etc.
  - Eg; http:api.www.example.com.
     - dot at end is root of all domain names(root servers).
     - .com is TLD.
     - example.com is SLD.
     - www.example.com is Sub Domain.
     - api.www.example.com is your Domain name.
     - http is protocol
     - Combine all we have Fully Qualified Domain Name(FQDM).

  ### DNS Working
  <img src="https://aws-course-resources.s3.amazonaws.com/dns-working.png" width="650" height="350" />

### Amazon Route 53
 - <img src="https://aws-course-resources.s3.amazonaws.com/amazon-route-53.png" width="290" height="250" >
 -  A higly available, scalable, fully managed and Athoritative DNS. Athoritative = customer(mean you) can update DNS records.
 - Also a domain regitrar.
 - Has Ability to check health of your resources.
 - Only AWS service to provide 100% availibility SLA.
 - Why Route 53 has 53 in name? 53 is reference to traditional DNS port.

### Records Types
- **A** - maps a hostname to IPV4
- **AAAA** - maps a hostname to IPV6
- **CNAME** - maps a hostname to another hostname
  - The target is a domain name which must be A or AAAA record
  - Can't create CNAME for top node of a DNS namespace(Zone apex).
  - eg; You can't create CNAME for example.com but can create for www.example.com
- **NS** - Name servers for Hosted Zone. They will respond to DNS queries.
   - Control how traffic is routed for domain.

### **Hosted Zones**
- Container for records that define how to route traffic to a domain and it's subdomains.
- **Public Hosted Zones** - contain records that specify how to route traffic on the internet(public domain names). eg application1.mypublicdomain.com.
- **Private Hosted Zones** - Contain records that specify how to route traffic within one or more VPCs(private domain names). we have seen this kind inside a corporate. eg; application1.company.internal
- You need to pay $0.50 per month per hosted zone.

### Records TTL(Time to Live)
- My client make DNS request eg;myapp.example.com to Route 53. 
- Then Route 53 will respond with response eg; 123.456.789.123 with a TTL.
- So next time client would make DNS request to Route 53 within TTL period.
- These request to Route 53 cost you in aws.

### CNAME vs Alias
- AWS resources(cloudfront, s3 hosted website, cloudfront, etc) expose an AWS hostname.
  - Suppose we waant to map lb1.us-east1.elb.amazonaws.com to myapp.example.com
- **CNAME**:
   - points hostname to other hostname eg;app.mydomain.com -> dm.mydomain.com
   - Can only map non-root domain eg;some.mydomain.com
   - we cannot map server, IP to root domain
- **Alias**:
   - Point hostname to an AWS resource(app.mydomain.com => blblb.amazonaws.com).
   - Work for root and non-root domain(eg;mydomain.com)
   - free charge.
   - native heath checks.


### Alias Records
- <img src="https://aws-course-resources.s3.amazonaws.com/alias-record-eg.png" width="400" height="400">
- Maps hostname to an AWS resource.
- Automatically recognizes changes in the resource IP address.
- Unlke CNAME it can be used for the top node of a DNS namespace(Zone apex).eg; example.com
- Alias record is always of type A or AAAA for AWS resources(IPV4, IPV6).
- You can't set TTL, it will automatically set by Route 53.

### Alias Record Targets
- Elastic Load Balancers
- CloudFront Distributions.
- API Gateway.
- Elastic Beanstack environments.
- S3 websites.
- VPC interface endpoints.
- Global accelerators.
- Route 53 record in same hosted zone.
- Note: We cannot set alias for an EC2 DNS name.

### Routing Policies
- Define how Route 53 going to respond to DNS queries.
- Don't get confused by word "Routing".
  - It's not same as load balancer routing which routes the traffic.
  - DNS does not route any traffic, it only responds to the DNS queries.
- Route 53 support following DNS Policies:
  - Simple
  - Weighted
  - Failover
  - Latency based.
  - Geolocation.
  - Multi-value answer.
  - Geoproximity(using route 53 traffic flow  feature).


### Routing Policy - Simple
- <img src="https://aws-course-resources.s3.amazonaws.com/routing-policy-simple.png" width="400" height="400"/>
- Typically route traffic to single resource.
- Can specify multiple values in same record.
- But client will choose random one at a time.
- When alias enabled, specify only one resource.
- Can't associate with health checks.
- By default routing policy is simple.

### Routing Policy - Weighted
- <img src="https://aws-course-resources.s3.amazonaws.com/routing-policy-weighted.png" width="400" height="300">
- Control the % of the requests that go to each specific resource.
- Assign each record with a relative weight:
  - traffic(%) = (Weight for a specific record)/(Sum of all the weights for all records)
  - Weights don't need to sum up to 100.
- DNS record must have same name and type
- Can associate with health checks.
- Use cases: load balancing between regions, testing new application versions,..
- Assign weight 0 for stop sending traffic to a resource.
- If all records have weight of 0, then all records will be returned equally.

### Routing Policy - Latency-based
- <img src="https://aws-course-resources.s3.amazonaws.com/routing-policy-latency-based.png" width="500" height="250">
- Redirect to the resource that has the least latency close to us.
- Super helpful when latency for users is a priority.
- Latency is based on traffic between users and AWS regions.
- Germany users may be redirected to the US(if that's the lowest latency).
- Can be associate with health checks(has a failover capability).

### Route 53 - Health checks
- HTTP health checks are only for public resources.
- Health checks are used for automated DNS failover:
  - Monitor endpoint(application, server and other AWS resources).
  - It could monitor other health checks(Calculated Health Checks).
- Health checks that monitor cloudwatch alarms(full control!!) - eg; throttles of DynamoDB, alarms of RDS, custom metrics, ...(helpful for private resources).
- Health checks have their own metrics aand we can view them in cloudwatch metrics.
- About 15 global health checkers will check endpoint health:
  - Health/unhealthy threshould - 3
  - Interval - 30 second(can set to 10sec - higher cost).
  - Support protocol - HTTP,HTTPS and TCP.
  - If > 18% of health checkers report the endpoint is health, Route 53 consider it Healthy. Otherwise unhealthy.
  - Ability to choose which locations you want Route 53 to use.  
- Health checks pass only when the endpoint respond with 2XX and 3XX status codes.
- Health checks can be setup to pass/fail based on the text in the first 5120 bytes of the response.
- Important: Configure your router/firewall to allow request froom health checks to load balancer.
- <img src="https://aws-course-resources.s3.amazonaws.com/health-checks-monitor-endpoint.png" height="300" width="400">

### Calculated Health checks
- <img src="https://aws-course-resources.s3.amazonaws.com/calculated-health-checks.png" height="300" width="400">
- Combine result of multiple health checks into single health checks.
- You can use OR, AND or NOT.
- Can specify upto 256 child health checks.
- You can specify how many child health checks require to pass for parent to pass.
- Usage: Performance maintanance to your website without causing all health checks to fail.

### Health Checks - private Hosted Zones
- Route 53 Health checkers are outside VPC.
- They can't access private endpoints(private VPC or on-primise reources).
- Solution: Create cloudwatch metric and associate a CloudWatch Alarm, then create the health checks the alarm itself.
- <img src="https://aws-course-resources.s3.amazonaws.com/route53-health-check-for-private-hosted-zones.png" height="300" width="400">

### Routing Policy - Gelocation
- Not same as latency-based
- Routing is based on user location
- Specify location by continent, country or by US state(on overlapping most precious location is slected).
- Should create "Default" record(in case there is no match on location).
- Use cases: Website localisation, restrict content distribution, load balancing,...
- Can be associated with Health checks.

### Routing Policy - Geoproximity
- Route traffic based on geographic location of users and resources.
- Ability to shift more traffic to resource based on the defined bias.
- To change size of geographic region specify bias values:
  - To expand(1 to 99) - more traffic to resource.
  - To shrink(-1 to -99) - less traffic to resource.
- Resource can be:
  - AWS resources(spcify AWS region to aws to do exact routing).
  - Non-AWS resources(specify lat,lng). for your own premise data centre.
- Use Route 53 Traffic Flow(advanced) to use this feature.
- <img src="https://aws-course-resources.s3.amazonaws.com/routing-policies-geoproximity-bias-0.png" height="200" width="600"/>
- <img src="https://aws-course-resources.s3.amazonaws.com/routing-policy-geoproximity-bias-unequal.png" height="200" width="600">

### Routing Policy - Multi-value
- <img src="https://aws-course-resources.s3.amazonaws.com/routing-policy-multi-value.png" height="200" width="600"/>
- Route 53 return multiple IP values to client from which it can call any one randomly. Domain is same.

## AWS S3 
- <img src="https://aws-course-resources.s3.amazonaws.com/s3-pic.png" height="50" width="50">
- Advertise as "infinitely scalable" storage.
- Can be used to host website.
- Allow to store objects(file) in "buckets"(directories).
- Each bucket should have globally unique name.
- Buckets are defined at regional level but available at Global level.
- Naming convention:
   - No uppercase.
   - No underscore.
   - 3-63 characters long.
   - Not an IP.
   - Must start with lowercase letter or number.
- <p style="color:#34bdeb">Key</p> is the FULL path(path from bucket root to that file).
   - s3://my-bucket/<p style="color:#34bdeb">my_file.txt</p>
   - s3://my-bucket/<p style="color:#34bdeb">folder1/my_file.txt</p>
- Key is composed of prefix + Object.
  - s3://my-bucket/my_folder1/another_folder/my_file.txt
  - my_folder1/another_folder -> Prefix, my_file.txt -> object
- There is no concept of directories in S3(UI may trick you on this).
- Max obj size is 5TB(5000GB).
- If uploading more than 5GB, must use "multi-part upload".
- Version Id(if enabled).

### AWS S3 versioning
  - Enable at bucket level.
  - version with same key created.
  - On delete version object not delete object but add delete marker.

### AWS S3 encryption for objects
  - 4 methods of encrypt objects in S3.
     - **SSE-S3**: Encrypt S3 objects using keys handled & managed by AWS.
       - Object is encrypted server side.
       - Use AES-256 encryption type.
       - In HTTP header must set =>  "x-amz-server-side-encryption": "AES256"
       - <img src="https://aws-course-resources.s3.amazonaws.com/sse-s3-encrypt.png" height="300" width="400">
     - **SSE-KMS**: Leverage AWS Key Management Service to manage encryption keys.
        - User control(who has access to keys) + audit trail.
        - In HTTP header must set =>  "x-amz-server-side-encryption": "aws:kms"
     - **SSE-C**: When we want to manage our own encryption keys.
       - Amazon S3 does not store encryption key you provided.
       - HTTPS must be used.
       - Encryption key must be pass in header of every HTTP request.
     - **Client Side Encryption**
         - S3 will not apply any encryption and client is responsible for it.
         - encrypt before send to s3.
         - decrypt when get data from s3.

### S3 Security
 - **User based**:
   - IAM policies - which API calls should be allowed for a specific user from IAM console. Create IAM user with specific service & resource access and scope.
 - **Resource Based**:
   - Bucket Policies - bucket wide rules from S3 console - allow cross account.
   - Object ACL - fine grain.
   - Bucket ACL - less common.
 - Note: IAM principle can access an S3 object if the user IAM permissions allow it OR the resource policy allow it. and no explicit DENY mean there should not be case where IAM policy allow user to access bucket but bucket policy not.

### S3 Bucket policies
 - JSON based policies.
 - Resource: buckets and objects.
 - Actions: Set of API to allow or Deny.
 - Effect: Allow/Deny.
 - Principal: The account or user to apply the policy to
 - Use S3 bucket policy to:
   - Grant public access to the bucket.
   - Force objects to be encrypted at uploded.
   - Grant access to another account(Cross account).
   - There is way to block public, cross-account access to buckets and objects though any public buckets

 - <img src="https://aws-course-resources.s3.amazonaws.com/s3-security-other.png"> 

- S3 is from Dec2020 is strongly consistent mean after write/ovverwrite/delete if we do read we will receive latest version.
- When using aws cli we can run api calls as --dry-run option to simulate call.

### AWS CLI STS Decode
- AWS STS) as a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for users you authenticate (federated users).
- When we do api call from aws-cli we get a long encoded message so if we want to decode it we can use STS decode comand. eg; aws sts decode-authorization-message --encoded-message < value-of encoded message >. But for this user should have permission of AWS service.

### AWS EC2 Instance Metadata


### AWS CLI profiles
- We could be using multiple aws credential in our machine so we can add profile using coommand => aws configure --profile {name of profile}. same can be used with API call eg; aws s3 ls --profile my-aws-profile

### Use MFA with CLI
- To use MFA with CLI, we must create a temporary session.
- To do so we must run <b>STS GetSessionToken API</b> call through MFA.
- aws sts get-session-token --serial-number arn-of-the-mfa-device --token-code {toke from MFA app} --duration-seconds 3600
- To use this aws credential we can create it's profile with addition of aws_session_token to do API calls.
- AWS CLI use python sdk boto3

### Exponential Backoff(AWS Services)
- If you get ThrottlingException(API call rate limit reached) intermittenly, use exponenetial backoff.
- Retry mechanism already included in AWS SDK but with AWS API you are responsible for that(also only retry for error code 5XX).

### AWS CLI Credentials Provider Chain
AWS CLI look for credential in this order:
- **Command line options**: --region, --output, --profile
- **Environment variables**: AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN
- **CLI crendential file**: aws configure
~/.aws/credentials on Linux / Mac & C:\Users\user\.aws\credentials on Windows
- **Container credentials**: For ECS tasks.
- **Instance profile credentials** - for EC2 Instance Profiles.

### AWS credentials best practices
- Overall, never ever store aws credentails in your code.
- Best practice is to be inherited from the credentials chain.
- If working within AWS, use IAM roles:
  - EC2 instances Role for EC2 instances.
  - EC2 Roles for EC2 tasks.
  - Lambda Roles for lambda functions.
- If working outside AWS use environment variables/ named profiles.

### Signing AWS API requests
- With AWS HTTP API call you need to sign the request so that AWS identify you using your AWS credentials.
- Request from AWS SDK, CLI don't require this.
-  AWS use Signature v4(Sig V4) for siging request which we need to use when doing AWS HTTP API Call.

Get EC2 instance info from within instance http://169.254.169.254/latest/meta-data/
IAM Roles can not be attached to on-premises servers.

### AWS S3 Access Log
- For audit purpose you may want to log all access to S3 buckets.
- Any request made to S3 from any account, authirized, unauthorized will be logged into another s3 bucket.
- Do not make your application bucket to be same as logging bucket as it make bucket to grow exponentially. Reason is on log in that bucket will get log and that log get log again as object which make size expo grow.

### S3 Replication (CRR & SRR)
- Must enable versioning in source and destination
- Cross Region Replication(CRR). Use case: compliance, low latency access, replication accross accounts.
- Same Region Replication(SRR). Use case: log aggregation, live replication between production and test accounts.
- Buckets can be in different account. Use S3 replication.
- Copying is asynchronous. But is very fast.
- Must give proper IAM permission to S3.
- **Note**:
  -  After activiation, only new objects of bucket are copied(not retractive).
  -  No chaining of bucket 1-> 2->3.
  - Delete with same version ID is not copied for avoid malicious delete.

### S3 presigned URLs
- Can generate presigned URLs using SDK or CLI:
   - For upload (harder, use AWS SDK).
   - For download (easy, can use CLI).
- Valid for a default 3600seconds but could be change using --expire-in or using AWS SDK.
- Users given a presigned URL inherit the permissions of the person who generated the URL for GET/PUT. This mean if there is a IAM user for you and it doesn't have permission to upload  to S3 then presigned URL would not be able to upload it to.
- Examples:
   - Allow only login-user to download a premium video from your S3 bucket.
   - Allow user to download files from S3 by generating URLs dynamically.
### S3 Storage Classes
- Amazon S3 Standard - General Purpose.
- Amazon S3 Standard-Infrequent Access(IA).
- ... cover later.

### S3 Lifecycle Rule
- <img src="https://aws-course-resources.s3.amazonaws.com/s3-moving-btw-storage-classes.png" height="290" width="350"> 
- You can transition object between storage classes.
- For frequent access object, move them to STANDARD_IA.
- For archive objects which you don't need in real-time/frequently use GLACIER or DEEP_ARCHIVE.
- Moving objects can be automated using lifecycle configuration.
- Lifecycle rules define when objects are transitioned to another storage class.
- Move objects to Standard IA class 60days after creation.
- Move to Glacier after 6 month. 
- <b>Expiration actions</b>:
  - Access log files to get deletede after 365 days.
  - Delete old veresion of objects (if versioning is enabled).
  - Delete incomplete multi-part uploads.
- Rules can be created for certain prefix(ex - s3://mybucket/mp3/*).
- Rules can be created for certain object tags. eg; dep.finanace

### S3 Performance
- Amazon S3 can automatically scales to high request rates, latency 100-200ms.
- You application can archive at least 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix in a bucket.

### KMS as limitation for S3 performance
- If SSE-KMS is enabled then S3 will call KMS service on your behalf and decrypt object and send that object to user. 
- This cause performance per second to little less(5500, 10000, 30000 req/s  based on region).
- But we can increase quota using Quota Service Console.
- But we can optimise performance using below methods:
  - **Multi-part upload**:
     - File divied in parts and upload data in parallel to S3 
     - recommanded for files > 100MB, must use for file > 5GB.
     - Help to parallelize uploads(speed up transfer).
  - **S3 Transfer Acceleration**:
     - Increase file transfer speed by transferring file to the AWS Edge location which will be forwarded to the S3 bucket in target region.
     - Compatible with multi-part upload.
  - **S3 Byte-Range Fetches**:
     - Parellize GETs by requesting specific byte ranges.
     - Better resilience in case of failure as you can retry to get range of bytes in a file.

### AWS S3 Select & Glacier Select
- Filter day from S3 object using SQL which perform server side filtering.
- Can filter by row & column. Very fast & cheaper.

### S3 Event Notification
-  <img src="https://aws-course-resources.s3.amazonaws.com/s3-event-notification.png" height="290" width="350">
- Event use to happen in S3 when object created, deleted. eg;S3:ObjectCreated, S3:ObjectDelete,etc.
- Can filter object by regex for notify. eg; /*.jpg
- Use case: generate thumbnail after video got uploaded to S3.
- S3 event delivered in second but could take a minute or longer sometime.
- As it could be possible that 2 write happening in single object so it better to have version enabled.  Reason is with no version we will got only one event.

### Amazon Athena
 - Serverless query service to performa analytics against s3 objects.
 - Use standard SQL language to query files.
 - Only support JSON, CSV, ORC, Avro, Parquet(build on pesto).
 - Pricing: $5.00 per TB of data scanned.
 - Use cases:Business Intelligence/analytics/reporting, analyze.

### AWS Cloudfront
 - It is Content Delivery Network(CDN).
 - Improve read performance, content is cached at the edge location(A site that CloudFront uses to cache copies of your content for faster delivery to users at any location). 
 - 216 Point of Presence globally(edge locations).
 - CDN give DDoS protection, integration with Shield, AWS Web Application Firewall.
 - Can expose external HTTPS by loading certificates and talk to internl HTTPS backends if you needs to encrypt the traffic as well.
 - Edge location cache data and get from cache next time instead of redirecting request to Cloufront orgin. 
 - Files are cache for TTL.
 - Great for static websites.
 - Setup must for each region where your appliation will serve.
 - Files updated in near real-time.

### Cloudfront Origins
 - **S3**:
   - For distributing files and caching at the edge.
   - Enable security with CloudFront Origin Access Identity(OAI). Only cloudfront can access it.
   - Can be used as an ingress(upload files to S3 from anywhere in the world).
   - OAI use to provide role(same as IAM) to cloudfront edge though which it will access S3 and cache resource.
  
 - **Custom Origin (HTTP)**:
   - Application Load Balancer.
   - EC2 instance.
   - S3 website(must enable S3 as static site website).
   - Any HTTP endpoint.

 - **EC2 instance**:
   -  Security group must allow cloudfront to access EC2 instance.
   - For this we must allow public IP of edge location to be added to security group of EC2 instance.
   - This same apply to ALB.

### Cloudfront caching:
  -  Based on(for create/update cache):
     - Headers
     - Session cookies.
     - Query String parameters
  - Behaviour of caching decided by policy.
  - Can create multiple origin in distribution using url path. eg; /api/* -> ALB, /images/* -> S3.
  - Cache lives at each Cloudfront edge Location.  
  - TTL from 0sec to 1year.
  - We can invalidate part of cache using CreateInvalidation API.
  - Caching can be static like for S3 static content and dynamic if Cloudfront layer interact with some servers and ALB.
  - <img src="https://aws-course-resources.s3.amazonaws.com/cloudfront-distribution-type.png" height="290" width="450">

### Cloudfront Geo restriction
  - We can restrict who can access your distribution based on geography.
  - There will be 2list:
    - **Whitelist**: mention in list country where user can access it.
    - **Blocklist**: mention in list country where user can not access it.
  - Country is detemined using 3rd party Geo-IP database.
  - Use case: Copyright Laws to control access to content.

### Cloudfront and HTTPS
  - Viewer Protocol Policy:
    - Redirect HTTP to HTTPS
    - use HTTPS only.
  - Origin Protocol Policy(HTTP or S3):
    - HTTPS only.
    - or Match viewe(HTTP => HTTP & HTTPS => HTTPS).

  Note: S3 bucket "websites" don't support HTTPS.

### Cloudfront URL /Cookies
... later write notes.

### Cloudfront signed URL - Key groups + Hands On
  - Cloudfront use keys public and private to create sign URL(using private key by backend) and to verify it by cloudfront using public key.
  - Type of signers:
    - Either trusted key group(recommended): Can leverage APIs to create and rotate keys(IAM for API security).
    - An AWS account that contains a Cloudfront Key Pair: Manage keys using root account and AWS console. Not recommended.

### Cost
- Cost vary with each edge locations.
- Classes:
   - Price Class All:
.. read seperatly.

- **Origin group**
  - Use primary origin to fetch resource. If primary fail use secondary origin.

Chinto@1997
## ECS
 - Amazon ECS makes it easy to deploy, manage, and scale Docker containers running applications, services, and batch processes. Amazon ECS places containers across your cluster based on your resource needs and is integrated with familiar features like Elastic Load Balancing, EC2 security groups, EBS volumes and IAM roles
### ECS Clusters
- An Amazon ECS cluster is a regional grouping of one or more container instances on which you can run task requests. Each account receives a default cluster the first time you use the Amazon ECS service. Clusters may contain more than one Amazon EC2 instance type.
- EC2 clusters are logical grouping of EC2 instances.
- EC2 instances run then EC2 agent(The Amazon ECS container agent is a software that AWS has developed for its Amazon EC2 Container Service that allows container instances to connect to your cluster).
- EC2 agents registers  then instance to the cluster.
- EC2 instances run special AMI made specially for ECS.
- Config for cluster will be found in /etc/ecs/ecs.config

### ECS task Definitions
- Metadata in json format to tell ECS how to run a Docker container.
- Contain crusial information about:
  - Image name
  - Port binding for container and host.
  - Memory and CPU required.
  - Environment variables.
  - Networking information.
  
### Service Type of task schedular  
  - Replica: The replica scheduling strategy places and maintains the desired number of tasks across your cluster. By default, the service scheduler spreads tasks across Availability Zones. You can use task placement strategies and constraints to customize task placement decisions

  - Daemon: The daemon scheduling strategy deploys exactly one task on each active container instance that meets all of the task placement constraints that you specify in your cluster. When using this strategy, there is no need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies.

  ...


## DynamoDB

## WCU(Write Capacity Unit)
 - One WCU is one write per second for an item of size upto 1KB in size.
 - eg we write 10items per second, with item of 2KB. We will require 10*(2/1)=20WCUs.
 - on-demand is more expensive then provised capacity mode.

## Strongly Consistent Read Vs Eventually Consistent Read
 - <img src="https://aws-course-resources.s3.amazonaws.com/dynamodb-consistent-read-types.png" height="300" width="400">
 - In **Eventually Consistent Read** if we read just after write then it's possible we will get some stale data because of replication.
 - In **Strongly Consistent** if we read just after write we will get correct data
 - For SC you need to set "ConsistentRead" parameter to True in API calls(GetItems, BatchGetItems, Query, Scan). Consume twice the RCUs. SC is slow.
 - 1RCU=1Strongly consistent Read per second or 2 Eventually Consistent Reads per second, for item upto 4KB in size. For item size > 4KB more RCU are consumed
 - Note if size is something which is not fully divisible with 4 then roundoff it. eg if size is 6KB->8KB