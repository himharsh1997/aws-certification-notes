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
