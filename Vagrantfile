# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 1.8.0"

## if you change these, change vagrant/hosts too
#
# would _dearly_ love to make this less clunkable,
# and just read vagrant/hosts directly.

hosts = [
  {:name => "db1",    :ip => "10.42.100.10", :ram => 768},
  {:name => "web1", :ip => "10.42.100.15", :ram => 768},
  {:name => "web2", :ip => "10.42.100.20", :ram => 768},
]

Vagrant.configure("2") do |config|

  # setup /etc/hosts on each vm
  # - requires the vagrant-hostmanager plugin
  config.hostmanager.enabled = true
  # set to 'false' if you don't wan't to manage _your_ /etc/hosts
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true

  hosts.each do |host|
    config.vm.define host[:name] do |c|

      c.vm.box = "box-cutter/centos68"

      # stop Vagrant 'helping'
      c.ssh.insert_key = false

      c.vm.hostname = host[:name]

      c.vm.network :private_network, ip: host[:ip], netmask: "255.255.255.0"

      c.vm.provider("virtualbox") do |vb|
        vb.memory = host[:ram]
        vb.linked_clone = true
      end

      # turn off shared folder
      c.vm.synced_folder ".", "/vagrant", :disabled => true
    end
  end
end
