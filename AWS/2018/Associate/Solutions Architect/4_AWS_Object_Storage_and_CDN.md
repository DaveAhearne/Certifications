# AWS Certified Solutions Architect

## S3 101

S3 is almost as old as AWS itself, and is one of the first services ever made public. The name S3 stands for **Simple Storage Service**, it provides simple, durable, secure, object storage as a service

S3 has a simple web interface, and allows you to store and retrieve data from anywhere on the web. It **doesn't have storage limits** but you do pay per gigabyte and there are some other fees involved e.g. transfer

**What do we mean by object storage?** - There are two main types of storage with AWS:
* **Object Storage** - This pertains to things like simple files, think of it like a folder that you can drag things into on a pc
* **Block Based Storage** - This is similar to your hard drive in that it would allow applications to be installed to it

**Why is S3 safe?** - Because your data is duplicated and **distributed across multiple availability zones (A minimum of three) in the region that you create the S3 bucket**, this means in the event that a data center fails your data is still secure

### S3 Facts

* S3 is Object based
* Single files can be 0 Bytes to 5 Terrabytes
* There is **no** limit on total storage
* Files are stored in **Buckets** (Top Level Folders)
* S3 **Buckets must be globally unique in their names**
* You get a 200 OK back if the upload was successful

Each bucket created *has its own directly accessible DNS address* this is why the S3 bucket name must be globally unique, the S3 urls are typically in the format of:
https://s3-{REGION}.amazonaws.com/{BUCKET NAME}

### Data Consistency Model

*Read after Write consistency for PUTS of new objects* - This means that if we write a file then immediately read it back the consistency will be the fact that **the newly created object will always be available to read**

*Eventual consistency for overwrite PUTS and DELETES* - This means that when we update a file or delete it, there isn't a garuntee that it will be done immediately or within an expected timeframe, but that it will definately be done *at some point in the future* (Because S3 is spread accross multiple availability zones for redundancy, changes take time to propagate through the system)

Therefore:
* If you're creating a new file, you *will* always get the latest version
* If you're updating or deleting a file, you *may or may not* get the latest version

### Storage Model

S3 object storage is key based, this means that each file stored in S3 consists of:
* A Key - The name of the object e.g. my_spreadsheet.xls
* The value - The data inside the file
* A version ID - Important for versioning
* Metadata - e.g. first uploaded at, tags, last modified at, etc.

**Access Control Lists** - This is extra controls that you can add on a file by file basis, for example if you had a very sensitive document such as Payroll or HR information, you could limit access to this file in a way that it would only be visible to the relevant people

You can also *torrent* the file as peer to peer between users - In this scenario AWS acts as a seeder, so the total cost of transferring files would be slightly lower than client/server because the clients are able to transfer data between each other

### S3 Overview

S3 is **built for 99.99%** availability, and Amazon will guarantee **99.9%** availability - SLA for S3

S3 has **eleven nines** for durability of s3 information, this means that its incredibly unlikely that any information that you store in S3 will be lost - Practically speaking this is 100% durability

S3 also has a **lifecycle management utility** built in, this means that for example we wanted to remove all files that were older than 90 days, perhaps in the case of rolling log files, we can have AWS automatically remove them when they reach this age or move them onto different storage for archiving

S3 implements versioning of files, so if we wanted the ability to store versions of a document e.g. records of all staff for each month

S3 supports encryption in transit and at rest

Access control is supported with Access Control Lists at the bucket level, so we could prevent developers from accessing any files in the Finance bucket for example

S3 has a number of tiers of storage type, depending on your requirements these are:
* **S3 Standard** - Two nines of availability, Nine nines of availability and is stored accross multiple devices in multiple facilities - Its designed to sustain the loss of two facilities (Availability Zones) or multiple devices (Disks) concurrently
* **S3 - IA (Infrequently Accessed)** - This is good for files that arn't accessed often, but you need fast access when they're requested *(Think obscure help documents on your company website)* - It has a **lower fee than S3 standard, but you're charged a retrieval fee**
* **S3 One Zone - IA** - The same as standard IA, but only stored in a single availability zone this means that you lose some of the durability but in exchange you're charged less than the other two options
* **Glacier** - The cheapest of all of the S3 storage options, this is typically used for long term data backup archives e.g. old customer transactions for compliance reasons, It takes a *long* time to retrieve data back the standard time is between 3-5 hours


