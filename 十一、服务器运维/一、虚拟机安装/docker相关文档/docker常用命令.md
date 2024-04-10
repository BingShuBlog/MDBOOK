## 一、docker 基础命令

```shell
启动docker
systemctl start docker
关闭docker
systemctl stop docker
重启docker
systemctl restart docker
docker设置随服务启动而自启动
systemctl enable docker
查看docker 运行状态
systemctl status docker
查看docker 版本号信息
docker version
docker info
docker 帮助命令
docker --help
docker pull --help
```



## 二、docker 镜像命令

```shell
查看自己服务器中docker 镜像列表
docker images
搜索镜像
docker search 镜像名
docker search --filter=STARS=9000 mysql 搜索 STARS >9000的 mysql 镜像
拉取镜像 不加tag(版本号) 即拉取docker仓库中 该镜像的最新版本latest 加:tag 则是拉取指定版本
docker pull 镜像名 
docker pull 镜像名:tag
docker run 镜像名
docker run 镜像名:Tag
#删除一个
docker rmi -f 镜像名/镜像ID
#删除多个 其镜像ID或镜像用用空格隔开即可 
docker rmi -f 镜像名/镜像ID 镜像名/镜像ID 镜像名/镜像ID
#删除全部镜像  -a 意思为显示全部, -q 意思为只显示ID
docker rmi -f $(docker images -aq)
#强制删除镜像
docker image rm 镜像名称/镜像ID
保存镜像
docker save 镜像名/镜像ID -o 镜像保存在哪个位置与名字
加载镜像
docker load -i 镜像保存文件位置
```



## 三、docker 容器命令

```shell
#查看正在运行容器列表
docker ps -a

#运行一个容器
# -it 表示 与容器进行交互式启动 -d 表示可后台运行容器 （守护式运行）  --name 给要运行的容器 起的名字  /bin/bash  交互路径
docker run -it -d --name 要取的别名 镜像名:Tag /bin/bash
#开机自启
使用docker run命令时 添加参数--restart=always 便表示
docker run -itd --name redis002 -p 8888:6379 --restart=always  redis:5.0.5 /bin/bash
#1. 拉取redis 镜像
docker pull redis:5.0.5
#2.命令启动
docker run -it -d --name redis001 redis:5.0.5 /bin/bash
#3.查看已运行容器
docker ps
# netstat是控制台命令,是一个监控TCP/IP网络的非常有用的工具，它可以显示路由表、实际的网络连接以及每一个网络接口设备的状态信息
netstat -untlp
#删除一个容器
docker rm -f 容器名/容器ID
#删除多个容器 空格隔开要删除的容器名或容器ID
docker rm -f 容器名/容器ID 容器名/容器ID 容器名/容器ID
#删除全部容器
docker rm -f $(docker ps -aq)
#容器端口与服务器端口映射 -p 宿主机端口:容器端口
docker run -itd --name redis002 -p 8888:6379 redis:5.0.5 /bin/bash
#进入容器方式
docker exec -it 容器名/容器ID /bin/bash
#进入 前面的 redis001容器   
docker exec -it redis001 /bin/bash
#进入容器方式二 —推荐使用 exec 方式
docker attach 容器名/容器ID
#从容器内 退出到自己服务器中 需注意 两个退出命令的区别
#-----直接退出  未添加 -d(持久化运行容器) 时 执行此参数 容器会被关闭  
exit
# 优雅提出 --- 无论是否添加-d 参数 执行此命令容器都不会被关闭
Ctrl + p + q
#停止容器
docker stop 容器ID/容器名
#重启容器
docker restart 容器ID/容器名
#启动容器
docker start 容器ID/容器名
#kill 容器
docker kill 容器ID/容器名
#容器文件拷贝 —无论容器是否开启 都可以进行拷贝
#docker cp 容器ID/名称:文件路径  要拷贝到外部的路径   |     要拷贝到外部的路径  容器ID/名称:文件路径
#从容器内 拷出
docker cp 容器ID/名称: 容器内路径  容器外路径
#从外部 拷贝文件到容器内
docker  cp 容器外路径 容器ID/名称: 容器内路径
#查看容器日志
docker logs -f --tail=要查看末尾多少行 默认all 容器ID
#数据文件挂载
-v 宿主机文件存储位置:容器内文件位置
如此操作，就将 容器内指定文件挂载到了宿主机对应位置，-v命令可以多次使用，即一个容器可以同时挂载多个文件
-v 宿主机文件存储位置:容器内文件位置 -v 宿主机文件存储位置:容器内文件位置 -v 宿主机文件存储位置:容器内文件位置
# 运行一个docker redis 容器 进行 端口映射 两个数据卷挂载 设置开机自启动
docker run -d -p 6379:6379 --name redis505 --restart=always  -v /var/lib/redis/data/:/data -v /var/lib/redis/conf/:/usr/local/etc/redis/redis.conf  redis:5.0.5 --requirepass "password"
#不想删容器，又想让这个容器设置开机自启动，那么我们修改其启动配置即可！
docker  update --restart=always 容器Id 或者 容器名
或
docker container update --restart=always 容器Id 或者 容器名
#更换容器名
docker rename 容器ID/容器名 新容器名
```



## 四、docker 运维命令

```shell
#查看docker工作目录
sudo docker info | grep "Docker Root Dir"
#查看docker磁盘占用总体情况
du -hs /var/lib/docker/
#查看Docker的磁盘使用具体情况
docker system df
#删除无用的容器和 镜像
#删除异常停止的容器
docker rm `docker ps -a | grep Exited | awk '{print $1}'` 
#删除名称或标签为none的镜像
docker rmi -f  `docker images | grep '<none>' | awk '{print $3}'`
#清除所有无容器使用的镜像
#注意，此命令只要是镜像无容器使用（容器正常运行）都会被删除，包括容器临时停止
docker system prune -a
#查找大文件
find / -type f -size +100M -print0 | xargs -0 du -h | sort -nr
#查找指定docker使用目录下大于指定大小文件
find / -type f -size +100M -print0 | xargs -0 du -h | sort -nr |grep '/var/lib/docker/overlay2/*'
```

