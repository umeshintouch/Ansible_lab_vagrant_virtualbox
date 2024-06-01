# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrantfile for creating CentOS 7 VMs
#
# Requirements:
# - Vagrant (https://www.vagrantup.com/downloads)
# - VirtualBox (https://www.virtualbox.org/wiki/Downloads)
#
# Usage:
# 1. Create a directory for your project and navigate into it:
#    mkdir my-centos7-vm
#    cd my-centos7-vm
#
# 2. Create a file named Vagrantfile in your project directory and paste this code into it.
#
# 3. Start the VM:
#    vagrant up
#
# 4. SSH into the VM:
#    vagrant ssh
#
# 5. Managing the VM:
#    To halt the VM: vagrant halt
#    To destroy the VM: vagrant destroy
#    To reload the VM (reboot with configuration changes): vagrant reload
#

Vagrant.configure("2") do |config|
  # Define webserver VMs
  (1..2).each do |i|
    config.vm.define "webserver-#{i}" do |machine|
      machine.vm.box = 'centos/7'
      machine.vm.hostname = "webserver-#{i}"
      # Use default NAT network by not specifying any network configuration
      machine.vm.provider "virtualbox" do |vb|
        vb.name = "webserver_nginx-#{i}"
        vb.cpus = 2
        vb.memory = 1024
      end
      machine.vm.provision "shell", inline: <<-SHELL
        yum -y update
        yum -y install epel-release
        yum -y install vim git
      SHELL
    end
  end

  # Define ansible VM
  config.vm.define "ansible" do |machine|
    machine.vm.box = 'centos/7'
    machine.vm.hostname = "ansible"
    # Use default NAT network by not specifying any network configuration
    machine.vm.provider "virtualbox" do |vb|
      vb.name = "ansible"
      vb.cpus = 2
      vb.memory = 1024
    end
    machine.vm.provision "shell", inline: <<-SHELL
      yum -y update
      yum -y install epel-release
      yum -y install vim git
    SHELL
  end

  # Configure proxy if vagrant-proxyconf plugin is installed
  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http     = ENV['HTTP_PROXY'] || "http://proxy.mgmt.intern:80"
    config.proxy.https    = ENV['HTTPS_PROXY'] || "http://proxy.mgmt.intern:80"
    config.proxy.no_proxy = "localhost,127.0.0.1"
  end
end
