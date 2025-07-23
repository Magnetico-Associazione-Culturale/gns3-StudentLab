# -*- mode: ruby -*-
# vi: set ft=ruby :

#Vagrant.configure("2") do |config|
#  # Define the box to use. The name "gns3-vm" corresponds to the name
#  # students will add the box with. The 'file' option points to the local .box file.
#  config.vm.box = "gns3-vm"
#  config.vm.box_url = "./gns3-vm.box" # Instructs Vagrant to use the local .box file
#  config.vm.box_version = "1.0.1" 
#
#
#  # SSH authentication for GNS3 VM
#  config.ssh.username = "gns3"
#  config.ssh.password = "gns3"
#  config.ssh.insert_key = false
#
#  # Forwards host port 3080 to guest port 80
#  # config.vm.network "forwarded_port", guest: 80, host: 3080, auto_correct: true
#  config.vm.network "private_network",
#    type: "hostonly",
#    ip: "192.168.56.101" # This is the static IP for the GNS3 VM on the Host-Only network.
#                        # Ensure this IP is unique and within the subnet range
#                        # of VirtualBox's default host-only adapter (usually 192.168.56.x/24).
#                        # This IP will be used by your host GNS3 GUI to connect to the VM.
#
#  
#  # Configure VirtualBox specific settings for the VM
#  config.vm.provider "virtualbox" do |vb|
#    vb.name = "GNS3_VM" # How the VM will appear in VirtualBox Manager for students
#    vb.memory = "4096"               # Allocate 4GB RAM (adjust as needed for your labs)
#    vb.cpus = "2"                    # Allocate 2 CPU cores
#    vb.gui = false                   # Run the VM headlessly (no VirtualBox GUI window)
#
#    # --- IMPORTANT ---
#    # We are NOT explicitly defining network adapters here in the Vagrantfile.
#    # Vagrant will preserve the network configuration as it was in the exported
#    # VirtualBox VM. This means:
#  end
#
#  # No config.vm.network block here to preserve existing VM network settings.
#end

Vagrant.configure("2") do |config|
  # Define your VM and give it a name (e.g., "gns3-lab-vm")
  config.vm.define "gns3-lab-vm" do |gns3_config| # 'gns3_config' is just a variable name for this VM's configuration
    gns3_config.vm.box = "gns3-vm"
    gns3_config.vm.box_url = "./gns3-vm.box"
    #gns3_config.vm.box_version = "1.0.1"

    # SSH authentication for GNS3 VM
    gns3_config.ssh.username = "gns3"
    gns3_config.ssh.password = "gns3"
    gns3_config.ssh.insert_key = false

    # Configure the Host-Only network with a static IP
    gns3_config.vm.network "private_network",
      type: "hostonly",
      ip: "192.168.56.101"


    # Configure VirtualBox specific settings for the VM
    gns3_config.vm.provider "virtualbox" do |vb|
      vb.name = "Student_Lab-GNS3-VM" # This name is what appears in VirtualBox UI
      vb.memory = "4096"
      vb.cpus = "2"
      vb.gui = false
    end
  end
end