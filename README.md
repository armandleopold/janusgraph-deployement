
# janusGraphEnv
This is a dockerized environment For [JanusGraph + ElasticSearch + Cassandra + GraphExp] 
What you will get : 

* Elasticsearch : 7.10.1  [http://localhost:9200](http://localhost:9200)
* Kibana : 7.10.1    [http://localhost:5601](http://localhost:5601)
* JanusGraph : 0.5
* Cassandra : latest
* portainer : latest  [http://localhost:9000](http://localhost:9000) 
* graphexp : latest   [http://localhost:8183](http://localhost:8183)
* landing page :   [http://localhost:8081](http://localhost:8081)

## Getting Started : 
To run the stack you need to set the correct permissions to edit the data folders.
### Prerequisites : 
- You need to have **Docker Engine** installed version 18.0 or higher. [https://docs.docker.com/install/](https://docs.docker.com/install/)
- You need to have **Docker compose** installed too. [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)
- For the elasticsearch container : 
run : `sysctl -w vm.max_map_count=262144` (to know more about it : [Virtual memory Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html#vm-max-map-count))

### Installation :
1. To build & run the stack : 
`docker-compose up`
2. Go to : [http://localhost:9000](http://localhost:9000) & setup for a **local docker** engine.
3. You are good to go !

If you want to have information to your elasticsearch database content, please download [elastic-head plugin](https://chrome.google.com/webstore/detail/elasticsearch-head/ffmkiejjmecolpfloofpjologoblkegm) and connect to http://localhost:9200

### Portal :
To have a bookmark of the environment + useful links : 
Go to : [http://localhost:8081](http://localhost:8081) 

## Use the environment :
### Connect remotely to your JanusGraph container & run commands:
As you can see there is an apache-tinkerpop client in the project folder, it has the right version to connect to janusgraph and therefore can be used as a remote gremlin-client for the gremlin-server embedded in JanusGraph.

1. Run a terminal & execute the container run command :
```
docker run --rm -e GREMLIN_REMOTE_HOSTS=janusgraph -it --network=janusgraph-deployement_janusGraphEnvNetwork janusgraph/janusgraph:latest ./bin/gremlin.sh
```
 Be aware that the **network** may be different. use `docker network ls` to find the right docker network to connect to.
2. You need to connect to the gremlin-server on the janusgraph container with this command : 
```
:remote connect tinkerpop.server conf/remote.yaml
```
3. You need to specify that you want to execute **remote** commands : 
```
:remote console
```
4. Now you can run commands with your console in remote to the janusgraph container !
5. Exemples : 
```
gremlin> g.addV('person').property('name', 'chris')
==>v[4160]
gremlin> g.V().values('name')
==>chris
```

### To visualize your Work :

GraphExp is a simple, opensource lighweight solution for visualizing janusgraph content in a browser Using D3.js graphic library.
just go to : [http://localhost:8183](http://localhost:8183) to access graphExp.

If you want to visualize datas by default, you can load the janusGraph god graph by connecting remotely to the gremlin-server of JanusGraph : 

See the documentation here : 
[https://docs.janusgraph.org/getting-started/basic-usage/](https://docs.janusgraph.org/getting-started/basic-usage/)

### To stop properly the stack :

To stop correctly the stack and ensure any kind of data loss do this : 

1. Verify that there are no Janusgraph transaction still pending or processing.
2. Go to the root directory of the project (janusGraphEnv/) and do : `docker-compose down`
