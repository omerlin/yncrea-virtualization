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

!!! Tip
    For a future use:  
    Under git-bash or any WSL Linux machine (if virtualbox is added to Environment PATH variable)  
    ``` VBoxManage.exe showvminfo worker1|grep NIC```

# Labs 1: Using Linux Box with Vagrant

## Step 1: Start a VM
To create a VM using Vagrant you have several options.
By default, you have Cloud base VM Box available ![](https://app.vagrantup.com/boxes/search "Vagrant Cloud Box search")

By default, vagrant will go to internet to get the Vagrant Boxes.  
It will download the image only one times.  
Sometime we prefer to work **OFFLINE** for many reasons ( No access, security, network bandwidth ... )  

We will first download the box [bionic64](https://app.vagrantup.com/debian/boxes/buster64)... then integrates it locally using **vagrant box**  

You have just to do something like:
```powershell
  vagrant box add buster64 file:///$Env:USERPROFILE/Downloads/
  vagrant box list
```

Then, create your own directory, for instance %USERPROFILE%/MyApp/vagrant.  

```powershell
  cd $Env:USERPROFILE/MyApp/vagrant
  # This command will create a Vagrantfile
  vagrant init buster64
  vagrant up
```
The first command {==vagrant init==} generate a {==Vagrantfile==}  
The second command {==vagrant up==} deploy/instantiate the VM.



!!! Note
    The default machine configuration may use a lot of ressources (memory / CPU ) ... take care.

!!! Note 
    Vagrant networks options are quite limited on Virtualbox ( Nat and ==intnet== )  
    To manage a Nat Network with vagrant: [Vagrant virtual networking](https://www.vagrantup.com/docs/providers/virtualbox/networking)

!!! Remark
    if you have issues with proxy

    ```
       set http_proxy=http://user:password@host:port
       set https_proxy=https://user:password@host:port
       vagrant plugin install vagrant-proxyconf
    ```

    then

    ```
       set VAGRANT_HTTP_PROXY=%http_proxy%
       set VAGRANT_HTTPS_PROXY=%https_proxy%
       set VAGRANT_NO_PROXY="127.0.0.1"
       vagrant up
    ```


## Step 2: access the VM using ssh
You have several way to access the VM using SSH.
The easiest way is by doing a:

```powershell
   # this will be "default" if we don't define the name
   vagrant status
   vagrant ssh default
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

```
ssh vagrant@127.0.0.1 -p 2200
ssh vagrant@127.0.0.1 -p 2222
```

# Integrates all together

We will configure the 3 machines - so they belongs to the same internal network and are connected using Mobaxterm.

To see the vagrant environments (and clean old one):
```
vagrant global-status --prune
```

Then you can find the "private_key" of each vagrant VMs using a Unix (or Windows) "find" command.