Vagrant.configure("2") do |config|
  config.vm.define "control" do |control| 
        control.vm.box = "bento/ubuntu-22.04" 
        control.vm.hostname = "control" 
        control.vm.network "private_network", ip: "192.168.10.2" 
        #config.vm.provision "shell", inline: "ifup enp0s8", run: "always" 
  end

  config.vm.define "web01" do |web01| 
        #web01.vm.box = "eurolinurx-vagrant/centos-stream-9"
	#modified on 6/11/2024 due to previous package is no longer available.
	web01.vm.box = "geerlingguy/centos7"
        web01.vm.hostname = "web01" 
        web01.vm.network "private_network", ip: "192.168.10.3"
        #config.vm.provision "shell", inline: "ifup enp0s8", run: "always"
        #config.vm.provision "shell", run: "always", inline: "ifconfig enp0s8 192.168.10.3 netmask  255.255.255.0 up"
  end 

  config.vm.define "web02" do |web02|
        web02.vm.box = "eurolinux-vagrant/centos-stream-9"
        web02.vm.hostname = "web02"
        web02.vm.network "private_network", ip: "192.168.10.4"
	#config.vm.provision "shell", inline: "ifup enp0s8", run: "always"
        #config.vm.provision "shell", run: "always", inline: "ifconfig enp0s8 192.168.10.4 netmask  255.255.255.0 up"
  end

  config.vm.define "db01" do |db01|
        db01.vm.box = "eurolinux-vagrant/centos-stream-9"
        db01.vm.hostname = "db01"
        db01.vm.network "private_network", ip: "192.168.10.5"
	#config.vm.provision "shell", inline: "ifup enp0s8", run: "always"
        #config.vm.provision "shell", run: "always", inline: "ifconfig enp0s8 192.168.10.5 netmask  255.255.255.0 up"
  end

  config.vm.provider "virtualbox" do |vb|
      # Display the VirtualBox GUI when booting the machine
      # vb.gui = true
      # vb.grub_menu_key = "c"

      # Customize the amount of memory on the VM:
      vb.memory = "2048"
      vb.cpus = "2"
  end
end
