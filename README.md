# Project-VM-Ansible
Project with Vagrant VM for Ansible control node managing 2 WEB servers and 1 DB server

1. There are few slightly differences when using AWS vs VM for key-profile and AWS-SecurityGroup setngs. 
2. The contents are verified and this project data are based on VM settings with Virtualbox. The provided Vagrantfile defined the control node, 2 web server and 1 db server used for the project.
3. The Vagrantfile contains the settings for control, web01, web02 and db01.
$ cat Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.define "control" do |control|
        control.vm.box = "bento/ubuntu-22.04"
        control.vm.hostname = "control"
        control.vm.network "private_network", ip: "192.168.10.2"
        #config.vm.provision "shell", inline: "ifup enp0s8", run: "always"
  end

  config.vm.define "web01" do |web01|
        web01.vm.box = "eurolinurx-vagrant/centos-stream-9"
        #web01.vm.box = "centos/7"
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

4. Follow the specific host OS to setup the control node with ansible packages: this project the control node is based on Ubuntu whose official installation link is here or https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu

$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible

5. use 'systemctl status ansible' to confirm the status of installation.


6. The following files will be used for this project: 
inventory: this is the file describes the hosts to be engaged and related grouping. 
web-db.yaml : the playbook in yaml describes the package to be distrubted.

***
Vagrant VM for ansible

/etc/ansible/hosts – Default inventory file

/etc/ansible/ansible.cfg – Config file, used if present

~/.ansible.cfg – User config file, overrides the default config if present


example of cli

ansible-playbook -i inventory web-db.yaml
---
- name: Webserver setup
  hosts: webservers
  become: yes
  tasks:
    - name: Install httpd
      ansible.builtin.yum:
        name: httpd
        state: present

    - name: Start service
      ansible.builtin.service:
        name: httpd
        state: started
        enabled: yes

- name: DBServer setup
  hosts: dbservers
  become: yes
  tasks:
    - name: Install mariadb-server
      ansible.builtin.yum:
        name: mariadb-server
        state: present
    - name: Start mariadb-server
      ansible.builtin.service:
        name: mariadb
        enabled: yes

ubuntu@ip-172-31-32-89:~/exercise5$ ansible-playbook -i inventory web-db.yaml
*** cicd class
PLAY [Webserver setup] *********************************************************

TASK [Gathering Facts] *********************************************************
ok: [web02]
ok: [web01]

TASK [Install httpd] ***********************************************************
changed: [web01]
changed: [web02]

TASK [Start service] ***********************************************************
changed: [web01]
changed: [web02]

PLAY [DBServer setup] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [db01]

TASK [Install mariadb-sever] ***************************************************
changed: [db01]

PLAY RECAP *********************************************************************
db01                       : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
web01                      : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
web02                      : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0


