Vagrant.configure("2") do |config| 
	config.vm.box = "bento/centos-7.5"
	config.vm.provider "virtualbox" do |vb|
		vb.gui = false
		vb.memory = 512
		vb.cpus = 2
	end

	config.trigger.before :up do |trigger|
		trigger.name = "Hello world"
		trigger.info = "I am running before vagrant up!!"
	end

	config.trigger.after :up do |trigger|
		trigger.name = "Hello world"
		trigger.info = "I am running after vagrant up!!"
	end

#config.trigger.after :up do |trigger|
#	trigger.name = "Finished Message"
#	trigger.info = "Machines is up!"
#end

config.vm.define "server1" do |server1|
	server1.vm.hostname = "server1"
	server1.vm.network "private_network", ip: "192.168.0.10"
	server1.vm.provision "shell", inline: <<-SHELL
		yum install git -y
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
	SHELL

	#пинговать после того как все машины стартанут
	#ping -c 1 -q server2
    server1.trigger.after :up do |trigger|
		trigger.info = "More information with ruby magic"
		trigger.ruby do |env,machine|
        puts `VBoxManage showvminfo #{machine.id} --machinereadable | grep ostype`
		end
    end
end

config.vm.define "server2" do |server2|
	server2.vm.hostname = "server2"
	server2.vm.network "private_network", ip: "192.168.0.11"
	server2.vm.provision "shell", inline: <<-SHELL
		#действия после установки ВМ
	SHELL

	#пинговать после того как все машины стартанут
	#ping -c 1 -q server1
    server2.trigger.after :up do |trigger|
		trigger.info = "More information with ruby magic"
		trigger.ruby do |env,machine|
        puts `VBoxManage showvminfo #{machine.id} --machinereadable | grep ostype`
		end
    end
end

config.vm.provision "shell", inline: <<-SHELL
	#проверка, если нет записи в файле /etc/hosts, то добавить или не делать ничего
	grep -q 'server[1-2]' '/etc/hosts' && echo "++++ hosts file is good" || echo "192.168.0.10 server1\n192.168.0.11 server2\n" >> /etc/hosts;
	yum install mc -y
SHELL

end
