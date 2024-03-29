# Kubernetes installation
## Introduction

Kubernetes can be installed in many ways. 
We will concentrate here on Private Home Office installation.  
As this is an **incredibly changing world**, we are testing some tools having in mind following {==KISS==} principle even for production ready cluster.  

| **Method**  | **Scope / Usage** |  Alternative   |
|-------------|-------------------|----------------|
| `minikube`  | Dev environment   | [microk8s](https://microk8s.io/) [k3d](https://k3d.io) or kubeadm |
| `k3s`       | Edge computing, ARM | microk8s also |
| `rke`       | Production cluster | [kubeadm](https://kubernetes.io/fr/docs/setup/production-environment/tools/kubeadm/install-kubeadm/) |
 
!!! warning
    After many test, it seems to me better and more reliable to rely on K3s for our hands-on
    ==minikube== is also a good choice ... but experimented bad/strange behavior on my Windows 7 laptop

## Checking installation

Link: [This is how to check Kubernes is really working](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-service/)


## K3S from Rancher

* Quick Installation and architecture is here: [Lightweight Kubernetes](https://k3s.io/)
* Reference documentation is here: [Reference K3S documentation](https://rancher.com/docs/k3s/latest/en/)

* [master k3s installation with vagrant video](https://www.youtube.com/watch?v=JLnjMCRLcCo)
* [worker k3s installation with vagrant video](https://www.youtube.com/watch?v=ahavAvGau_4)
* [K3S cluster local installation](https://tferdinand.net/creer-un-cluster-kubernetes-local-avec-vagrant/)

### Installation on our Vagrant environment

!!! note
    ~~we increased the number of core=2 and memory=2048m on worker1~~
    
    ***WE CAN STOP DOCKER SERVICE***

#### master installation

```
VBoxManage snapshot worker1 restore snap-worker1-initial
VBoxManage startvm worker1 --type headless
VBoxManage startvm worker2 --type headless
```

Then `ssh worker1`:
```
# root
sudo -i
root@box1:~$ curl -sfL https://get.k3s.io | sh -
[INFO]  Finding release for channel stable
[INFO]  Using v1.20.2+k3s1 as release
[INFO]  Downloading hash https://github.com/rancher/k3s/releases/download/v1.20.2+k3s1/sha256sum-amd64.txt
[INFO]  Downloading binary https://github.com/rancher/k3s/releases/download/v1.20.2+k3s1/k3s
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
[INFO]  Skipping /usr/local/bin/ctr symlink to k3s, command exists in PATH at /usr/bin/ctr
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
Created symlink /etc/systemd/system/multi-user.target.wants/k3s.service → /etc/systemd/system/k3s.service.
[INFO]  systemd: Starting k3s
vagrant@box1:~$
vagrant@box1:~$
vagrant@box1:~$ sudo k3s kubectl get node
NAME   STATUS   ROLES                  AGE   VERSION
box1   Ready    control-plane,master   55s   v1.20.2+k3s1
```

!!! important
    The vagrant installation deploy the fixed NAT address on eth0
    ... but the communication interface is {==eth1==} (10.0.3.1/24)
    so we need to tell to the CNI that the real network is on this interface

    To do this, we have to update the k3s service `/etc/systemd/system/k3s.service`
    and add this :
    
    ```bash
    ExecStart=/usr/local/bin/k3s \
        server \
        --flannel-iface 'eth1'
    ```

A script to play this patch:

```bash
echo "    --flannel-iface 'eth1'">>/etc/systemd/system/k3s.service
systemctl daemon-reload
systemctl restart k3s
```

To get rights as vagrant user:
```
vagrant@box1:~$ export KUBECONFIG="/etc/rancher/k3s/k3s.yaml"
vagrant@box1:~$ sudo chmod o+r /etc/rancher/k3s/k3s.yaml
vagrant@box1:~$ kubectl get pods -A
NAMESPACE     NAME                                      READY   STATUS      RESTARTS   AGE
kube-system   metrics-server-86cbb8457f-45xvh           1/1     Running     0          3m11s
kube-system   local-path-provisioner-7c458769fb-c7rbv   1/1     Running     0          3m11s
kube-system   coredns-854c77959c-mh47k                  1/1     Running     0          3m11s
kube-system   helm-install-traefik-hpjb2                0/1     Completed   0          3m11s
kube-system   svclb-traefik-kt4k4                       2/2     Running     0          2m29s
kube-system   traefik-6f9cbd9bd4-wmnh6                  1/1     Running     0          2m29s
```

!!! note
    This install is really light as it doesn't use Docker at all but only containerd 

!!! warning
    There is currently an issue to create an agent - and so create a k3scluster
    So we will work on a standalone node.

#### woker node installation

The installation is extremely straightforward
You need to get the server authentication token (on box1) here: `/var/lib/rancher/k3s/server/node-token`


```bash
sudo -i
#
#  token comes from worker1(master) file /var/lib/rancher/k3s/server/node-token
# TAKE THE FULL TOKEN !!!!
#
scp 10.0.3.6:/var/lib/rancher/k3s/server/node-token /tmp/node-token
export AUTH_TOKEN=$( cat /tmp/node-token)
root@box2:~# curl -sfL https://get.k3s.io | K3S_TOKEN=$AUTH_TOKEN K3S_URL=https://10.0.3.6:6443 sh -
#
# post-install: TO NOT FORGET
# echo "    --flannel-iface 'eth1'">>/etc/systemd/system/k3s-agent.service
systemctl daemon-reload
systemctl stop k3s-agent
systemctl start k3s-agent
```

!!! warning
    Take the Token from the Server node (Worker1) from the file /var/lib/rancher/k3s/server/node-token
    Don't trunc or alter the Token !!!


At the end on **the master node** (box1), you should see this:
```
root@box1:~# sudo k3s kubectl get node -o wide
NAME   STATUS   ROLES                  AGE   VERSION        INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
box2   Ready    <none>                 13m   v1.20.2+k3s1   10.0.3.7      <none>        Ubuntu 18.04.3 LTS   4.15.0-58-generic   containerd://1.4.3-k3s1
box1   Ready    control-plane,master   27m   v1.20.2+k3s1   10.0.3.6      <none>        Ubuntu 18.04.3 LTS   4.15.0-58-generic   containerd://1.4.3-k3s1
```

#### Cluster validation

{==LABS==}: You have to validate the cluster is working well using [This is how to check Kubernes is really working](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-service/)

### Clean up agent manually

To repair bad configuration without restoring a snapshot :smile:

```bash
  sudo /usr/local/bin/k3s-killall.sh
  sudo /usr/local/bin/k3s-agent-uninstall.sh
```

### K8S major Tools installation

#### k9s GUI cli tool

**k9s** is a wonderful cli GUI interface. [k9S url](https://github.com/derailed/k9s)  
Trying it is adopting it.  
It makes all operations more easy on the K8S cluster.  

A script to install it.  

```bash
curl -v -L https://github.com/derailed/k9s/releases/download/v0.27.2/k9s_Linux_amd64.tar.gz -o /tmp/k9s_Linux_amd64.tar.gz
cd /tmp
tar -zxvf k9s_Linux_amd64.tar.gz
sudo cp /tmp/k9s /usr/local/bin
sudo chmod +x /usr/local/bin/k9s
mkdir ~/.kube
# if you want t get it remotly
# scp 10.0.3.6:/etc/rancher/k3s/k3s.yaml $HOME/.kube/config
cp /etc/rancher/k3s/k3s.yaml $HOME/.kube/config
```

!!! Note:
    If you copy the file to another node, you need to edit the file $HOME/.kube/config  
    and change the 127.0.0.1 by the master node IP: 10.0.3.6

## K3d

k3d is a lightweight wrapper to run k3s (Rancher Lab’s minimal Kubernetes distribution) in docker.

k3d makes it very easy to create single- and multi-node k3s clusters in docker, e.g. for local development on Kubernetes.

We will use {==K3d==} instead of {==minikube==} for several reasons:

* We have docker desktop - so we can benefit from it
* {==minikube==} is single node and has not the flexibility of {==k3d==} to create many clusters 

### K3d installation

On windows:  
``` 
choco install K3d
```

On Linux like:
Follow this link [to install latest release](https://raw.githubusercontent.com/rancher/k3d/main/install.sh)

### K3d usage

```bash
# one cluster
k3d cluster create demo --api-port 6550 --servers 1 --agents 3 --port 8080:80@loadbalancer --wait
# another one
k3d cluster create another --api-port 7550 --servers 1 --agents 1 --port 9080:80@loadbalancer --wait
```

!!! Tip
    install the following tools: **kubectx, kubens, k9s**

### Demo
The best thing is to clone this repository: https://github.com/iwilltry42/k3d-demo

## RKE from Rancher

==RKE== is to deploy efficiently a production Kubernetes cluster.  
Why RKE ?

Read this [introduction to Rke2](https://docs.rke2.io/)

!!! Tip
    Do a Virtualbox snapshot between all LABS ... thus you can come back to your snapshot easily.
    Rigor is really a good thing when talking about integration

### Installation

```
curl -LO https://github.com/rancher/rke/releases/download/v1.0.16/rke_linux-amd64
sudo cp rke_linux-amd64 /usr/local/bin/rke
sudo chmod +x /usr/local/bin/rke
```
Then we have to manage the SSH communication with other nodes
```
ssh-keygen
```
Then we install the ```id_rsa.pub``` public key in the ```vagrant@worker2:/home/vagrant/.ssh/authorized_keys```

!!! note
    Normally all these operations are done automatically with an automation tool like `ansible`
    It's important to check manually ssh connectivity


We create the cluster configuration file: cluster.yml

```yaml 
ssh_key_path: "/home/vagrant/.ssh/id_rsa"
ssh_cert_path: ""
ssh_agent_auth: false
nodes:
- address: "10.0.3.6"
  role:
    - controlplane
    - etcd
    - worker
  user: "vagrant"
- address: "10.0.3.7"
  role:
    - worker
  user: "vagrant"
```

!!! tip
    probably it would be better to add the worker1 as a worker too ... to not spoil resources

Then it's almost finished:

```
rke up
```

This can take a long time if your network is slow ... as it will download ~2Gb of data
If everything is fine, you need just to configure `kubectl`

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
cp kubectl /usr/local/bin/kubectl
sudo chmod +x /usr/local/bin/kubectl
export KUBECONFIG="/home/vagrant/kube_config_cluster.yml"
vagrant@box1:~$ kubectl version
Client Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.2", GitCommit:"faecb196815e248d3ecfb03c680a4507229c2a56", GitTreeState:"clean", BuildDate:"2021-01-13T13:28:09Z", GoVersion:"go1.15.5", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.17", GitCommit:"f3abc15296f3a3f54e4ee42e830c61047b13895f", GitTreeState:"clean", BuildDate:"2021-01-13T13:13:00Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/amd64"}
```

You are done !

```bash
vagrant@box1:~$ kubectl get nodes
NAME       STATUS   ROLES               AGE   VERSION
10.0.3.6   Ready    controlplane,etcd   29m   v1.17.17
10.0.3.7   Ready    worker              29m   v1.17.17
```

## Minikube (obsolete)

Installation link: [Minikube installation](https://kubernetes.io/docs/setup/minikube/)

##  (Minikube) Windows Installation

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


