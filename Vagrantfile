# -*- mode: ruby -*-
# vi: ft=ruby :

PUBLIC_KEY = File.read("id_rsa.pub")
IP_RANGE = "192.168.100"
VMBOX = "centos/7"
N = 1

Vagrant.configure("2") do |config|
    # Creating ansible nodes
    (1..N).each do |i|
        config.vm.define "node#{i}" do |node|
            node.vm.box = "#{VMBOX}"
            node.vm.hostname = "node#{i}"
            node.vm.network "private_network", ip: "#{IP_RANGE}.#{10+i}"

            node.vm.provision :shell, :inline =>"
            echo 'Copying public key to VM'
            echo '#{PUBLIC_KEY}' >> .ssh/authorized_keys
            ", privileged: false
        end
    end

    # Ansible controller
   config.vm.define "controller" do |controller|
        controller.vm.box = "#{VMBOX}"
        controller.vm.hostname = "controller"
        controller.vm.network "private_network", ip: "#{IP_RANGE}.10"

        controller.vm.provision "file", source: "ansible.cfg", destination: "ansible.cfg"
        controller.vm.provision "file", source: "id_rsa", destination: ".ssh/id_rsa"
        controller.vm.provision :shell, :inline =>"
            chmod 0600 /home/vagrant/.ssh/id_rsa
            echo '#{PUBLIC_KEY}' >> .ssh/authorized_keys
            chmod -R 0600 .ssh/authorized_keys
            echo 'Host #{IP_RANGE}.*' >> .ssh/config
            echo 'StrictHostKeyChecking no' >> .ssh/config
            chmod -R 600 .ssh/config
            ", privileged: false

        # Messy method for creating ansible inventory
        File.open("hosts" ,'wb') do | f |
            f.write "[all]\n"
            (1..N).each do | i |
                f.write "#{IP_RANGE}.#{10+i}\n"
            end
            f.write "\n"
            (1..N).each do | i |
                f.write "[node#{i}]\n"
                f.write "#{IP_RANGE}.#{10+i}\n"
            end
            f.write "\n"
        end

        controller.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "playbook.yml"
        end
    end

    # Display the VirtualBox GUI when booting the machine
    config.vm.provider "virtualbox" do |vb|
        vb.gui = true
        vb.memory = "2048"
        vb.cpus = 2
    end
end
