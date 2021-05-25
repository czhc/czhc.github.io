---
title: "Accessing Kibana dashboards for Amazon Elasticsearch with VPC Support"
description: "Quickstart to deploy Kibana access to Elasticsearch in private subnets via SSH proxy"
categories: [guides]
tags: [aws, analytics, elk, elasticsearch, kibana]
last_modified_at: "2020-03-17"
published: true
---

## 1. Creating the Cognito User Pool and Identity Pool


### Cognito User Pool Walkthrough

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

### Cognito Identity Pool Walkthough

1. Identity pool name: Movies_IdentityPool
2. Unauthenticated identities: skip
3. Authentication flow settings: skip
4. Authentication providers:
	1. Cognito: provide the User Pool Id and App Client Id you have from your existing User Pool or the one you created in [previous step](#Cognito_User_Pool_Walkthrough)
5. Create Pool

6. You're not done yet! In the next screen *Identify the IAM roles to use with your new identity pool*, click on View Detais:
	1. Select Create a new IAM Role for authenticated idenities with Role Name: *Movies_IdentityPoolAuth_Role*
	1. Select Create a new IAM Role for UNauthenticated identities with Role Name: *Movies_IdentityPoolAuth_Role*


## 2. Creating the AES domain

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
    	3. Security group: create or use an existing security group for your ES cluster. This security group will be used to allow ingress from your proxy only. Otherwise, create a new security group for your ES cluster. This security group should allow the following Inbound Rules.
    		1. Type: All TCP, Source: *the security group itself* to allow node-to-node ingress.

    	4. IAM Role: Select `AWSServiceRoleForAmazonElasticsearchService`. This is a service-linked role used by the AWS service to manage your ES clusters.

    2. Fine-grained access control:
    	1. :ballot_box_with_check: Enable fine-grained access control. FGAC allows you to use features such as document-level security, field-level security and read-only Kibana users to further restrict access in your Kibana deployment.
    	2. Select **Set IAM ARN as master user**. With this you will be using an IAM entity as a master, as opposed to using HTTP basic auth (single user+password).
    	3. Set the IAM ARN to the Cognito AuthRole you have created earlier e.g. `arn:aws:iam::XXMY_AWS_ACCOUNT_IDXX:role/Cognito_ESIdentityPoolAuth_Role`

    3. SAML authentication for Kibana: skip this as we are using Amazon Cognito instead.

    4. Amazon Cognito Authentication for Kibana
    	1. :ballot_box_with_check: Enable Amazon Cognito authentication
    	2. Region: US East (N. Virginia) or the region you are doing this in.
    	3. Cognito User Pool: Select the User Pool you have created in the last step
    	4. Cognito Identity Pool: Select the Identity Pool you have created in the last step

    5. Access Policy
    	1. Access policies control is used to accept or reject signed requests to the Elasticsearch API. This will be used to identify the http Get and Puts sent to ES. For more information on how to sign your http requests with sigv4, refer to [this](https://aws.amazon.com/blogs/database/get-started-with-amazon-elasticsearch-service-an-easy-way-to-send-aws-sigv4-signed-requests/)
    	2. To keep this simple, we will allow any IAM user in our account to use ES APIs.
    	3. Select *Allow open access to the domain*


    5. Require HTTPS for all traffic, enable node-to-node encryption. In this example as we are only using 1 node, this option may not be customizable.


You have now created your Elasticsearch domain. Wait for the domain status to turn to **ACTIVE** before proceeding.


## 1.1. (Optional) Loading data into your ES domain

// todo

From the AWS console, launch Cloudshell and wait for the environment to become available.

1. Create a new local file called `bulk_movies.json`. Paste the following content into the file.

	```
	{ "index" : { "_index": "movies", "_id" : "2" } }
	{"director": "Frankenheimer, John", "genre": ["Drama", "Mystery", "Thriller", "Crime"], "year": 1962, "actor": ["Lansbury, Angela", "Sinatra, Frank", "Leigh, Janet", "Harvey, Laurence", "Silva, Henry", "Frees, Paul", "Gregory, James", "Bissell, Whit", "McGiver, John", "Parrish, Leslie", "Edwards, James", "Flowers, Bess", "Dhiegh, Khigh", "Payne, Julie", "Kleeb, Helen", "Gray, Joe", "Nalder, Reggie", "Stevens, Bert", "Masters, Michael", "Lowell, Tom"], "title": "The Manchurian Candidate"}
	{ "index" : { "_index": "movies", "_id" : "3" } }
	{"director": "Baird, Stuart", "genre": ["Action", "Crime", "Thriller"], "year": 1998, "actor": ["Downey Jr., Robert", "Jones, Tommy Lee", "Snipes, Wesley", "Pantoliano, Joe", "Jacob, Ir\u00e8ne", "Nelligan, Kate", "Roebuck, Daniel", "Malahide, Patrick", "Richardson, LaTanya", "Wood, Tom", "Kosik, Thomas", "Stellate, Nick", "Minkoff, Robert", "Brown, Spitfire", "Foster, Reese", "Spielbauer, Bruce", "Mukherji, Kevin", "Cray, Ed", "Fordham, David", "Jett, Charlie"], "title": "U.S. Marshals"}
	{ "index" : { "_index": "movies", "_id" : "4" } }
	{"director": "Ray, Nicholas", "genre": ["Drama", "Romance"], "year": 1955, "actor": ["Hopper, Dennis", "Wood, Natalie", "Dean, James", "Mineo, Sal", "Backus, Jim", "Platt, Edward", "Ray, Nicholas", "Hopper, William", "Allen, Corey", "Birch, Paul", "Hudson, Rochelle", "Doran, Ann", "Hicks, Chuck", "Leigh, Nelson", "Williams, Robert", "Wessel, Dick", "Bryar, Paul", "Sessions, Almira", "McMahon, David", "Peters Jr., House"], "title": "Rebel Without a Cause"}

	```

1. Run `sudo pip3 install awscurl`. `awscurl` is a python package that wraps around curl to authenticate via AWS Signature V4 Signing Process.
2. Run the following command to download a sample data of movies into your ES index. Replace $ES with your domain VPC endpoint

	```shell
	awscurl --service es -XPOST $ES/_bulk -H 'Content-Type: application/json' -d "@bulk_movies.json"
	```
3.



## 3. Deploying a Proxy

Whne your domain is ACTIVE, you will be provided with the **VPC endpoint** and **Kibana** URL in the Overview.

Notice that when you click on the Kibana URL, you will be taken to a blank page which times out. This is because the ES domain is not publicly accessible from your local network and browser.

To allow public access into a private resource, we will need a proxy or a jump host that is deployed in the public subnet. We will do this using a simple EC2 instance.


1. Create new EC2 instances from the EC2 console.
2. Use the following configurations:
	1. AMI: Amazon Linux 2
	2. Family: t2.micro (free-tier eligible)
	3. Instance details:
		1. Network: select your main VPC again
		2. Subnet: select a **Public** subnet here.
	4. Configure Security Group: You can use an existing bastion/jump box group here or create a new one with the following details:
		1. Type: SSH, Source: My IP
		2. Type: HTTPS, Source: My IP
		3. Type: Custom TCP Rule, Port Range: **8157**, Source: My IP
		4. Name this security group as `es-kibana-proxy-secgroup`
	5. Launch the instance. You may be asked to use an existing key pair, or create a new SSH key pair. If you are using a new key-pair, you can use a custom name such as `es-kibana-proxy-secgroup` and download the key pair.
	6. When downloading the key pair, take note to modify the permissions using `chmod 0400 _my_key_pair.cer`

3. Once your EC2 instance (proxy) is running, take note of the public IPv4 DNS and test SSH-ing into your instance.

	```sh
		ssh -i /path/to/key-pair ec2-user@your-proxy-public-DNS
	```


4. If you are able to SSH into your EC2 instance, test
//todo


## 4. Setting up an SSH tunnel

1. In your local machine, open an SSH tunnel with the following command. You will notice that the command is suspended if the tunnel is open.

	```
	ssh -i /path/to/key-pair.cer ec2-user@your-proxy-public-DNS -ND 8157`
	```

2. Download and install the following browser extension: [FoxyProxy](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp)

3. Click on the browser icon, and select *Use proxies based on their pre-defined patterns and priorities*

4. Click on Options, and make the following configurations:
	1. Add New Proxy
	2. In General, Proxy Name: Kibana Proxy
	3. Proxy Details:
		1. Manual Proxy configuration
		2. Host or IP adddress: `localhost`, Port: `8157`
		3. :ballot_box_with_check` SOCKS proxy?, select *SOCKS v5*
	4. URL Patterns:
		1. Add new pattern with Pattern Name: `Movies Kibana endpoint`, and paste in your ES domain VPC endpoint value here.
		2. Save!


With the SSH tunnel still in-place, you should now be able to access your Kibana URL. Because Kibana is authenticated with Cognito, you will first be redirected to your Cognito User Pool sign-in page.

You can now return to your Cognito console and create new users to access your Kibana dashboard.

And you're done!



