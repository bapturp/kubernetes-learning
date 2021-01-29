# Config Maps

## Create a config map - Imperative

```sh
kubectl create configmap app-config --from-literal=APP_COLOR=blue
kubectl create configmap app-config --from-file=app-config.properties
```

## Create a config map - declarative

```sh
kubectl create -f configmap.yaml
```

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: app-config
data:
    APP_COLOR: blue
    APP_MODE: prod
```

## Pod definition with ConfigMap

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
    ports:
    - containerPort: 8080
    env: # Reference to a single Key in Config Map
    - configMapKeyRef:
      name: app-config
      key: APP_COLOR
    envFrom: # Reference to a whole Config Map
    - configMapRef:
      name: app-config
    volumes: # volume
    - name: app-config-volume
      configMap:
        name: app-config

```
