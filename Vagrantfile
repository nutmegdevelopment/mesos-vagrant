# -*- mode: ruby -*-
# vi: set ft=ruby :

master_ip = "192.168.33.10"
slave_ip = "192.168.33.11"

mesos_version = "0.24.1-centos-7"
marathon_version = "0.11.0-centos"
chronos_version = "2.4.0-centos-7"
zookeeper_version = "3.4.6-centos"
mesos_secret = "testing-secret"

Vagrant.configure(2) do |config|
  config.vm.box = "atomic-23-20151201"

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.enable :docker
  end

  config.vm.define "master" do |master|
    master.vm.hostname = "master"

    master.vm.network :private_network, ip: master_ip

    master.vm.provider :virtualbox do |v, override|
      v.customize ["modifyvm", :id, "--memory", 2048]
      v.customize ["modifyvm", :id,  "--cpus",  "2"]
    end

    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/master.yml"
      ansible.extra_vars = {
        mesos_version: mesos_version,
        marathon_version: marathon_version,
        chronos_version: chronos_version,
        zookeeper_version: zookeeper_version,
        mesos_secret: mesos_secret,
        ipaddr: master_ip,
        ansible: {
          sudo: true,
          sudo_user: "fedora"
        }
      }
    end
  end

  config.vm.define "slave" do |slave|
    slave.vm.hostname = "slave"

    slave.vm.network :private_network, ip: slave_ip

    slave.vm.provider :virtualbox do |v, override|
      v.customize ["modifyvm", :id, "--memory", 2048]
      v.customize ["modifyvm", :id,  "--cpus",  "2"]
    end

    slave.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/slave.yml"
      ansible.extra_vars = {
        mesos_version: mesos_version,
        mesos_secret: mesos_secret,
        ipaddr: slave_ip,
        master_addr: master_ip,
        ansible: {
          sudo: true,
          sudo_user: "fedora"
        }
      }
    end
  end

end
