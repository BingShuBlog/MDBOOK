# 一、docker 安装nginx

```
docker run \
-p 80:80 \
--name nginx \
--restart=always \
-v /u01/app/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /u01/app/nginx/conf/conf.d:/etc/nginx/conf.d \
-v /u01/app/nginx/log:/var/log/nginx \
-v /u01/app/nginx/html:/usr/share/nginx/html \
-d nginx:latest
```

