# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  #
  # Upgrading the server to the latest versions
  #
  config.vm.provision "shell", inline: <<-SHELL
    apt update
    apt upgrade -y
  SHELL

  #
  # Run Ansible from the Vagrant host to install jenkins
  #
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yaml"
  end

  #
  # VM 
  #
  config.vm.box = "debian/bullseye64"
  #config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "jenkins"
  #config.vm.network "public_network", type: "dhcp", bridge: ""
  config.vm.network "forwarded_port", guest: 8080, host: 8080, protocol: "tcp"
  # Jenkins is a resource heavy, setting up the recommended ammount
  config.vm.provider "virtualbox" do |vb|
    vb.cpus   = 4
    vb.memory = "2048"
  end

end
