# TODO

## Add Docker compose labs



# Plant UML

https://blog.anoff.io/2018-07-31-diagrams-with-plantuml/

https://blog.anoff.io/puml-cheatsheet.pdf

https://github.com/plantuml/plantuml-server

# TO ADD

ansible demo

correct the issue on "docker volume"

# Other materials 

## Some links to keep in mind
https://tomaszs2.medium.com/5-best-go-projects-manage-security-containers-and-build-backends-be0fc5f06cd4

https://asankov.dev/blog/2022/05/22/demystifying-the-kubernetes-iceberg-part-2/

## Kubernetes 20 questions 

From: https://medium.com/@croguerrero/top-20-kubernetes-interview-questions-e2fe0dcc6c52


Kubernetes is the greatest orchestration technology on the market, making this open-source system one of the most in-demand options. Today, Kubernetes is used by several international corporations, including Yahoo, SAP, SoundCloud, Huawei, and eBay.

1. What is software/DevOps orchestration?
Orchestration integrates various apps or services to automate a process or synchronize data in real time. Orchestration allows microservices in distinct containers to interact smoothly to accomplish a purpose.

2. How does Kubernetes orchestrate?
Kubernetes simplifies DevOps activities, including deployment, scalability, and setup. Containers host most distributed apps. App microservices may operate together in a container. Externally managed containers must be scheduled, distributed, and load-balanced to support infrastructure. Data persistence and network settings make clustered container management difficult.

3. What are Kubernetes’ benefits?
Kubernetes simplifies container management. It speeds up application deployment, improving customer service. Kubernetes automates rollbacks and scheduling. Kubernetes can cluster hosts across private, public, and hybrid clouds, making it perfect for cloud-native programs demanding quick scalability.

4. What are Kubernetes namespaces?
Kubernetes namespaces split cluster resources across users. This is beneficial when consumers are geographically dispersed.

5. What is a node in Kubernetes?
Nodes are the smallest computational unit. A minion is a single computer in a cluster that executes pods. Kubernetes master components govern the actual or virtual computer.

6. What are Kubernetes’ two primary components?
Master and worker nodes make up Kubernetes’ architecture. The master node includes an API server, scheduler, controller manager, etcd. Each worker node runs container runtime and Kube proxy.

7. What’s a pod’s purpose?
Kubernetes pods hold containers. Each pod may accommodate different containers based on settings. The pod’s containers share resources and a local network, making communication more straightforward.

8. Where does Kubernetes store cluster data?
Etcd is Kubernetes’ primary data store. Here is cluster data.

9. How are pods and jobs different in Kubernetes?
Their functions vary. Pods run containers. A task ensures pods run to completion.

10. Describe a kubelet.
Kubelet is Kubernetes’ lowest-level component. It ensures that all containers in a pod are functioning during their lifespan.

11. What’s kube-proxy?
Kube-proxy handles host sub-netting and provides system services. It directs incoming requests depending on IP and port number, aiding load balancing.

12. How does Kubernetes provide high availability of applications in a Cluster?
A Deployment Controller is present in a Kubernetes cluster. This controller keeps track of the Kubernetes instances in a cluster. Deployment Controller will replace a node if it or the system hosting fails. It is a self-healing technique in Kubernetes that ensures application availability. As a result, in a Kubernetes cluster, the Kubernetes Deployment Controller is in charge of initiating and replacing instances in the event of a failure.

13. What’s a Kubernetes scheduler?
Kube-scheduler distributes workload across worker nodes. It schedules jobs and monitors resource utilization to ensure proper allocation.

14. What are Daemon sets?
Daemon sets are one-time-use pods. They’re utilized for host layer properties like network monitoring, which execute once per host.

15. How do you ensure a pod always runs?
Liveness probes can determine this. It checks whether a pod’s app is running. Failure restarts the container.

16. How can Kubernetes APIs be secured?
Kubernetes API security approaches include:

    Use the correct authorization mode with the API server
    Use API authentication
    Ensure that TLS protects all incoming traffic
    Use authorization-mode=Webhook to make kubeless protect the API
    Use restrictive RBAC role policy on the kube-dashboard
    Remove any default service account permissions

17. What is a Load Balancer?

Load balancing enables network services. Internal load balancing means automatically balancing and assigning pod loads. External load balancing directs traffic to backend pods.

18. What is a controller manager?
Kubernetes controller manager is a daemon that embeds system control loops. Multiple Master node processes assist.

19. What are Google Kubernetes Engine’s uses?
GKE manages Docker containers and clusters. GKE orchestrates Google’s public cloud container clusters.

20. Kubernetes vs Docker Swarm?

Both methods divide applications into containers, allowing for easy application administration and scalability automation. Here is a broad breakdown of their distinctions:

    Kubernetes is an open-source and modular orchestration framework that provides an efficient container orchestration solution for high-demand applications with extensive setup.
    Docker Swarm prioritizes usability, making it best suited for basic applications that are fast to install and maintain.

Additional resource:https://www.bmc.com/blogs/kubernetes-vs-docker-swarm/