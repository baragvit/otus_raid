Vagrant.configure("2") do |config|
  # Use an Ubuntu box (you can change this to any Ubuntu version or another box you prefer)
  config.vm.box = "ubuntu/bionic64"  # This is Ubuntu 18.04 LTS

  # Define the provider (in this case, VirtualBox)
  config.vm.provider "virtualbox" do |vb|
    # Create the first additional disk (10GB)
    vb.customize ["createhd", "--filename", "disk1.vdi", "--size", 10240]  # 10GB disk
    vb.customize ["storageattach", :id, "--storagectl", "SATA", "--port", 1, "--device", 0, "--type", "hdd", "--medium", "disk1.vdi"]
    
    # Create the second additional disk (20GB)
    vb.customize ["createhd", "--filename", "disk2.vdi", "--size", 20480]  # 20GB disk
    vb.customize ["storageattach", :id, "--storagectl", "SATA", "--port", 2, "--device", 0, "--type", "hdd", "--medium", "disk2.vdi"]
    
    # Optionally, you can configure other VirtualBox settings like memory and CPU
    vb.memory = "1024"  # Set memory for the VM (in MB)
    vb.cpus = 2         # Set the number of CPUs
  end

  # Configure a private network (optional, for VM networking)
  config.vm.network "private_network", type: "dhcp"

  # Set up a simple shell provisioner to format and mount the disks
  config.vm.provision "shell", inline: <<-SHELL
    # Format and mount disk1
    sudo mkfs.ext4 /dev/sdb
    sudo mkdir -p /mnt/disk1
    sudo mount /dev/sdb /mnt/disk1
    echo '/dev/sdb /mnt/disk1 ext4 defaults 0 0' | sudo tee -a /etc/fstab

    # Format and mount disk2
    sudo mkfs.ext4 /dev/sdc
    sudo mkdir -p /mnt/disk2
    sudo mount /dev/sdc /mnt/disk2
    echo '/dev/sdc /mnt/disk2 ext4 defaults 0 0' | sudo tee -a /etc/fstab
  SHELL
end

