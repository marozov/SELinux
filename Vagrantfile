# -*- mode: ruby -*-
# vi: set ft=ruby :
MACHINES = {
	:selinux => {
		:box_name => "centos/7",
		:cpus => 1,
		:memory => 1024,
		#:ip_addr => '192.168.11.150',
		#:path => './******.sh'
	},
}

Vagrant.configure("2") do |config|
	config.vm.boot_timeout = 1200
	MACHINES.each do |boxname, boxconfig|

		config.vm.define boxname do |box|
			box.vm.host_name = boxname.to_s
			box.vm.box = boxconfig[:box_name]
			box.vm.host_name = boxname.to_s
			#box.vm.network "private_network", ip: boxconfig[:ip_addr]

			box.vm.provider :virtualbox do |vb|
				vb.memory = boxconfig[:memory]
				vb.cpus = boxconfig[:cpus]
			end

		#box.vm.provision	:shell,	:path => boxconfig[:path]
		
		config.vm.provision "shell", inline: <<-SHELL
			yum install -y epel-release
			yum update -y
			yum install -y nano
			yum upgrade -y
			yum install -y policycoreutils-python setroubleshoot
		SHELL
			end
	end
end
