# Scheduler

Ensure pods are assigned to a node. Basically the scheduler will inspect every pod object and check weather or not it is assigned to a node.

A node in `PENDING` state means it's waiting to be assigned to a node. If a node remains in `PENDING` there's a good chance that the scheduler isn't working properly.

To manually assign a pod to a node we can specified the variable `nodeName`, it can done only during the creation time.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: hello-world
spec:
  containers:
    - images: nginx:alpine
      name: nginx
      port:
        - containerPort: 8080
  nodeName: node01
```

To force a pod to be assigned to a node we can use a Binding object:
```yaml
apiVersion: v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: node02
```

And use the REST Api like follow:
```sh
curl --header "Content-Type: application/json" --request POST --data '{<json formatted binding definition>}' https://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/
```
