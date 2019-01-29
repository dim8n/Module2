Vagrant.configure("2") do |config| 
	config.vm.box = "bento/centos-7.5"
	config.vm.provider "virtualbox" do |vb|
		vb.gui = false
		vb.memory = 512
		vb.cpus = 2
	end

config.vm.define "server1" do |server1|
	server1.vm.hostname = "server1"
	server1.vm.network "private_network", ip: "192.168.0.10"
	#server1.vm.network :forwarded_port, guest: 22, host: 2201
	server1.vm.provision "shell", inline: <<-SHELL
		yum install git -y
		#проверить, если есть каталог /git/Module2, то просто выполнить git pull
		#или просто удалить и создать заново?
		if [ -d /git/ ]; then
			echo "++++ deleting /git folder"
			rm -Rf /git/
		fi
		mkdir /git
		chown vagrant /git
		chmod -R 775 /git
		cd /git
		git clone http://github.com/dim8n/Module2.git -b task2 -q
		cd /git/Module2
		cat /git/Module2/test.txt
		#пинговать нужно после того как все машины стартанут
		#ping -c 1 server2
	SHELL
end

config.vm.define "server2" do |server2|
	server2.vm.provision "shell", inline: <<-SHELL
		#пинговать нужно после того как все машины стартанут
		#ping -c 1 server1
	SHELL
	server2.vm.hostname = "server2"
	server2.vm.network "private_network", ip: "192.168.0.11"
	#server2.vm.network :forwarded_port, guest: 22, host: 2202
end

config.vm.provision "shell", inline: <<-SHELL
	#проверка, если нет записи в файле /etc/hosts, то добавить или не делать ничего
	grep -q 'server[1-2]' '/etc/hosts' && echo "++++ hosts file is good" || echo "192.168.0.10 server1\n192.168.0.11 server2\n" > /etc/hosts;
	yum install mc -y
SHELL

end
