

Vagrant.configure("2") do |config|
  
  config.vm.provider :libvirt do |libvirt|
    libvirt.host = (File.read '.install-os/host').gsub(/\s+/, "")
    libvirt.username = "stack"
    libvirt.connect_via_ssh = true
    libvirt.cpus = 2
    libvirt.nested = true
    libvirt.memory = 8192
    libvirt.machine_virtual_size = 100
  end

  config.vm.box = "centos/7"

  config.vm.define "control" do |control|
    control.vm.network "private_network", ip: "192.168.0.100"
	control.vm.network "private_network", ip: "192.168.1.100"
  end

  (1..2).each do |i|
    config.vm.define "compute#{i}" do |compute|
      compute.vm.network "private_network", ip: "192.168.0.10#{i}"
	  compute.vm.network "private_network", ip: "192.168.1.10#{i}"
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "prepare-guests.yml"
  end

end
