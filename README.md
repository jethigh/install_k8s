*** DESCRIPTION
Installs kubernetes with cri-o as container runtime using Vagrant

*** REQUIRMENTS
For modprobe
ansible-galaxy collection install community.general
For selinux
ansible-galaxy collection install ansible.posix


*** ArgoCD
Required local installation of ansible with kubernetes.core collection
On masternode kubernetes python library is installed

How to run playbook:
https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml