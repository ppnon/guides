# Docker Registry

Prepare directories to use as volumes

```bash

# create the direcotry to share the credentials
mkdir auth

# create credentials
htpasswd -Bbn registry registry > auth/htpasswd

# create the directory to share the ssl certificates
# copy .crt and .key certificates into /certs
mkdir certs

# create the directory to share the data
mkdir data

```

Create Registry container

```bash

docker run -d \
  --name registry \
  --restart=always \
  -p 5000:5000 \
  --network=local \
  -v "$(pwd)"/auth:/auth \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -v "$(pwd)"/certs:/certs \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/nginx.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/nginx.key \
  -v "$(pwd)"/data:/var/lib/registry \
  registry:2

```

Login to the new registry


```bash

docker login opendevj.com:5000

```

# Docker Registry UI

Create Registry UI container

```bash

docker run \
  -d \
  --name registry-ui \
  --restart=always \
  -p 5080:80 \
  --network=local \
  -e ENV_DOCKER_REGISTRY_HOST=regitry \
  -e ENV_DOCKER_REGISTRY_PORT=5000 \
  konradkleine/docker-registry-frontend:v2

```

# Add new registry to Docker

Edit or create the the file **/etc/docker/daemon.json**

```json

{
	"insecure-registries": ["opendevj.com:5000"]
}

```

Restart Docker service

```bash

docker tag {image:tag} local.registry:5000/{user}/{image-name}:{tag}
docker push local.registry:5000/{user}/{image-name}:{tag}

docker pull local.registry:500/{user}/{image-name}:{tag}

```

# Reference

- https://docs.docker.com/registry/deploying/