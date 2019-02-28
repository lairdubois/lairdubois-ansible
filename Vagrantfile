Vagrant.configure(2) do |config|

  # Debian box
  config.vm.box = "debian/stretch64"
  config.vm.define "lairdubois-vbox"

  # Don't sync anything
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Specify SSH port
  config.vm.network :forwarded_port, guest: 22, host: 3322, id: "ssh"
  config.vm.network :forwarded_port, guest: 80, host: 3380, id: "http"
  config.vm.network :forwarded_port, guest: 3443, host: 3443, id: "https"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "lairdubois-devbox"
    vb.memory = "4096"
    vb.cpus = 4
  end

  # Inject my own SSH keys for root and vagrant
  config.ssh.insert_key = false
  config.vm.provision "shell" do |s|
    ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
    s.inline = <<-SHELL
      echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
      mkdir -p /root/.ssh
      echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
    SHELL
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "lairdubois.yml"
    ansible.verbose = "v"
    ansible.limit = "all"
    ansible.inventory_path= "environments/dev"
    ansible.vault_password_file = "~/Private/ansible/lairdubois"
    ansible.groups = {
      "lairdubois" => ["lairdubois-vbox"]
    }
  end

end
