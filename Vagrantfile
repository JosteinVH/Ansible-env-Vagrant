Vagrant.configure("2") do |config|
    config.vm.define "controller" do |controller|
        controller.vm.box = "centos/7"
        controller.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "playbook.yml"
        end
    end

   config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     vb.gui = true
     # Customize the amount of memory on the VM:
     vb.memory = "1024"
   end
end
