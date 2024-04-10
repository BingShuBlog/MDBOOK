# linux多节点时间同步ntp

## 一、所有节点安装chrony

```shell
# 安装服务
yum -y install chrony
# 查看状态
systemctl status  chronyd
# 重启chronyd
systemctl restart chronyd
```



## 二、检查时区设置

```shell
#查看时区
timedatectl

#筛选式查看在亚洲S开的上海可用时区：
timedatectl list-timezones |  grep  -E "Asia/S.*"

# 设置当前系统为Asia/Shanghai上海时区：
timedatectl set-timezone Asia/Shanghai

# 修改日期时间（可选，主节时钟源无法同步，可以先关闭NTP同步）
timedatectl set-ntp false
timedatectl set-time "2022-04-17 15:50:20"

# 开启 NTP
timedatectl set-ntp true

#设置完时区后，强制同步下系统时钟：
chronyc -a makestep
```



## 三、防火墙设置

因NTP使用123/UDP端口协议，所以允许NTP服务即可。但是我一般都会把防火墙和selinux禁止。

```shell
# 查看防火墙状态
systemctl status firewalld.service

# 启动防火墙
systemctl start firewalld.service

# 防火墙内放行NTP服务
firewall-cmd --add-service=ntp --permanent

# reload才能生效
firewall-cmd --reload

#内部访问一般直接关闭防火墙

systemctl stop firewalld
systemctl disalbe firewalld
```



## 四、服务器配置

```shell
ntp1.aliyun.com

授时中心参考

210.72.145.44 国家授时中心
ntp.aliyun.com 阿里云
s1a.time.edu.cn 北京邮电大学
s1b.time.edu.cn 清华大学
s1c.time.edu.cn 北京大学
s1d.time.edu.cn 东南大学
s1e.time.edu.cn 清华大学
s2a.time.edu.cn 清华大学
s2b.time.edu.cn 清华大学
s2c.time.edu.cn 北京邮电大学
s2d.time.edu.cn 西南地区网络中心
s2e.time.edu.cn 西北地区网络中心
s2f.time.edu.cn 东北地区网络中心
s2g.time.edu.cn 华东南地区网络中心
s2h.time.edu.cn 四川大学网络管理中心
s2j.time.edu.cn 大连理工大学网络中心
s2k.time.edu.cn CERNET桂林主节点
s2m.time.edu.cn 北京大学
ntp.sjtu.edu.cn 202.120.2.101 上海交通大学
```



## 五、主节点编辑配置文件

```shell
vim /etc/chrony.conf 
```

```conf
# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
# server 0.centos.pool.ntp.org iburst
# server 1.centos.pool.ntp.org iburst
# server 2.centos.pool.ntp.org iburst
#server 3.centos.pool.ntp.org iburst
# 公网时间服务器
server ntp1.aliyun.com iburst

# Record the rate at which the system clock gains/losses time.
driftfile /var/lib/chrony/drift

# Allow the system clock to be stepped in the first three updates
# if its offset is larger than 1 second.
makestep 1.0 3

# Enable kernel synchronization of the real-time clock (RTC).
rtcsync

# Enable hardware timestamping on all interfaces that support it.
#hwtimestamp *

# Increase the minimum number of selectable sources required to adjust
# the system clock.
#minsources 2

# Allow NTP client access from local network.
# 指定一台主机、子网，或者网络以允许或拒绝访问本服务器
allow 172.16.24.0/24

# Serve time even if not synchronized to a time source.
# 即使server指令中时间服务器不可用，也允许将本地时间作为标准时间授时给其它客户端chronyc命令
local stratum 10

# Specify file containing keys for NTP authentication.
#keyfile /etc/chrony.keys

# Specify directory for log files.
logdir /var/log/chrony

# Select which information is logged.
#log measurements statistics tracking
```



## 六、客户端节点配置

其他**work**节点使用主节点（master01）作为时钟源，增加内网时钟源

server master01 iburst

```
vim /etc/chrony.conf 
```

```
# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
# server 0.centos.pool.ntp.org iburst
# server 1.centos.pool.ntp.org iburst
# server 2.centos.pool.ntp.org iburst
#server 3.centos.pool.ntp.org iburst
# 内网时钟服务端
server master01 iburst

# Record the rate at which the system clock gains/losses time.
driftfile /var/lib/chrony/drift

# Allow the system clock to be stepped in the first three updates
# if its offset is larger than 1 second.
makestep 1.0 3

# Enable kernel synchronization of the real-time clock (RTC).
rtcsync

# Enable hardware timestamping on all interfaces that support it.
#hwtimestamp *

# Increase the minimum number of selectable sources required to adjust
# the system clock.
#minsources 2

# Allow NTP client access from local network.
#allow 192.168.0.0/16

# Serve time even if not synchronized to a time source.
#local stratum 10

# Specify file containing keys for NTP authentication.
#keyfile /etc/chrony.keys

# Specify directory for log files.
logdir /var/log/chrony

# Select which information is logged.
#log measurements statistics tracking

```



## 七、启动chrony服务

```shell
# 设置开机启动
systemctl enable chronyd.service
# 重启服务
systemctl restart chronyd.service
# 查看时间同步状态
timedatectl status

# 手动强制同步下系统时钟
chronyc -a makestep
# 查看时间同步源
chronyc sources -v
# 查看时间同步源状态
chronyc sourcestats -v
# 硬件时间默认为UTC
timedatectl set-local-rtc 1
# 启用NTP时间同步
timedatectl set-ntp yes

```

