# ReplicaSet

A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods.

Deployment is a higher-level concept that manages ReplicaSets. Do not use a ReplicaSet directly.

## Base template
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: frontend
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: frontend
    spec:
      containers:
        - name: nginx-container
          image: nginx
  replicas: 3
  selector:
    matchLabels: # match labels under spec.template.metadata.labels
      type: frontend
```

## Associated kubectl commands

```sh
kubectl get replicaset
kubectl get rs
kubectl describe rs myreplicaset
kubectl scale rs myreplicaset --replicas=2 # scale replicaset
```
