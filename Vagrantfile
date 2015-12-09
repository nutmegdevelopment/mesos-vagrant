# -*- mode: ruby -*-
# vi: set ft=ruby :

private_ip = "192.168.33.10"

Vagrant.configure(2) do |config|
  config.vm.box = "atomic-23-20151201"

  config.vm.provider :virtualbox do |vb, override|

  vb.customize ["modifyvm", :id, "--memory", "#{1024*2}"]
  vb.customize ["modifyvm", :id,  "--cpus",  "2"]

    
    override.vm.network :private_network, ip: private_ip
    override.vm.provision :hosts do |provisioner|
      provisioner.add_host private_ip , [ config.vm.hostname ]
    end

    # for mesos web UI.
    override.vm.network :forwarded_port, guest: 5050, host: 5050
    # for Marathon web UI
    override.vm.network :forwarded_port, guest: 8080, host: 8080
    # for Chronos web UI
    override.vm.network :forwarded_port, guest: 8081, host: 8081


  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.extra_vars = {
      mesos_version: "0.24.1-ubuntu-14.04",
      marathon_version: "0.11.0-ubuntu",
      chronos_version: "2.4.0-ubuntu-14.04",
      zookeeper_version: "3.4.6-ubuntu",

      mesos_secret: "testing-secret",
      ipaddr: private_ip,
      ansible: {
        sudo: true,
        sudo_user: "fedora"
      }
    }
  end
end