With Glacier there are a few retrieval types with pros and cons for each:
* Expedited - Data is returned within a few minutes, but there is a significant cost associated with this
* Standard - Between 3-5 hours to retrieve data, the middle of the road option for cost
* Bulk - Typically used for *large* amounts of data transfer, the lowest cost option of the three but takes the longest at between 5-12 hours

So typically:
* If you want huge storage at a cheap price, but you don't care about retrieval times - **S3 Glacier**
* If you want cheaper storage for something that's not hit a lot, but fast retrieval times - **S3 Infrequently Accessed**
* If you don't care about cost, its hit a lot and you need fast retrieval or a big focus on durability - **S3 Standard**

**S3 Glacier has NO availability SLA**

**S3 Standard is the only S3 storage that does NOT have a retrieval fee**

### S3 Charges

Things that you will be charged for in S3 are:
* Storage - Per Gigabyte
* Requests - Number of Requests
* Storage Management Pricing - Charged for any metadata/tags
* Data Transfer Pricing - Moving data from one region to another
* Transfer Accelleration - Speeds up file transfer between the S3 bucket and the end user by leveraging the CloudFront edge locations CDN

### Exam Tips

* S3 is Object based (Allows file uploading, but no installing)
* Single files can be up to 5 Terrabytes in size
* Unlimited Total Storage
* Files are stored in Buckets
* The bucket names **must** be globally unique
* The S3 urls are in the format of https://s3-{REGION}.amazonaws.com/{BUCKET NAME}
* Read after write consistency on New objects
* Eventual consistency on Update and Delete on Objects
* Has a 200 OK status code on upload
* READ THE S3 FAQ BEFORE THE EXAM - ITS THE OLDEST SERVICE AND IT COMES UP A LOT

Objects always consist of:
* A key (Name)
* A value - the data of the object
* Version ID - to track the version of the object
* Metadata - keeps track of things like access times, created at dates, etc.
* Access Control Lists - Limits access to a known group of users/group
* Torrent Information

**S3 is for file storage only, you cannot install anything on it**

## Creating an S3 Bucket

S3 is under the storage tab of the AWS console overview, you'll notice that when you go onto the S3 overview the region changes to **global** and clicking on it will show "S3 does not rqeuire region selection", this is because S3 is a global service

**You can deploy your buckets in any region, but the service overview is global**

To create a new bucket:
1. Click create bucket on the S3 splash screen overview
2. Add a name, remember that this name **must be globally unique and also DNS compliant e.g. no ! because they arn't valid in an address** 
3. Choose a Region to deploy the bucket into
4. If you want at this stage you can just hit create and it will create the bucket or you can refine configuration and permissions in further steps
5. Select relevant options that you want e.g. adding tagging to the bucket to track costs by team/deparment/division etc
6. Review the permissions, by default buckets are now private until explicitly told otherwise
7. Review the bucket
8. Create the bucket

By default now, on the permissions page **none of your S3 files in the bucket will be public** due to the concern around sensitive information in buckets being public by default. Therefore the default access control lists will block you making anything in your bucket public **until you remove the blocking Access Control List**


### Uploading Files

We can now go into the bucket that we've created by clicking on the bucket name, because the bucket is empty by default it will show us a splash screen with some detail on things that we can do such as uploading a new object

Details on the bucket are in tabs along the top detailing things like, Properties, Permissions and Management

Properties contains a list of things such as:
* Disabling/Enabling versioning of files
* Setting up access logging
* Hosting a static website from this bucket
* Enabling encryption

As well as some advanced settings like triggering events from transfers or enabling transfer acceleration, **any of these properties can be changed at any time**


