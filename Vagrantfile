# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "app", primary: true do |app|
    app.vm.box = "ubuntu/xenial64"

    app.vm.hostname = "financeapp.dev"

    app.vm.network "private_network", ip: "192.168.33.10"

    app.vm.provider :virtualbox do |vb|
      vb.linked_clone = false
      vb.name = "financeapp.dev"
      vb.memory = 1024
      vb.cpus = 1
      # vb.customize ['modifyvm', :id, '--ioapic', 'on']
      vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
      vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
      vb.customize ['setextradata', :id, 'VBoxInternal/Devices/VMMDev/0/Config/GetHostTimeDisabled', 0]
    end

    if Vagrant.has_plugin?('vagrant-hostsupdater')
      app.hostsupdater.remove_on_suspend = true
    end

    if Vagrant.has_plugin?('vagrant-vbguest')
      app.vbguest.auto_update = false
    end

    app.vm.provision "ansible_local" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "provision/ansible/site.yml"
    end
  end
end
