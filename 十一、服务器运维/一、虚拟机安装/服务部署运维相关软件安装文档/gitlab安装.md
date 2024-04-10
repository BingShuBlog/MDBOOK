## 一、docker 安装gitlab

```
docker run -d \
--hostname gitlab.xxx.com \
--name gitlab \
--restart always \
-p 8082:443 -p 8083:80 -p 8084:22 \
-v /etc/localtime:/etc/localtime:ro \
-v /u01/app/gitlab_data/gitlab/config:/etc/gitlab \
-v /u01/app/gitlab_data/gitlab/logs:/var/log/gitlab \
-v /u01/app/gitlab_data/gitlab/data:/var/opt/gitlab \
gitlab/gitlab-ce
```



# 二、docker安装redis

```
docker run --name redis -p 8085:6379 \
-v /u01/app/redis/data:/data \
-v /u01/app/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis:bullseye redis-server /etc/redis/redis.conf 
```



# 三、docker 安装mysql

```
docker run -p 8086:3306 --name mysql5.7 \
-v /u01/app/mysql5.7/conf:/etc/mysql/mysql.conf.d \
-v /u01/app/mysql5.7/log:/var/log/ \
-v /u01/app/mysql5.7/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=Hsj362155. \
-d mysql:5.7
```



# 四、docker 安装mongodb

```
docker run -itd --name mongo -v /u01/app/mongodb/data:/data/db -p 8087:27017 mongo --auth

docker exec -it mongo mongo admin
db.createUser({ user:'root',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'},'readWriteAnyDatabase']});

//文档
https://blog.csdn.net/w598882992/article/details/129465550
```



# 五、docker 安装mq

```
docker run -p 8088:1883 -p 8089:4369 -p 8090:5671 \
-p 8091:5672 -p 8092:8883 \
-p 8093:15672 -p 8094:25672 \
-p 8095:15675 \
-d --hostname my-rabbitmq \
--name some-rabbitmq \
-e RABBITMQ_DEFAULT_USER=admin \
-e RABBITMQ_DEFAULT_PASS=Hsj362155. \
rabbitmq

https://www.jianshu.com/p/3e06ee6d5482
```



# 六、docker 安装es

```
sudo docker run --name elasticsearch -p 8096:9200  -p 8097:9300 \
-e "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms84m -Xmx512m" \
-v /u01/app/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /u01/app/elasticsearch/data:/usr/share/elasticsearch/data \
-v /u01/app/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch

https://blog.csdn.net/qq_44732146/article/details/120744829


docker run -d --network bridge --privileged=true \
--name kibana -p 8098:5601 \
-v /u01/kibana/config/kibana.yml:/usr/share/kibana/config/kibana.yml \
-v /u01/kibana/data:/usr/share/kibana/data \
-v /u01/kibana/plugins:/usr/share/kibana/plugins \
kibana


https://blog.csdn.net/weixin_38937840/article/details/122716338





sudo docker run --network elastic --name elasticsearch -p 8096:9200 -p 8097:9300 \
-e ES_JAVA_OPTS="-Xms4g -Xmx4g" \
-v /u01/app/elasticsearch/data:/usr/share/elasticsearch/data \
-v /u01/app/elasticsearch/config:/usr/share/elasticsearch/config \
-v /u01/app/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d docker.elastic.co/elasticsearch/elasticsearch:8.7.0

注意配置文件的docker id
  
  
docker run -d --name kibana  --net elastic -p 8098:5601 \
-v /u01/app/kibana/config:/usr/share/kibana/config \
-v /u01/app/kibana/logs:/var/log/kibana \
-v /u01/app/kibana/data:/usr/share/elasticsearch/data \
docker.elastic.co/kibana/kibana:8.7.0


docker run --name kibana --net elastic -p 8098:5601 docker.elastic.co/kibana/kibana:8.7.0
./elasticsearch-create-enrollment-token -s kibana


docker run --network elastic --name elasticsearch -p 8096:9200 -p 8097:9300 \
-e ES_JAVA_OPTS="-Xms4g -Xmx4g" \
-it docker.elastic.co/elasticsearch/elasticsearch:8.7.0
```

```
 You must address the points described in the following [1] lines before starting Elasticsearch.\nbootstrap check failure [1] of [1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]", "ecs.version": "1.2.0","service.name":"ES_ECS","event.dataset":"elasticsearch.server","process.thread.name":"main","log.logger":"org.elasticsearch.bootstrap.Elasticsearch","elasticsearch.node.name":"aa439124ec73","elasticsearch.cluster.name":"docker-cluster"}
ERROR: Elasticsearch did not exit normally - check the logs at /usr/share/elasticsearch/logs/docker-cluster.log


docker run --name chatgptproxy -p 8099:8081 \
-v /u01/project/ChatGptProxy/logs:/logs \
chatgptproxy
```





