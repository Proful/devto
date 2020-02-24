---
published: false
title: "Beginner friendly Introduction to AWS IAM"
cover_image:
description:
tags: AWS, iam, AWS Certification
series:
canonical_url:
---

## What is IAM?
IAM stands for Identity Access Management and is one of the core services of AWS. Using IAM, you can control who can access to AWS resources. One important thing to remember is that IAM service is available globally. Usually, most of the AWS services available to specific regions that you selected. On the other hand, even though you have multiple regions, you will have to manage IAM in one place. This means users, groups, roles created in IAM will be available to all-region.


![IAM Global Service](./assets/1-iam.png)

## Who is IAM user? 
When I was starting with the myriad of AWS services, to get my head around IAM concepts is the most daunting task. 

I will start with an analogy of facebook. Whenever we think about users we picture ourselves as facebook users. My initial impressions about IAM user is analogous to facebook user and it's management. But this is completely wrong. IAM is not going to help you in managing your end customers who log in to your website. 

Then what is IAM user for? You can think of yourselves, developers, operation guys and whoever going to manage or interested in your AWS servers or services. Whoever wants to start/stop servers, monitor logs, etc. Also, for yourselves you can create multiple users when you wear different hats at different times. But there is an elegant way to wear different hats in the form of IAM roles, which we will discuss that later. 

It's not mandatory to use email address as user name. You can use any sensible string as user name as long as they are unique inside a accounts.

![IAM Users](./assets/2-iam.png)

### Accessing AWS Console using user name and password
As a starting point, an IAM user using it's user name and password can get access to the AWS console and play around with it. We can associate various permission what this particular user is allowed to do. In the later section, we shall delve in more details on the same.

### Accessing AWS Resources using CLI or programmatically
We can also access AWS resources, not using AWS console but using command-line interface or programmatically using shell script or java. To facilitate the access of AWS resources from CLI, we have to generate access key id and secret access key for a particular user. And there are various ways to store the two pairs of access key in your local system.

```
➜ cat ~/.aws/credentials
[default]
aws_access_key_id = AKIAI44QH8DHBEXAMPLE
aws_secret_access_key = je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
```

Also we can per each session, we can, we can create environment variables and we can store this access key and secret. And whenever we execute the AWS CLI commands we no need to pass access key ID and secret to each for each command exit execution. So automatically those two key pairs will be used, and then authentication, authorization will happen based upon that.

```
➜ export AWS_ACCESS_KEY_ID="AKIAI44QH8DHBEXAMPLE"
➜ export AWS_SECRET_ACCESS_KEY="je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY"
```

### Is it possible to access AWS resources using SSH key attached to IAM user?

To access any virtual server usage of the SSH keys is very common. So you generate a pair of SSH key one is public and another one is private, the private key you keep within your local laptop and copy the public key and store into the remote server. Using this approach you no need to pass credentials each time you want to access. So you might be thinking, you can copy-paste your public key to a particular IAM user and you should able to access all the ec2 servers you created in AWS account. But this doesn't work in that fashion. There is no way you can copy-paste the public key to the user level, and the user will become a power user where it can access all the ec2 instances. 

There exist EC2 key pair which we can generate and associate with ec2 instance for its access. However, we cannot associate the ec2 keypairs to a specific IAM user. 

But if you talk about SSH keys concerning IAM user, there is ***one exception***. As an analogy, If you're using GitHub or Bitbucket to access code repository. You need to configure your SSH public key in their server as well. Similarly, to access AWS Code Commit from a IAM user point of view, you need to configure SSH public key to that particular user. There is no need to go to the code commit to configure SSH keys.

### Accessing AWS Resources using X.509 Certificates
This is a new mechanism to authenticate a user. It is considered to be more secure than user name and password or access key based approach. AWS IoT (Internet of Thing) uses this approach. In this case, private key is stored in the device in a more secure way and you can't copy paste into another device.

### Recommended not to use root user
Whenever you create a AWS account using your email address and your credit card details, then you ideally create a AWS root user, which is extremely powerful. And it is possible to create access key id and secret key for the same, but it is not recommended to generate and use it. It's recommended to create another user and use it accordingly. Sometime the novice or the beginner users will use their root user for doing any operation or for the learning purposes. However, it is recommended not to use your root user because it can have catastrophic consequences if credentials compromised. It is advisable to create strong password and enable multi-factor authentication(MFA). I'll discuss more about multi-factor authentication later. 


### Brief introduction to ARN
Each Amazon resource has a unique ID (alphanumeric) associated with it. This unique id is known as ARN(Amazon Resource Name). Each IAM user has its ARN. We can use this ARN to be more specific while giving permissions or in API calls.

```
arn:aws:iam::account-id:user/user-name-with-path
arn:aws:iam::123456789012:user/Steve
```

## IAM Group
Groups created to make life of administrator easier. Instead of granting policies/permission to individual level, it is more easy and efficient to manage those in group level. There is no special credentials in group level and using group you cannot access any AWS resources directly.

## Why we need IAM Role?
To access AWS resources for example, s3, or RDS or any other AWS resources, we can use access key or access secret key programmatically for making a read or write operation, depending upon the permission provided to that particular user. But this is not a recommended or secure way to access any AWS resources, because keys can be viewed in plain text who has access to the respective EC2 instance.

AWS IAM provides an elegant way to manage permission and provide a secure way of communication, in the form of IAM roles. IAM role, basically is a logical entity created where we can associate particular permission, for example, read or write access to s3, or read-only access to RDS database. We can associate the permission or policy to an IAM role. Whenever needed, an IAM user or a group or ec2 instance can assume the particular IAM role and hence it has privilege to do that particular operation. And, and it can do without storing or worry about the credential.

![IAM Role](./assets/3-iam.png)

### How to assume an IAM role?
Applications or AWS services such as EC2 can assume an IAM role by calling AWS Security Token Service (STS) AssumeRole APIs. These API will return a set of temporary credentials which is valid for a certain duration. Application can use those temporary credentials to perform any desired operation on target resources such as S3. We will discuss in details in another article.

### AWS service role for an EC2 instance
It is possible to store access keys as described earlier and application can use those credentials when needed. However, it is not very secure practice to store keys in plain text and like password we should change in timely manner, access keys also should be changed. 

We can create an IAM role with desired permission and attach to EC2 instance. Application inside the EC2 can retrieve the temporary credentials whenever it makes API calls using AWS SDK. No need to write additional code to retrieve the credentials and store it separately.