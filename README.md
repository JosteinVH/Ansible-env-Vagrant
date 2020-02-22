# Ansible-env-Vagrant
Creates a lab environment for Ansible using Vagrant.

## Requirements
* [Virtual Box](https://www.virtualbox.org/wiki/Download_Old_Builds_6_0) 6.0.14 
* [Vagrant](https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.msi) 2.2.6
* [Ansible](https://docs.ansible.com/ansible/latest/index.html) 2.9.3

## Usage
2. Generate key pair:

   ```Shell
   ssh-keygen -f Ansible-env-Vagrant
   ```

3. From repository root, run:

   ```Shell
   vagrant up
   ```