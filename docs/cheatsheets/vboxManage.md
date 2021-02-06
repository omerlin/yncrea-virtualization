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

Running VMs :
```
vboxmanage list runningvms
```

!!! Info 
    Includes hidden VMs created by Vagrant

## Start VM
In headless mode
```
VBoxManage startvm worker1 --type headless
```


## Stop VM
Clean way to stop machines:
```
vboxmanage controlvm worker1 poweroff soft
```

## Snapshot VM
```
VBoxManage snapshot worker1 take snap-worker1-initial --description="initial state"
```

## Restore a snapshot

```
VBoxManage snapshot { uuid|vmname } restore { snapshot-name }
```

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