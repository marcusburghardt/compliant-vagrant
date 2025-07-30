# This Vagrant file will create a Fedora VM using the libvirt provider.
Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  config.vm.provider "libvirt" do |libvirt|
    libvirt.memory = "2048"
    libvirt.cpus = 2
  end

  config.vm.define "fedora" do |fedora|
    fedora.vm.box_url  = "https://download.fedoraproject.org/pub/fedora/linux/releases/42/Cloud/x86_64/images/Fedora-Cloud-Base-Vagrant-VirtualBox-42-1.1.x86_64.vagrant.virtualbox.box"
    fedora.vm.box      = "f42-cloud-base"
    fedora.vm.hostname = "fedora42"
  end

  # --- Custom commands to small hacks ---
  config.vm.provision "shell", name: "Custom commands", inline: <<-SHELL
    # Create ansible.cfg on the guest VM to silent warning
    mkdir -p /etc/ansible
    echo "[defaults]" | sudo tee /etc/ansible/ansible.cfg > /dev/null
    echo "interpreter_python = auto_silent" | sudo tee -a /etc/ansible/ansible.cfg > /dev/null

    # Show VM memory and cpu info
    free -h
    cat /proc/cpuinfo | grep cores | tail -n1
  SHELL

  # --- PRE-DEPLOYMENT HARDENING PROVISIONER ---
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "vm_hardening.yml"
  end

  # --- PRE-DEPLOYMENT VALIDATION PROVISIONER ---
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "validate_vm_complyctl.yml"
  end
end
