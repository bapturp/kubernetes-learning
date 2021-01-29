# Node selectors

Limit a pod to run on a specific node

```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp
spec:
  containers:
    - name: myapp-compute
      image: myapp-compute
  nodeSelector:
    size: Large
```

`size: Large` is a label that must be defined on nodes:
```sh
kubectl label nodes node01 size=Large
kubectl get pod redis --show-labels
```

# Node Affinity

Ensure a pod is assigned to a specific node

```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp
spec:
  containers:
    - name: myapp-compute
      image: myapp-compute
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In # values can be In, Existse
            values:
              - Large
```

`requiredDuringSchedulingIgnoredDuringExecution`: Will not run de pod if the node wanted is unavailable
`preferedDuringSchedulingIgnoredDuringExecution`: Will run the pod on a different node is the selected one is unavailable.
