# Fluentd Docker Playground.

------
## Steps.

## Start docker-machine.
```bash
docker-machine start default  
eval "$(docker-machine env default)"  
docker-compose build  
docker-compose up  
```

## Behavior Fluentd's worker and aggregator.
### Append logs
  * Execute bash on fluentd worker's on docker.
```bash
    docker exec -it fluentddockerplayground_fluentd-worker_1 /bin/bash  
```  
  * Create log files.
```bash
    cd /fluentd/log      
    touch application.log  
```

  *  Try to append any logs.  
```bash
(echo "2016-03-02 12:58:46 +0900 [ERROR] modules.filters ForkJoinPool-1-worker-5"; echo "method=GET requestId=1 uri=/polling/services/ abc") >> application.log  
(echo "2016-03-02 12:58:46 +0900 [INFO] modules.filters ForkJoinPool-1-worker-5"; echo "method=GET requestId=1 uri=/polling/services/ abc") >> application.log
(echo "2016-03-02 12:58:46 +0900 [INFO] modules.filters ForkJoinPool-1-worker-5"; echo "method=GET requestId=1 uri=/polling/services/ abc") >> application.log    
```

## Behavior tracking logs.

* BashConsole(Worker STDOUT)  
  docker logs -f fluentddockerplayground_fluentd-worker_1

* BashConsole(Aggregator STDOUT)  
  docker logs -f fluentddockerplayground_fluentd-aggregator_1
  
* Elasticsearch-Kibana (ERROR-LOG Only.)
  * http://192.168.99.100:5601/


## Tips.
### usual Docker's command.
### watch docker logs.
```bash
docker logs -f {ID}  
```

## Run Bash on Docker.
```bash
docker exec -it {ID} /bin/bash
```
