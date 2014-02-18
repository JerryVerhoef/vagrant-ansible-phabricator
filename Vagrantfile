# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "ubuntu-server-12042-x64-vbox4210-nocm"
    config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-12042-x64-vbox4210-nocm.box"

    config.vm.provider :lxc do |lxc, override|
      override.vm.box_url = "http://bit.ly/vagrant-lxc-precise64-2013-10-23"
    end

    config.vm.network :forwarded_port, guest: 80, host: 1223

    IP_ADDRESS = "10.0.3.251"
    config.vm.provider :virtualbox do |vb, override|
      override.vm.network "private_network", :ip => IP_ADDRESS
      vb.memory = 2048
    end
    # NOTE: Vagrant LXC does not support Vagrant private networks,
    # instead we can specify the IP address directly
    config.vm.provider :lxc do |lxc, override|
      # NOTE: lxcbr0 defaults to 10.0.3.1/24
      lxc.customize 'network.ipv4', "#{IP_ADDRESS}/24"
    end

    # Enable the vagrant-cachier plugin if it has been installed. The
    # plugin will cache downloaded packages (APT, composer, gem, etc..)
    # between VM instances. Note that the plugin is not required but
    # greatly speeds up destroying and (re)creating VMs. More
    # information can be found at https://github.com/fgrehm/vagrant-cachier
    if Vagrant.has_plugin?("vagrant-cachier")
      config.cache.auto_detect = true
    end


    config.vm.provision :ansible do |ansible|
      ansible.playbook = "provisioning/phabricator.yml"
      ansible.inventory_path = "provisioning/hosts_vagrant"
      ansible.sudo = true
      # debug is on
      ansible.verbose = "vvvv"
    end


end
