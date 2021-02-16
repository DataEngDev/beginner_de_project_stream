### Quick start

#### Build
```bash
git clone https://github.com/DataEngDev/beginner_de_project_stream.git
cd beginner_de_project_stream

docker-compose up -d # -d mean run in detached mode (in the background)
docker ps # display all running containers
```

#### Check (kafka)
```bash
# topic server-logs --from-beginning --max-messages 10 # used to check the first 10 messages in the server-logs  topic
docker exec -t beginner_de_project_stream_kafka_1 kafka-console-consumer.sh --bootstrap-server :9092  --topic alerts --from-beginning --max-messages 10 # used to check the first 10 messages in the alerts 
```

#### Check (Postgre)
```bash
containers_id="`docker ps -a | grep postgres:12.2 | awk '{print $1}'`"
echo $containers_id
docker exec -it  $containers_id bash

psql -h localhost -p 5432 -U startdataengineer events 

root@102eb1e502a5:/# 
root@102eb1e502a5:/# psql -h localhost -p 5432 -U startdataengineer events 

psql (12.2 (Debian 12.2-2.pgdg100+1))
Type "help" for help.

events=# 
events=# select * from server_log limit 5; -- should match the first 5 from the server-logs topic
               eventid                | userid |    eventtype    |     locationcountry      |   eventtimestamp    
--------------------------------------+--------+-----------------+--------------------------+---------------------
 7166d250-ae90-4cf2-9b79-caf404b47858 |   9607 | other           | Spain                    | 2021-02-13-04-15-52
 68c129d9-4961-42b7-8fa9-33be77a43fe0 |   6858 | update-settings | Brazil                   | 2021-02-13-04-15-54
 60f16a98-91ab-497c-a3ed-76452667e051 |   3310 | update-settings | United States of America | 2021-02-13-04-15-54
 c4ec4c88-722a-4edb-a92a-fbbb3d93f255 |   4936 | click           | Japan                    | 2021-02-13-04-15-54
 cf31af9e-0e68-4543-8d72-ef96c2e49db4 |   9265 | click           | Canada                   | 2021-02-13-04-15-54
(5 rows)

events=# select count(*) from server_log; -- 100000
 count  
--------
 100000
(1 row)

events=# 
```