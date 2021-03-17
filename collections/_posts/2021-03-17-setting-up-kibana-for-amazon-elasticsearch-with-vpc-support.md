---
title: "Accessing Kibana dashboards for Amazon Elasticsearch with VPC Support"
description: "Quickstart to deploy Kibana access to Elasticsearch in private subnets via SSH proxy"
categories: [guides] 
tags: [big-data, analytics, elk, elasticsearch, kibana]
last_modified_at: "2020-03-17"
published: true
---

## 1. Creating the AES domain

1. From the AES dashboard, click on *Create a new domain*
1. Choose Deployment type: 
    1. Deployment type: `Development and testing`
    2. Version: `7.9 (latest)`
2. Configure domain
    1. Elasticsearch domain name: `movies`
    2. Instance type: `t3.small.elasticsearch`
    3. Number of nodes: `1`
    4. Data nodes storage type: `EBS`
    5. EBS Volume type: `General Purpose`
    6. EBS storage size per node: `10` GiB
    7. Dedicated master nodes: <skip>
    8. Optional ES cluster settings > Allow APIs that can span multiple indices and bypass index-specific access policies > :ballot_box_with_check:
      
3. Configure access and security 

    1. **VPC access (Recommended)**
    	1. VPC: Select your target VPC which has both public and private subnets. For more information on standard 3-tier VPC architectures, see [here](https://aws.amazon.com/quickstart/architecture/vpc/)
    	2. Subnet: Select a choice of _private_ subnet. This is where your ES node/cluster will be placed in. 
    	3. Security group: create or use an existing security group for your ES cluster. This security group will be used to allow ingress from your proxy only. //todo 
    	4. IAM Role: Select `AWSServiceRoleForAmazonElasticsearchService`. This is a service-linked role used by the AWS service to manage your ES clusters. 

    2. Fine-grained access control: 
    	1. **Check** Enable fine-grained access control. FGAC allows you to use features such as document-level security, field-level security and read-only Kibana users to further restrict access in your Kibana deployment. 
    	2. Select **Set IAM ARN as master user**. With this you will be using an IAM entity as a master, as opposed to using HTTP basic auth (single user+password). 
    	3. Set the IAM ARN to the Cognito AuthRole you have created earlier e.g. `arn:aws:iam::XXMY_AWS_ACCOUNT_IDXX:role/Cognito_ESIdentityPoolAuth_Role`

    3. SAML authentication for Kibana: skip this as we are using Amazon Cognito instead. 

    4. Amazon Cognito Authentication for Kibana
    	1. **Check** Enable Amazon Cognito authentication
    	2. Region: US East (N. Virginia) or the region you are doing this in. 
    	3. Cognito User Pool: Here you can use your own user pool or [Create a New User Pool](#Cognito_User_Pool_Walkthrough)  
    	4. Cognito Identity Pool: Here you can use your own identity pool or [Create a New Identity Pool](#Cognito_Identity_Pool_Walkthrough)

    5. Access Policy
    	1. Access policies control is used to accept or reject signed requests to the Elasticsearch API. This will be used to identify the http Get and Puts sent to ES. For more information on how to sign your http requests with sigv4, refer to [this](https://aws.amazon.com/blogs/database/get-started-with-amazon-elasticsearch-service-an-easy-way-to-send-aws-sigv4-signed-requests/)
    	2. To keep this simple, we will allow any IAM user in our account to use ES APIs. 
    	3. Select *Allow open access to the domain*
        

    5. Require HTTPS for all traffic, enable node-to-node encryption. In this example as we are only using 1 node, this option may not be customizable. 


You have now created your Elasticsearch domain. Wait for the domain status to turn to *ACTIVE* before proceeding


## 2. 


## Cognito User Pool Walkthrough


1. Pool-name: Movies-UserPool
2. Review defaults
3. Create Pool
4. After the User Pool is created, we need to create App clients for integration. 
	1. In the sidebar, go to  App integration
	2. Domain: Add domain
	3. Amazon Cognito domain: use a custom domain name e.g. `kibana-demo-movies` or import your domain via ACM. All cognito domains have to be unique and cannot contain `cognito`. Save your changes.
5. In General Settings, go to App client settings
	1. Add an app client
	2. App client name: Kibana-Movies
	3. Create App Client
	4. Show details. Take note of the *App client Id* 


## Cognito Identity Pool Walkthough

1. Identity pool name: Movies_IdentityPool
2. Unauthenticated identities: skip
3. Authentication flow settings: skip
4. Authentication providers: 
	1. Cognito: provide the User Pool Id and App Client Id you have from your existing User Pool or the one you created in [previous step](#Cognito_User_Pool_Walkthrough)
5. Create Pool

