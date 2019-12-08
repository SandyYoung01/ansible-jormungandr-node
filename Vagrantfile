Vagrant.configure("2") do |config|

    config.ssh.insert_key = true

    config.vm.define "jormungandr-node-1" do |guest|
        guest.vm.box = "ubuntu/bionic64"
        guest.vm.hostname = "jormungandr-node-1"
        guest.vm.network "private_network", ip: "192.168.50.10"        
    end

    config.vm.define "jormungandr-node-2" do |guest|
        guest.vm.box = "ubuntu/bionic64"
        guest.vm.hostname = "jormungandr-node-2"
        guest.vm.network "private_network", ip: "192.168.50.11"
    end

end