# -*- mode: ruby -*-
# vi: set ft=ruby :
DOMAINNAME = 'example.com'
IPSPACE = '192.168.13'

#script = <<-SCRIPT
sudo yum install vim
SCRIPT

unless Vagrant.has_plugin?("vagrant-hostmanager")
	raise 'use first: vagrant plugin install vagrant-hostmanager'
end

Vagrant.configure("2") do |config|
	config.hostmanager.enabled = true
	config.hostmanager.manage_guest = true
	config.hostmanager.manage_host = true
	config.hostmanager.ignore_private_ip = false
	config.hostmanager.include_offline = true
	config.vm.define "control" do |control|
		control.vm.box = "centos/7"
		control.vm.box_version="1809.01"
		control.vm.hostname="control.#{DOMAINNAME}"
		control.vm.network :private_network, ip: "#{IPSPACE}.11"
		control.vm.provision "shell", inline: $script
		control.vm.provider :virtualbox do |vb|
				vb.customize [
					"modifyvm", :id,
					"--memory", 1024,
					"--nic3", "intnet"
				]
			end
	end
	config.vm.define "managed" do |managed|
		managed.vm.box = "centos/7"
		managed.vm.box_version="1809.01"
		managed.vm.hostname="managed.#{DOMAINNAME}"
		managed.vm.network :private_network, ip: "#{IPSPACE}.12"
		managed.vm.provision "shell", inline: $script
		managed.vm.provider :virtualbox do |vb|
				vb.customize [
					"modifyvm", :id,
					"--memory", 1024,
					"--nic3", "intnet"
				]
		end
	end
end
