# Network Policies

Network policies allows to allow/denied pods from being reachable by others.

Network Policies works only if the network stack deployed within k8s supports it.

## Network Policies

```yaml
---
# Allow Ingress tcp/3306 from pod named "api-pod"
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod
    ports:
    - protocol: TCP
      port: 3306
```
