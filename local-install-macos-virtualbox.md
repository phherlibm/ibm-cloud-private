#Local installation in a Virtual Box on Mac OS
As of this writing, the current version of ICP CE is 2.1.0.2 and the installation procedure is described [here](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.2/installing/install_containers_CE.html).
To install ICP on a local machine with Virtualbox, there is a process based on Vagrant that is descrived [here](https://github.com/IBM/deploy-ibm-cloud-private/blob/master/docs/deploy-vagrant.md).
But for those who what to experiment and educate on the actual installation process, here are some tips to configure you VirtualBox VM.

##Step 1: Define a Host Network
This will create a private virtual network interface between the host and the guest VM.
Go to Global Tools, tab Host Network Manager and create a network. This should create a virtual network named vboxnet0 with network mask 192.168.56.1/24. 192.168.56.1 is the host. Disable DHCP.
Check with an ifconfig command on a terminal on the hos. You should see an interface named like the network (vboxnet0).

##Step 2: Parameterize network interfaces.
##Step 1: Create a Ubuntu Server VM
This is a standard task ; nothing particular to say, just provision for at least 12GB RAM and 40GB disk.
Ensure open ssh server is installed.

