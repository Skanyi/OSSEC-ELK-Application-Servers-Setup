servers = {
     "ossec-server" => { :ip => "192.168.33.10", :cpus => 1, :mem => 1024 },
     "elk-server" => { :ip => "192.168.33.11", :cpus => 2, :mem => 4096 },
     "app-server" => { :ip => "192.168.33.12", :cpus => 1, :mem => 1024 }
 }
Vagrant.configure(2) do |config|
     servers.each_with_index do | (hostname, info), index|
        config.vm.define hostname do |config|
            config.vm.provider :virtualbox do |vb, override|
                config.vm.box = "ubuntu/bionic64"
		    config.vm.provision :shell, path: "bootstrap.sh"
                override.vm.network :private_network, ip: "#{info[:ip]}"
                override.vm.hostname = hostname
                vb.name = hostname
                vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus], "--hwvirtex", "on"]
            
            config.ssh.insert_key = false
            config.ssh.private_key_path = ['~/.vagrant.d/insecure_private_key',
            '~/.ssh/ossec']
            config.vm.provision "file", source: "~/.ssh/ossec.pub", destination:
            "~/.ssh/authorized_keys"
            end
         end
     end
 end
