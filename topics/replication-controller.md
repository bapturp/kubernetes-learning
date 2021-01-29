# ReplicationController


## Declarative
```
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: frontend
spec:
  template: # pod-definition
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
```