# kubectl

## Pods
```sh
kubectl get pods
kubectl run nginx --image=nginx # run a pod named nginx based on the nginx image
kubectl run --generator=run-pod/v1 redis --image=redis:alpine --labels="tier=db" # deprecated
kubectl get pod nginx
kubectl get pod redis --show-labels
kubectl describe pod/nginx
kubectl get pods -o wide
kubectl edit pod/nginx
kubectl delete pod/nginx
```

## Replication Controller

```sh
kubectl create -f rc-definition.yml
kubectl get replicaset
kubectl create -f replicaset-definition.yml
kubectl replace -f replicaset-definition.yml # update replicaSet
kubectl scale --replicas=6 myapp
kubectl delete replicaset myapp-replicaset

```

## Deployment
```sh
kubectl get deployments
kubectl create deployment/httpd --image=httpd
kubectl apply -f deployment-definition.yml
kubectl describe deployment/httpd
kubectl edit deployment/httpd
kubectl delete deployment/httpd
```

## Namespace
```shell
kubectl create namespace dev
kubectl get pods --namespace=dev
kubectl get pods --all-namespaces
# switch to another namespace by default
kubectl config set-context $(kubectl get current-context) --namespace=dev
```

## Context
```sh
kubectl get current-context
```

## Service

```sh
kubectl expose deployment hello-world --name=hello-world-service --port=6389 --target-port=6379 --type=ClusterIP
```

## Extras
```shell
# Dry run and output in yaml format
kubectl create deployment httpd --image=httpd --dry-run=client -o yaml >> test.yml
```

## Labels
```sh
kubectl label nodes node01 size=Large
```

## DaemonSet

```sh
kubectl get daemonsets
kubectl describe daemonsets myapp
```

## Drain and Cordon
```sh
kubectl drain <node_name> # Gracefully terminate all pods on the node (i.e. for maintenance purpose)
kubectl drain <node_name> --ignore-daemonsets # same but ignore-daemonsets
kubectl drain <node_name> --ignore-daemonsets --force # same but --force
kubectl cordon <node_name> # Mark node as unschedulable
kubectl uncordon <node_name> # Mark node as schedulable
```

# Generators
```sh
kubectl create deployment myapp --image=nginx --dry-run=client -o yaml
```
