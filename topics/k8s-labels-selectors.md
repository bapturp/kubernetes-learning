# Labels and Selector

Used to group objects together.

pods, deployments, services are all objects from a kubernetes point of view.
One way to filter them is by grouping them by types, applications or functionality.

## Pods labels

### Labels creation
```yml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
  labels:
    app: myapp
    tier: frontend

spec:
  containers:
		- name: nginx-container # pod name
			image: nginx # image from docker hub
```

### Labels select
```sh
kubectl get pods --selector app=App1,env=dev,tier=frontend
kubectl get all -l env=dev
```

## ReplicatSet definition with matchlabel

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: simple-webapp
  labels: # Labels of the ReplicaSet
    app: App1
    function: Front-end
  annotations:
    owner: Baptiste Dromer
    email: baptiste.dromer@gmail.com
spec:
  replicas: 3
  selector:
    matchLabels: # Labels used to match (or select) the pods we want to group together in the replicaset
      app: App1
  template:
    metadata:
      labels: # Labels of the Pods
        app: App1
        function: Front-end
    spec:
      containers:
        - name: simple-webapp
          image: simple-webapp
```
`matchlabel` require only one label but many can be used.

`annotations` are used to store some value that does not need to be a label such has git-hash, version, 