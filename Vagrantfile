# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.
  config.ssh.forward_x11 = true

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.

  required_plugins = %w( vagrant-vbguest vagrant-disksize )
    _retry = false
    required_plugins.each do |plugin|
        unless Vagrant.has_plugin? plugin
            system "vagrant plugin install #{plugin}"
            _retry=true
        end
    end

    if (_retry)
        exec "vagrant " + ARGV.join(' ')
    end

  config.vm.box = "ubuntu/xenial64"
  config.disksize.size = "20GB"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 22, host: 2222

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "VM_share", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true
  
     # Customize the amount of memory on the VM:
  	vb.memory = "4096"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
  $script1 = <<-SHELL
	apt-get -y update
	apt-get -y install curl git gawk wget tftpd tftp isc-dhcp-server nfs-kernel-server make bc
	apt-get -y install gcc-aarch64-linux-gnu docker.io device-tree-compiler libncursesw5-dev libncurses5-dev
	mkdir -p /home/vagrant/grapeboard/ && cd /home/vagrant/grapeboard/
    tar xvzf /vagrant_data/Files/flexbuild_lsdk1712.tgz -C /home/vagrant/grapeboard
    cd /home/vagrant/grapeboard/flexbuild/
    patch < /vagrant_data/Files/grapeboard_support_lsdk-1712.patch  -p1
    source setup.env
    flex-builder -i mkrfs -a arm64 -m grapeboard -B additional_packages_list_full_grapeboard
    flex-builder -c linux -a arm64 -m grapeboard
    tar xvzf /vagrant_data/Files/components_arm64.tgz -C /home/vagrant/grapeboard/flexbuild/build/images
    flex-builder -i merge-component -a arm64 -m grapeboard
  SHELL

  config.vm.provision "root_action", type: "shell", inline: $script1

  $script2 = <<-SHELL


  	

  SHELL

  config.vm.provision "user_action", type: "shell", inline: $script2, privileged: false


end
