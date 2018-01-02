# -*- mode: ruby -*-
# vi: set ft=ruby :

require './vagrant-provision-reboot-plugin'

Vagrant.configure("2") do |config|
  # ubuntu 14.04
  # config.vm.box = "ubuntu/trusty64"
  # ubuntu 16.04
  # config.vm.box = "ubuntu/xenial64"
  config.vm.box = "centos/7"

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = false
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.hostname = "ceph.zhato.com"
  config.vm.network :private_network, :ip => "192.168.56.71"

  config.vm.provider "virtualbox" do |v|
    # unless File.exist?("disk-0.vdi")
    #   v.customize ['storagectl', :id, '--name', 'SATAController', '--add', 'sata']
    # end
    # (0..2).each do |d|
    #   v.customize ['createhd', '--filename', "disk-#{d}", '--size', '11000'] unless File.exist?("disk-#{d}.vdi")
    #   v.customize ['storageattach', :id, '--storagectl', 'SATAController', '--port', 3 + d, '--device', 0, '--type', 'hdd', '--medium', "disk-#{d}.vdi"]
    # end

    v.cpus = 2
    v.memory = 2048
  end

  config.vm.provision "shell", inline: <<-SHELL
    setenforce 0
    systemctl disable firewalld && systemctl stop firewalld
    sed 's/^SELINUX=enforcing/SELINUX=disabled/g' -i /etc/selinux/config
    sed 's/^PasswordAuthentication no/PasswordAuthentication yes/g' -i /etc/ssh/sshd_config
    systemctl restart sshd

    sed "s/127\.0\.0\.1.*ceph\.zhato\.com.*ceph/127\.0\.0\.1 ceph/" -i /etc/hosts

    yum -y update
  SHELL

  # Reboot
  config.vm.provision :unix_reboot
end
