# Taint, Toleration and Node Affinity

Taints are applied to nodes and prevent pods to be scheduled/executed on the node if they do not tolerate the taint.

Tolerations are applied to nodes and allow them to tolerate node taints.

__/!\\__ Taint and toleration  will only ensure that certain pods cannot be scheduled on certain node. It does not force a pod to be scheduled on a certain node. This is achieved through another concept called node affinity.

Node affinity

## Taint

Taints are set on a node.

A taint consist of a _key-value pair_ and a _taint-effect_. The taint effect can take 3 values:
- `NoSchedule`: Pod will not be scheduled on the node
- `PreferNoSchedule`: Scheduler will try to not schedule the pod on the node
- `NoExecute`: Any pods that do not tolerate the taint will be evicted immediatly, 

```sh
kubectl taint node node01 color=blue:NoSchedule
```

## Tolerations

Tolerations are set on pod:
```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
    - name: nginx-container
      image: nginx
  tolerations: # toleration
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"
```

## Node affinity

```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp
spec:
  affinity:
    nodeAffinity: # node affinity
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/e2e-az-name
            operator: In
            values:
            - e2e-az1
            - e2e-az2
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: another-node-label-key
            operator: In
            values:
            - another-node-label-value
  containers:
  - name: with-node-affinity
    image: k8s.gcr.io/pause:2.0
```



## Master node

The master node is a specific node that runs the management software but yet it's a node. The master node have by default a taint that prevent the scheduler to schedule any pod in it.

```sh
kubectl describe node kubemaster | grep Taint
```