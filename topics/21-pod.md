# Pod

## Create a Pod imperative way

```
kubectl run nginx --image nginx
```

## Declarative way

### Base template

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
```

### Specified image repository template
```yaml
# Pull nginx image with 
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  containers:
  - name: nginx
    image: my-private-registry.io/apps/nginx
  imagePullSecrets:
  - name: my-private-registry
```

### Resource management

Resource *requests* specify the minimum amount of a resource required to run the application.
Resource *limits* specify the maximum amount of resource that an application can consume.

Resource *requests* and *limits* are defined per container (spec.containers.[].resources.)

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
      resources:
        requests:
          memory: 1Gi # 1G != 1Gi, allowed G,M,K,None, Gi,Mi,Ki,None,
          cpu: 1
        limits:
          memory: 2Gi
          cpu: 2
```

### Volumes

Volumes can be defined in `spec.volumes` array or under containers in `volumeMounts` array.

- `spec.volumes` all containers of the pod can mount the volume.
- A single volume defined with `volumeMounts` can be defined for multiple con.

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
  volumes:
  - name: kuard-data
    hostPath: # mounted on host
      path: "var/lib/kuard"
```

### Accessing a pod

#### Port forward

Port forward is a simple way to access a pod even if it's not serving traffic on the internet.
```sh
kubectl port-forward kuard 8080:8080
```

#### Logs

```sh
kubectl logs kuard
```

#### Running commands

```sh
kubectl exec kuard -- date
kubectl exec kuard -it -- bash # interactive session
```

#### Copying files to and from containers

```sh
kubectl cp <pod-name>:/tmp/foo.txt ~/foo.txt
kubectl cp ~/bar.txt <pod-name>:/tmp/bar.txt 
```

### Health checks

#### Liveness Probe

Liveness Probe check if the application is running properly. Containers that fail liveness are restarted.

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
        name: http
        protocol: TCP
      livenessProbe:
        httpGet:
          path: /healthy
          port: 8080
        initialDelaySeconds: 5
        timeoutSeconds: 1
        periodSeconds: 10
        failureThreshold: 3
```

#### Readiness Probe

Readiness Probe describes when a container is ready to serve user request. Container that fail readiness are removed from service load balancers. 

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
        name: http
        protocol: TCP
      readynessProbe:
        httpGet:
          path: /ready
          port: 8080
        periodSeconds: 2
        initialDelaySeconds: 0
        successThreshold: 1
        failureThreshold: 3
```