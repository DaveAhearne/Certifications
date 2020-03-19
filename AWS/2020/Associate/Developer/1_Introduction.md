# Introduction

## Background 

The developer associate exam is now very different to the solutions architect qualification so while previously there was more overlap its worth studying both courses indepedently

Things this course covers:
* IAM and what it means
* EC2
* S3 
* Serverless computing
* Using other serverless services such as polly for text to speech conversion
* DynamoDB
* KMS
* Application services like SNS & SQS

General development lifecycle stuff such as:
* Continuous integration
* Continuous deployment
* Code Commit & Code Pipelines
* Advanced Identity Management

## For Previous Cloud Architect Certificate Holders

There is *some* overlap between the courses, however not as much as there used to be. It used to be that you could just do the dynamodb section of the developer course and sit both exams however **this is no longer the case**

The new developer exam **is the most difficult associate level exam today**

However if you do want to skip portions the portions to skip are:
* Basic Identity Access management - Because it covers things like how to set up users/groups which has been covered before
* Basic EC2 stuff - Because it covers basic things like setting up simple instances, connecting to them etc.
* **Parts** of the servless websites and Alexa Skills - If you've done it in the solutions architect course before, *you will need to do the parts that concern X-Ray & Step Functions*

## The Exam Blueprint

There are five different domains that the exam covers that have different weightings applied to them:
1. Deployment (22%)
2. Security (26%)
3. Development with AWS Services (30%)
4. Refactoring (10%)
5. Monitoring and Troubleshooting (12%)

The exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS_Certified_Developer_Associate-Exam_Guide_EN_1.4.pdf)

The recommended experience level for this exam is that you either:
* Have created an AWS application
* Have had experience in maintaining an AWS base application for 1+ years

Also knowledge of atleast *one* programming language is recommended because of indepth material that covers development methodologies

While the exam guide does list some white-papers that you can cover if you want to follow the material in depth in a single area its not neccessary in order to just pass the exam. But in general some areas to cover should be:
* Blue/Green deployments - Comes up in elastic beanstalk deployments

The exam content will be **multiple choice and multiple response** format (One or many correct answers) with the score being from 100-1000 with **720 being the passing grade**

There is a bigger focus on security this time round in the course now, in particular the focus on using IAM role based authentication over secret access keys in code - If you ever get an exam question around this the *IAM role based option is normally the correct answer*

Refactoring is also a bigger thing in this exam, in terms of "Optimizing applications to best use AWS services and features and migrating code to AWS" comes up fairly heavily in the *API Gateway* section **especially when using XML/SOAP currently**

Details of the exam are:
* 135 Minutes long
* 65 Questions long
* You will get your results immediately
* The passmark is 720/1000
* Its Multiple choice
* **The qualification is valid for two years**
* The questions are all scenario based