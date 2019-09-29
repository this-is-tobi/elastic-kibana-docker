
# Docker-compose for elasticsearch and kibana

Use [docker-compose](https://docs.docker.com/compose/) to start docker containers:

 - elasticsearch
 - kibana

Note:  
* Containers are already set-up to communicate together   
* This project contains a data sample (movies_elastic.json) that is up to use    
* Do not upload data file that is more dans 100mb (this case need more configuration in the docker-compose file)    
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

Chose your dataset and save it in "/elastic-kibana-docker/data":

    https://opendata.paris.fr/explore/?sort=modified

Note: You can use another data source but be careful of the JSON format of your file (adding data using bulk need one object by line preceded by header).    

| Add header for each object in the JSON array (needed to send data to Elasticsearch with bulk method) |    
Open terminal, go to "/elastic-kibana-docker/data" and run:

    cat inputFileName.json | jq -c ' .[] | {"index": {"_index": "examples", "_type": "example"}}, . ' > outputFileName.json

Note: Change the inputFileName.json for the name of your input file and chose the output file name. Change "_index" name for what you are working with and do it again for "_type"


| Start Elasticsearch and Kibana containers |    
Open terminal, go to "/elastic-kibana-docker" and run:
  
    docker-compose up


| Add data to the Elasticsearch cluster |    
Open terminal and run: 

    docker ps 

Copy elasticsearch container id 

In terminal run: 

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

Open web browser to server (elasticsearch):

    open http://localhost:9200
    
Open web browser to client (kibana):    
    
    open http://localhost:5601


## Manage Kibana visualization

Check [kibana documentation](https://www.elastic.co/guide/en/kibana/6.3/index.html) to create new data graphs