****************************************
Base Image for Testing Ansible Playbooks
****************************************

:title: Base Image for Testing Ansible Playbooks
:date: 2021-02-25
:category: Posts
:slug: base-image-for-testing-ansbile-playbooks
:tags: unix, devops

At nexB, we manage most of our infrastructure via Ansible, allowing us to deploy to many
different environments from private clouds to bare-metal infrastructure. In order to test
application provisions and deployments, we often use guest VMs as the targets for our
Ansible playbooks. It has been very useful to have a "template" VM image that can be
replicated for repeated test deployments.

There are a few things to consider when creating a base image for Ansible deployments,
depending on the distribution being used. At nexB we mainly support the Ubuntu LTS 
distribution, so this post will be related to this distribution specifically.

Provision the Virtual Machine
#############################
The very first step is to download the installation image we wish to use as our base VM. 
Once downloaded, create the initial virtual machine and run through the installation
process. You can use any hypervisor for this; I use Virtualbox personally, as I am
familiar with its settings and their locations. During this step, provision the VM's
resources and network configuration accordingly, including forwarding ports if the VM
network interface will be running under NAT.

Install the Distribution on the Virtual Machine
###############################################
Next, download the installation image of the Linux distribution you wish to test your
deployments on. Once this image is downloaded, mount the installation medium on the 
virtual machine and step through the installation process. Choose the appropriate settings
and set up some default ``sudo`` user. Most installers allow you to select some standard
set of packages; choose any that are appropriate but be sure to install OpenSSH. If you
miss this step or your distro does not give you a package selection option, you can
install OpenSSH after the installation process.

Initial Boot of the Virtual Machine
###################################
After installation, the system will need to run through the initial boot process to
generate various files and default settings, such as ssh and gpg keys. Once the initial
boot is complete, log in to the account that was created during the installation process.
Once logged in, there are two final steps that I follow in order to have a good base VM
image that can be used for testing Ansible deployments. 

First, I usually run an ``sudo apt update && sudo apt dist-upgrade`` to upgrade all the
packages in the base system. This will make sure that security patches and package version
upgrades happen before we start running playbooks against the VM.

Second, the US English locale is not generated in a vanilla Ubuntu server installation
and can lead to some program incompatibility if not set properly. Run the following
commands to set the ``en_US.UTF-8`` locale:
::

    sudo locale-gen en_US.UTF-8
    update-locale LANG=en_US.UTF-8
    sudo reboot

Make the Final VM Snapshot
##########################
Once the machine is restarted, it is now in an up-to-date and configured state that can
be used as a basis for testing Ansible playbooks. At this point, a snapshot should be
taken of the virtual machine; this snapshot can then be cloned into multiple machines to
test your playbooks. Having a snapshot also allows the user to reset the VM's state after
running a playbook, making it easy to test the repeatability of your Ansible roles and
tasks.
