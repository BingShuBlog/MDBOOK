# Docker容器命令总结

## 一、docker容器基本操作

##### 1.查看版本。

```shell
docker version
```

![微信截图_20181110175747](D:\github\MDBOOK\微服务\dockerImage\微信截图_20181110175747.png)





2.查看ubuntu内核版本。

```shell
uname -a
```



3.检查Device Mapper

```shell
ls -l /sys/class/misc/device-mapper
```



4.Docker ubuntu的安装。

```shell
#安装命令
apt-get install docker.io 
#更新配置命令
source /etc/bash_completion.d/docker.io
```



5.Docker维护的安装方法。

```shell
#安装 curl
apt-get install -y curl
#安装docker
curl -sSL https://get.docker.com/ubuntu/ |sudo sh
```



6.使用非root用户:

```shell
#添加docker用户组
groupadd docker
#将当前用户添加到用户组中
gpasswd -a ${user} docker
#重启docke服务
service docker restart
```



7.启动容器

```shell
docker run IMAGE [COMMAND] [ARG...]
```



8.启动交互式容器：

```shell
# -i --interactive=true |fasle 默认是false 为容器始终打开标准输入
# -t --tty=true |false  默认是false 为创建的容器分配伪tty终端
docker run -i -t IMAGE /bin/bash
```

 

 9.退出容器

```shell
#退出的时候交互式容器也停止。
exit
```



10.查看容器

```shell
# -a列出所有容器
# -l列出最新创建容器
docker ps [-a] [-l]
```



11.查看容器配置信息。

```shell
docker inspect [id] [name]
```



12.如何使用自定义容器的名字。

```shell
docker run --name=自定义名字 -i -t IMAGE /bin/bash
```



13.重新启动停止容器。

```shell
#-i 使用交互的方式
docker start [-i] 容器名
```



14.删除停止的容器。

```shell
docker rm 容器名
```



15.以守护的方式运行容器。

```shell
#方法一
docker run -i -t IAGE /bin/bash
Ctrl+P 和Ctrl+Q

#方法二
docker run -d 镜像名 [COMMEND] [ARG...]
```



16.附加到运行中的容器。

```shell
docker attach 容器名
```



17.查看容器日志。

```shell
# -f --follows=true |fales 默认false
# -t --timestamps=true|fales 默认fales
# --tail ="all"
docker logs [-f] [-t] [--tail="次数"] 容器名
```



18.查看容器内的进程。

```shell
docker top 容器名
```



19.在运行中的容器内启动新的进程。

```shell
docker exex [-d] [-i] [-t] 容器名 [COMMAND] [ARG...]
```



20.停止守护时的容器。

```shell
#等待容器停止
docker stop 容器名
#直接kill容器
docker kill 容器名
```



21.部署静态网站。

```shell
# 1.创建映射80端口的交互式容器
  docker run -p 80 --name web -i -t ubuntu /bin/bash
# 2.安装Nginx
  apt-get install -y nginx
# 3.安装vim
  apt-get install -y vim 
# 4.创建静态页面
  mkdir /var/www/html
  vim index.html
# 5.修改Nginx配置文件
 whereis nginx 
 vim  /etc/nginx/sites-enabled/default
# 6.运行Nginx
根目录
nginx
# 7验证网站并且访问
curl http://ip:端口
通过 docker inspect web 可以查看容器端口
重启容器事ip端口重新分配
docker exec web nginx启动容器内进程
```



22.设置容器端口映射。

```shell
#-P暴露容器所有接口进行映射
# —P,--publish-all=true |false 默认false
# 例如 docker run -P -i -t ubuntu /bin/bash

#-p, --publish=[]
#例如 docker run -p 80 -i -t ubuntu /bin/bash

#有四种方式
# 1.只指定容器端口
#  containerPort
#  docker run -p 80 -i -t ubuntu /bin/bash

# 2.hostProt:containerPort 宿主机端口和容器端口
# docker run -p 8080:80 -i -t ubuntu /bin/bash

# 3.ip::containerPort
#docker run -p 0.0.0.0:80 -i -t ubuntu /bin/bash

# 4.ip:hostProt:containerPort
#docker run -p 0.0.0.0:8080:80 -i -t ubuntu /bin/bash
run [-P] [-p]
```





## 二、docker镜像操作

1.查看docker信息

```shell
docker info
```



2.镜像列表

```shell
# -a,--all=fales 默认false 显示所有镜像
# -f，--filter=[]    
# --no-trunc =false  不截断镜像id
# -q, --quiet=false  只显示镜像id
#显示列表
# 1.REGISTRY包含了REPOSITORY包含了镜像
# 2.TAG标签名
# 3.IMAGE ID
# 4.CREATED
# 5.SIZE
docker images [OPTIONs] [REPOSITORY]
```



3.查看镜像详细信息

```shell
# -f, --format=""
docker inspect [OPTION] CONTAINER|IMAGE [CONTAINER|IMAGE...]
```



4.删除镜像

```sh
# -f,--force=false 强制删除
# --no-prune=false
docker rmi [OPTION] IMAGE[IMAGE...]

#删除所有镜像
docker rmi $(docker images -q ubuntu)
```



5.查找进行。

```shell
# --automated=false 自动化选项
# --no-trunc=false  
# -s,stars=0  限定结果星级
docker search [OPTIONS]TERM
```



6.拉去镜像

```shell
# -a,--all-tags=false
docker pull [OPTION] NAME[:TAG]
```



7.指定拉去镜像的位置。

```shell
#执行这个脚本
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io

或者修改重新启动
vim /etc/default/docker
```



8.推送镜像。

```shell
docker push 镜像
```



9.构建镜像。

```shell
#第一种 通过容器构建
# -a,--author="" 作者
# -m, --message="" 提交信息
# -p    指定不暂停容器
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

#第二种
# --force-rm=false
# --no-cache=false
# --pull=false
# -q,quite=false
# --rm=true
# -t,--tag=""
docker build 通过DockerFile构建
```



10.DockerFile创建

```dockerfile

```



   