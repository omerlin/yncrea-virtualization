# Why Docker machine ?

Integrates this : https://docs.docker.com/machine/#whats-the-difference-between-docker-engine-and-docker-machine

## Installation of DockerMachine:

Follow: [Docker machine installation link](https://github.com/docker/machine/releases)

``` bash
$ if [[ ! -d "$HOME/bin" ]]; then mkdir -p "$HOME/bin"; fi && \
curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-Windows-x86_64.exe > "$HOME/bin/docker-machine.exe" && \
chmod +x "$HOME/bin/docker-machine.exe"
```

!!! Important
    To play the above script, you will need {==git-bash==}

# Why RancherOS ?


## Installation of RancherOs using Docker machine

Download the latest release locally on your computer. 
[ Link to get the last RancherOS release](https://releases.rancher.com/os/latest/rancheros.iso)

!!! Question "why?"
    To avoid using too much bandwidth as we will reuse the image several time

[ The Rancher Official installation documentation] (https://rancher.com/docs/os/v1.x/en/installation/workstation/docker-machine/)

!!! Warning
    Use {==Git Bash==} to execute the following commands (**mobaXterm** won't work well here)
    Take care to change the {==virtualbox-boot2docker-url==} parameter value to refer the local file

``` bash
$ docker-machine create -d virtualbox \
        --virtualbox-boot2docker-url https://releases.rancher.com/os/latest/rancheros.iso \
        --virtualbox-memory <MEMORY-SIZE> \
        <MACHINE-NAME>
```

```
$ VBoxManage list runningvms | grep <MACHINE-NAME>
```     

## LABS: install 2 RancherOS machines

### Instruction
Install 2 machines named "server" and "worker1"

### Checking installation

!!! Example "Your turn"
    Ask some questions about the driver interface created ?
    
    * which type ?
    * could you check your networks possibility ?
    * pinging machine
    * access to internet ?

!!! Note
    Documentation is here: https://rancher.com/docs/os/v1.x/en/overview/

!!! Tip
    To start the VirtualBox guest addition:

    ```
           ros service enable virtual-box
           ros service start virtual-box
    ```

    You need to reboot the VM to really activate the service


## RancherOS commands summary

|COMMAND       |	DESCRIPTION                                                     |
------------------------------------------------------------------------------------|
|docker	       | Good old Docker, use that to run stuff.                            |
|system-docker | The Docker instance running the system containers. (root rights)   |
|ros	       | Control and configure RancherOS                                    |
