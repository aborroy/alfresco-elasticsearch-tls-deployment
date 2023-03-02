# AWS Deployment for Alfresco with Search Enterprise 3 (Elasticsearch) using OpenSearch

Local Deployment of ACS Stack using an OpenSearch AWS managed service for Alfresco Search Enterprise 3

## Docker Compose

Docker Compose template includes following files:

```
.
├── .env
├── docker-compose.yml
└── keystores
    ├── certificates
    │   ├── *.eu-west-1.es.amazonaws.com.cer
    │   ├── Amazon Root CA 1.cer
    │   ├── Amazon.cer
    │   └── eu-west-1-truststore.jceks
    └── eu-west-1-truststore.jceks
```

* `.env` includes common values and service versions to be used by Docker Compose, you may need to review this values according to your environment
* `docker-compose.yml` is a regular ACS Docker Compose, including Elasticsearch Connector and the endpoint provided by OpenSearch service in AWS
* `keystores` folder includes a truststore for clients communicating with Elasticsearch (Alfresco Repository and Elasticsearch Connector)
* `keystores/certificates` folder includes `eu-west-1` Amazon certificates, so you can build keystore file by your own. If you're using `eu-west-1` zone to deploy OpenSearch in AWS, you can use default `eu-west-1-truststore.jceks`


## Creating an OpenSearch service in AWS

Before starting Docker Compose deployment, OpenSearch service must be available in AWS.

[Amazon OpenSearch Service](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/gsg.html) can be deployed using the AWS Web Console. Following settings are recommended to be used with this ACS Docker Compose for testing purposes.

| Property                           | Value                                |
|------------------------------------|--------------------------------------|
| Name                               | acs-elasticsearch                    |
| Deployment type                    | Development and testing              |
| Version                            | 1.3                                  |
| Auto-Tune                          | Disable                              |
| Availability Zones                 | 1-AZ                                 |
| Instance type                      | r6g.large.search                     |
| Number of nodes                    | 1                                    |
| Storage type                       | EBS                                  |
| EBS volume type                    | General purpose (SSD)                |
| EBS storage size per node          | 20                                   |
| Network                            | Public access                        |
| Enable fine-grained access control | True                                 |
| Create master user                 | True                                 |
| Master username                    | <OPENSEARCH_MASTER_USERNAME>                           |
| Master password                    | <OPENSEARCH_MASTER_PASSWORD>                           |
| Domain access policy               | Only use fine-grained access control |

Once the Domain is available, the hostname (for instance `search-acs-elasticsearch-qpmpsomtkszxgrrryehiwxkhfm.eu-west-1.es.amazonaws.com`) will be available using HTTPs in port 443 and you should be able to connect using a browser with Master credentials.

Add the values for your environment in Docker Compose `.env` file

```
ELASTICSEARCH_SERVER_NAME=<OPENSEARCH_SERVER_NAME>
ELASTICSEARCH_USER=<OPENSEARCH_MASTER_USERNAME>
ELASTICSEARCH_PASS=<OPENSEARCH_MASTER_PASSWORD>
```

If you are deploying AWS OpenSearch in `eu-west-1` region, no additional steps are required.


## Creating truststore file for a different Amazon region

If you are deploying AWS OpenSearch in a region different from `eu-west-1`, building the `eu-west-1-truststore.jceks` truststore file is required.

Following sample, available in `keystores\certificates` folder, builds the truststore for `eu-west-1` region.

```
$ keytool -import -file Amazon.cer -alias Amazon -keystore eu-west-1-truststore.jceks
$ keytool -import -file Amazon\ Root\ CA\ 1.cer -alias AmazonRootCA1 -keystore eu-west-1-truststore.jceks
$ keytool -import -file \*.eu-west-1.es.amazonaws.com.cer -alias eu-west-1 -keystore eu-west-1-truststore.jceks
```

>> Note that default password `password` has been used for this sample

Alternatively, to build a truststore file in PKCS12 format, following script can be used:

https://github.com/AlfrescoLabs/alfresco-opensearch-aws/blob/main/scripts/5-build-truststore.sh


## Using

```
$ docker-compose up --build --force-recreate
```

## Service URLs

http://localhost:8080/workspace

ADW
* user: admin
* password: admin

http://localhost:8080/alfresco

Alfresco Repository
* user: admin
* password: admin
