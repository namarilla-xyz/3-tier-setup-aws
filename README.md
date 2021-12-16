# 3 Tier AWS Setup


[![Capture.png](https://i.postimg.cc/xCBt8LkB/Capture.png)](https://postimg.cc/N971P9kx)

The diagram consist on the following:

It is a simple three tier AWS architecture diagram, that host a web made on Node.Js, with different tiers that you can visualize on the architecture. Also, it has a couple of notification services, a monitoring service, Git Repository with the corresponding flow of CI/CD, a S3 bucket used as backup for the DB tier and also work as a static frontend for the app, among other things. Below, I'll explain on a high level and then in detail, all the tiers.

- DB Tier: Consist on a private subnet called "3" on AZ 1, hosting 2 DB's, one for Postgres working as master, and another one no relational DB for Mongo, also working as master.
  The other private subnet, called "4" on AZ 2, host 2 DB's also for Postgres and Mongo, but these 2 are the slaves. A synchronous replication is configured between DB's to ensure high availability. These 2, are also connected to an amazon S3 bucket, with daily snapshot backup configured.
There is a security group configured with corresponding ports open (27017 for mongo and 5432 for Postgres) for an specific IP block to ensure a high security standard.

- App Tier: Consisting on 2 subnets called 1 and 2, hosted on different AZ's to ensure high availability of the service. Private Subnet 1, is hosting an AWS Fargate service that is in charge of creating the corresponding containers. Container 1, is hosting the webApp, and container 2, the serverApp. Both containers are being managed by AWS Elastic Container Service. Also, I've set up an AWS App Mesh working between both subnets, to provide application-level networking and ensure comunication across different containers. Is there also an auto-scaling policy set up between both subnets to ensure availability. The security group has port 80 open for the proper IP block to make connections secure. 

- Web Tier: Just consist on a Elastic Load Balancer, making the comunication between both subnet 1 and 2, ensuring the stability of the service. This one, is also working with Amazon Cloudfront, and on the back, using AWS WAF to ensure security from outside to inside our infra. Route 53 working as our DNS resolution tool, and then, making it happen for the users to enter to our website. 


Our Git repository, is working with different development tools from AWS as CodeBuild (CI stage), CodeDeploty (CD Stage) and CodePipeline (moving our changes to reality). 

And finally, Amazon SES and SNS working as notification services contributing with the monitoring of the infra that is being watched by Amazon CloudWatch.

NOTE: You will realize that some things were added up to the diagram but not to the .tf file because it's not the ussual on a 3 tier common setup. Hope you find this scalable and secure.

Thanks for the opportunity. This challenge was really fun! 

Take care, bye!
