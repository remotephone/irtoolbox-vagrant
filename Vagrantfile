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

  # Create a private network, which generally matched to a NATed network.
  config.vm.network "private_network", type: "dhcp"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # Forward SSH and ports for CyberChef/Splunk to localhost only
  config.vm.network "forwarded_port", id: 'ssh', guest: 22, host: 2222, host_ip: "127.0.0.1", auto_correct: true
  config.vm.network "forwarded_port", guest: 8000, host: 8001, host_ip: "127.0.0.1", auto_correct: true
  config.vm.network "forwarded_port", guest: 8888, host: 8002, host_ip: "127.0.0.1", auto_correct: true

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  
  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.

  # Update all files, install some required files and docker, awscli, enable docker at boot, add user ubuntu to group, and clone splunk-docker
  # Update the system

  config.vm.provision "shell", inline: <<-SHELL

    sudo apt-get update
    sudo apt-get upgrade -y
  SHELL

  # Install some required pacakges

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get install apt-transport-https ca-certificates software-properties-common whois curl lynx autoconf bzip2 dnsutils p7zip-full git libgtk2.0-dev virtualenv python-dev python3-dev python-pip libffi-dev libssl-dev nmap linux-image-extra-$(uname -r) -y
    sudo update-ca-certificates
    pip install awscli
  SHELL

  # Install docker

  config.vm.provision "shell", inline: <<-SHELL
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt-get update
    sudo apt-get install docker-ce -y
    sudo usermod -aG docker ubuntu
    sudo systemctl start docker
    sudo systemctl enable docker
  SHELL

  # Reboot VM So user/group changes take effect.
  config.vm.provision :reload


  # Get all your repositories and build some containers
  config.vm.provision "shell", inline: <<-SHELL
    sudo su ubuntu
    mkdir -p /home/ubuntu/gits/
    cd /home/ubuntu/gits
    git clone https://github.com/decalage2/oletools.git
    git clone https://github.com/1aN0rmus/TekDefense-Automater.git
    git clone https://github.com/nolze/ms-offcrypto-tool.git
    find /home/ubuntu/gits -name requirements.txt -exec pip install -r {} ';'
    git clone https://github.com/Neo23x0/yarGen.git
    git clone https://github.com/jesparza/peepdf.git
  SHELL

  # Configure splunk
  config.vm.provision "shell", inline: <<-SHELL
    git clone https://github.com/splunk/docker-splunk.git /home/ubuntu/gits/docker-splunk/
    docker build -t splunkminfree /home/ubuntu/gits/docker-splunk/enterprise/
  SHELL

  end
end
