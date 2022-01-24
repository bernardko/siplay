#!/usr/bin/env ruby
Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/focal64"
  
    extra_files = File.expand_path("./test-inventory/files/", File.dirname(__FILE__))
  
    # Provide enough RAM to build python 3
    config.vm.provider :virtualbox do |virtualbox|
      virtualbox.customize ["modifyvm", :id, "--memory", 2048]
    end
  
    config.vm.define :vagranthost do |node|
      node.vm.box = "ubuntu/focal64"
      node.vm.hostname = "vagranthost"
      node.vm.network :forwarded_port, guest: 80, host: 8080
      node.vm.network :private_network, ip: "192.168.139.100"
      node.vm.provision :ansible do |ansible|
        ansible.playbook = File.expand_path("./playbooks/setup.yml")
        ansible.become = true
        ansible.inventory_path = "./test-inventory/vagrant"
        ansible.extra_vars = { env:"vagrant", inventory_name:"test-inventory", files:extra_files }

        # Cannot have spaces in your raw_arguments - fails without errors
        # if using expand_path, check for spaces in the generated path
        ansible.raw_arguments = ["--extra-vars=@./test-inventory/vagrant.vault"]
      end
    end
  
  end