# Docker machine startup on bash.
## Commands.
eval "$(docker-machine env default)"  
docker-compose build  
docker-compose up  

# Fluentd usecase.
## append worker logs
  * Login Fluentd   
    docker exec -it efkdocker_fluentd-worker_1 /bin/bash
  * create logs
    cd /fluentd/log  
    touch application.log
  *  append logs 
     (echo "2016-03-02 12:58:46 +0900 [ERROR] modules.filters ForkJoinPool-1-worker-5"; echo "method=GET requestId=1 uri=/polling/services/ abc") >> application.log
     (echo "2016-03-02 12:58:46 +0900 [INFO] modules.filters ForkJoinPool-1-worker-5"; echo "method=GET requestId=1 uri=/polling/services/ abc") >> application.log


# Elasticseach-Kibana
## Open kibana
  * http://192.168.99.100:5601/

# Tips.
## Docker usual command.
### watch log
docker logs -f {ID}  

## execute bash
docker exec -it {ID} /bin/bash
