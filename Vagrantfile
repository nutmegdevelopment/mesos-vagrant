# -*- mode: ruby -*-
# vi: set ft=ruby :

master_ip = "192.168.33.10"
slave_ip = "192.168.33.11"

mesos_version = "0.24.1-ubuntu-14.04"
marathon_version = "0.11.0-ubuntu"
chronos_version = "2.4.0-ubuntu-14.04"
zookeeper_version = "3.4.6-ubuntu"

Vagrant.configure(2) do |config|
  config.vm.box = "atomic-23-20151201"

  config.vm.define "master" do |master|
    master.vm.hostname = "master"

    master.vm.network :private_network, ip: master_ip

    # for mesos web UI.
    master.vm.network :forwarded_port, guest: 5050, host: 5050
    # for Marathon web UI
    master.vm.network :forwarded_port, guest: 8080, host: 8080
    # for Chronos web UI
    master.vm.network :forwarded_port, guest: 8081, host: 8081

    master.vm.provider :virtualbox do |v, override|
      v.customize ["modifyvm", :id, "--memory", 2048]
      v.customize ["modifyvm", :id,  "--cpus",  "2"]

      # for mesos web UI.
      override.vm.network :forwarded_port, guest: 5050, host: 5050
      # for Marathon web UI
      override.vm.network :forwarded_port, guest: 8080, host: 8080
      # for Chronos web UI
      override.vm.network :forwarded_port, guest: 8081, host: 8081
    end

    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/master.yml"
      ansible.extra_vars = {
        mesos_version: mesos_version,
        marathon_version: marathon_version,
        chronos_version: chronos_version,
        zookeeper_version: zookeeper_version,
        mesos_secret: "testing-secret",
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

    # for mesos web UI.
    slave.vm.network :forwarded_port, guest: 5051, host: 5051

    slave.vm.provider :virtualbox do |v, override|
      v.customize ["modifyvm", :id, "--memory", 2048]
      v.customize ["modifyvm", :id,  "--cpus",  "2"]
    end

    slave.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/slave.yml"
      ansible.extra_vars = {
        mesos_version: mesos_version,
        mesos_secret: "testing-secret",
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
