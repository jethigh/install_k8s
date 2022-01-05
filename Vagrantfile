IMAGE_NAME = "generic/centos7"
CENTOS1_IP = '192.168.1.200'
CENTOS2_IP = '192.168.1.201'
CENTOS3_IP = '192.168.1.202'

$script = <<-EOF
echo "$1 centos1" >> /etc/hosts
echo "$2 centos2" >> /etc/hosts
echo "$3 centos3" >> /etc/hosts
EOF

Vagrant.configure("2") do |config|
  config.vm.box_version = "3.5.2"
  config.vm.provider "virtualbox" do |v|
      v.memory = 2024
      v.cpus = 2
  end

  config.vm.define "centos1" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.network "public_network", ip: CENTOS1_IP
    master.vm.hostname = "centos1"
    master.vm.synced_folder "./", "/vagrant/"
    master.vm.provision "shell", inline: "cat /vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
  end
 
  config.vm.define "centos2" do |node|
    node.vm.box = IMAGE_NAME
    node.vm.network "public_network", ip: CENTOS2_IP
    node.vm.hostname = "centos2"
    node.vm.synced_folder "./", "/vagrant/"
    node.vm.provision "shell", inline: "cat /vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
  end

  config.vm.define "centos3" do |node|
    node.vm.box = IMAGE_NAME
    node.vm.network "public_network", ip: CENTOS3_IP
    node.vm.hostname = "centos3"
    node.vm.synced_folder "./", "/vagrant/"
    node.vm.provision "shell", inline: $script, args: [CENTOS1_IP, CENTOS2_IP, CENTOS3_IP]
    node.vm.provision "shell", inline: "cat /vagrant/keys/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
    node.vm.provision "ansible_local" do |ansible|
      ansible.inventory_path = "/vagrant/hosts"
      ansible.limit = "all"
      ansible.galaxy_role_file = "/vagrant/requirements.yml"
      ansible.galaxy_roles_path = "/etc/ansible/roles"
      ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path} --force"
      ansible.raw_arguments = ['--private-key', '/vagrant/keys/id_rsa']
      ansible.playbook = "playbook.yml"
    end
  end
end
