IMAGE_NAME = "generic/centos8s"
MASTER_IP = '192.168.1.200'
WORKER1_IP = '192.168.1.201'
WORKER2_IP = '192.168.1.202'

$script = <<-EOF
echo "$1 master" >> /etc/hosts
echo "$2 worker1" >> /etc/hosts
echo "$3 worker2" >> /etc/hosts
EOF

Vagrant.configure("2") do |config|
  config.vm.box_version = "4.2.2"
  config.vm.provider "virtualbox" do |v|
      v.memory = 2024
      v.cpus = 2
  end

  config.vm.define "master" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.network "public_network", ip: MASTER_IP
    master.vm.hostname = "master"
    master.vm.synced_folder "./", "/vagrant"
    master.vm.provision "shell", inline: "cat /vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
  end
 
  config.vm.define "worker1" do |node|
    node.vm.box = IMAGE_NAME
    node.vm.network "public_network", ip: WORKER1_IP
    node.vm.hostname = "worker1"
    node.vm.synced_folder "./", "/vagrant"
    node.vm.provision "shell", inline: "cat /vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
  end

  config.vm.define "worker2" do |node|
    node.vm.box = IMAGE_NAME
    node.vm.network "public_network", ip: WORKER2_IP
    node.vm.hostname = "worker2"
    node.vm.synced_folder "./", "/vagrant"
    node.vm.provision "shell", inline: $script, args: [MASTER_IP, WORKER1_IP, WORKER2_IP]
    node.vm.provision "shell", inline: "cat /vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
    node.vm.provision "ansible_local" do |ansible|
      ansible.inventory_path = "/vagrant/hosts"
      ansible.limit = "all"
      ansible.galaxy_role_file = "/vagrant/requirements.yml"
      ansible.galaxy_roles_path = "/etc/ansible/roles"
      ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path} --force"
      ansible.raw_arguments = ['--private-key', '/vagrant/keys/id_rsa']
      ansible.extra_vars = { APISERVER_ADVERTISE_ADDRESS: MASTER_IP }
      ansible.playbook = "playbook.yml"
    end
  end
end
