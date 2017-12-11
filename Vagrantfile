

Vagrant.configure("2") do |config|
  
  config.vm.provider :libvirt do |libvirt|
    libvirt.host = (File.read '.install-os/host').gsub(/\s+/, "")
    libvirt.username = "stack"
    libvirt.connect_via_ssh = true
    # libvirt.storage_pool_name = "images"
    libvirt.nested = true
  end

  config.vm.define :control do |control|
    control.vm.box = "centos/7"
	control.vm.provision "ansible" do |ansible|
	  ansible.playbook = "prepare-control.yml"
	end
  end

  config.vm.define :compute do |compute|
    compute.vm.box = "centos/7"
	compute.vm.provision "ansible" do |ansible|
	  ansible.playbook = "prepare-compute.yml"
	end
  end

end
