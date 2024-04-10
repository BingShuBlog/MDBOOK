# docker安装文档

## 一、删除旧Docker并安装Docker依赖的工具

```shell
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
sudo yes | yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```





## 二、配置Docker yum源(国外服务器可直接使用官方yum源；国内服务器使用阿里云yum源更快)



```shell
#国外使用docker官方yum源
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
#国内使用阿里云yum源
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```



## 三、安装和启动Docker

```shell
sudo yes | yum install -y docker-ce docker-ce-cli containerd.io
sudo systemctl enable docker
sudo systemctl start docker
docker -v
```



## 四、配置镜像加速器（国内服务器）

```shell
访问地址
https://cr.console.aliyun.com/ap-southeast-1/instances/mirrors

sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://rdssnqkq.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



## 五、修改docker网段

```shell
修改/etc/docker/daemon.json 文件
{
  "registry-mirrors": ["https://1xg83l73.mirror.aliyuncs.com"],
  "bip": "162.17.43.1/24"
}
sudo systemctl daemon-reload
sudo systemctl restart docker
```

