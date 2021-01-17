# Why RancherOS ?

^^Several reasons :^^ :thumbsup:

* This is because [Rancher](https://rancher.com/) is a ==cool== company delivering KISS[^1] cloud tools

* For Kubernetes, we will see 2 good "On Premise"[^2] Rancher tools

    * ==K3S== a very light kubernetes dedicated to Edge[^3] Computing
      It really nice as you can start really easily a Kubernetes cluster on Raspberry PI

    * ==RKE== (Rancher Kubernetes Engine) the professional Kubernetes packaging easy to use
  
  [^1] Keep It Simple and Stupid
  [^2] "On Premise" means "private", we can say also "In House"
  [^3] Edge computing could be seen as a micro Data center (IOT / 5G)


!!! note
    Another very cool company you should have a look to: [Hashicorp](https://www.hashicorp.com/) +
    Example of nice products: vagrant, packer, terraform, vault


## Links

* [ RancherOS release](https://releases.rancher.com/os/latest/rancheros.iso)
* [ Rancher OS installation with dockerMachine](https://rancher.com/docs/os/v1.x/en/installation/workstation/docker-machine/)
* [ Rancher OS overview](https://rancher.com/docs/os/v1.x/en/overview/)


## LABS: installing RancherOS machines

They are several objectives behind this LAB
* Show you another virtual machine deployments
* Explore and understand another modern packaging
* Have machines ready to test docker
* Have machines ready for a future Kubernetes cluster

### Instruction
You need to install 2 machines named "ROSServer" and "ROSWorker1"

### Deployment instructions
To deploy a RancherOS machine, you will need this command. (you will have to adapt it)

```bash
$ docker-machine create -d virtualbox \
        --virtualbox-boot2docker-url https://releases.rancher.com/os/latest/rancheros.iso \
        --virtualbox-memory <MEMORY-SIZE> \
        <MACHINE-NAME>
```

!!! Warning
    Use ==Git Bash== to execute the following commands (**MobaXterm** won't work well here)

!!! Tip
    To avoid using too much bandwidth you can download the ISO locally (around 140Mb)
    and then change the parameter ==virtualbox-boot2docker-url== value to refer a local file

To check the VM are deployed:

```
$ VBoxManage list runningvms | grep <MACHINE-NAME>
```     

### Understand the installation

!!! Example "Your turn"
    Ask you some questions about the driver interface created ?

    - [x] What are the interface type ?
    - [x] Could you check your networks possibility ?
    - [x] Pinging machine (from your localhost, between the machines)
    - [x] Access to internet ?


!!! Tip
    To start the VirtualBox guest addition:

    ```bash
       ros service enable virtual-box 
       ros service start virtual-box
    ```

    You need to reboot the VM to really activate the service

## RancherOS commands summary

|COMMAND       |DESCRIPTION                                                      |
|:------------:|:---------------------------------------------------------------:|
|docker        | Good old Docker, use that to run stuff.                         |
|system-docker | The Docker instance running the system containers. (root rights)|
|ros           | Control and configure RancherOS                                 |
