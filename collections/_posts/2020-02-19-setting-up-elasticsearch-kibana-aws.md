---
title: "Setting up Elasticsearch and Kibana on AWS"
description: "Quickstart on rich and simple data analytics using the EXK stack"
categories: [guides]
tags: [aws, analytics, elk, elasticsearch, kibana]
last_modified_at: "2019-02-19"
published: true
---

## Creating the AES Domain

1. Choose Deployment type:
    1. Deployment type: `Development and testing`
    2. Version: `7.1`

2. Configure domain
    1. Elasticsearch domain name: `movies`
    2. Instance type: `t2.small.elasticsearch`
    3. Number of nodes: `3`
    4. Data nodes storage type: `EBS`
    5. EBS Volume type: `General Purpose`
    6. EBS storage size per node: `10` GiB
    7. Dedicated master nodes: <skip>
    8. Optional ES cluster settings > Allow APIs that can span multiple indices and bypass index-specific access policies > :ballot_box_with_check:

3. Configure access and security

    1. Public access
    2. Fine-grained access control: skip
    3. Enable Cognito Authentication:
        1. Create Cognito User Pool.
        2. In User Pool, add App integration > Domain name.
        3. Create Cognito Identity Pool. Check User Pool App integration for App client ID.
        4. :ballot_box_with_check: _Enable access to unauthenticated identities_ in Identity Pool

    4. JSON defined access policy:

        ```json
        {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Effect": "Allow",
              "Principal": {
                "AWS": "*"
              },
              "Action": "es:*",
              "Resource": "arn:aws:es:us-east-1:<ACCOUNT_ID>:domain/<DOMAIN_ID>/*",
              "Condition": {
                "IpAddress": {
                  "aws:SourceIp": "<YOUR_IP>/24"
                }
              }
            }
          ]
        }
        ```

    5. Require HTTPS for all traffic, enable node-to-node encryption. (t2.small does not support encryption at rest)

## Loading and Reading Data into Domain

1. From working directory, run

    ```shell
        awscurl --service es -XPOST $ES/_bulk -H 'Content-Type: application/json' -d "@bulk_movies.json"
    ```

    `awscurl` is a python package that wraps around curl to authenticate via AWS Signature V4 Signing Process. Install it via `pip`.

    Output:

    ```shell
        {"took":19053,"errors":false,"items":[{"index":{"_index":"movies","_type":"_doc","_id":"2","_version":1,"result":"created","_shards":{"total":2,"successful":2,"failed":0},"_seq_no":0,"_primary_term":1,"status":201}},{"index":
        ...
    ```
2. Query inserted data:

    ```sh
        awscurl --service es -XGET "$ES/movies/_search?q=Thriller"
    ```

    Output:

    ```sh
        "took":82,"timed_out":false,"_shards":{"total":5,"successful":5,"skipped":0,"failed":0},"hits":{"total":{"value":2,"relation":"eq"},"max_score":0.41501677,"hits":[{"_index":"movies","_type":"_doc","_id":"2","_score":0.41501677,"_source":{"director": "Frankenheimer, John", "genre": ["Drama", "Mystery", "Thriller", "Crime"], "year": 1962, "actor": ["Lansbury, Angela", "Sinatra, Frank", "Leigh, Janet", "Harvey, Laurence", "Silva, Henry", "Frees, Paul", "Gregory, James", "Bissell, Whit",...
    ```

## Viewing Data on Kibana

1. Create `admin` user in User Pool with a password
2. Check that Identity Pool is created
2. Add the following into domain Access Policy:

    ```json
        {
          "Effect": "Allow",
          "Principal": {
            "AWS": [
              "arn:aws:iam::<ACCOUNT_ID>:role/Cognito_<ES DOMAIN>Auth_Role"
            ]
          },
          "Action": [
            "es:ESHttp*"
          ],
          "Resource": "arn:aws:es:<REGION>:<ACCOUNT_ID>:domain/<DOMAIN-NAME>/*"
        }
    ```

3. Open Kibana endpoint and Log in.

![](/assets/img/Screenshot%202020-02-18%20at%204.49.12%20AM.png)

4. Create index pattern as `movies`.
5. Create visualization. Try a vertical bar graph and set X-Axis to `Aggregation:Terms`, `Field:genre.keyword`.

![](/assets/img/Screenshot%202020-02-18%20at%204.48.46%20AM.png)


This demo does not cover concepts on cluster sizing, dedicated master and replication, deploying AES in a VPC with fine-grained access controls.


/shrug

![](/assets/img/demo-dashboard.png)



\[edit\] This guide deployes an ES cluster that is publicly accessible and access control is managed via IPv4 address whitelisting. If you are looking to use ES clusters deployed in a private network (in a VPC), feel free to check out this updated guide: [HERE](/collections/_posts/2021/03/17/setting-up-kibana-for-amazon-elasticsearch-with-vpc-support)




