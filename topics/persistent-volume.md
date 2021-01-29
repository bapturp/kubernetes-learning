# Persistent Volume and Persisten Volume Claim

## Persistent Volume (PV)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-vol1
spec:
  accessModes:
    - ReadWriteOnce
  capacity:
    storage: 1Gi
  hostPath:
    path: /tmp/data
```

## Persistent Volume Claim (PVC)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv1
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

## PVC in Pod def

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
    - mountPath: /var/www
      name: mypv
  volumes:
  - name: mypv
    persistentVolumeClaim:
      claimName: pv1
```
