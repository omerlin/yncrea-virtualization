# Why RancherOS ?

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