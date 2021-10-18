# Common variables
BOOTSTRAP_PATH = "bootstrap"
CPU            = "2"
IMAGE_NAME     = "generic/ubuntu1604"
MEMORY         = "1024"

# Gluster variables
GLUSTER_NODES  = 1
GLUSTER_PREFIX = "gluster"

# Master variables
MASTER_NODES  = 1
MASTER_PREFIX = "master"

# Network variables
NETWORK = "private_network"
SUBNET  = "192.168.50"

# Worker variables
WORKER_NODES  = 2
WORKER_PREFIX = "worker"
N = 2

# Vagrant deployment
Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "kvm" do |v|
        v.memory = MEMORY
        v.cpus = CPU
    end
      
    # Master nodes
    HOST_PREFIX = MASTER_PREFIX
    (1..MASTER_NODES).each do |i|
        config.vm.define "#{HOST_PREFIX}-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + 10}"
            node.vm.hostname = "#{HOST_PREFIX}-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "kubernetes-setup/master-playbook.yml"
                #ansible.playbook = "#{BOOTSTRAP_PATH}/#{HOST_PREFIX}.yml"
                ansible.extra_vars = {
                    node_ip: "#{SUBNET}.#{i + 10}",
                }
            end
        end
    end

    # Worker nodes
    HOST_PREFIX = WORKER_PREFIX
    (1..WORKER_NODES).each do |i|
        config.vm.define "#{HOST_PREFIX}-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + 10}"
            node.vm.hostname = "#{HOST_PREFIX}-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                #ansible.playbook = "#{BOOTSTRAP_PATH}/#{HOST_PREFIX}.yml"
                ansible.extra_vars = {
                    node_ip: "#{SUBNET}.#{i + 10}",
                }
            end
        end
    end

    # Storage GlusterFS
    if ENV['STORAGE'] == "gluster"
        HOST_PREFIX = GLUSTER_PREFIX
        (1..GLUSTER_NODES).each do |i|
            config.vm.define "#{HOST_PREFIX}-#{i}" do |node|
                node.vm.box = IMAGE_NAME
                node.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + 10}"
                node.vm.hostname = "#{HOST_PREFIX}-#{i}"
                node.vm.provision "ansible" do |ansible|
                    ansible.playbook = "#{BOOTSTRAP_PATH}/#{HOST_PREFIX}.yml"
                    ansible.extra_vars = {
                        node_ip: "#{SUBNET}.#{i + 10}",
                    }
                end
            end
        end
    end
end