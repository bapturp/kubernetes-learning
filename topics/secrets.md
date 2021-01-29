# Secrets

## Create secrets
### Imperative

```sh
kubectl create secret generic app-secret --from-literal=DB_USER=postgres
```

```sh
kubectl create secret generic app-secret --from-file=app_secrets.txt
```

```txt app_secrets.txt
DB_HOST=db01.example.com
DB_USER=postgres
DB_PASSWORD=secret
```

### Declarative


```sh
kubectl create -f secrets-definition.yaml
```

```yaml
apiVersion: v1
kind: Secret
metadata: app-secret
data:
  DB_HOST: ZGIwMS5leGFtcGxlLmNvbQ== # base64, use `echo -n 'secret' | base64` to get base64 hash
  DB_USER: cG9zdGdyZXM=
  DB_PASSWORD: c2VjcmV0
```

## Get secrets

```sh
kubectl get secrets
kubectl describe secret/webapp
kubectl get secret/webapp -o yaml
```

## Secrets in Pods

### Secret as a single environment variable.
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
    env:
      name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: app-secret
          key: DB_PASSWORD
```

### Secrets as environment variables.
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
    envFrom:
    - secretRef:
        name: app-secret 
```

### Secrets as volume
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
    volumes:
    - name: app-secret-volume
      secret:
        secretName: app-secret
```

Secrets are available under: `/opt/app-secret-volume/`. Each secret is a file named after the secret name with a single line representing the value.