```
/usr/share/elasticsearch/data
config
```



# 七、安装可视化容器

```shell
docker run -d -p 8120:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /u01/app/portainer/data:/data portainer/portainer-ce:latest

password Hsj18731362155.
```

```bash
java -jar apm-agent-attach-cli.jar \
    --exclude-user root \
    --include-main ChatGptProxyApplication app.jar \
    --include-vmarg elastic.apm.attach=true \
    --continuous \
    --config service_name=chat-gpt-service \
    --config server_url=http://172.31.189.152:8200 \
    -g debug \
    --log-file agent.log
```

https://repo1.maven.org/maven2/co/elastic/apm/apm-agent-attach-cli/1.36.0/apm-agent-attach-cli-1.36.0.jar



```
java -javaagent:./elastic-apm-agent-1.36.0.jar \
-Delastic.apm.service_name=ChatGptProxy \
-Delastic.apm.secret_token= \
-Delastic.apm.server_url=http://localhost:8200 \
-Delastic.apm.environment=测试环境 \
-Delastic.apm.log_sending=true \
-Delastic.apm.application_packages=com.chatgptproxy \
-jar app.jar
```





```
docker run --network elastic --name elasticsearch890 -p 8096:9200 -p 8097:9300 -e ES_JAVA_OPTS="-Xms4g -Xmx4g" -it docker.elastic.co/elasticsearch/elasticsearch:8.9.0

sysctl -w vm.max_map_count=262144
sysctl -p

docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password

pwd: YzPc91bAiso3Rj1QWfOC

docker cp es01:/usr/share/elasticsearch/config/certs/http_ca.crt .

curl --cacert http_ca.crt -u elastic https://localhost:8096


运行
docker run --name kibana890 --net elastic -p 8098:5601 docker.elastic.co/kibana/kibana:8.9.0

es获取token
docker exec -it es-node01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana

kibana获取验证码
/usr/share/kibana/bin/kibana-verification-code

```



启动提示信息

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Elasticsearch security features have been automatically configured!
✅ Authentication is enabled and cluster connections are encrypted.

ℹ️  Password for the elastic user (reset with `bin/elasticsearch-reset-password -u elastic`):
  ypNrJVzS_VNz-*eKkgIn

ℹ️  HTTP CA certificate SHA-256 fingerprint:
  a58487f0c21ff07b5b16ef393bf9528fd3565993800aeaca786c103abca2304b

ℹ️  Configure Kibana to use this cluster:
• Run Kibana and click the configuration link in the terminal when Kibana starts.
• Copy the following enrollment token and paste it into Kibana in your browser (valid for the next 30 minutes):
  eyJ2ZXIiOiI4LjcuMCIsImFkciI6WyIxNzIuMTkuMC4yOjkyMDAiXSwiZmdyIjoiYTU4NDg3ZjBjMjFmZjA3YjViMTZlZjM5M2JmOTUyOGZkMzU2NTk5MzgwMGFlYWNhNzg2YzEwM2FiY2EyMzA0YiIsImtleSI6Ik9YU0Q0b2tCWUZjTk9qbkJ0RDU0OnhpUkZ1ZmFfUWotVFktTklYb2Q5Q2cifQ==

ℹ️ Configure other nodes to join this cluster:
• Copy the following enrollment token and start new Elasticsearch nodes with `bin/elasticsearch --enrollment-token <token>` (valid for the next 30 minutes):
  eyJ2ZXIiOiI4LjcuMCIsImFkciI6WyIxNzIuMTkuMC4yOjkyMDAiXSwiZmdyIjoiYTU4NDg3ZjBjMjFmZjA3YjViMTZlZjM5M2JmOTUyOGZkMzU2NTk5MzgwMGFlYWNhNzg2YzEwM2FiY2EyMzA0YiIsImtleSI6Ik8zU0Q0b2tCWUZjTk9qbkJ0RDU0OnBOUzh3U09jU18yYjVQNEQ3OGUyR1EifQ==

  If you're running in Docker, copy the enrollment token and run:
  `docker run -e "ENROLLMENT_TOKEN=<token>" docker.elastic.co/elasticsearch/elasticsearch:8.7.0`
```

```
1. # 通过查看容器日志，确定是什么问题,docker logs -f -t --tail 行数 容器名
    docker logs -f -t --tail 100 <containerID> 
2. # 将容器中导致错误的文件拷贝到宿主机上
    docker cp containerID:container_path host_path 
    ***说明***
    	containerID：容器ID
    	container_path：容器内文件路径（需拷贝的源文件）
    	host_path：宿主机路径（拷贝文件的目标）
3. # 修改拷贝出来的配置文件, 再将配置文件拷贝回去容器 
    docker cp host_path containerID:container_path
4. # 再次启动容器
    docker start containerID

```

