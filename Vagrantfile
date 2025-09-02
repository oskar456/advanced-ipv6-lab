# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"
  # config.vm.box_version = "20210603.0.0"

  #config.vm.synced_folder ".", "/vagrant", type: "rsync",
  #  rsync__exclude: ".git/"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1", id: "web"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = "2048"
  end
  config.vm.provision "shell", reset: true, inline: <<-SHELL
     add-apt-repository -y ppa:ansible/ansible
     export DEBIAN_FRONTEND=noninteractive
     apt-get -y install ansible-core python3-packaging incus jool-dkms
     ansible-galaxy collection install community.general community.mysql ansible.posix -p /usr/share/ansible/collections
     gpasswd -a vagrant lxd
     gpasswd -a vagrant incus-admin
     rcvboxadd quicksetup all
     echo "jool" > /etc/modules-load.d/jool.conf
     echo "jool_siit" >> /etc/modules-load.d/jool.conf
  SHELL
  config.vm.provision "ansible_local" do |ansible|
    ansible.install = false
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "ansible/site.yaml"
    ansible.inventory_path = "ansible/inventory.ini"
    ansible.verbose = false
    ansible.limit = "all"
  end
end
