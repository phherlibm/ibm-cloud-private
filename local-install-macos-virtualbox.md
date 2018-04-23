# Local installation in a Virtual Box on Mac OS
As of this writing, the current version of ICP CE is 2.1.0.2 and the installation procedure is described [here](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.2/installing/install_containers_CE.html).

To install ICP on a local machine with Virtualbox, there is a process based on Vagrant that is descrived [here](https://github.com/IBM/deploy-ibm-cloud-private/blob/master/docs/deploy-vagrant.md).

But for those who want to experiment and educate on the actual installation process, here are some tips to configure you VirtualBox VM.

## Step 1: Define a Host Network
This will create a private virtual network interface between the host and the guest VM.

Go to Global Tools, tab Host Network Manager and create a network. This should create a virtual network named `vboxnet0` with network mask 192.168.56.1/24. 192.168.56.1 is the host. Disable DHCP.

Check with an `ifconfig` command on a terminal on the host. You should see an interface named like the network (vboxnet0).

## Step 2: Create a VM.
This is a standard task ; nothing particular to say, just provision for at least 12GB RAM and 40GB disk. Ensure "Standard system utilities", "Virtual Machine Host" and "OpenSSH server" are installed.

## Step 3: Parameterize network interfaces
Before launching the VM, change the Network settings:
1. On the network tab, choose NAT for Card 1 and Para-Virtual Network (virtio-net) as the card type.
2. Enable Card 2: Choose Private Host Network as the type, select the virtual network created earlier and choose the virtio-net card type; set Allow all for the promiscuity mode.

## Step 4: Set the network interfaces of the guest VM
Launch the VM. Install Ubuntu.

After the installation, login, and check the network interfaces. You should see interface enp0s3 with IP 10.0.2.15. This is the IP associated with Card 1. Try to ping google.com: this checks you have access to the Internet.

Edit (sudo) the /etc/network/intefaces file. And define the enp0s8 interface as follow. This interface is associated with card 2 and enable communication between host and guest :
```
auto enp0s8
iface enp0s8 inet static
address 192.168.56.15
netmask 255.255.255.0
```

As an example, my complete interfaces file looks like this:
```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto enp0s3
iface enp0s3 inet dhcp


auto enp0s8
iface enp0s8 inet static
address 192.168.56.15
netmask 255.255.255.0
```

Save. Type `ifup enp0s8`. Now you should be able to ping (and ssh) from your host to your guest, and therefore access IBM Cloud Private console from your local navigator !



