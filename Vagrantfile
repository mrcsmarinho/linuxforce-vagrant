# -*- mode: ruby -*-
# vi: set ft=ruby :
$script = <<-SCRIPT
 echo I am prosivion...
 date > /etc/vagrant_provisioned_at
SCRIPT

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
  config.vm.box = "geerlingguy/debian9"
  
  config.vm.define "intranet" do |intra|
    intra.vm.box = "geerlingguy/debian9"
    intra.vm.network "private_network", ip: "172.17.177.40"
    intra.vm.hostname = "intranet"
    intra.vm.provision "shell", path: "update.sh"
    intra.vm.provision "puppet" do |pup|
      pup.manifests_path = "manifests"
      pup.module_path = "modules"
      pup.manifest_file = "install.pp"
    end
    intra.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.name = "intranet"
        vb.cpus = 1
      end
  end

  config.vm.define "datacenter" do |dtc|
    dtc.vm.box = "geerlingguy/debian9"
    dtc.vm.network "private_network", ip: "172.17.177.42"
    dtc.vm.hostname = "datacenter"
    dtc.vm.provision "shell", path: "update.sh"
    dtc.vm.provision "shell", inline: $script
    dtc.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.name = "datacenter"
        vb.cpus = 1
      end
  end
  config.vm.define "gateway" do |gtw|
    gtw.vm.box = "centos/7"
    gtw.vm.network "private_network", ip: "172.17.177.43"
    gtw.vm.hostname = "gateway"
    gtw.vm.provision "shell", inline: "yum update -y && yum install epel-release -y"
    gtw.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.name = "gateway"
        vb.cpus = 1
      end
  end
  
  #config.vm.define "novointranet" do |nintra|
  #  nintra.vm.box = "novointranet"
  #end
  
 # config.vm.define "ansible" do |ans|
 #   ans.vm.network "private_network", ip: "172.17.177.46"
 #   ans.vm.box = "ansible/tower"
 #   ans.vm.hostname = "ansible"
 #   ans.vm.provision "shell", inline: "yum update -y"
 #   config.vm.provider "virtualbox" do |v|
 #     v.name = "ansible"
 #   end
 # end

  config.vm.define "controle" do |ctl|
    ctl.vm.network "private_network", ip: "172.17.177.41"
    ctl.vm.box = "ansible/tower"
    ctl.vm.hostname = "controle"
    ctl.vm.provision "shell", inline: "yum update -y"
    config.vm.provider "virtualbox" do |v|
      v.name = "controle"
#      v.memory = "2048"
      end
  end
  config.vm.define "puppet" do |pup|
    pup.vm.network "private_network", ip: "172.17.177.45"
    pup.vm.box = "geerlingguy/debian9"
    pup.vm.provision "shell", path: "update.sh"
    pup.vm.provision "puppet" do |puppet|
      puppet.manifests_path = "manifests"
      puppet.module_path = "modules"
      puppet.manifest_file = "site.pp"
    end
    pup.vm.hostname = "puppet"
    pup.vm.provider "virtualbox" do |v|
      v.name = "puppet"
    end
  end

  config.vm.define "master" do |master|
    master.vm.box = "geerlingguy/centos7"
    master.vm.network "public_network", ip: "192.168.0.150"
    master.vm.hostname = "master"
master.vm.provider "virtualbox" do |v|
  v.name = "master"
v.cpus = 2
v.memory = 2048
 end
end

config.vm.define "node1" do |node1|
        node1.vm.box = "geerlingguy/centos7"
        node1.vm.network "public_network", ip: "192.168.0.151"
        node1.vm.hostname = "node1"
  node1.vm.provider "virtualbox" do |v|
  v.name = "node1"
v.cpus = 2
v.memory = 2024
 end
end	    
config.vm.define "node2" do |node2|
        node2.vm.box = "geerlingguy/centos7"
        node2.vm.network "public_network", ip: "192.168.0.152"
        node2.vm.hostname = "node2"  
  node2.vm.provider "virtualbox" do |v|
  v.name = "node2"
v.cpus = 2
v.memory = 2024
 end
end


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
  # config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "private_network", type: "dhcp"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  #config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "./data", "/var/config", create: true, owner:"root",group:"root"
  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #

  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
