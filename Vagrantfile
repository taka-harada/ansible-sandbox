# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  # common ssh-private-key
  config.ssh.insert_key = false
  config.ssh.private_key_path = "~/.vagrant.d/insecure_private_key"

  # copy to private-key
  config.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/id_rsa"
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    chmod 600 $HOME/.ssh/id_rsa
  SHELL

  # control-vm
  config.vm.define "ansible-controller" do |ansible_controller|
    ansible_controller.vm.hostname = "ansible-controller"
    ansible_controller.vm.network "private_network", ip: "192.168.50.101"
    ansible_controller.vm.synced_folder ".", "/vagrant", :mount_options => ['dmode=775', 'fmode=664']

    ansible_controller.vm.provider "virtualbox" do |vb|
      vb.memory = 512
      vb.cpus = 2
      vb.name = "ansible-controller"
    end

    ansible_controller.vm.provision "shell", privileged: false, inline: <<-SHELL
      sudo apt-add-repository ppa:ansible/ansible
      sudo apt-get update
      sudo apt-get install -y ansible python-apt
    SHELL
  end

  # node01-vm
  config.vm.define "ansible-node01" do |ansible_node01|
    ansible_node01.vm.hostname = "ansible-node01"
    ansible_node01.vm.network "private_network", ip: "192.168.50.102"

    ansible_node01.vm.provider "virtualbox" do |vb|
      vb.memory = 512
      vb.cpus = 2
      vb.name = "ansible-node01"
    end
  end

end