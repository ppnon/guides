# Installation:

https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux
https://kubernetes.io/docs/tasks/tools/install-minikube/

# Use

## Start Minikube

```bash

# check the client version
kubectl version --client

# start cluster
minikube start --vm-driver=virtualbox
minikube start --memory=16384 --cpus=4 --vm-driver=virtualbox

# check cluaster status
minikube status

# stop cluster
minikube stop

# delete all
minikube delete

```

## View all

To use a gui use the dashboard:

```bash

minikube dashboard

```
From terminal:

```bash

# list all
kubectl get all
kubectl get nodes
kubectl get deployments
kubectl get pods
kubectl get pods --all-namespaces
kubectl get events

# get cluster ip
minikube ip

# view cluster logs
minikube logs

# view cluster configuration
kubectl config view
kubectl config view --output='yaml'

# view pod log
kubectl logs ${pod_name}

```

## Deployments

```bash

kubectl create deployment ${name} --image=${image}:${tag}

kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node

# get status
kubectl get deploy
kubectl get pod
kubectl get pod -w

# delete deployment
kubectl delete deployment.apps/hello-node

```

## Services

Command line definition

```bash

kubectl expose deployment ${deployment_name} --type=${type: NodePort | LoadBalancer | ClusterIP } --port=${puerto} --target-port=${target_port}

kubectl expose deployment hello-node --type=LoadBalancer --port=8080

```

Yaml definition, hello-service.yml

```yml

apiVersion: v1
kind: Service
metadata:
  name: hello-node
spec:
  type: NodePort
  selector:
    app: hello-node
  ports:
    - protocol: TCP
      port: 80
```

```bash

kubectl apply -f hello-service.yml

```

Check service

```bash

# get service
kubectl get svc

# call service
curl "$(minikube ip):${target_port}"

# open in browser
minikube service hello-node

# to add and external ip address for LoadBalanced, creating a route
minikube tunnel
curl ${external_ip}:${port}

# delete services
kubectl delete svc hello-node
kubectl delete svc -l run=source-ip-app

```

# Reference

- https://kubernetes.io/docs/tutorials/hello-minikube/#create-a-deployment
- https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/