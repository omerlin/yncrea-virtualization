# VirtualBox cheatsheet

The CLI tool to do all Virtualbox operations: {==VBoxManage==}

## Useful links

* [VboxManage reference guide](https://www.virtualbox.org/manual/ch08.html)

!!! Tip
    Add your Virtualbox location to your environment PATH

## List VMs
```
VBoxManage list vms
```

!!! Info 
    Includes hidden VMs created by Vagrant

## Delete VM
```
VBoxManage unregistervm VMNAME --delete
```

## List forwarded VM ports
```
VBoxManage showvminfo VMNAME --machinereadable | Select-String -Pattern 'Forwarding'
```

!!! Note 
    Select-String is a PowerShell command

## VirtualBox Help

Simply type VBoxManage - You will see how rich is this CLI command
You can do all that is possible with the GUI and more ...