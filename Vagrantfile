# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  # VirtualBox.
  config.vm.define "virtualbox" do |virtualbox|
    virtualbox.vm.hostname = "virtualbox-centos7"
    virtualbox.vm.box = "file://builds/virtualbox-centos7.box"
    virtualbox.vm.network :private_network, ip: "172.16.3.2"

    config.vm.provider :virtualbox do |v|
      v.gui = false
      v.memory = 1024
      v.cpus = 1
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
    end

#    config.vm.provision "shell", inline: "cd /vagrant/scripts/harden && chmod +x main.sh get_me_back_in.sh && ./main.sh && ./get_me_back_in.sh && ansible-playbook -i ansible/hosts ansible/fstab.yml"
  end

end
