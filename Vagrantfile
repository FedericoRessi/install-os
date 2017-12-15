

Vagrant.configure("2") do |config|

  config.vm.provider "libvirt" do |vm|
    if File.file?('.install-os/host')
      vm.host = File.read('.install-os/host').gsub(/\s+/, "")
    end
    vm.username = "stack"
    vm.connect_via_ssh = true
    vm.cpus = 1
    vm.nested = true
    vm.memory = 4096
    vm.machine_virtual_size = 100
  end

  config.vm.provider "virtualbox" do |vm|
    vm.cpus = 1
    vm.memory = 4096
  end

  config.vm.box = "centos/7"

  config.vm.define "control" do |control|
    # management network
    control.vm.network "private_network", ip: "192.168.10.100"
    # provider network
	  control.vm.network "private_network", ip: "192.168.11.100"
	  control.vm.provider "libvirt" do |vm|
	    vm.memory = 8192
	  end
    config.vm.provider "virtualbox" do |vbox|
      vbox.memory = 8192
    end
  end

  config.vm.define "block" do |control|
    # management network
    control.vm.network "private_network", ip: "192.168.11.200"
  end

  (1..2).each do |i|
    config.vm.define "object#{i}" do |compute|
      # management network
      compute.vm.network "private_network", ip: "192.168.10.20#{i}"
    end
  end

  (1..2).each do |i|
    config.vm.define "compute#{i}" do |compute|
      # management network
      compute.vm.network "private_network", ip: "192.168.10.10#{i}"
      # provider network
	  compute.vm.network "private_network", ip: "192.168.11.10#{i}"
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
