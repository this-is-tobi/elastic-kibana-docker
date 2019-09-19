
# Docker-compose for elasticsearch and kibana

Use [docker-compose](https://docs.docker.com/compose/) to start docker containers:

 - elasticsearch
 - kibana

Note:  
* Containers are already set-up to communicate together  
* Every edit in the local data folder will automatically be reflected in the elasticsearch container (data folder)   
* Security isn't setup in containers (don't use this docker-compose for production)   
            

## Purpose

A simple data visualization introduction for elasticsearch + kibana that is up to use.


## Prereq

The stuff you need to get going:

    install [docker-machine](https://docs.docker.com/machine/) (OSX)
    install [docker-compose](https://docs.docker.com/compose/)
    install [jq](https://stedolan.github.io/jq/) (it's a commandline json processor that can modify your json data)
    git clone <this project>
        
Note: You do not need elasticsearch or kibana installed on your host machine. It's all installed in the docker containers.    


## Discover

Chose your dataset and save it in "/elastic-docker/data":

    https://opendata.paris.fr/explore/?sort=modified

-> Add header for each object in the JSON array (needed to send data to Elasticsearch with bulk method)
Open terminal, go to "/elastic-docker/data" and run:

    cat inputFileName.json | jq -c ' .[] | {"index": {"_index": "examples", "_type": "example"}}, . ' > outputFileName.json

Note: Change the inputFileName.json for the name of your input file and chose the output file name. Change "_index" name for what you are working with and do it again for "_type"

-> Start Elasticsearch and Kibana containers
open terminal and go to "/elastic-docker" and run:
  
    docker-compose up

-> Add data to the Elasticsearch cluster:
open terminal and run: 

    docker ps 

copy elasticsearch container id 

in terminal run: 

    docker exec -it elasticsearchContainerId bash
    cd data
    curl -XPUT localhost:9200/_bulk -H "Content-Type: application/json" --data-binary @outputFileName.json

Note: Change "elasticsearchContainerId" for the id you previously copyed. Change "outputFileName.json" for the file name you previously chose.


## Manage dockers with docker-compose

Using [docker-compose config file](./docker-compose.yml). List dockers, bring them up, down...

    docker-compose ps
    docker-compose up
    docker-compose down


## Access services (elasticsearch and kibana)

open web browser to server (elasticsearch):

    open http://localhost:9200
    
open web browser to client (kibana):    
    
    open http://localhost:5601