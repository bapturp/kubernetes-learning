# JSON Path kubectl

Show node name in the cluster
```sh
kubectl get nodes -o=jsonpath='{$.items[*].metadata.name}'
```


Show cpu architecture of the node
```sh
kubectl get nodes -o=jsonpath='{$.items[*].status.nodeInfo.architecture}'
```

Show the number of cpus core
```sh
kubectl get nodes -o=jsonpath='{$.items[*].status.capacity.cpu}'
```

Show node name and cpu count on two lines
```sh
kubectl get nodes -o=jsonpath='{$.items[*].status.nodeInfo.architecture}{"\n"}{$.items[*].status.capacity.cpu}'


kubectl get nodes -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"}{end}'
```

## Custom columns
```
kubectl get nodes -o=custom-columns=NODE:.metadata.name,CPU:.status.capacity.cpu --sort-by=.metadata.name
```
