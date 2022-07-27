# Sample Deployment for Alfresco with Search Enterprise 3 (Elasticsearch) using Basic Auth and TLS

From ACS 7.1 two Search Engines are available:

* [Search Services 1.x and 2.x](https://docs.alfresco.com/search-services/latest/), that is relaying on a customization of [Apache SOLR 6.6](https://solr.apache.org/guide/6_6/)
* [Search Enterprise 3.x](https://docs.alfresco.com/search-enterprise/latest/), that is a standard client for [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/index.html)

This project provides a *sample* Docker Compose template for ACS 7.2 with Alfresco Search Enterprise 3.1.0 using Basic Auth and TLS to communicate with Elasticsearch. Note that deploying the product in *production* environments would require additional configuration.

Docker Images from [quay.io](https://quay.io/organization/alfresco) are used, since this product is only available for Alfresco Enterprise customers. If you are Enterprise Customer or Partner but you are still experimenting problems to download Docker Images, contact [Alfresco Hyland Support](https://community.hyland.com) in order to get required credentials and permissions.

## Docker Compose

Docker Compose template includes following files:

```
.
├── .env
├── docker-compose.yml
├── helper
│   ├── create-certs.yml
│   └── instances.yml
└── keystores
    ├── elasticsearch
    │   ├── ca
    │   │   └── ca.crt
    │   └── elasticsearch
    │       ├── elasticsearch.crt
    │       └── elasticsearch.key
    └── truststore
        └── elasticsearch-truststore.jceks
```

* `.env` includes common values and service versions to be used by Docker Compose
* `docker-compose.yml` is a regular ACS Docker Compose, including Elasticsearch Connector and Elasticsearch server with Kibana
* `helper` folder includes a template to create Elasticsearch server certificates to configure the service with TLS (https), since it's not required to run this configuration
* `keystores/elasticsearch` folder includes the certificates generated for Elasticsearch
* `keystores/truststore` folder includes a truststore for clients communicating with Elasticsearch (Alfresco Repository and Elasticsearch Connector)

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
