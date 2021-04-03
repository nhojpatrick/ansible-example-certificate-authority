# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # default box name
  config.vm.box = "ubuntu/focal64"

  # do not auto upgrade on boot
  config.vm.box_check_update = false
  config.vbguest.auto_update = false
  config.vbguest.no_remote = true

  # setup custom ssh keys
  config.ssh.insert_key = false
  config.ssh.private_key_path = [
    'provisioning/.ssh/id_rsa',
    '~/.vagrant.d/insecure_private_key'
  ]
  config.vm.provision 'file', 
    source: 'provisioning/.ssh/id_rsa.pub', 
    destination: '~/.ssh/authorized_keys'
  config.vm.provision 'file',
    source: 'provisioning/.ssh/id_rsa',
    destination: '~/.ssh/id_rsa'

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.cpus = "1"
    vb.memory = "512"
    vb.customize ["modifyvm", :id, "--audio", "none"]
  end

  config.vm.define "touchdown", autostart: false do |inst|
    # ubuntu 20.04 Focal Fossa 2020-04 - eol 2025-04
    inst.vm.box = "ubuntu/focal64"
    inst.vm.boot_timeout = 1200
    inst.vm.hostname = "touchdown"
    inst.vm.network "private_network", ip: "192.168.134.199"
    inst.vm.synced_folder ".", "/vagrant"

    # automated ansible install fails so manual install
    inst.vm.provision "shell", inline: "sudo apt update"
    # 2.10+ for to ansible galaxy 'community.crypto'
    inst.vm.provision "shell", inline: "sudo add-apt-repository ppa:ansible/ansible"
    #inst.vm.provision "shell", inline: "sudo apt install -y ansible"
    inst.vm.provision "shell", inline: "sudo apt install -y ansible-base"

    # ansible setup of touchdown
    inst.vm.provision "ansible_local" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "provisioning/touchdown_setup.yaml"
    end
  end

  config.vm.define "cahost", autostart: true do |inst|
    # ubuntu 20.04 Focal Fossa 2020-04 - eol 2025-04
    inst.vm.box = "ubuntu/focal64"
    inst.vm.boot_timeout = 1200
    inst.vm.hostname = "cahost"
    inst.vm.network "private_network", ip: "192.168.134.190"
  end

end
