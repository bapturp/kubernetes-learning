# RBAC

Roles define what a user can or cannot do.

Roles binding allow to map user to role.

## Create a role

__Role definition:__
```yaml
apiVerrsion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
- apigroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```

```sh
kubectl create -f developer-role-definition.yaml
```

## Create a role binding defintion

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
subjetcs:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```

```sh
kubectl create -f developer-role-binding-definition.yaml
```

## Create a Cluster Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-administrator
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["list", "get", "create", "delete"]
```


## Create a Cluster Role Binding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin-role-binding
subjects:
- kind: User
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-administration
  apiGroup: rbac.authorization.k8s.io
```

## Check roles

```sh
kubectl get roles
kubectl get rolebindings
kubectl describe role developer
```

## Check role permission

```sh
kubectl auth can-i create deployments
kubectl auth can-i delete nodes
kubectl auth can-i create deployments --as dev-user # impersonate
kubectl auth can-i create pods --as dev-user # impersonate
```
