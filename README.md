# Heritage Connector deployment

An easy way to deploy everything from Heritage Connector. The following services are included and all configured through environment variables:

* **fuseki** - RDF triplestore 
* **thor** - front end for performing SPARQL queries
* **thor-cors-proxy** - CORS proxy to enable thor to connect to fuseki
* **heritage-connector-vectors** - an nearest neighbours on knowledge graph embeddings
* **heritage-connector-apis** - API endpoints to wrap some common SPARQL queries and the nearest neighbours API. The API documentation is hosted at `<api-endpoint>/docs`.

The application also comes with a frontend, which can be accessed at `<host-name>:8010/view_connections`.

## Prerequisites and Environment Variables

The following URLs should be added to `.env`, as well as the rest of the variables present in `.env.sample`.

* triples file in *ntriples* format - `TRIPLES_URL`
* entity embeddings in numpy format - `VECTORS_ENTITY_EMBEDDING_URL`, `VECTORS_ENTITY_MAPPING_URL`, `VECTORS_RELATION_EMBEDDING_URL`, `VECTORS_RELATION_MAPPING_URL`

## Deployment

### Infrastructure

The contents of this repo have been tested on an EC2 `t3a.large` instance, which has 2vCPUs and 8GB RAM.

To setup the EC2 machine:

1. Create a machine, choosing *Deep Learning Base AMI (Ubuntu 18.04) Version 44.0* as your Amazon Machine Image. (This is handy because it comes installed with docker and python).
2. Choose `t3a.large` for the instance type.
3. Choose 75GB (or larger) for the storage size.
4. Click *next* until you reach the *Configure Security Group* screen.
5. Open ports 80, 8080, 8020, 3030 and 8010 for the various microservices. Details of which port is served by which microservice can be found in `docker-compose.yml`.
6. Launch the machine.


### Setup Steps

``` bash
# 1. clone the repo
git clone https://github.com/TheScienceMuseum/heritage-connector-deployment

# 2. get code for submodules
git submodule update --init

# 3. fill in env variables
cp .env.sample .env
# -- Add env variables to data files as per the prerequisities above. --
# -- The value for SPARQL_ENDPOINT_PROXIED should be                  --
# -- "http://<IP-address-of-deployed-application>:8080"               --

# 4. start containers
docker-compose up
```