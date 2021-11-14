# ####################################################################
# ################### CONFIGURATION VARIABLES ########################
# ####################################################################
IMAGE_NAME = "bento/ubuntu-21.04"   	# Image to use
NAME="nesting-vm"                	# Worker prefix node name 
MEM = 4096                   		# Amount of RAM
CPU = 4                   		# Number of CPUs 
IP = "192.168.56.10"           	# IP of Nesting VM



# ################### NESTING VM CREATION ####################
# ############################################################
Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    # RAM and CPU config
    config.vm.provider "virtualbox" do |v|
        v.memory = MEM
        v.cpus = CPU
        #TEnable Virtualization to allow Vm inside VM
        v.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
    end


    # Master node config
    config.vm.define NAME do |nestingvm|
        
        # Hostname and network config
        nestingvm.vm.box = IMAGE_NAME
        nestingvm.vm.network "private_network", ip: "#{IP}"
        nestingvm.vm.hostname = NAME


	nestingvm.vm.provision "shell", inline: <<-SHELL
		sudo apt update
		sudo apt install python3-pip -y
		sudo pip install -U ansible
		sudo apt-get install dkms build-essential linux-headers-`uname -r` -y	
		sudo apt-get install virtualbox vagrant -y
		cd /vagrant/nested
		sudo vagrant up

	SHELL

      
    end
end


