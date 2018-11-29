# -*- mode: ruby -*-
# vi: set ft=ruby :

# check for vagrant-docker-compose plugin dependency
unless Vagrant.has_plugin?("vagrant-docker-compose")
    system("vagrant plugin install vagrant-docker-compose")
    puts "Dependencies installed, please try the command again."
    exit
end

Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/trusty64"

    config.vm.network "forwarded_port", guest: 8080, host: 8080
    config.vm.network "forwarded_port", guest: 3000, host: 3000

    config.vm.provider "virtualbox" do |vb|
        # Display the VirtualBox GUI when booting the machine
        # vb.gui = true
        vb.name = 'docker-compose-vm'

        # Customize the amount of memory on the VM:
        vb.memory = "2048"
        vb.cpus = 2

        # This setting makes the NAT engine use the host's resolver mechanisms to handle DNS requests. See <https://www.virtualbox.org/manual/ch09.html#nat-adv-dns>
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        # This setting makes the NAT engine proxy all guest DNS requests to the host's DNS servers. See <https://www.virtualbox.org/manual/ch09.html#nat-adv-dns>
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

    # update the installed packages if any
    # config.vm.provision :shell, inline: "apt-get update"

    # set up Docker in the new VM:
    config.vm.provision :docker

    # install docker-compose into the VM
    config.vm.provision :docker_compose

    # install docker-compose into the VM and run the docker-compose.yml file - if it exists - whenever the VM starts
    # config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", run:"always"
end
  