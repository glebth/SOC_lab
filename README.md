# SOCLab

Mini-soclab docker-ready!

## Structure

- elasticsearch
- kibana
- metricbeat
- filebeat

```
                      +---------+
                      | Kibana  |
                      +---------+
                           ^
                           |
                           v
                       +--------+
                       |  es01  |
                       |Elastic |
                       +--------+
                         ^  ^  ^
      -------------------+  |  +-------------------
      |                     |                      |
      v                     v                      v
+-------------+      +-------------+       +---------------+
| filebeat01  |      | logstash01  |       | metricbeat01   |
|-------------|      |-------------|       |----------------|
| - custom    |      | - custom    |       | collecting     |
|   file      |      |   file      |       | metrics:       |
|   ingest    |      |   ingest    |       | - logstash     |
| - harvesting|      |             |       | - elasticsearch|
|   logs from |      |             |       | - kibana       |
|   containers|      |             |       |                |
+-------------+      +-------------+       +----------------+
```

## How to build

### 0. Preparations

Docker install:

```
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" |
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Check
docker --version
docker compose version
```

Give more memory as [recommended](https://www.elastic.co/docs/deploy-manage/deploy/self-managed/vm-max-map-count):
```
sudo nano /etc/sysctl.conf
```

Add in the end of the file:
```
vm.max_map_count=262144
```

### 1. Git it

```
git clone https://github.com/glebth/SOC_lab.git
cd ./SOC_lab
```

### 2. Compose and use

> Update `.env_sample` file with your actual data and credentials.
> Change its name: `mv ./.env_sample ./.env`

Docker Compose

> To not use sudo with docker - add yourself to the docker group: `sudo usermod -aG docker $USER && newgrp docker`

```
docker compose -f ./ek.yml up -d
```

If everything is good and running - go to `http://your_docker_host_ip:5601/app/monitoring` - to verify!

-----

#### Troubleshooting

Verify started contatiners:
```
docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Names}}\t{{.Status}}"
```

Check out container logs and errors, e.g:
```
docker logs soc-lab-elasticsearch-1
```

Disable all containers:
```
docker compose -f docker-compose.core.yml down
```

-----

### Tested on

ELK Stack version 9.1.2

----

Thanks to this amazing guide: https://www.elastic.co/blog/getting-started-with-the-elastic-stack-and-docker-compose ([github](https://github.com/elkninja/elastic-stack-docker-part-one/tree/main)) 
