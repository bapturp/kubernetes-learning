# Deployment

## Base template

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels: # deployment label
    app: myapp

spec:
  replicas: 3
  selector:
    matchLabels: # Define which 'pod' label to match
      app: myapp
  template:
    metadata:
      labels: # pods label
        app: myapp
    spec:
      containers:
        - name: nginx
          image: nginx
```
