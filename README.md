# Kovtun

Установка docker compose на linuxКоманды:

git clone https://github.com/skl256/grafana_stack_for_docker.git && \

cd grafana_stack_for_docker && \

sudo mkdir -p /mnt/common_volume/swarm/grafana/config && \

sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data} && \

sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana} && \

touch /mnt/common_volume/grafana/grafana-config/grafana.ini && \

cp config/* /mnt/common_volume/swarm/grafana/config/ && \

mv grafana.yaml docker-compose.yaml && \

docker compose up -d не скачиваем

sudo yum install curl

COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d" -f4)

sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose

sudo chmod +x /usr/bin/docker-compose

docker-compose --version

sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce

sudo systemctl enable docker --now

docker compose up -d

sudo vi docker-compose.yaml Нажимаем insert

пишем:

version: '3.9'

services:

prometheus:image: prom/prometheus:latest

volumes:- ./prometheus:/etc/prometheus/

container_name: prometheushostname: prometheus

command:- --config.file=/etc/prometheus/prometheus.yml

ports:- 9090:9090

restart: unless-stoppedenvironment:

TZ: "Europe/Moscow"networks:

- default

node-exporter:image: prom/node-exporter

volumes:- /proc:/host/proc:ro

- /sys:/host/sys:ro- /:/rootfs:ro
  
container_name: exporterhostname: exporter

command:- --path.procfs=/host/proc

- --path.sysfs=/host/sys- --collector.filesystem.ignored-mount-points
  
- ^/(sys|proc|dev|host|etc|rootfs/var/lib/docker/containers|rootfs/var/lib/docker/overlay2|rootfs/run/docker/netns|rootfs/var/lib/docker/aufs)($$|/)
  
ports:- 9100:9100

restart: unless-stoppedenvironment:

TZ: "Europe/Moscow"networks:

- default
- 
networks:default:

ipam:driver: default

config:- subnet: 172.28.0.0/16

sudo vi prometheus.yaml

node-exporter:image: prom/node-exporter

volumes:- /proc:/host/proc:ro

- /sys:/host/sys:ro- /:/rootfs:ro
  
container_name: exporterhostname: exporter

command:- --path.procfs=/host/proc

- --path.sysfs=/host/sys- --collector.filesystem.ignored-mount-points
  
- ^/(sys|proc|dev|host|etc|rootfs/var/lib/docker/containers|rootfs/var/lib/docker/overlay2|rootfs/run/docker/netns|rootfs/var/lib/docker/aufs)($$|/)
  
ports:- 9100:9100

restart: unless-stoppedenvironment:

TZ: "Europe/Moscow"networks:

- default
  
Обязательно что бы файл назывался .yaml
      config:
      
        - subnet: 172.28.0.0/16

