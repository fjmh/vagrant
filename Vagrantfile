# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

config.vm.define :tomcat do |tomcat|
  # the VM will display the name 'default' if there isn't a name defined. 'config' is the default param syntax instead of 'tomcat'
  tomcat.vm.box = "centos/admiral"
  tomcat.vm.box_check_update = false
  tomcat.vm.hostname = "centos.example.com"
  
  # accessing "localhost:8080" will access port 80 on the guest machine.
  tomcat.vm.network "forwarded_port", guest: 80, host: 8080   # ---> guest additions need to be installed in the box.
  
  # a private network will be created, which allows host-only access to the machine using a specific IP.
  tomcat.vm.network "private_network", ip: "10.90.90.5"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  tomcat.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is the path on the host to the actual folder.
  # The second argument is the path on the guest to mount the folder.
  # And the optional third argument is a set of non-required options.
  #config.vm.synced_folder ".", "/vagrant", type: "virtualbox"   # ---> guest additions need to be installed in the box.

  # Disable rsync for the synced_folder
  tomcat.vm.synced_folder ".", "/vagrant", disabled: true

  # Provider-specific configuration 
  tomcat.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
      vb.memory = "2048"   # ---> because of java and tomcat
  end
  # View the documentation for the provider you're using for more information on available options.


  # Enable provisioning with Puppet stand alone. Puppet manifests are contained in a directory path relative to this Vagrantfile.
  # You will need to create the manifests directory and a manifest in the file default.pp in the manifests_path directory.
  #
  # config.vm.provision "puppet" do |puppet|
  #   puppet.manifests_path = "manifests"
  #   puppet.manifest_file  = "default.pp"
  # end


  # Enable provisioning with a shell script. Additional provisioners such as Puppet, Chef, Ansible, Salt, and Docker are also available.
  # Please see the documentation for more information about their specific syntax and use.
  tomcat.vm.provision "shell", inline: <<-SHELL

   echo "-------------------------------------------------------------------------------"
   echo "==> System packages update and apache installation <=="
   echo "-------------------------------------------------------------------------------"
   yum update -y
   yum install -y httpd
   echo "-------------------------------------------------------------------------------"
   echo "==> Enabling the apache service"
   echo "-------------------------------------------------------------------------------"
   systemctl enable httpd.service
   systemctl start httpd.service
   echo " "
   echo "-------------------------------------------------------------------------------"
   echo " "
   echo "==> Done! Go to: "
   echo "http://10.90.90.5:8080 and check your HTTPD apache installation <=="
   echo " "
   echo "-------------------------------------------------------------------------------"
   SHELL
  #config.vm.provision :ansible do |ansible|
  #  ansible.playbook = "cproc_centos.yml/site.yml"
     # ---> https://adamcod.es/2014/09/23/vagrant-ansible-quickstart-tutorial.html
     # ---> https://www.vagrantup.com/docs/provisioning/ansible.html
  #end
end
end

cat << EOF > ansible_hosts
[servers_group]
cproc_centos ansible_ssh_host=10.0.90.5
EOF

cat << EOF > cproc_prov.yaml
---
- hosts: cproc_centos
  sudo: yes

EOF

Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
