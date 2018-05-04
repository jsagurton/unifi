# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # You probably don't need this. I have clash issues with the default 10.0.2.0/24 subnet used for the NAT interface
  config.vm.provider "virtualbox" do |vb|
	  vb.customize ['modifyvm', :id, '--natnet1', '172.16.0.0/12']
	  end
  config.vm.box = "debian/stretch64"
  config.vm.define "unifi" do |unifi|
	  unifi.vm.hostname = "unifi-controller"



  # Use bridged network adapter for VM
  # http://docs.vagrantup.com/v2/networking/public_network.html
  # You probably don't need the ", use_dhcp_assigned_default_route => true" unless you need to talk across VLANs.
	  unifi.vm.network "public_network", bridge: "eth1", :use_dhcp_assigned_default_route => true
	  end

  config.vm.provision "shell", inline: <<-shell
    set -e;

    apt-get update
    apt-get install dirmngr
    apt-key adv --keyserver keyserver.ubuntu.com --recv 06E85760C0A52C50
    echo 'deb http://www.ubnt.com/downloads/unifi/debian stable ubiquiti' | tee /etc/apt/sources.list.d/100-ubnt-unifi.list
    # Modify /etc/hosts because Unifi will only bind to the IPv6 address for $reasons if there's an IPv6 defn of localhost
    head -n -3 /etc/hosts > /etc/hosts.new
    mv /etc/hosts.new /etc/hosts
    apt-get update
    apt-get upgrade -y
    apt-get install -y openjdk-8-jre-headless jsvc mongodb-server unifi
    echo 'java.net.preferIPv4Stack=true' >> /usr/lib/unifi/data/system.properties

  shell

end
