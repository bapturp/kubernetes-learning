# Volumes

## Basic volume mounted on host

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
  labels:
    app: myapp
    tier: frontend
spec:
  containers:
  - name: nginx-container
    image: nginx
    volumeMounts:
    - mountPath: /data
      name: data-volume
  volumes:
  - name: data-volume
    hostPath:
      path: /data
      type: Directory
```

## Volume mounted on external provider

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
  labels:
    app: myapp
    tier: frontend
spec:
  containers:
  - name: nginx-container
    image: nginx
    volumeMounts:
    - mountPath: /data
      name: data-volume
  volumes:
  - name: data-volume
    awsElasticBlockStore:
      volumeID: jhu2skn23
      fsType: ext4
```
