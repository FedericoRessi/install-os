

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  config.vm.box_version = "1710.01"

  # We don't need synced folders because we fully rely on ansible for provisioning.
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "libvirt" do |vm|
#     if File.file?('.install-os/host')
#       vm.host = File.read('.install-os/host').gsub(/\s+/, "")
#       vm.username = "stack"
#       vm.connect_via_ssh = true
#     end
    vm.cpus = 1
    vm.nested = true
    vm.memory = 4096
    vm.machine_virtual_size = 100
  end

  config.vm.provider "virtualbox" do |vm|
    vm.cpus = 1
    vm.memory = 4096
  end

  config.vm.define "control" do |control|
    # management network
    control.vm.network "private_network", ip: "192.168.10.100"
    # Open httpd port to host machine for accessing to access to dashboard
    control.vm.network "forwarded_port", guest: 80, host: 8080

    control.vm.provider "libvirt" do |vm|
      vm.memory = 8192
    end
    control.vm.provider "virtualbox" do |vm|
      vm.memory = 8192
    end
  end

  config.vm.define "block" do |block|
    # management network
    block.vm.network "private_network", ip: "192.168.10.200"

    block.vm.provider "libvirt" do |vm|
      # Add block device /dev/vdb
      vm.storage :file
    end
    block.vm.provider "virtualbox" do |vm|
      if Vagrant.has_plugin?("vagrant-persistent-storage")
        # Add block device /dev/sdb
        block.persistent_storage.enabled = true
        block.persistent_storage.location = "./.install-os/block-volume.vdi"
        block.persistent_storage.size = 5000
        block.persistent_storage.partition = false
      end
    end
  end

  (1..2).each do |i|
    config.vm.define "compute#{i}" do |compute|

      # management network
      compute.vm.network "private_network", ip: "192.168.10.10#{i}"

      compute.vm.provider "libvirt" do |vm|
        vm.cpus = 1
        vm.memory = 8192
      end
      compute.vm.provider "virtualbox" do |vm|
        vm.cpus = 1
        vm.memory = 8192
      end
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "install-os.yml"
  end

end
