# -*- mode: ruby -*-
# vi: set ft=ruby :

# Usage:        

VAGRANTFILE_API_VERSION = "2"

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

  # Docker sets up a NAT with  tcp dpt:32770 to:172.17.0.2:80
  # To see this do vagrant ssh and from there do:
  # sudo iptables -t nat -L -n
  config.vm.network "forwarded_port", guest: 80, host: 9050
  config.vm.network "public_network"
  config.vm.hostname = "myubuntu"

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

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
config.vm.provider "virtualbox" do |vb|
  # Display the VirtualBox GUI when booting the machine
  # vb.gui = true
  
  config.vm.provision "shell", inline: <<-SHELL
    # Install Docker
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -   
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt-get update -y
    sudo apt-get install -y docker-ce
    sudo usermod -aG docker ubuntu

    # Install OpenShift Origin
    curl -fsSL https://github.com/openshift/origin/releases/download/v3.10.0-rc.0/openshift-origin-client-tools-v3.10.0-rc.0-c20e215-linux-64bit.tar.gz > openshift-origin-client-tools-v3.10.0-rc.0-c20e215-linux-64bit.tar.gz
    tar -xvzf openshift-origin-client-tools-v3.10.0-rc.0-c20e215-linux-64bit.tar.gz
    rm -f openshift-origin-client-tools-v3.10.0-rc.0-c20e215-linux-64bit.tar.gz
    mv openshift-origin-client-tools-v3.10.0-rc.0-c20e215-linux-64bit openshift
    sudo chown -R  ubuntu:ubuntu openshift
    if ! grep -s \"openshift\" .profile; then
      echo 'PATH=$PATH:$HOME/openshift' >> .profile
    fi

    # Setup OpenShift Insecure registries
    # Cf https://github.com/openshift/origin/issues/8997
    sudo cat << EOF > /etc/docker/daemon.json
    {
        "insecure-registries" : [ "172.30.0.0/16" ]
    }
EOF
    sudo systemctl daemon-reload
    sudo systemctl restart docker

  SHELL

  # Customize the amount of memory on the VM:
  vb.memory = "8192"
end

end
