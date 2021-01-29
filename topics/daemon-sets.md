# Daemon Sets

Daemon Sets is a similar object as a Replica Set.
A Daemon Set will run 1 pod per node.
The primary use is for a monitoring, log and internal kube component such as proxy network.

```yml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: myapp
spec:
  selector:
    matchLabels:
      app: myapp
  template: # pod template
    metadata:
      name: myapp-pod
      labels:
        app: myapp # matched label
    spec:
      containers:
        - name: nginx-container
          image: nginx
```
