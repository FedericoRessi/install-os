

Vagrant.configure("2") do |config|
  
  config.vm.provider :libvirt do |libvirt|
    libvirt.host = (File.read '.install-os/host').gsub(/\s+/, "")
    libvirt.username = "stack"
    libvirt.connect_via_ssh = true
    libvirt.cpus = 1
    libvirt.nested = true
    libvirt.memory = 4096
    libvirt.machine_virtual_size = 100
  end

  config.vm.box = "centos/7"

  config.vm.define "control" do |control|
    # management network
    control.vm.network "private_network", ip: "192.168.0.100"
    # provider network
	control.vm.network "private_network", ip: "192.168.1.100"
	control.vm.provider :libvirt do |libvirt|
	  libvirt.memory = 8192
	end
  end

  config.vm.define "block" do |control|
    # management network
    control.vm.network "private_network", ip: "192.168.0.200"
  end

  (1..2).each do |i|
    config.vm.define "object#{i}" do |compute|
      # management network
      compute.vm.network "private_network", ip: "192.168.0.20#{i}"
    end
  end

  (1..2).each do |i|
    config.vm.define "compute#{i}" do |compute|
      # management network
      compute.vm.network "private_network", ip: "192.168.0.10#{i}"
      # provider network
	  compute.vm.network "private_network", ip: "192.168.1.10#{i}"
      compute.vm.provider :libvirt do |libvirt|
        libvirt.cpus = 1
		libvirt.memory = 8192
	  end
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "prepare-guests.yml"
  end

end
