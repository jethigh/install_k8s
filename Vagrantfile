IMAGE_NAME = "generic/centos7"

Vagrant.configure("2") do |config|
  config.vm.box_version = "3.5.2"

  config.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 4
  end

  config.vm.define "k8s-master" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.network "public_network", ip: "192.168.1.200"
    master.vm.hostname = "k8s-master"
  end
 
  config.vm.define "node-1" do |node|
    node.vm.box = IMAGE_NAME
    node.vm.network "public_network", ip: "192.168.1.201"
    node.vm.hostname = "node-1"
  end

  config.vm.define "node-2" do |node|
    node.vm.box = IMAGE_NAME
    node.vm.network "public_network", ip: "192.168.1.202"
    node.vm.hostname = "node-2"
  end
end
