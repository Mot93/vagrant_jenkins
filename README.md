# Setting up Debian/Ubuntu Jenkins server
This project help create a Jenkins server on a Debian/Ubuntu server.

## Before you begin
1. (Optional) If you want to make the server to be available from the host network, in the `Vagrantfile` you must specify on what interface to expose them.

config.vm.network "public_network", type: "dhcp", bridge: "interface_name"

2. In the `Vagrantfile`, in the resource definition, remember to update the virtualization backend `provider` to yours.

        config.vm.provider "<provider>" do |vb|

    Since I use `VirtualBox`, mine is:

        config.vm.provider "virtualbox" do |vb|

## Creating the VM
The VM can will be created by [Vagrant](https://www.vagrantup.com/).

In order to create and launch the VM, use the following Vagrant command.

vagrant up

Once done, destroy the vms:

vagrant destroy

## Launching ansible against the vagrant machines
Once the machines have been created, they have to be provisioned.
The tool used by this example to set-up the cluster is [Ansible](https://www.redhat.com/en/technologies/management/ansible).

### TIPS:
It's possible to relaunch the playbook `playbook.yml` against the machines made by Vagrant using an inventory auto-generated by Vagrant specifying the inventory path:

ansible-playbook playbook.yml -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory

To automate this process, it's possible to configure ansible via the `ansible.cfg` file.
To create the file: 

ansible-config init --disabled > ansible.cfg

Configure the `ansible.cfg` file:

1. Set the inventory to the one generated by vagrant:

inventory=./.vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory

2. Prevent ansible from checking the new VM ssh key:

host_key_checking=False

3. Specify where the role folder is stored:
roles_path=/path/to/role/

## Jenkins
Jenkins is, by default, listening on `port 8080`. 
To reach it visit the address [`http://localhost:8080/`](http://localhost:8080/).

In order to unlock Jenkins the first time is launched, get the password from the file `/var/lib/jenkins/secrets/initialAdminPassword` inside the VM:

vagrant ssh
cat /var/lib/jenkins/secrets/initialAdminPassword
