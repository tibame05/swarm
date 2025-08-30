# swarm 啟動 container

## 刪除所有 container

    docker rm -f $(docker ps -a -q)

# GCE docker swarm 部署

## GCE ubuntu 安裝 docker
	sudo apt-get update
	sudo apt-get install docker.io -y

## 登入 docker
	docker login -u {docker hub 帳號}

## 將帳號加入 docker group
	sudo usermod -aG docker $USER

## init

    docker swarm init

## 初始化後，會出現一段訊息，讓 Node 加入 Swarm 集群

    docker swarm join --token SWMTKN-xxxx xxx.xxx.xxx.xx:2377

## 忘記 docker swarm join 以上指令怎麼辦?

	docker swarm join-token worker

## 建立 MySQL volume:

	docker volume create mysql

- `docker volume create {volume 名稱}`

## create-network:

	docker network create --scope=swarm --driver=overlay --attachable etf_lib_network

- `docker network create --scope=swarm --driver=overlay --attachable {network 名稱}`

#### 參數解釋
- `--scope=swarm` : 限定該網路僅在 Docker Swarm 模式中使用
- `--driver=overlay` : 使用 overlay driver，支援「多主機」上的容器間通訊
- `--attachable` : 建立一個 overlay 網路時，允許非 Swarm 管理的容器（例如 docker run 指令或 DockerOperator 啟動的臨時容器） 主動加入這個網路

## 部屬 Portainer 服務

	docker pull portainer/portainer-ce:2.0.1
	docker pull portainer/agent
	docker stack deploy -c portainer.yml por

## 打開 Portainer 服務

http://127.0.0.1:9000



## deploy MySQL:
	docker stack deploy --with-registry-auth -c mysql.yml mysql

- `docker stack deploy --with-registry-auth -c mysql.yml {stack 名稱}`

## deploy rabbitmq:
	docker stack deploy --with-registry-auth -c rabbitmq.yml rabbitmq

- `docker stack deploy --with-registry-auth -c rabbitmq.yml {stack 名稱}`

## deploy crawler worker:
	DOCKER_IMAGE=winston07291/web_crawler_etf:0.0.7 docker stack deploy --with-registry-auth -c worker.yml crawler

- `DOCKER_IMAGE={docker hub 名稱}/{image 名稱}:{版本號} docker stack deploy --with-registry-auth -c worker.yml {stack 名稱}`

## deploy crawler producer:
	DOCKER_IMAGE=winston07291/web_crawler_etf:0.0.7 docker stack deploy --with-registry-auth -c producer.yml crawler

- `DOCKER_IMAGE={docker hub 名稱}/{image 名稱}:{版本號} docker stack deploy --with-registry-auth -c producer.yml {stack 名稱}`

## deploy API:
	DOCKER_IMAGE=winston07291/fast_api:0.0.5 docker stack deploy --with-registry-auth -c api.yml api

- `DOCKER_IMAGE={docker hub 名稱}/{image 名稱}:{版本號} docker stack deploy --with-registry-auth -c api.yml {stack 名稱}`

## deploy airflow
	DOCKER_IMAGE_VERSION=0.0.4 docker stack deploy --with-registry-auth -c airflow.yml airflow

- `DOCKER_IMAGE_VERSION={版本號} docker stack deploy --with-registry-auth -c airflow.yml {stack 名稱}`

## deploy redash
	docker stack deploy --with-registry-auth -c redash.yml redash

- `docker stack deploy --with-registry-auth -c redash.yml {stack 名稱}`

## 刪除 stack
	docker stack rm {stack 名稱}

## 離開 docker swarm
	docker swarm leave --force
