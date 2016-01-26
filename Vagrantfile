# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.define "master" do |master|

    private_ip = "192.168.33.10"

    master.vm.box = "atomic-23-20160106"

    master.vm.provider :virtualbox do |vb, override|

    vb.customize ["modifyvm", :id, "--memory", "#{1024*2}"]
    vb.customize ["modifyvm", :id,  "--cpus",  "2"]

      
      override.vm.network :private_network, ip: private_ip
      override.vm.provision :hosts do |provisioner|
        provisioner.add_host private_ip , [ master.vm.hostname ]
      end

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
        mesos_version: "0.26.0",
        marathon_version: "v0.15.0-RC2",
        chronos_version: "2.4.0-ubuntu-14.04",
        zookeeper_version: "3.4.6-ubuntu",

        mesos_secret: "testing-secret",
        ipaddr: private_ip,
        node_name: "master",
        ansible: {
          sudo: true,
          sudo_user: "fedora"
        }
      }
    end

  end

  config.vm.define "slave-1" do |slave1|

    private_ip = "192.168.33.11"
    master_ip = "192.168.33.10"

    slave1.vm.box = "atomic-23-20160106"

    slave1.vm.provider :virtualbox do |vb, override|

    vb.customize ["modifyvm", :id, "--memory", "#{1024*2}"]
    vb.customize ["modifyvm", :id,  "--cpus",  "2"]

      
      override.vm.network :private_network, ip: private_ip
      override.vm.provision :hosts do |provisioner|
        provisioner.add_host private_ip , [ slave1.vm.hostname ]
      end

    end

    slave1.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/slave.yml"
      ansible.extra_vars = {
        mesos_version: "0.26.0",
        marathon_version: "v0.15.0-RC2",
        chronos_version: "2.4.0-ubuntu-14.04",
        zookeeper_version: "3.4.6-ubuntu",

        mesos_secret: "testing-secret",
        ipaddr: private_ip,
        master_addr: master_ip,
        node_name: "slave-1",
        ansible: {
          sudo: true,
          sudo_user: "fedora"
        }
      }
    end

    slave.vm.network :private_network, ip: slave_ip

    slave.vm.provider :virtualbox do |v, override|
      v.customize ["modifyvm", :id, "--memory", 2048]
      v.customize ["modifyvm", :id,  "--cpus",  "2"]
    end

  config.vm.define "slave-2" do |slave2|

    private_ip = "192.168.33.12"
    master_ip = "192.168.33.10"

    slave2.vm.box = "atomic-23-20160106"

    slave2.vm.provider :virtualbox do |vb, override|

    vb.customize ["modifyvm", :id, "--memory", "#{1024*2}"]
    vb.customize ["modifyvm", :id,  "--cpus",  "2"]

      
      override.vm.network :private_network, ip: private_ip
      override.vm.provision :hosts do |provisioner|
        provisioner.add_host private_ip , [ slave2.vm.hostname ]
      end

    end

    slave2.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/slave.yml"
      ansible.extra_vars = {
        mesos_version: "0.26.0",
        marathon_version: "v0.15.0-RC2",
        chronos_version: "2.4.0-ubuntu-14.04",
        zookeeper_version: "3.4.6-ubuntu",

        mesos_secret: "testing-secret",
        ipaddr: private_ip,
        master_addr: master_ip,
        node_name: "slave-2",
        ansible: {
          sudo: true,
          sudo_user: "fedora"
        }
      }
    end

  end

end
