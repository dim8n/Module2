Vagrant.configure("2") do |config| 
	config.vm.box = "bento/centos-7.5"
	config.vm.provider "virtualbox" do |vb|
		vb.gui = false
	end

config.vm.define "server1" do |server1|
  server1.vm.hostname = "server1"
  server1.vm.network "private_network", ip: "192.168.0.10"
end

config.vm.define "server2" do |server2|
  server2.vm.hostname = "server2"
  server2.vm.network "private_network", ip: "192.168.0.11"
  server2.vm.provision "shell", inline: <<-SHELL
  yum install mc -y
  yum install git -y
  mkdir /git
  chown vagrant /git
  cd /git
  git clone http://github.com/dim8n/Module2.git -b task2 -q
  cat /git/Module2/test.txt
SHELL
end

end
