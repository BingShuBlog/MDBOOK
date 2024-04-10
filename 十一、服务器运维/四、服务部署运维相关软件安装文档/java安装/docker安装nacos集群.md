# docker安装nacos集群

```shell
docker run --name nacos01 --network net-nacos\
-e MODE=cluster \
-e NACOS_SERVERS="192.168.31.177:18848 192.168.31.177:28848" \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-v /home/nacos/application.properties:/home/nacos/conf/application.properties \
-v /root/nacos/logs/nacos01:/home/nacos/logs \

-p 8848:8848 \
-d \
-p 9848:9848 \
-p 9849:9849 \
--restart always nacos/nacos-server:latest

docker run --name nacos02 --network net-nacos\
-e MODE=cluster \
-e NACOS_SERVERS="192.168.31.177:8848 192.168.31.177:28848" \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-v /home/nacos/application.properties:/home/nacos/conf/application.properties \
-v /root/nacos/logs/nacos01:/home/nacos/logs \
-p 18848:8848 \
-d \
-p 19848:9848 \
-p 19849:9849 \
--restart always nacos/nacos-server:latest

docker run --name nacos03 --network net-nacos\
-e MODE=cluster \
-e NACOS_SERVERS="192.168.31.177:18848 192.168.31.177:8848" \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-v /home/nacos/application.properties:/home/nacos/conf/application.properties \
-v /root/nacos/logs/nacos01:/home/nacos/logs \
-p 28848:8848 \
-d \
-p 29848:9848 \
-p 29849:9849 \
--restart always nacos/nacos-server:latest
```

