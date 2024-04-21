# 一、脚本部署jar

```shell
#!/bin/sh
port=9999 #定义变量等号左右不能有空格
jar_name=blockchain.jar
log_path=./log/`date +%y%m%d`blockchain.log

#运行脚本提示信息
tips(){
    echo "-------------------------------------"
    echo ""
    echo "项目端口: ${port}"
    echo "项目jar目录： ${jar_name}"
    echo ""
    echo "你可以使用如下参数进行操作"
    echo "-status -查看当前项目运行状态"
    echo "-start  -启动当前项目"
    echo "-stop  -停止当前项目"
    echo "-restart -重启当前项目"
    echo ""
    echo "-------------------------------------"
}

#查看状态
status(){
    #查询端口的PID {print $7}-取出打印的第七个值
    pid=`netstat -apn |grep $port |awk '{print $7}' |cut -d/ -f 1`
    #判断端口是否被占用
    if [ -z "${pid}" ];then
        echo "没有项目在运行"
    else
        echo "项目正在运行中"
    fi
}

#启动项目
start(){

  # 如果存在命令行第一个参数，替换默认 profile
  if [ -n "$1" ]; then
    my_profile="$1"
  fi

  echo "  >>> begining lauch project $jar_name ....."
  # 搜索已经启动的 Spring Boot 服务进程 ID
  pid=$(jps -l|grep $jar_name|cut -d ' ' -f1)
  echo "  >>> source project process id : $pid"

  if [ $pid ]
   then
     echo "  >>> kill source project ......"
     # 杀掉 已存在服务进程
     kill -9  $pid
     sleep 3
     echo "  >>> restart project ......"
   else
    echo "  >>> start project......"
  fi

  # 新启动并后台运行 Spring Boot 服务
   nohup nohup java -jar blockchain.jar > $log_path 2>&1 &

  # 循环等待日志文件生成，同时通过动态转动字符表示等待
  spin='-|/'
  i=0
  until [ -f $log_path  ]
  do
  i=$(( (i+1) %4 ))
  printf "\r[${spin:$i:1}]"
  sleep 0.1
  done
  printf "\n\n >>> tail -f "$log_path"\n\n"

  # 显示日志内容
  tail -5f $log_path


#    pid=`netstat -apn |grep $port |awk '{print $7}' |cut -d/ -f 1`
#    stop()
#    if [ -z "${pid}" ];then
#        echo "正在启动......"
#        nohup nohup java -jar webase-sign.jar > $log_path 2>&1 &
#    else
#        echo "项目运行中或端口已被占用"
#    fi

}

#停止项目
stop(){
    pid=`netstat -apn |grep $port |awk '{print $7}' |cut -d/ -f 1`
    if [ -z "${pid}" ];then
        echo "没有项目在运行，请先启动"
    else
        kill -9 $pid
        echo "已杀死端口为 ${port} 的应用"
    fi
}

#重启项目
restart(){
    pid=`netstat -apn |grep $port |awk '{print $7}' |cut -d/ -f 1`
    echo "正在杀死端口 ${port} 的pid ${pid} 中..."
    if [ -z "${pid}" ];then
        echo "项目未启动"
    else
        kill -9 $pid
    fi
    sleep 5 #睡眠五秒
    start  #调用启动方法
    echo "项目重启成功！"
}

#参数选项
case "$1" in
    "-status")
     status
     ;;
    "-start")
     start
     ;;
    "-stop")
     stop
     ;;
    "-restart")
     restart
     ;;
    *)
     tips
     ;;
esac
```



```shell
sh ./launch.sh -start
sh ./launch.sh -stop
```

