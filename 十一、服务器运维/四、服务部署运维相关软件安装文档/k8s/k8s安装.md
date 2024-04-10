# K8s安装

## 一、基本配置，修改host

```shell
vim /etc/hosts

10.128.0.9   hushaojun-k8s-1
10.128.0.10  hushaojun-k8s-2
10.128.0.11  hushaojun-k8s-3
10.128.0.12  hushaojun-k8s-4
10.128.0.13  hushaojun-k8s-5
```



## 二、关闭防火墙

```shell
systemctl disable --now firewalld
systemctl disable --now dnsmasq
systemctl disable --now NetworkManager  #centos8无需关闭
setenforce 0

vim /etc/sysconfig/selinux
修改SELINUX=disabled

swapoff -a && sysctl -w vm.swappiness=0
swapoff -a
yum -y install ipvsadm ipset sysstat conntrack libseccomp

cat <<EOF >  /etc/sysctl.d/k8s.conf  
net.ipv4.tcp_keepalive_time=600
net.ipv4.tcp_keepalive_intvl=30
net.ipv4.tcp_keepalive_probes=10
net.ipv6.conf.all.disable_ipv6=1
net.ipv6.conf.default.disable_ipv6=1
net.ipv6.conf.lo.disable_ipv6=1
net.ipv4.neigh.default.gc_stale_time=120
net.ipv4.conf.all.rp_filter=0 
net.ipv4.conf.default.rp_filter=0
net.ipv4.conf.default.arp_announce=2
net.ipv4.conf.lo.arp_announce=2
net.ipv4.conf.all.arp_announce=2
net.ipv4.ip_local_port_range= 45001 65000
net.ipv4.ip_forward=1
net.ipv4.tcp_max_tw_buckets=6000
net.ipv4.tcp_syncookies=1
net.ipv4.tcp_synack_retries=2
net.bridge.bridge-nf-call-ip6tables=1
net.bridge.bridge-nf-call-iptables=1
net.netfilter.nf_conntrack_max=2310720
net.ipv6.neigh.default.gc_thresh1=8192
net.ipv6.neigh.default.gc_thresh2=32768
net.ipv6.neigh.default.gc_thresh3=65536
net.core.netdev_max_backlog=16384
net.core.rmem_max = 16777216 
net.core.wmem_max = 16777216
net.ipv4.tcp_max_syn_backlog = 8096 
net.core.somaxconn = 32768 
fs.inotify.max_user_instances=8192 
fs.inotify.max_user_watches=524288 
fs.file-max=52706963
fs.nr_open=52706963
kernel.pid_max = 4194303
net.bridge.bridge-nf-call-arptables=1
vm.swappiness=0 
vm.overcommit_memory=1 
vm.panic_on_oom=0 
vm.max_map_count = 262144
EOF


modprobe overlay
modprobe br_netfilter

cat > /etc/modules-load.d/containerd.conf << EOF
overlay
br_netfilter
ip_conntrack
EOF

设置为开机启动
systemctl enable --now systemd-modules-load.service







```



## 三、安装文档

```
yum update -y && yum -y install  wget psmisc vim net-tools nfs-utils telnet yum-utils device-mapper-persistent-data lvm2 git tar curl
```

```
rm -rf /etc/yum.repos.d/*
cat > /etc/yum.repos.d/123.repo  << EOF 
[cby]
name=CentOS-$releasever - Media
baseurl=file:///u01/package/k8s/
gpgcheck=0
enabled=1
EOF
yum install /u01/package/k8s/* --skip-broken -y
```

```
# apt install -y sshpass
yum install -y sshpass
ssh-keygen -f /root/.ssh/id_rsa -P ''
export IP="10.128.0.9 10.128.0.10 10.128.0.11 10.128.0.12 10.128.0.13"
export SSHPASS=Hsj362155.
for HOST in $IP;do
     sshpass -e ssh-copy-id -o StrictHostKeyChecking=no $HOST
done
```

```
wget  https://github.com/Mirantis/cri-dockerd/releases/download/v0.2.5/cri-dockerd-0.2.5.amd64.tgz
```

```
Master='hushaojun-k8s-5 hushaojun-k8s-4'
Work='hushaojun-k8s-1 hushaojun-k8s-2 hushaojun-k8s-3'

for NODE in $Master; do echo $NODE; scp /usr/local/bin/kube{let,ctl,-apiserver,-controller-manager,-scheduler,-proxy} $NODE:/usr/local/bin/; scp /usr/local/bin/etcd* $NODE:/usr/local/bin/; done

for NODE in $Work; do     scp /usr/local/bin/kube{let,-proxy} $NODE:/usr/local/bin/ ; done

mkdir -p /opt/cni/bin
```