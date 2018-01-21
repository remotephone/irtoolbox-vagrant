# -*- mode: ruby -*-
# # vi: set ft=ruby :

### REQUIREMENTS ###
# You need to install vagrant plugins vagrant-disksize and vagrant-reload for this to work
# vagrant plugin install vagrant-disksize
# vagrant plugin install vagrant-reload

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/xenial64"

  # Configure Disk size, requires vagrant-disksize plugin
  config.disksize.size = '25GB'

  # Configure Virtualbox to user host DNS servers
  config.vm.provider "virtualbox" do |vb, override|
  # resolve DNS requests through NAT'ing
  vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  #  From https://github.com/clong/DetectionLab/blob/master/Vagrant/Vagrantfile with mods
  vb.customize ["modifyvm", :id, "--memory", 4096]
  vb.customize ["modifyvm", :id, "--cpus", 2]
  vb.customize ["modifyvm", :id, "--vram", "32"]
  vb.customize ["setextradata", "global", "GUI/SuppressMessages", "all" ]
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
  config.vm.network "forwarded_port", id: 'ssh', guest: 22, host: 2222, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 8000, host: 8001, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 8888, host: 8002, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a private network, which generally matched to a NATed network.
  config.vm.network "private_network", type: "dhcp"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.
 
 ## DEFAULT USER CONFIGURATION
 ## Once the box is built, this password needs to be changed
  config.ssh.username = "ubuntu"
  config.ssh.password = "ubuntu"
  
  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get upgrade -y
    sudo apt-get install whois p7zip-full git docker.io virtualenv python-dev python3-dev python-pip libffi-dev libssl-dev nmap linux-image-extra-$(uname -r) -y
    sudo update-ca-certificates
    pip install awscli
    sudo usermod -aG docker ubuntu
    sudo systemctl start docker
    sudo systemctl enable docker
    mkdir -p /home/ubuntu/gits
    git clone https://github.com/remotephone/docker-splunk.git /home/ubuntu/gits/docker-splunk/
  SHELL

  config.vm.provision :reload
  
  config.vm.provision "shell", inline: <<-SHELL
    docker build -t splunkminfree /home/ubuntu/gits/docker-splunk/enterprise/
  SHELL

  end
end

