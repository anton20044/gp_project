home = ENV['HOME']
ENV["LC_ALL"] = "en_US.UTF-8"

MACHINES = {
   :dns01 => {
        :box_name => "jammy",
        :vm_name => "dns01",
	:cpus => 2,
        :memory => 1024,
        :net => [
                   ["192.168.56.15", 2, "255.255.255.0", "net1"],
                   ["192.168.57.15", 3],
                ]
  },
   :pg01 => {
        :box_name => "jammy",
        :vm_name => "pg01",
	:cpus => 2,
        :memory => 1024,
        :net => [
                   ["192.168.56.2", 2, "255.255.255.0", "net1"],
                   ["192.168.57.2", 3],
                ]
  },
  :minio01 => {
        :box_name => "jammy",
        :vm_name => "minio01",
	:cpus => 2,
        :memory => 2048,
        :net => [
                   ["192.168.56.3", 2, "255.255.255.0", "net1"],
                   ["192.168.57.3", 5],
                ]
  },
  :clickhouse01 => {
        :box_name => "jammy",
        :vm_name => "clickhouse01",
	:cpus => 2,
        :memory => 2048,
        :net => [
                   ["192.168.56.4", 2, "255.255.255.0", "net1"],
                   ["192.168.57.4", 5],
                ]
  },
  :router01 => {
        :box_name => "jammy",
        :vm_name => "router01",
	:cpus => 2,
        :memory => 1024,
        :net => [
                   ["192.168.56.5", 2, "255.255.255.0", "net1"],
                   ["192.168.57.5", 5],
                ]
  },
  :gpmaster => {
        :box_name => "jammy",
        :vm_name => "gpmaster",
	:cpus => 4,
        :memory => 4096,
        :net => [
                   ["192.168.56.6", 2, "255.255.255.0", "net1"],
                   ["192.168.57.6", 5],
                ]
  },
  :gpstandby => {
        :box_name => "jammy",
        :vm_name => "gpstandby",
	:cpus => 4,
        :memory => 4096,
        :net => [
                   ["192.168.56.7", 2, "255.255.255.0", "net1"],
                   ["192.168.57.7", 5],
                ]
  },
  :gpseg01 => {
        :box_name => "jammy",
        :vm_name => "gpseg01",
	:cpus => 4,
        :memory => 4096,
        :net => [
                   ["192.168.56.8", 2, "255.255.255.0", "net1"],
                   ["192.168.57.8", 5],
                ]
  },
  :gpseg02 => {
        :box_name => "jammy",
        :vm_name => "gpseg02",
	:cpus => 4,
        :memory => 4096,
        :net => [
                   ["192.168.56.9", 2, "255.255.255.0", "net1"],
                   ["192.168.57.9", 5],
                ]
  },
  :gpseg03 => {
        :box_name => "jammy",
        :vm_name => "gpseg03",
	:cpus => 4,
        :memory => 4096,
        :net => [
                   ["192.168.56.10", 2, "255.255.255.0", "net1"],
                   ["192.168.57.10", 5],
                ]
  },
  :gpseg04 => {
        :box_name => "jammy",
        :vm_name => "gpseg04",
	:cpus => 4,
        :memory => 4096,
        :net => [
                   ["192.168.56.11", 2, "255.255.255.0", "net1"],
                   ["192.168.57.11", 5],
                ]
  },
  :log01 => {
        :box_name => "jammy",
        :vm_name => "log01",
        :cpus => 4,
        :memory => 4096,
        :net => [
                   ["192.168.56.12", 2, "255.255.255.0", "net1"],
                   ["192.168.57.12", 3],
                ]
  },
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
    
    config.vm.define boxname do |box|
   
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]

      box.vm.provider "virtualbox" do |v|
        v.memory = boxconfig[:memory]
        v.cpus = boxconfig[:cpus]
       end

       # needsController = false
       # boxconfig[:disks].each do |dname, dconf|
        #  unless File.exist?(dconf[:dfile])
        #    customize ['createhd', '--filename', dconf[:dfile], '--variant', 'Fixed', '--size', dconf[:size]]
        #      needsController =  true
        #    end
        #end
      #if needsController == true
       # vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata" ]
       # boxconfig[:disks].each do |dname, dconf|
       #   vb.customize ['storageattach', :id,  '--storagectl', 'SATA', '--port', dconf[:port], '--device', 0, '--type', 'hdd', '--medium', dconf[:dfile]]
       # end
      #end
     #end

      box.vm.synced_folder ".", "/vagrant", disabled: true

        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
          cp ~vagrant/.ssh/auth* ~root/.ssh
          sed -i 's/^PasswordAuthentication.*$/PasswordAuthentication yes/' /etc/ssh/sshd_config.d/60-cloudimg-settings.conf        
          systemctl restart sshd.service
        SHELL


      #if boxconfig[:vm_name] == "log01"
      #   box.vm.network "forwarded_port", guest: 5601, host: 5601
      #end

      #if boxconfig[:vm_name] == "hp01"
      #   box.vm.network "forwarded_port", guest: 7000, host: 7000
      #end
  
      #if boxconfig[:vm_name] == "hp02"
      #   box.vm.network "forwarded_port", guest: 7000, host: 7001
      #end

      #if boxconfig[:vm_name] == "monitor"
      #   box.vm.network "forwarded_port", guest: 3000, host: 3000
      #   box.vm.network "forwarded_port", guest: 9090, host: 9090
      #   box.vm.network "forwarded_port", guest: 9093, host: 9093
      #end

      #needsController = false
      #boxconfig[:disks].each do |dname, dconf|
	#unless File.exist?(dconf[:dfile])
        #  vb.customize ['createhd', '--filename', dconf[:dfile], '--variant', 'Fixed', '--size', dconf[:size]]
         #   needsController =  true
         # end
      #end
      #if needsController == true
       #              vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata" ]
        #             boxconfig[:disks].each do |dname, dconf|
         #                vb.customize ['storageattach', :id,  '--storagectl', 'SATA', '--port', dconf[:port], '--device', 0, '--type', 'hdd', '--medium', dconf[:dfile]]
          #           end
           #       end
      #end

      boxconfig[:net].each do |ipconf|
        box.vm.network("private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3])
      end

      if boxconfig[:vm_name] == "router01"
         box.vm.network "public_network", ip: "192.168.50.238", adapter: 3,  bridge: "br0"
      end

      if boxconfig[:vm_name] == "gpmaster" or boxconfig[:vm_name] == "gpstandby" or boxconfig[:vm_name] == "gpseg01" or boxconfig[:vm_name] == "gpseg02" or boxconfig[:vm_name] == "gpseg03" or boxconfig[:vm_name] == "gpseg04"
	box.vm.disk :disk, size: "40GB", primary: true, name: "primary"
	box.vm.disk :disk, size: "20GB", primary: false, name: "extra_storage"+boxconfig[:vm_name]
	box.vm.provision "shell", inline: <<-SHELL
          parted /dev/sdc mklabel gpt
          parted /dev/sdc mkpart primary xfs 0% 100%
	  mkdir /data1
	  mkfs.xfs /dev/sdc1
	  echo "/dev/sdc1 /data1 xfs rw,nodev,noatime,inode64 0 0" >> /etc/fstab  
	  mount -a
        SHELL
      end
      
      #if boxconfig[:vm_name] == "app02"
       #box.vm.provision "ansible" do |ansible|
        #ansible.playbook = "ansible/provision.yml"
        #ansible.inventory_path = "ansible/hosts"
        #ansible.host_key_checking = "false"
        #ansible.limit = "all"
       #end
      #end

      


     end
  end
end
