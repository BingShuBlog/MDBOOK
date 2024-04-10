# Prometheus学习文档

## 一、安装

docker-compose.yaml 方法安装

```
version: '3.3'

volumes:
  prometheus_data: {}
  grafana_data: {}

networks:
  monitoring:
    driver: bridge

services:
  prometheus:
    image: prom/prometheus
    container_name: prometheus2
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/usr/share/prometheus/console_libraries'
      - '--web.console.templates=/usr/share/prometheus/consoles'
      #热加载配置
      - '--web.enable-lifecycle'
      #api配置
      #- '--web.enable-admin-api'
      #历史数据最大保留时间，默认15天
      - '--storage.tsdb.retention.time=30d'  
    networks:
      - monitoring
    links:
      - alertmanager
      - cadvisor
      - node_exporter
    expose:
      - '30001'
    ports:
      - 30001:9090
    depends_on:
      - cadvisor

  alertmanager:
    image: prom/alertmanager
    container_name: alertmanager
    command:
      - '--config.file=/etc/alertmanager/config.yml'
      - '--storage.path=/alertmanager'
    networks:
      - monitoring
    expose:
      - '30002'
    ports:
      - 30002:9093

  cadvisor:
    image: google/cadvisor
    container_name: cadvisor
    restart: always
    networks:
      - monitoring
    expose:
      - '30003'

  node_exporter:
    image: prom/node-exporter
    container_name: node-exporter
    command: 
      - '--path.procfs=/host/proc' 
      - '--path.sysfs=/host/sys'
      - '--collector.filesystem.ignored-mount-points=^/(sys|proc|dev|host|etc|rootfs/var/lib/docker)($$|/)'
    networks:
      - monitoring
    ports:
      - '30004:9100'

  grafana:
    image: grafana/grafana
    container_name: grafana
    env_file:
      - ./grafana/config.monitoring
    networks:
      - monitoring
    links:
      - prometheus
    ports:
      - 30005:3000
    depends_on:
      - prometheus
```

