# Objectives

The objectives of this lab is helping you to master VirtualBox deployment on your Laptop.
The local deployment could be useful to:

* Prototyping application locally without network latency
* Work on air gap environment (isolated for security reason or on to spare resources)
* Have a local CI
* Reduce resource usage (Desktop or Server) by leveraging your development host

!!! Important
    These labs will be reused for the future deployment, so don't miss it please !

## Vboxmanage

```
vboxmanage showvminfo worker1
```
Look if VT-x options are activated

# Labs 1: Using Linux Box with Vagrant

## Step 1: Start a VM
To create a VM using Vagrant you have several options.
By default, you have Cloud base VM Box available ![](https://app.vagrantup.com/boxes/search "Vagrant Cloud Box search")

By default, vagrant will go to internet to get the Vagrant Boxes.
You have just to do something like:
```bash
  vagrant init ubuntu/trusty64
  vagrant up
```
The first command {==vagrant init==} generate a {==Vagrantfile==}
The second command {==vagrant up==} deploy/instantiate the VM.

!!! Note 
    Vagrant networks options are quite limited on Virtualbox ( Nat and ==intnet== )

## Step 2: access the VM using ssh
You have several way to access the VM using SSH.
The easiest way is by doing a:

```bash
   vagrant ssh BOX_NAME
```

The second way is by finding where the private key is located and then create a config file in your $HOME/.ssh directory.

## Alternative : 

you can use this labs correction: https://github.com/omerlin/yncrea-virtualization-labs


https://www.vagrantup.com/docs/vagrantfile/machine_settings

# Labs 2: create a local Network of Linux Box
You need to start 2 VM, create a Nat Network and check :

* VM are reachable on the Nat network
* The VM can access to internet
* You can reach the 2 guest VM from the host using ssh alias

## Correction

See: https://github.com/omerlin/yncrea-virtualization-labs
You have in the TwoBoxes the Vagrant file

ssh vagrant@127.0.0.1 -p 2200
ssh vagrant@127.0.0.1 -p 2222

# Labs 3: deploy RancherOS using DockerMachine

 