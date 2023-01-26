*** DESCRIPTION
This project installs Kubernetes on virtual machines provided by Virtual Box using Vagrant.  
Vagrant uses ansible provisioner to deploy Kubernetes cluster on provided VM's.  
Installed Kubernetes uses cri-o as container runtime

*** REQUIRMENTS 
To run this project You need:  
- VirtualBox  
- Vagrant  

*** HOW TO RUN  
Clone this project to your workstation and enter project directory.  
Change permission on provided keys for Ansible  
`chmod 600 keys/*`

Run vagrant to provide VM and install Kubernetes:  
`vagrant up`

