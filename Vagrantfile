# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "geerlingguy/centos7"
  config.ssh.insert_key = false
  config.vm.provider "virtualbox"

  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.cpus = 1
    v.linked_clone = true
  end

  # Define three VMs with static private IP addresses.
  boxes = [
    { :name => "node1", :ip => "192.168.33.71" },
    { :name => "node2", :ip => "192.168.33.72" },
    { :name => "node3", :ip => "192.168.33.73" },
  ]

  # Provision each of the VMs.
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:ip]
      config.vm.synced_folder ".", "/vagrant", disabled: true
    end
  end

  config.vm.define "ctrl" do |config|
    config.vm.provider :virtualbox do |v|
      v.memory = 512
      v.cpus = 1
    end
    config.vm.synced_folder ".", "/vagrant"
    config.vm.network :private_network, ip: "192.168.33.70"
    config.vm.network "forwarded_port", guest: 8123, host: 8123, auto_correct: true
    config.vm.provision "shell", inline: <<-SHELL
      #ATTENTION: ruby on windows cannot distinguish caase sensitive environment variables!
      export http_proxy=#{ENV["http_proxy"]}
      export https_proxy=#{ENV["https_proxy"]}
      export no_proxy=#{ENV["no_proxy"]}
      if [[ -z "$http_proxy" && -z "$https_proxy" ]]; then
        echo "proxy IS NOT set, doing nothing"
        rm -f /tmp/my_proxy
      else
        echo "proxy IS set, saving..."
        echo "http_proxy: http://${http_proxy#*://}" > /tmp/my_proxy
        echo "HTTP_PROXY: ${http_proxy#*://}" >> /tmp/my_proxy
        echo "https_proxy: http://${https_proxy#*://}" >> /tmp/my_proxy
        echo "HTTPS_PROXY: ${https_proxy#*://}" >> /tmp/my_proxy
        echo "NO_PROXY: ${no_proxy#*://}" >> /tmp/my_proxy
      fi
      sudo -E bash -c 'env'
      sudo -E bash -c 'yum -y --setopt=ip_resolve=4 install epel-release'
      sudo -E bash -c 'yum -y --setopt=ip_resolve=4 install python-pip'
      sudo -E bash -c 'pip install ansible'
    SHELL
    config.vm.provision "ansible_local" do |ansible|
      ansible.verbose = false
      ansible.install = false
      ansible.playbook = "vagrant.yml"
      ansible.config_file = "ansible.cfg"
      ansible.inventory_path = "inventory/vagrant.ini"
      ansible.limit = "all"
    end
  end
end
