registry
-p 5000:5000
restart always
volumes
	./volume:/var/lib/registry


---
version: '3'

services:
    docker-registry:
        container_name: docker-registry
        image: registry:2
        ports:
            - 5000:5000
        restart: always
        volumes:
            - ./volume:/var/lib/registry
    docker-registry-ui:
        container_name: docker-registry-ui
        image: konradkleine/docker-registry-frontend:v2
        ports:
            - 8080:80
        environment:
            ENV_DOCKER_REGISTRY_HOST: docker-registry
            ENV_DOCKER_REGISTRY_PORT: 5000


/etc/host

0.0.0.0 loca.registry

/etc/docker/daemon.json

{
	"insecure-registries": ["local.registry:5000"]
}

restart service

docker tag {image:tag} local.registry:5000/{user}/{image-name}:{tag}
docker push local.registry:5000/{user}/{image-name}:{tag}

docker pull docker-registry:500/{user}/{image-name}:{tag}

https://docs.docker.com/registry/deploying/