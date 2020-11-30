IMAGE_NAME = "bento/ubuntu-18.04"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 2
    end
      
    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.50.10"
        master.vm.hostname = "k8s-master"
        config.vm.synced_folder ".", "/vagrant", owner: "root", group: "root", type: "rsync"
        # master.vm.provision "ansible" do |ansible|
        #     ansible.playbook = "kubernetes-setup/master-playbook.yml"
        #     ansible.extra_vars = {
        #         node_ip: "192.168.50.10",
        #     }
        # end
        if Vagrant.has_plugin?("vagrant-proxyconf")
            config.proxy.no_proxy = "localhost,127.0.0.1,.example.com"
        end
    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
            node.vm.hostname = "node-#{i}"
            # node.vm.provision "ansible" do |ansible|
            #     ansible.playbook = "kubernetes-setup/node-playbook.yml"
            #     ansible.extra_vars = {
            #         node_ip: "192.168.50.#{i + 10}",
            #     }
            # end
            if Vagrant.has_plugin?("vagrant-proxyconf")
                config.proxy.no_proxy = "localhost,127.0.0.1,.example.com,192.168.50.10"
            end
        end
    end
end