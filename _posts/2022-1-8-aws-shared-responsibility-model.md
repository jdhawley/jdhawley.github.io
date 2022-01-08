---
author: jdhawley
title: AWS Shared Responsibility Model
additional-resources:
    - "[AWS Lambda Security Overview - Whitepaper](https://docs.aws.amazon.com/whitepapers/latest/security-overview-aws-lambda/security-overview-aws-lambda.pdf)"
    - "[AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)"
---

The AWS Shared Responsibility Model (AWS SRM) is the most important security document to become familiar with when moving to the cloud. It describes the roles and responsibilities for each party in maintaining security in the cloud.

With cloud technologies becoming more and more common every day and the growing number of cybersecurity attacks, understanding the AWS SRM (or similar models for other cloud providers) is uniquely important.

The AWS SRM essentially delineates security responsibilities as either "security of the cloud" or "security in the cloud". AWS is responsible for security *of* the cloud, and the customer is responsible for security *in* the cloud.

It's a fairly simple idea, but it is applied differently depending on the AWS service being used. Let's use an example to illustrate how the responsibilities change depending on the service.

## Different Responsibilities for Different Services

Let's say Company XYZ is building a REST API and is weighing the pros and cons of using two different AWS services - AWS Lambda and AWS EC2.

With AWS Lambda, Company XYZ writes the code to process the API request and hooks it into an event trigger such as API Gateway. To use Lambda, Company XYZ will write the Lambda code and configure the connection between API Gateway and Lambda. Company XYZ will not configure the servers, instances, or containers that actually execute the Lambda function - we can say these systems are **abstracted** away from the Company XYZ.

On the other hand, EC2 is a virtual machine service. Company XYZ would be responsible for patching the operating system, managing the containers that process the API requests, and any other "plumbing" needed to service the request. Many of the implementation details that Lambda abstracted away are now very important security considerations for Company XYZ.

## AWS Security Responsibilities (Security *of* the Cloud)

By taking responsibility for ensuring security of the cloud, AWS is holding themselves accountable for securing all services they offer *to the extent that the service is abstracted away from the customer*. 

By the very nature of the cloud, this will always include the physical security of the hardware running the cloud services. Protecting the physical hardware helps keep AWS services online and secure by keeping all the bad guys outside of the data warehouse.

With Lambda, it also means that AWS is responsible for the security of any service implementations that are abstracted away from the customer. This includes the servers, instances, and containers used to execute the Lambda functions - none of which the customer controls.

When working with EC2, Company XYZ will be responsible for many of the systems that AWS would manage in Lambda. For example, managing the operating system of the virtual machine is now a responsibility of Company XYZ, and therefore, AWS is no longer responsible for keeping the operating system up to date with the latest security patches.

## Customer Security Responsibilities (Security *in* the Cloud)

Contrary to security of the cloud, security *in* the cloud includes everything AWS exposes to customers. This includes Identity and Access Management (IAM), data encryption, security of any applications that integrate with AWS services, and pretty much anything else that a customer can see or control when using an AWS service.

For Lambda specifically, security in the cloud means ensuring security of the Lambda function code, proper resource configuration, and IAM.

With EC2, Company XYZ has a little bit more work to do. They have to keep up with security patches for the operating system and any additional software they may install on the virtual machine. They are also responsible for proper configuration of the security group. And don't forget, these responsibilities are on top of all the existing responsibilities for maintaining security outside the walls of EC2 - IAM, writing secure code, and proper resource configuration all still apply.

## Conclusion

There's a lot to think about when it comes to maintainin security in cloud-based applications and systems. When using AWS services, it's absolutely crucial to properly understand the Shared Responsibility Model and how to interpret the shared responsibilities for any given service.

Security of services is just one of the many factors to take into consideration when building systems in the cloud. A small team without dedicated security professional may opt for more abstracted services to minimize their exposure to self-imposed security vulnerabilities, whereas a large company or government agency may choose less abstracted services to maintain greater control or consistency over their suite of software.

As always, the best option is to do proper research and ask a qualified professional when the situation is unclear.

Until next time.