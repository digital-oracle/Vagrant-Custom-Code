# -*- mode: ruby -*-
# vi: set ft=ruby :

# Variables
var_box            = 'bento/oracle-7'   ## Search from 
var_vm_name        = 'OL7_122_VAGRANT_GG1'
var_mem_size       = 4096  # More would be better.
var_cpus           = 2 
var_non_rotational = 'on' # SSD by default
var_disk1_name     = './ol7_122_u01.vdi'
var_disk2_name     = './ol7_122_u02.vdi'
var_hostname	   = 'sgghost'
var_disk_size      = 100  ## Define 100G for /u01 & /u02
var_private_network  = '192.168.2.90'


Vagrant.configure("2") do |config|
  config.vm.box = var_box
  config.vm.hostname=var_hostname
  config.vm.network "forwarded_port", guest: 8080, host: 8081
  config.vm.network "private_network", ip: var_private_network
  config.vm.network "public_network"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = var_mem_size
    vb.cpus   = var_cpus
    vb.name   = var_vm_name

    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', '0', '--nonrotational', var_non_rotational]

    unless File.exist?(var_disk1_name)
      vb.customize ['createhd', '--filename', var_disk1_name, '--size', var_disk_size * 1024]
    end
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--nonrotational', var_non_rotational, '--medium', var_disk1_name]

    unless File.exist?(var_disk2_name)
    vb.customize ['createhd', '--filename', var_disk2_name, '--size', var_disk_size * 1024]
    end
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--nonrotational', var_non_rotational, '--medium', var_disk2_name]
  end

  config.vm.provision "shell", inline: <<-SHELL
    sh /vagrant/scripts/setup.sh
  SHELL
end
