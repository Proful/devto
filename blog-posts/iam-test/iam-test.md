---
published: false
title: "IAM Part I"
cover_image:
description:
tags: aws, iam
series:
canonical_url:
---

## What is IAM?
IAM stands for Identity Access Management and is one of the core services of AWS. Using IAM, you can control who can access to AWS resources. One important thing to remember is that IAM service is available globally. Usually, most of the AWS services available to specific regions that you selected. On the other hand, even though you have multiple regions, you will have to manage IAM in one place. This means users, groups, roles created in IAM will be available to all-region.


![IAM Global Service](./assets/1-iam.png)

## Who is IAM user? 
When I was starting with the myriad of AWS services, to get my head around IAM concepts is the most daunting task. 

I will start with an analogy of facebook. Whenever we think about users we picture ourselves as facebook users. My initial impressions about IAM user is analogous to facebook user and it's management. But this is completely wrong. IAM is not going to help you in managing your end customers who log in to your website. 

Then what is IAM user for? You can think of yourselves, developers, operation guys and whoever going to manage or interested in your AWS servers or services. Whoever wants to start/stop servers, monitor logs, etc. Also, for your selves you can create multiple users when you wear different hats at different times. But there is an elegant way to wear different hats in the form of IAM roles, we will discuss that later. 

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



Also we can per each sesson, we can, we can create environment variables and we can store this access key and secret. And whenever we execute the AWS CLI commands we no need to pass access key ID and secret to each for each command exit execution. So automatically those two key pair will be used, and then authentication, authorization will happen based upon that.

```
export AWS_ACCESS_KEY_ID="AKIAI44QH8DHBEXAMPLE"
export AWS_SECRET_ACCESS_KEY="je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY"
export AWS_SESSION_TOKEN="IQoJb......3J=="
```



### Is it possible to access AWS resources using SSH key attached to IAM user?

To access any virtual server usage of the SSH keys is very common. So you basically generate a pair of SSH key one is public key and another one is private key, the private key you keep within your own local laptop and copy that public key and store into the remote server. Using this approach you no need to pass credentials each time you want to access. So you might be thinking, you can copy paste your public key to a particular IAM user and you should able to access all the ec2 servers you created in aws account. But this doesn't work in that fashion. There is no way you can copy paste the public key to the user level, and the user will be become a power user where it can access all the ec2 instances. But there are different way to access ec2 instance for example you can generate a key pair. And using key pair you can able to access that, you know, a ec2 level, and also using the public private key you have to configure into each ec2 instances individually but that is a separate different topic. But if you talk about SSG keys, there is one exception. In this case, and IAM user level, you can copy your public key, but for a particular use and particular use only. If you're using GitHub or Bitbucket to access code repository. You need to configure your SSH public key in their server as well. So for example, AWS also has not very popular. But one of the code repository available which is known as code commit, which is the AWS managed service. It is basically, similar to GitHub, GitHub or Bitbucket where equally you create your Git repository and you can basically there's a git repository and you can use all your git commands. So to access code commit from a user point of view, you have to generate this SSH key and you have to store the private key in the local machine and public key you have to configure inside a particular user. So, using that SSH key, whenever you like to access a code commit repository, you can do that. So you don't need to go to the code commit to do that basically here in the root user level. You can set up this SSH key and it will take care of the rest. 

### Reocommended not to use Root user
IAM root user is one of the very powerful aws user, whenever you were created a AWS account using using your email address and your credit card details, then you ideally created a IAM root user, which is extremely powerful. And you can, it is possible to create a, you know, access key ID and secret key ID for the same, but it is not recommended to generate, because you can always create a another user. And you can generate those credential for the same. So also, some sometime the novice or the beginner users will use their root user only for doing any operation or the for the learning purposes. But for any, any use case it is recommended not to use your root account user because it can have catastrophic consequences. So, better, you should create a very hard to use the very difficult email password ensuring proper place. And also it is recommended to do the MFA multi factor authentication. I'll discuss more about multi factor authentication later. But users to ensure that those do lock it properly and create a different user for all another accesses. 


### Brief introduction to ARN
Each Amazon resource have a unique ID (alpha numeric) associated with it. This unique id is known as ARN(Amazon Resource Name).Each IAM user has its ARN. 

## Why we need IAM Role?
To access AWS resources for example, s3, or RDS or any other AWS resources. We can use access key or access secret key associated with a particular user programmatically to making a read or write operation, depending upon the permission provided to that particular user. But this is not a recommended or secure practice to access any resources using the keys provided and AWS IAM provides a elegant way to manage the permission and provide a secure way of communication, by means of IAM role. So IAM role, basically is a logical entity created where we can associated a particular permission for read or write access to s3, or to read only access to a RDS database. So whichever the permission we needed. We can associate it to a IAM role. And this IAM role we no need to directly attach to a particular user or group or a ec2 instance. So whenever needed a IAM user or a group or ec2 can assume the particular IAM role with it has permission or which is it has privileged to to do that particular operation, for example, a ec2 instance would like to read from an s3 bucket one file, then it can assume a particular role, which allow this ec2 instance to do that operation. And, and it can do without, without storing or worry about the credential, because aws platform by means of where IAM role it eliminates all the temporary credential for us, and which is quite, quite. We don't need to worry about the credential management, so it won't be it is a very secret communication, we can achieve.

![IAM Role](./assets/3-iam.png)