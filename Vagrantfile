public_key = File.read("id_rsa.pub")

Vagrant.configure("2") do |config|
    config.vm.define "controller" do |controller|
        controller.vm.box = "centos/7"
        controller.vm.hostname = "controller"
        controller.vm.network "private_network", ip: "192.168.100.10"
        
        controller.vm.provision "file", source: "id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
        controller.vm.provision :shell, :inline =>"
            chmod 0600 /home/vagrant/.ssh/id_rsa
            echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys
            chmod -R 0600 /home/vagrant/.ssh/authorized_keys
            echo 'Host 192.168.*.*' >> /home/vagrant/.ssh/config
            echo 'StrictHostKeyChecking no' >> /home/vagrant/.ssh/config
            chmod -R 600 /home/vagrant/.ssh/config
            ", privileged: false

        controller.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "playbook.yml"
        end

    end

    (11..11).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = "centos/7"
            node.vm.hostname = "node-#{i}"
            node.vm.network "private_network", ip: "192.168.100.#{i}"
            node.vm.boot_timeout = 500

            node.vm.provision :shell, :inline =>"
            echo 'Copying ansible-vm public SSH Keys to the VM'
            echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys
            ", privileged: false
        end
    end

   config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     vb.gui = true
     # Customize the amount of memory on the VM:
     vb.memory = "1024"
   end
end
