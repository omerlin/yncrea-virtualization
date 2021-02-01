# Introduction

Kubernetes can be installed in many ways. 
We will concentrate here on Private Homer Office installation.
This is an incredibly changing world, we are testing some tools having in mind to cover simple development deployment to production one.  

| **Method**  | **Scope / Usage** |  Alternative   |
|-------------|-------------------|----------------|
| `minikube`  | Dev environment   | [microk8s](https://microk8s.io/) |
| `k3s`       | Edge computing, ARM | microk8s also |
| `rke`       | Production cluster | [kubeadm](https://kubernetes.io/fr/docs/setup/production-environment/tools/kubeadm/install-kubeadm/) |

 
# Minikube

## Requirements
You need virtualbox or Hyper-v

Installation link: [Minikube installation](https://kubernetes.io/docs/setup/minikube/)

## Installation
On Windows:
```
choco install minikube
```
!!! info
    Kubectl (kubernetes client) installation ==no more needed==, as this comes with minikube installation
    ```
    choco install kubernetes-cli 
    ```

### Recovery procedure

!!! warning
    `For Windows`
    This procedure allows recovering to a sane environment and replay installation
  

```
minikube stop
```

```
choco uninstall kubectl
```

* Go to your USER home directory
* Delete ALL files under subdirectory `.minikube`
* Delete All files under subdirectory `.kube`
* (Optional) set your company proxy
* Install minikube
```
choco install minikube
```

* Put minikube.exe and kubectl.exe in your windows PATH
* (Optional) set your company proxy
* minikube start
* Test with kubectl:
```
kubectl get nodes 
```

