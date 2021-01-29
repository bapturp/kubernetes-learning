# Service

## ClusterIP service

ClusterIP is aervice object that will expose the pods inside of a cluster.
A DNS object will be automatically created.

```sh
kubectl create deployment alpaca-prod \
  --image=gcr.io/kuar-demo/kuard-amd64:blue \
  --replicas=2 \
  --port=8080
kubectl expose deployment alpaca-prod
```

```sh
kubectl get services
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: alpaca-prod

spec:
  type: ClusterIP
  ports:
    - port: 8080
      protocol: TCP
      targetPort: 8080
  selector:
    app: alpaca-prod
```


## NodePort service

```yaml
apiVersion: v1
kind: Service
metatdata:
  name: myapp-service

spec:
  type: NodePort
  ports:
    - targetPort: 80 # pod, if not provided it will be assumed it's the same as 'port'
      port: 80 # service
      nodePort: 30008 # node, if not provided it will be given a random port on in a predefined range (usually >30000)
  selector:
    app: myapp
```


