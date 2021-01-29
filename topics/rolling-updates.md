# Rolling Upgrades and Rollbacks

## CLI

```sh
kubectl create -f deployment-definition.yaml
kubectl get deployments
kubectl apply -f deployment-definition.yaml # apply a new definiton file (rollout)
kubectl set image deployment/myapp nginx=nginx:1.9.1 # Changing the version will also trigger a rollout
kubectl rollout status deployment/myapp # Check the rollout status
kubectl rollout history deployment/myapp # check the history of rollouts
kubectl rollout undo deployment/myapp # rollback
``` 
