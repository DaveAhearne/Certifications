# AWS Certified Solutions Architect

## 10,000ft Overview

### History of AWS

You will never be asked questions like *what year was EC2 launched* or anything like that in an exam situation

AWS gives you the freedom to experiment with services and infastructure but doesn't lock you into contracts, so you have the freedom to try things out. See them fail and then iterate on them to improve them or abandon them without prohibitive costs


Some overview of the history is:
* 2003 - Chris Pinkham and Benjamin Black present a paper on what Amazons internal infastructure should be, and proposed selling it as a service
* 2004 - SQS is launched (Simple Queue Service)
* 2006 - AWS is oficially launched
* 2010 - All of Amazon.com is moved onto AWS
* 2012 - First Re:Invent AWS conference
* 2013 - First certifications launched
* 2017 - AWS releases a number of AI services
* 2018 - Release Machine learning speciality certificate


### Overview

There are a **lot** of AWS services, you don't need to know them all to pass the exam and there isn't anybody that knows them all in great detail

Aws is a big toolbox full of lots of tools, you'll find that a lot of them are simply not relevant to your day to day operations but that you'll have a core set of a few that are useful to you and then you'll reach for specialized tooling when you need it e.g. Media transcoding or authentication

In the top right of the AWS console you will see the **Region Selector** this is important as *most* of the services are located within that specialized region *(Some such as s3 are region independant)* this may be important as sometimes in newer regions **not all services are offered in all regions**, also you may have **compliance reasons** for not wanting data to leave the country it was collected in

US East (N.Virginia) is the oldest AWS region, and will be the region where **new AWS services are trialed first**


In terms of the breakdown of categories, all of the services sit on top of the **AWS Global Infastructure** in seperate categories:
* Compute - EC2/Lambda 
* Storage - S3 for putting files into buckets
* Databases - Relational (RDS) / Non-Relational (DynamoDB)
* Migration & Transfer -Putting things into and getting stuff from AWS (Snowball)
* Network and CDN - Virtual Private Clouds & Cloudfront CDN
* Developer Tools
* Robotics
* Blockchain
* Satellite
* Management & Governance
* Media Services
* Machine Learning e.g. SageMaker
* Security, Identity and Compliance (IAM)
* Analytics
* Mobile
* AR & VR
* Application Integeration
* Cost Management
* Customer Engagement
* Business Apllicatins
* Desktop and App Streaming
* IOT
* Game Development

Remember, *you don't need to know all of these services* go for breadth in learning not depth as there may be **one or two** questions on other services in the exam but these will be high level

**Whats the difference between a region and an availability zone?** Currently there are **19 Regions and 57 Availability Zones in the world as of December 2018** with plans for 5 more regions and 15 more availability zones in 2019

**Availability Zone** - Like a data center, it can actually be several data centers but if they are in close proximity to one another its considered to be a single availability zone. The reason for this is if a natural disaster hits it may take out all of the data centers for the availability zone

**Region** - A geographical area consisting of **two or more avaiability zones** e.g. Sydney, Tokyo, London etc.

Some examples of the current regions in the US are:
* US East (Virginia)
* US East (Ohio)
* US West (Oregon)
* US West (North California)
* GovCloud(US-West)
* GovCloud(US-East)
* Canada

**Whats a GovCloud?** GovCloud are AWS regions that are operated by US citizens on US soil, and there is a certification process involved in getting access to these. Its not strictly limited to government use, it can be used for private use 

**Whats an Edge Location?** These are endpoints that you can use for caching e.g. CloudFont - These don't consist of full availability zones that you can put services in, they exist purely to put content geographically closer to the end user to improve latency

Some other examples of regions are:
* South America - Sao Paulo
* Europe / Middle East / Africa - Ireland
* Europe / Middle East / Africa - Frankfurt
* Europe / Middle East / Africa - London
* Europe / Middle East / Africa - Paris
* Asia Pacific - Singapore
* Asia Pacific - Tokyo
* Asia Pacific - Osaka
* Asia Pacific - Sydney
* Asia Pacific - Seoul
* Asia Pacific - Mumbai
* Asia Pacific - Bejing
* Asia Pacific - Ningxia

What we need to know in order to cover the Solutions Architect Exam is:
* Cost Control
* Compute
* Storage
* Databases
* Migration & Transfer
* Network & Content Delivery
* Management & Governance
* Analytics
* Security & Identity Compliance
* Machine Learning
* Desktop & App Streaming

Of these, the **Core** services we need to know in depth are:
* Security, Identity & Compliance
* Network & Content Delivery
* Compute
* Storage
* Databases

**EC2, Lambda, S3, RDS, DynamoDB, Redshift, Route53, VPC (Virtual private cloud) and Security Identity and Management through IAM are all the core components of passing the exam**


#### Tips

Some things to know are:
* The difference between a Region, an Availability Zone and an Edge Location

### AWS Free Tier

The email address that you sign up with will be known as the **Root Account Access**, you will need to **always** have access to this email address

When creating an account you'll be asked to select a support plan, the variations are:
* Free - 24/7 access to forums and resources, includes best practices checks and health status and notifications
* Developer Plan - $29 a Month - Same as Free but includes Email access to AWS Support during regular business hours, a **single** primary contact can open unlimited support cases, a 12 hours SLA
* Business Plan - $100 a Month - Anytime support, unlimited support cases on unlimited contacts, 1 hour SLA

If you need a Technical Account Manager for Enterprise account level support, its **$15,000** a month











