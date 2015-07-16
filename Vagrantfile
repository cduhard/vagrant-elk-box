# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # set to false, if you do NOT want to check the correct VirtualBox Guest Additions version when booting this box
  if defined?(VagrantVbguest::Middleware)
    config.vbguest.auto_update = true
  end
  config.vm.box = "ericmann/trusty64"
  config.vm.synced_folder "../../scratch/logs", "/var/log/pss"
  config.vm.network :forwarded_port, guest: 5601, host: 5601
  config.vm.network :forwarded_port, guest: 9200, host: 9200 #elasticsearch
  config.vm.network :forwarded_port, guest: 9300, host: 9300
  config.vm.network :forwarded_port, guest: 5672, host: 5672 #rabbitmq
  config.vm.network :forwarded_port, guest: 15672, host: 15672 # rabbitmq management

  config.vm.provider :hyperv do |hv|
      hv.memory = "2048"
      hv.cpus = "2"
  end

  config.vm.provider :virtualbox do |vb|
      vb.box = "puppetlabs/ubuntu-14.04-64-puppet"
      vb.customize ["modifyvm", :id, "--cpus", "2", "--memory", "2048"]
  end

  config.vm.provider "vmware_fusion" do |v, override|
     ## the puppetlabs ubuntu 14-04 image might work on vmware, not tested?
    v.provision "shell", path: 'ubuntu.sh'
    v.box = "phusion/ubuntu-14.04-amd64"
    v.vmx["numvcpus"] = "2"
    v.vmx["memsize"] = "2048"
  end
  config.vm.provision "shell", path: 'ubuntu.sh'
  config.vm.provision "shell", path: 'setup.sh'
  config.vm.provision "puppet", manifests_path: "manifests", manifest_file: "default.pp"
end
