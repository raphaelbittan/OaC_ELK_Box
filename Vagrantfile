# -*- mode: ruby -*-
# vi: set ft=ruby :
 
# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
numvcpus = 2
memsize = 3072

# Create a minimal Ubuntu box
Vagrant.require_version ">= 2.2.10" 
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    # set to false, if you do NOT want to check the correct VirtualBox Guest Additions version when booting this box
    if defined?(VagrantVbguest::Middleware)
        config.vbguest.auto_update = true
    end

    # Configure the base box
    config.vm.define "ubuntu" do |ubuntu|

        # Find the latest images on https://app.vagrantup.com/ubuntu
        ubuntu.vm.box = "ubuntu/bionic64"
        ubuntu.vm.hostname = "elastic-stack"
        ubuntu.vm.network :forwarded_port, guest: 5601, host: 5601
        
        # If you export the box switch to the second line to keep the /elk-stack/ folder
        ubuntu.vm.synced_folder "elastic-stack/", "/elastic-stack/"

        # Disable the new default behavior introduced in Vagrant 1.7, to
        # ensure that all Vagrant machines will use the same SSH key pair.
        # See https://github.com/mitchellh/vagrant/issues/5005
        #ubuntu.ssh.insert_key = false
    end


    # Configure the VirtualBox parameters
    config.vm.provider "virtualbox" do |vb|
        vb.name = "ELK-STACK-BOX"
        vb.customize [ "modifyvm", :id, "--cpus", numvcpus.to_s, "--memory", memsize.to_s ]
    end


    # Configure the box with Ansible
    config.vm.provision "ansible_local" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.version = "2.8.8"
        ansible.become = true
        ansible.limit = "all"
        ansible.verbose = "vv"
        ansible.install = true
        ansible.install_mode = "pip"
        ansible.playbook = "/elastic-stack/0_install.yml"
        ansible.inventory_path = "/elastic-stack/inventory"
    end
end
