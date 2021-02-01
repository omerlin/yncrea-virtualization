# Introduction

Kubernetes can be installed in many ways. 
We will concentrate here on Private Homer Office installation.
As this is an incredibly changing world, we are testing some tools having in mind following KISS principle even for production ready cluster.  

| **Method**  | **Scope / Usage** |  Alternative   |
|-------------|-------------------|----------------|
| `minikube`  | Dev environment   | [microk8s](https://microk8s.io/) or kubeadm |
| `k3s`       | Edge computing, ARM | microk8s also |
| `rke`       | Production cluster | [kubeadm](https://kubernetes.io/fr/docs/setup/production-environment/tools/kubeadm/install-kubeadm/) |
 
# Minikube

Installation link: [Minikube installation](https://kubernetes.io/docs/setup/minikube/)

## Windows Installation

### Requirements
You need virtualbox or Hyper-v

### Installation

On Windows, from ==an elevated command shell== :
```
choco install minikube

minikube start
```

!!! info ""
    The download ISO for minikube is around 170 Mb

!!! info
    Kubectl (kubernetes client) installation ==no more needed==, as this comes with minikube installation
    ```
    choco install kubernetes-cli 
    ```

Of course, you may need to upgrade a previous installation:
```
choco upgrade minikube -y
```

### Recovery procedure

!!! warning
    `For Windows`
    This procedure allows recovering to a sane environment and replay installation
  

```
minikube stop
choco uninstall minikube;kubernetes-cli
```

* Go to your USER home directory
* Delete ALL files under subdirectory `.minikube`
* Delete All files under subdirectory `.kube`
* (Optional) set your company proxy
* Install minikube
```
choco install minikube
```

* {--Put minikube.exe and kubectl.exe in your windows PATH--}
* (Optional) set your company proxy
* minikube start
* Test with kubectl:
```
kubectl get nodes 
NAME       STATUS   ROLES    AGE   VERSION
minikube   Ready    master   1m    v1.10.0
```

!!! warning
    We have an installed version 1.10.0 because we are ==not based on Windows 10 and Docker for Windows==
    So we suggest in that case to move to Linux installation

# K3S from Rancher

