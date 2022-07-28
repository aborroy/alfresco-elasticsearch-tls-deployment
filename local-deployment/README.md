# Local Deployment for Alfresco with Search Enterprise 3 (Elasticsearch) using Basic Auth and TLS

Local Deployment using Docker Compose for ACS using Alfresco Search Enterprise 3 with Basic Auth and TLS

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
