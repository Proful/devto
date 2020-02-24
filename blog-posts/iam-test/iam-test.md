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
IAM stands for Identity Access Management. IAM is one of the core services of AWS and this is available Globally. Usually, most of the AWS services available to specific regions that you selected. On the other hand, even though you have multiple regions, you will have to manage IAM in one place. This means users, groups, roles created in IAM will be available to all-region.


![IAM Global Service](./assets/1-iam.png)

## Who is IAM user? 
When I was starting out with myriad of AWS services, to get my head around IAM concepts is the most daunting task. 

I will start with an analogy of facebook. Whenever we think about users we picture ourselves as facebook users. My initial impressions about IAM user is analogous to facebook user and it's management. But this is completely wrong. IAM is not going to help you in managing your end customers who log in to your website. 

Then what is IAM user for? You can think of yourselves, developers, operation guys and whoever going to manage or interested in your AWS servers or services. Whoever wants to start/stop servers, monitor logs, etc. Also, for your selves also you can create multiple users when you wear different hats at different times. But there is an elegant way to wear different hats in the form of IAM roles, we will discuss that later. 

Usually, to start with an IAM user using it's user name and password get access to the AWS console and play around with it. 


![IAM Users](./assets/2-iam.png)

Each Amazon resource have a unique ID (alpha numeric) associated with it. This unique id is known as ARN(Amazon Resource Name).Each IAM user has its ARN. 
![IAM Groups](./assets/3-iam.png)