Permissions contains things such as:
* Public access settings - Blocks public access to data until explicitly removed
* Access Control Lists - Regulates access to singular objects
* Bucket Policy - Regulates access to entire buckets
* CORS configuration - Defines allowed other origins that can make requests

Management settings contains things like:
* Lifecycle rules - e.g. clear out files after X
* Replication 
* Analytics
* Metrics
* Inventory

To upload a file in the GUI, go to the overview page and click upload and select the file you want to upload from the directory prompt, you will be prompted to choose permissions such as read/write access and the **storage class** of the file that you want, for example Standard/Standard-IA/Etc after which you'll get a **200 Response Code** back to confirm the successful upload

If you click on the file in the overview you'll see a breakdown of the file including its prperties, owner and permissions, there is a link to the file in the S3 bucket as a HTTP URL, however remember that **by default, anything you upload to an S3 bucket is private**

### To make a file in a bucket public

You used to be able to do the following:
1. Click the file in the S3 overview
2. Go to the actions dropdown
3. Click on "Make Public"

However, this process has changed due to some of the previous security concerns around S3 buckets containing unintentionally public information if you attempt to do this you'll see an error notification at the bottom of the screen 

What you need to do is:
1. Go up to the overview of the s3 service where the buckets are listed
2. Click a bucket and select "Edit public access settings"
3. Disable all of the public access ACL settings (These prevent you from making any objects in your bucket public till they're disabled)
4. Save this

This will change the access of the public so that objects **can** be made public, this doesn't mean that everything in the bucket is made public **only that when you attempt to select files to make them public, you won't get the error anymore**

### Exam Tips

* Buckets names must be globally unique
* Uploading an object to S3 returns a 200 OK
* S3, S3-IA and S3 Single Zone - IA / S3 reduced redundancy all offer S3 storage at different price points depending on how quickly you need access
* Encryption is offered at client side or server
* Server side encryption is offered through **Amazon S3 Managed Keys (SSE-S3), KMS (SSE-KMS) or Customer provided keys (SSE-C)**
* You can control access to the bucket using ACL or a policy
* By default **buckets are private** and you cannot change this until you explicitly disable the relevant ACL


## Version control

To enable versioning for a bucket:
1. go to the bucket overview
2. Go to the properties tab
3. Select versioning
4. Enable versioning

You'll see a purple tick to confirm that verisoning is enabled, you'll notice that once you enable versioning **you can only suspend it**, so its not possible to completely disable versioning after its been created *(As what would we do with the versions if any we currently have stored if we attempted to completely remove versioning?)*

So if you have an original file, and you upload a new version of the file, at the top of the list of files for the bucket you'll see a selector *Versions hide/show* by default the versions are hidden but toggling this option will show them in the UI


**If you have a public file, and you upload a new version - You need to remake the file public again as it will default back to private**

If you delete a file from the bucket it appears to be deleted, but if you go to show all versions you'll see that it is in there but it has an additional version called a **delete marker**, the important thing to remember here is that when you delete you don't actually delete anything

To restore a file:
1. Show the versions of the file
2. Delete the delete marker on the file

To permenantly delete a file:
1. Show the versions of the file
2. Go through each version and manully delete each version entry

If you do go in and delete a later version **note how the url changes to reflect the version number as a query parameter**, also if you make a file public, upload a new version then delete the new version so you're only left with the original **AWS remembers the permissions given and the original version will still be public**

**You can put Multi Factor Auth around delete if need be**

### Exam tips:

* Versioning stores all versions of an object, including writes and deletes
* Its good as a backup utility
* After enabling, versioning can only be suspended not removed
* It can be integrated into lifecycle rules
* Versioning can have Multi Factor added to the delete capability for extra security

## Cross Region Replication

If you want to replicate the contents of one bucket into another thats located in another region to improve latency to the end user geographically or to improve redundancy

**Cross region replication buckets must exist outside of the current region** for example you couldn't cross region replicate a bucket from London to London, and **If the source bucket has versioning enabled, the target must also have versioning enabled**

If you:
1. Go to the bucket settings
2. Go to the management tab
3. Select the replication option
4. Select the Get started button
5. Define the source of the information which can either be the entire bucket, or certain files with a prefix(Sub-folders) or specific tags associated
6. Define the destination bucket **this can either be this account or another account**
7. You have the option to change the storage class of the desintation also, so for example you could replicate older files into an infrequently accessed bucket to reduce cost
8. You will have to either create a new role, or specify an existing role for the S3 replication to run under
9. Hit save to enable

**When you Cross Region Replicate, the existing files are *not* copied to the destination, only further changes or files after enabling**

To just copy files from one bucket to another you'll need to install the AWS command line tools

### AWS Command line tools

Search and download the AWS bundled tools from Amazon via the installer

Once you've installed the tools run:
```
	aws configure
```

Then enter the supplied access key id you recieved when you created the IAM account with SDK access, followed by the secret access key

Declare the default region name that you want to log into, hit enter on "output form"

To test that you can see S3 from the console and that your login worked correctly, run the following:
```
	aws s3 ls
```

**This will list all of the S3 buckets visible to that user**

To copy all of the files from one S3 bucket to another:
```
	aws s3 cp --recursive {Source} {Destination}
```

Where the Urls are in the format:
```
	s3://my-bucket-name-goes-here
```

**The delete marker for files will *not* be replicated when using Cross Region Replication**, if you think of this being used in the context of a backup you would want this style of behavior, however any changes to the files **will** be replicated

At this time **daisy chaining** region replication is not permitted, *but this could change at any time*

## Lifecycle Management with Glacier & S3-IA

This is important for scenarios for example where data is only relevant for a period of time, in which time afterwards you might want to transition it away from high cost low latency storage to low cost high latency storage - For example the last 90 days of customer transactions you might want immediately accessible, but periods after that might be archived for compliance, but without the expectation of speedy retrieval

Some use cases for this might be:
* Keep files in S3 for first 30 days
* Transition to S3-Infrequently accessed for another 30 days
* After another 30 days either move to Glacier for long term storage or remove entirely

This feature of life cycle management for files is **built into the S3 storage service in AWS**

### Glacier

You'll see from the AWS console menu that the Glacier storage service that its separate from S3, you will also notice that **glacier storage is not available in every region**

To set up a lifecycle for a bucket:
1. Go to the management tab of a created bucket
2. Click to Add a lifecycle rule
3. Enter the rule name and a prefix/tag to limit the scope of affected objects if required
4. Select a transition version, so move the old objects or the current version of the object
5. Select a transition medium, so whether to move to S3-IA or glacier, and the number of days of retention before the move
6. Expiration is defined in days in the next screen but it can be left blank

**You can define multiple transitions within a single lifecycle management rule**

### Tips:
* Can be used with versioning
* can be applied to current and old versions of files
* Can transfer to S3-IA/Glacier storage
* Can expire objects after a cetain amount of time

## CloudFront CDN
 Its key to understand what the concept of a CDN is for the exam - A Content Delivery Network or **CDN**, is a system of distributed servers that deliver content to a user based on the geographic origin of the webpage and a content delivery server
Essentially a CDN is a proxy for content that is located closer to the user to speed up the delivery of content, rather than serving it all from a centralised 


In this scenario:
1. The user hits the distribution URL
2. This hits the Edge location for the edge location next to the user
3. The request is forwarded onto the Origin storage
4. The file is passed through the edge location back to the user
5. This file is then stored at the CDN (This cache lasts for the length of the TTL)
6. Subsequent requests use the cached version inside the edge location and do not pass through to the origin


### Key Terminology

* Edge Location - The location where the content will be cached, this is a seperate entity to a Region or Availability Zone
* Origin - The origin of the files to be distrubited, whether this is an S3 bucket, Ec2 instance, ELB or Route53
* Distribution - The name given to the CDN which is a cluster of Edge Locations (You create distributions like buckets)
* TTL - (Time To Live) - Defines the length of how long the cached content is valid for
* Web Distribution - Used for Websites
* RTMP - Used for Media Streaming (Real Time Media Protocol)

An important thing to understand is that when the cache is cold **the cold hit to the cache is not faster than a normal route, however its faster on subsequent calls** as by then the cache is warm and content can be served directly from the cache

CloudFront can be used to deliver:
* Dynamic content
* Static content
* Stremaing content
* Interactive content

It also integrates nicely with AWs serves like S3, EC2, ELB and Route53 **as well as as any non-aws origin servers** which can define the original versions of files. **Requests for content are automatically routed to the nearest edge location**

An important thing to remember is that **edge locations are not just read only endpoints** you can write to an edge location as well, and they're cached for the lifetime of the TTL. **You can clear a cache at any point, but theres a charge for it**

## Creating a Cloudfront CDN

To create a Cloudfront Distribution:
1. Go to the cloudfront service in the AWS console
2. Press Create Distribution
3. Select the distribution type that is relevant to your needs
4. Select the Origin Domain Name - This defines the source of the content to serve
5. The origin path is the folder within - e.g. within an S3 bucket this would be the path to the folder that you want to serve
6. Origin ID is a user description to represent that origin within the distribution e.g. "Toms source S3 bucket" - **they need to be unique, but only within the same distribution**
7. Choose Restrict Bucket Access - This prevents users using the direct S3 url and forces the distribution as a reverse proxy

Path Patterns can be used with regex to say *"If the requested path matches this pattern, then fetch the file from this origin instead of that one"*

Time To Live are *defined in seconds*

**Retrict Viwer Access (use Signed URLs or Signed Cookes)** - What if as a company, you had a large amount of material you wanted to distribute globally to all employees, but *only* employees? In this case you would want to restrict access to only these people, you do this via this option

It also gives us the option to **restrict the number of available edge locations to reduce cost** - The default is to use all of them for best performance, but you can limit it to only use US, Europe and Asia or only US and Europe to reduce cost at the price of increased latency the further the user is from a relevant edge location

CloudFront also has the option to *secure endpoints through a WAF or Web Application Firewall*, you can also supply alternate domain names via CNAMES as an option to access the distribution created as by default *a randomly generated prefix is supplied that isn't very human readable*

You can also choose your own certificate or the supplied one, *if you want to use your own domain you'll need to provide your own custom SSL certificate*

**Default Root Object** - If you hit the naked CloudFront URL, what object should be returned? E.g. if it was a web distribution you might want the index.html to be returned

**You can have multiple origins to a single distribution**

Then just hit create to start the creation, *it can take between 5-10 minutes to create the distribution*, when the status changes to deployed the CloudFront distribution has been created and can be used. After this point you can then use the CloudFront url as a substitute URL to access the origin content that you were originally pointing at

To view and change settings for the individual distribution go to *Distribution Settings* this panel shows you all of the options for the current distribution and and sources associated with it - **You can add multiple origins from here also**, behaviors can also be added and changed from here for example **"If the path contains .pdf, get the file from this source as this bucket contains all the pdf files"**, set custom error pages and **geo-restrict content** after enabling you can do it based on black/whitelist and select the countries you want to use

To remove an object from the CloudFront cache, go to the **invalidations** tab and create a new invalidation, bear in mind however that this has a cost associated with it

To delete a distribution you need to disable it first


## S3 - Security and Encryption


Some basics of security around S3 are:
* All newly created buckets are private by default
* Bucket Policies - To control access to the entire bucket
* Access Control Lists - To control access to items within a bucket
* You can enable access logs - Can be done to another bucket/account

S3 also supports **encryption**, for S3 there are two main categories of encryption:
* In Transit (SSL/TLS Based) - This is when the data is being moved anywhere, for example from S3 to another service such as a Lambda or from your PC to the bucket
* At Rest - This is when the data is sat inside the S3 bucket

For at rest encryption there are two main sub-categories,

Server Side Encryption:
* **(Most Common)** S3 Managed Keys - SSE-S3 - **AES-256** - Each object gets a unique key that its encrypted by, these keys are then encrypted with a master key that Amazon rotates for you regularily *(You just use the bucket as normal, the Encryption is handled seamlessly for you in the background)*

* AWS Key Management Service, Managed Keys - SSE-KMS - Where KMS stands for *Key Management Service*, this is similar to SSE-S3 but with some extra charges - There are seperate permissions for the **envelope key** this key is a key that **protects your data encryption key** which helps prevent unauthorized access, **it also provides an audit history of who used the keys and when** and you can also **manage the keys yourself** or use the default unique key (To the service and region)

* Server Side Encryption with Customer Provided Keys - **SSE-C** - This is when **AWS handles the encryption/decryption for you, but you manage the keys** 

Client Side Encryption:

This is where you encrypt the data on your side *before* you upload it to S3, this has some benefits in terms of ensuring that it is always encrypted even in transit but can take a little extra work as you will need to do the encryption/decryption by yourself on the client side


#### Things to remember
You can secure your buckets in a number of ways:
* Secure your entire bucket via a bucket policcy
* Secure individual objects via Access Control Lists
* Enable logging to see what people are doing
* Secure data travelling to and from S3 with SSL
* Stored data can be encrypted via SSE-S3, SSE-KMS, SSE-C or encrypted before it is sent on the client side

## Storage Gateway

Storage gateway is a service that connects an on-premise software appliance (A virtual machine) with cloud based storage, this lets you integrate securely between your on-premise infastructure and AWS. So the flow of this looks like:

* You set up a VM in a hypervisor
* You install the Storage Gateway appliance in this VM
* You then use this appliance, and any changes to its data are sync'ed back up to AWS S3/Glacier

Storage gateway serves the appliance as either VMware ESKi or Microsoft Hyper-V, once you've installed it you can choose the storage gateway option for what you need

There are **four** types of storage gateway:
* *File Gateway* (NFS) - Stores flat files in S3
* *Volume Gateway* (iSCSI) - Block based storage that you install things on, e.g. a drive that a VM is installed on etc 
* *Tape Gateway* (VTL) - Backup and archiving solution, lets you create virtual tapes e.g. might store these in Glacier

Volume gateways are further broken down into:
* Stored volumes - An entire copy of the data is kept on-site
* Cached volumes - Only the recently accessed data is stored locally


These **used to** be called:
* Cached Volumes -> Gateway Cached Volumes
* Stored Volumes -> Gateway Stored Volumes
* Tape Gateway -> Gateway Virtual Tape Library

**File Gateway** - Files are stored in S3 buckets and accessed via the Network File System (NFS), once they're transferred to S3 you can manage them as native objects so versioning, management, replication etc. You can connect a file gateway in three different ways, via the public internet, via a VPC or via AWS Direct Connect. The VPC connection means that its possible to have a File Gateway setup from an EC2 VPC subnet backing up to S3 via the file gateway as the Storage Gateway is just an application that is installed on a virtual machine *but the most common use case is to connect on-premise hardware to the cloud*

**Volume Gateway** - Presents your applications with a disk sitting on top of the iSCSI block protocol, any data to these disks is asyncrenously backed up to the cloud as snapshots, these snapshots are then stored in the cloud as EBS sanpshots. **Snapshots are incremental, and compressed** so only the changes from the last snapshot are stored

The size for gateway stored volumes **is from 1Gb to 16TB in size**

**Volume Gateway - cached Volumes** -  lets you use S3 as your primary data stroage which keeping frequently accessed data locally. Minimize the need to scale on-site storage to meet demand but keeps frequently accessed data close to keep latency down, **volumes can be 1GB to 32TB in size**

**Tape Gateway** - Uses a virutal tape based library, interfaces with existing backup tools suhc as NetBackup, Backup Exec, Veeam. Mainly used for backup and long term archiving of on-premise data for things such as transaction reports
, consists of virtual tape cartidges on the tape gateway

The storage gateway is covered under S3 because its an S3 product, as most of the backups will back onto the S3 service, even the block based iSCSI drives which are stored as images of the drive, typically exam questions are scenario based such as "Wanting to store a lot of flat files with storage costs at a minimum" in which case File Gateway would be our answer

## Snowball

AWS Import/Export was a service that accelerated the transfer process by using a physical portable storage device for transport. The import/export disk transfers your data directory onto and off AWS storage devices using AWS's high speed internal network and bypasses the public internet entirely

For example, if you want to put 10 Terrabytes of data into AWS for archival storage or big data processing, but you live in the countryside with a poor/unstable connection you could use a snowball to transfer this data, *the old way of doing this is that you could ship a hard drive to amazon* but this process for depreciated because of AWS having to deal with lots of different disk types/connections that was a nightmare to manage

There are three types of snowballs:
* Standard
* Edge
* Snowmobile

A snowball looks like a briefcase - Its tamper resistent, uses 256 bit encryption, uses a TPM module for full chain custody and is software erased on arrival at AWS, they can transport up to 80TB worth of data in a single snowball - **this data is then transferred into S3**


A snowball edge is similar to a standard snowball, but **the edge has some compute facilities** - this means that it can be used as a drop in replacement for your data centre in a pinch, for example you could take it on a plane and run lambdas on the input data then drop off the snowball edge back at a drop off point. They can carry up to 100TB of data

A snowmobile is for petabyte or exabyte level data transfer, its a big truck shipping container full of drives. **100 Petabytes of capacity per snowmobile** you can order up to 10 of these for a full exabyte, this would take roughly six months

If you're using glacier **you'll have to restore to s3 from glacier** before uploading the data to the snowball

### S3 Transfer Acceleration

Transfer acceleration uses the edge network to improve speeds when uploading to an S3 bucket, you will get a different URL for this in the format of:
*{your-bucket-name}.s3-accelerate.amazonaws.com*

*How does it work?* You upload the file to the edge location that is physically close to you, this file is then transferred to the target region by using amazons backbone network instead of the public internet, the advantage of doing this is that you're able to use amazons network which will be significantly faster

To enable transfer acceleration:
* Create or select an existing S3 bucket
* Go to the properties tab
* select the "Transfer Acceleration" property and enable it

To use transfer acceleration you **must use the generated endpoint in the property** as the regular url will upload to S3 directly

## Creating a static website in S3

This can be useful to do for a number of reasons for example:
* You don't have to worry about servers
* Maintainance
* Scaling
* Load-balancing

**You can't host dynamic content on S3** so while you could put a static site with JavaScript on S3 or a SPA React Application, **you would not be able to host .net or PHP**

To setup a new static website:
1. Create or Select an existing S3 Bucket
2. Edit the public access settings for the bucket to allow files inside the bucket to be made public
3. Go to the properties tab of the bucket
4. Select "Static Website Hosting" and enable it
5. Provide a root index.html page and an error.html page
6. Bucket Hosting will now have a purple tick to indicate that static site hosting is enabled


The endpoint for the website will be listed underneath the property name in the bucket settings, it typically takes the format of:
```
	https://[bucketName].s3-website-[regionName].amazonaws.com
```

By default, **any objects added to a bucket are private** but we can change this by:
1. Going to the Permissions tab under the bucket properties
2. Selecting the Bucket Policy tab
3. Add the policy from below to make everything in your bucket public by default
4. You'll get a warning that the entire bucket is public, but this is acceptable in this scenario so this warning can be ignored

The policy for making your S3 bucket completely public is:
```
{
	"Version": "2012-20-17",
	"Statement":
	{
		"Sid": "PublicReadGetObject",
		"Effect" "Allow",
		"Principal": "*",
		"Action": [
			"s3:GetObject"
		],
		"Resource": [
			"arn:aws:s3:::YOURBUCKETNAME/*"
		]
	}
}
```

## Summary

### S3 101

* S3 is object based file storage (You don't install stuff)
* Objects in S3 consist of a key (the object name) and the value (the actual data), version id, metadata, Access control lists
* Files can be 0 Bytes to 5 Terrabytes
* Unlimited Storage
* Files are stored in buckets
* Buckets have *universal* namespaces and must be globally unique
* Speed up uploads by using multi-part uploads
* Successful writes give you back a 200 OK
* S3 bucket names are in the format https://s3-[region].amazonaws.com/[bucketname]
* If you get an error about the file exceeding the upload size for an object, design your application to use the **multi-part upload api**

* S3 has **read after write consistency for new objects**
* S3 has **eventual consistency for PUTS and DELETES**

### S3 - Storage Tiers

* S3 Standard, 2x 9's availability, 11 x9 durability, stored redundantly accross multiple devices in multiple facilities, designed to sustain the loss of two facilities concurrently
* S3 - Infrequently Accessed - Cheaper storage cost but has a retrieval fee
* S3 - One Zone - IA (Replaces Reduced Redundancy Storage or RSS) -  99.99% Availability - Same as infreqently accessed but does not have the redundancy accross facilties, is cheaper than both
* Glacier - Cheapest of all, but used for archival storage only. Has retrieval options of Expedited, Standard or Bulk. Standard retrieval times are betwen 3-5 hours

### S3 - Versioning

* this stores all versions of an object, including all writes and even deletes
* You pay for **each version of an object** e.g. 1Gb file with 10 changes will charge for 10Gb storage
* deletes on versioned files simply have an additional delete marker written over the top of them
* Once you enable versioning, it can only be suspended not disabled
* You can use lifecyle rules
* MFA can be added to deletes
* For cross region replication, versioning must be enabled on both the destination and the source

### S3 - Lifecycle Management

* Can be used in conjunction with versioning
* Can be applied to current and previous versions
* Transitions to S3 IA must be 128kb large and 30 days after the creation date
* Transitions from S3 IA to Glacier can be done 30 days after IA (If transferring directly there is no limit)
* You can permanently delete after a period

### CloudFront

* Edge location is a place where content is cached, its a seperate entity to an Availability Zone or region
* Origin - Where the files will be served from
* Origins can be an S3 Bucket, Ec2 Instance, Elastic Load Balancer or Route53
* A Distribution is the name of the CDN that you've created, it consists of a collection of Edge Locations
* Two types of Distributions, Web Distributions for Websites and RTMP for media streaming
* We can **write to** edge locations as well as read from them, changes are cascaded up to the origin on the AWS Backbone
* Objects get cached for the Time To Live in seconds (TTL) default is 24h
* You can force a cache clear but it costs money

### S3 Security
* By default, buckets are completely private
* You can use bucket policies to set access control at a bucket level
* You can use Access Control Lists to set access control at an object level
* Can be set to create access logs
* In transit encryption is achieved via SSL/TLS

### S3 Encryption
* AWS use AES-256
* S3 Managed Keys - SSE-S3 - All keys are encrypted with a unique key, these keys are then encrypted with a master key that is managed and rotated by AWS. This is done seamlessly in the background
* S3 Key Managed Servvice - SSE-KMS - Similar to S3 but you have more control over the envelope key that wraps the object encryption keys
* S3 Customer Managed Keys - SSE-C - You manage all the keys yourself
* Client side - When you encrypt it yourself


### Storage Gateway
* File Gateway - A proxy to S3 for flat files only
* Volume Gateway - For proxying entire sCSCI disks to AWS
* Stored/Cached Volumes - The types of Volume gateway where you may keep some of the data on site for speed
* Gateway Virtual Tape Library - For Archival backup, to be used with existing storage soution

### Snowball
* Snowball - Pure storage and comes between 50Tb and 80Tb
* Snowball Edge - Storage and compute capability such as being able to run lambda on it
* Snowmobile - 100 petabyte storage, a storage container
* Snowballs transfer to and from S3
* Import/Export was the old service of posting drives to AWS but it was canned

### Transfer Acceleration
* Speeds up transfers to and from S3 by using the edge network, has the best improvement on geographically far away locations

## Static Website Hosting
* Can host websites in s3
* Scales automatically
* Serverless
* Cannot host dynamic sites such as PHP or .Net on it

## General
* You get a 200 back on writing to S3
* You can speed up S3 uploads by using multipart uploads
* Read the S3 FAQ before you take the exam






















