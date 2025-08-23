# SOCLab

Mini-soclab docker-ready!

## Contains

- elasticsearch
- kibana
- metricbeat
- filebeat

## How to build

### 0. Preparaions

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

```
docker compose -f ./ek.yml up -d
```

If everything is good and running - go to `http://your_docker_host_ip:5601/app/monitoring` - to verify!

-----

#### Troubleshooting

Verify stared contatiners:
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

Created thanks to this amazing guide: https://www.elastic.co/blog/getting-started-with-the-elastic-stack-and-docker-compose
