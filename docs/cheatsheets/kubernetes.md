# Kubernetes Cheatsheet

## Useful Links

The most complete reference :
  
* [Official kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
* [My favourite to debug kubernetes services](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-service/)

## Condensed cheat sheet

* Get information about all running pods:
```
kubectl get pods
```

* get pods, and also show labels attached to those pods
```
kubectl get pods --show-labels
```

* Get all pods on all namespaces
```
kubectl get pods --all-namespaces
```

* Describe one pod
```
kubectl describe pod <pod> 
```

* Expose the port of a pod (creates a new service)
```
kubectl expose pod <pod> --port=444 --name=frontend
```

* Port forward the exposed pod port to your local machine
```
kubectl port-forward <pod> 8080
```

* Attach to the pod
```
kubectl attach <podname> -i
```

* Execute a command on the pod
```
kubectl exec <pod> -- command
```

* Add a new label to a pod
```
kubectl label pods <pod> mylabel=awesome
```

* Run a shell in a pod - very useful for debugging
```
kubectl run -i --tty busybox --image=busybox --restart=Never -- sh
```

* Get information on current deployments
```
kubectl get deployments
```

* Get information about the replica sets
```
kubectl get rs 
```

* Get deployment status
```
kubectl rollout status deployment/helloworld-deployment
```

* Run k8s-demo with the image label version 2
```
kubectl set image deployment/helloworld-deployment k8s-demo=k8s-demo:2
```

* Edit the deployment object
```
kubectl edit deployment/helloworld-deployment
```

* Get the status of the rollout
```
kubectl rollout status deployment/helloworld-deployment
```

* Get the rollout history
```
kubectl rollout history deployment/helloworld-deployment
```

* Rollback to previous version
```
kubectl rollout undo deployment/helloworld-deployment
```

* Rollback to any version version
```
kubectl rollout undo deployment/helloworld-deployment --to-revision=n
```

## Abbreviations used

| Resource type | alias |
|---------------|-------|
| configmaps | cm |
| customresourcedefinition | crd | 
| daemonsets | ds | 
| horizontalpodautoscalers | hpa |
| ingres  | ing | 
| limitranges |limits |
| Namespace | ns |
| nodes | no | 
| persistentvolumeclaims | pvc |
| persistentvolumes | pv |
| pods |po |
| replicasets | rs |
| replicationcontrollers  | rc |
| resourcequotas | quota |
