# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = false
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.hostname = "ceph-single-node"
  config.vm.network :private_network, :ip => "192.168.56.71"

  config.vm.provider "virtualbox" do |v|
    (0..2).each do |d|
      v.customize ['createhd', '--filename', "disk-#{d}", '--size', '11000'] unless File.exist?("disk-#{d}.vdi")
      v.customize ['storageattach', :id, '--storagectl', 'SATAController', '--port', 3 + d, '--device', 0, '--type', 'hdd', '--medium', "disk-#{d}.vdi"]
    end

    v.cpus = 2
    v.memory = 2048
  end
end
