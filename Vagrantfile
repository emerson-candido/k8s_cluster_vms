ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'
# Common variables
BOOTSTRAP_PATH = "bootstrap"
CPU            = "2"
IMAGE_NAME     = "generic/ubuntu1604"
MEMORY         = "2048"

# Disks
DISK_DEVICE = "vdb"
DISK_FS     = "ext4"
DISK_NAME   = "data"
DISK_SIZE   = "20"

# Gluster variables
GLUSTER_DEPLOY    = "yes"
GLUSTER_DIRECTORY = "/data"
GLUSTER_NODES     = 3
GLUSTER_PREFIX    = "gluster"
GLUSTER_RANGE     = 30

# Longhorn variables
LONGHORN_DEPLOY    = "yes"
LONGHORN_DIRECTORY = "/var/lib/longhorn"

# Master variables
MASTER_NODES  = 1
MASTER_PREFIX = "master"
MASTER_RANGE  = 10

# Network variables
NETWORK  = "private_network"
SUBNET   = "192.168.50"
POD_CIDR = "10.244.0.0/16"
SVC_CIDR = "10.96.0.0/16"

# Worker variables
WORKER_NODES  = 3
WORKER_PREFIX = "worker"
WORKER_RANGE  = 20
        
# Vagrant deployment
Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider :libvirt do |libvirt|
        libvirt.memory = MEMORY
        libvirt.cpus = CPU
        libvirt.driver = "kvm"
        libvirt.storage :file, :size => "#{DISK_SIZE}"
    end
      
    # Master nodes
    (1..MASTER_NODES).each do |i|
        config.vm.define "#{MASTER_PREFIX}-#{i}" do |master|
            master.vm.box = IMAGE_NAME
            master.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + 10}"
            master.vm.hostname = "#{MASTER_PREFIX}-#{i}"
            master.vm.provision "ansible" do |ansible|
                ansible.playbook = "#{BOOTSTRAP_PATH}/#{MASTER_PREFIX}.yml"
                ansible.extra_vars = {
                    node_count: "#{i}",
                    node_ip: "#{SUBNET}.#{i + 10}",
                    node_prefix: "#{MASTER_PREFIX}",
                    node_range: "#{MASTER_RANGE}",
                    node_subnet: "#{SUBNET}",
                    pod_cidr: "#{POD_CIDR}",
                    svc_cidr: "#{SVC_CIDR}",
                }
            end
        end
    end

    ## Worker nodes
    #(1..WORKER_NODES).each do |i|
    #    config.vm.define "#{WORKER_PREFIX}-#{i}" do |worker|
    #        worker.vm.box = IMAGE_NAME
    #        worker.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + 20}"
    #        worker.vm.hostname = "#{WORKER_PREFIX}-#{i}"
    #        worker.vm.provision "ansible" do |ansible|
    #            ansible.playbook = "#{BOOTSTRAP_PATH}/#{WORKER_PREFIX}.yml"
    #            ansible.extra_vars = {
    #                disk_device: "#{DISK_DEVICE}",
    #                disk_directory: "#{LONGHORN_DIRECTORY}",
    #                disk_fs: "#{DISK_FS}",
    #                disk_name: "#{DISK_NAME}",
    #                disk_size: "#{DISK_SIZE}",
    #                longhorn_deploy: "#{LONGHORN_DEPLOY}",
    #                node_count: "#{i}",
    #                master_ip: "#{SUBNET}.11",
    #                node_ip: "#{SUBNET}.#{i + 20}",
    #                node_numbers: "#{WORKER_NODES}",
    #                node_prefix: "#{WORKER_PREFIX}",
    #                node_range: "#{WORKER_RANGE}",
    #                node_subnet: "#{SUBNET}",
    #            }
    #        end
    #    end
    #end

    ### Storage GlusterFS
    #if GLUSTER_DEPLOY == "yes"
    #    (1..GLUSTER_NODES).each do |i|
    #        config.vm.define "#{GLUSTER_PREFIX}-#{i}" do |gluster|
    #            gluster.vm.box = IMAGE_NAME
    #            gluster.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + 30}"
    #            gluster.vm.hostname = "#{GLUSTER_PREFIX}-#{i}"
    #            gluster.vm.provision "ansible" do |ansible|
    #                ansible.playbook = "#{BOOTSTRAP_PATH}/#{GLUSTER_PREFIX}.yml"
    #                ansible.extra_vars = {
    #                    disk_device: "#{DISK_DEVICE}",
    #                    disk_directory: "#{GLUSTER_DIRECTORY}",
    #                    disk_fs: "#{DISK_FS}",
    #                    disk_name: "#{DISK_NAME}",
    #                    disk_size: "#{DISK_SIZE}",
    #                    gluster_directory: "#{GLUSTER_DIRECTORY}",
    #                    node_count: "#{i}",
    #                    node_ip: "#{SUBNET}.#{i + 30}",
    #                    node_numbers: "#{GLUSTER_NODES}",
    #                    node_prefix: "#{GLUSTER_PREFIX}",
    #                    node_range: "#{GLUSTER_RANGE}",
    #                    node_subnet: "#{SUBNET}",
    #                }
    #            end
    #        end
    #    end
    #end
end