Vagrant.configure(2) do |config|

  # Debian 8
  config.vm.box = "debian/jessie"

  # config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |vb|
  	vb.name = "lairdubois-box"
    #   # Display the VirtualBox GUI when booting the machine
    #   vb.gui = true
    #
    #   # Customize the amount of memory on the VM:
    # 1.5GB
    vb.memory = "1524"
    vb.cpus = 2
  end

end
