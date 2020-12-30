# The "long" story of virtualization ...
## What and why

Main reasons to adopt virtualization fot IT administrators & Management

* Average 5-10 % of real resources used on traditional HW
* Hardware failure
* Single OS on top of Hardware

IT managers duties:

* Security
* Hw & Sw maintenance
* Hw Cooling and power management


![VIRTHW](./files/virtualization/virtualized_server_architecture.png "Virtualized HW")

## Where are we ?

In the late 1990s, VMware introduced a technology that enabled most of the
code to execute directly on the CPU without the requirement for translation
or emulation.

The concept of the “hypervisor” – a platform upon which IT could create and run virtual machines
comes from Mainframes

For years, VMware and its patents ruled the realm of virtualization. On the server side, running on bare metal,
VMware's ESX became the leading Type 1 (or native) hypervisor. 
On the client side, running within an existing desktop operating system, VMware Workstation was among the top Type
2 (or hosted) hypervisors.

Over the years, some interesting open-source projects emerged, including Xen and Quick EMUlator (QEMU). Neither was as fast or as
flexible as VMware, but they set a foundation that would prove worthy down the road. 
Around 2005, Advanced Micro Devices (AMD) and Intel created new processor extensions to the x86 architecture.
These extensions provided hardware assistance for dealing with privileged instructions.

![](./files/virtualization/timeline_virtualization1.png)

Called AMD-V and VT-x by AMD and Intel respectively, these extensions changed the landscape, eventually opening server
virtualization to new players.

For more details on Xen and KVM: [Xen and KVM](https://blog.octo.com/presentation-des-hyperviseurs-xen-et-kvm/)

Even Microsoft eventually got into the game with the release of Hyper-V in 2008. (Archi similar to Xen)
![TIMELINE2](./files/virtualization/timeline_virtualization2.png "Timeline part2")

When virtualization essentially became free, or at least accessible without expensive licensing fees,
new use cases came to light.
Most notably, Amazon began to use the Xen platform to rent some of its excess computing capacity to thirdparty customers.
Through their application programming interfaces (APIs), Amazon kicked off the revolution of elastic cloud computing, where the applications could self-provision resources to fit their workloads.

![TIMELINE3](./files/virtualization/timeline_virtualization3.png "Timeline part3")

Progressively, open-source hypervisors have matured and become pervasive in cloud computing.
Technology vendors developing solutions for virtual environments are increasingly required to support all major hypervisors (Xen and KVM)

![TIMELINE4](./files/virtualization/timeline_virtualization4.png "Timeline part4")

With this hypervisor parity, innovation became focused on the private/public cloud hardware architectures and the software ecosystems that surround them: storage architectures, softwaredefined networking, intelligent and autonomous orchestration, and
application APIs. 
This leads to new actors like Nutanix ant its HCI (Hyper Convergence Infrastructure) to emerge.

Legacy server applications are slowly retiring to give way to elastic, self-defining cloud applications (although they will coexist side by side for some time). 