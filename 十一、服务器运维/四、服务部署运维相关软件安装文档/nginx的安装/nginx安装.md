```
GET https://api.cloudflare.com/client/v4/zones/0d0b5bbb00b680cded1754e263fb8c45/ssl/certificate_packs
```

#### 1.1.1. 安装Nginx

（1）创建Nginx用户

```shell
useradd  nginx  
passwd  nginx  
```

（2）安装Nginx依赖库

```shell
sudo yes |  yum install gcc-c++  
sudo yum  install -y pcre pcre-devel  
sudo yum  install -y zlib zlib-devel  
yum  install -y openssl openssl-devel 
yum -y install make 
```

（3）下载&安装Nginx

```shell
cd  /u01/package  
wget  http://nginx.org/download/nginx-1.8.1.tar.gz  
tar -zxvf  nginx-1.8.1.tar.gz  
mv  nginx-1.8.1 /u01/package/nginx-1.8.1  
./configure  --prefix=/usr/src/nginx --with-http_stub_status_module --with-http_ssl_module  
sudo make  && sudo make install  
```

（4）修改启动权限

```shell
cd  /usr/src/nginx/sbin  
chown root  nginx  
chmod u+s  nginx  
```

（5）Nginx启动、停止命令

```shell
cd  /usr/src/nginx/sbin  
./nginx  #启动  
./nginx -s  reload  #重启  
./nginx -s  stop   #停止  
```

 