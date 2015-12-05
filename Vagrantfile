Vagrant.configure(2) do |config|

### SCRIPTS ###
$config_hosts_file=<<SCRIPTHOSTS

  echo "192.168.10.101     anode" >> /etc/hosts
  echo "192.168.11.102     bnode" >> /etc/hosts
  echo "192.168.11.2     cnode" >> /etc/hosts
  echo "192.168.11.103     dnode" >> /etc/hosts
  echo "192.168.10.2     enode" >> /etc/hosts

SCRIPTHOSTS
 
$config_test=<<SCRIPTTEST
 
  echo "echo TESTS  \$\(hostname\) \> \/tmp\/test\.out" > /tmp/test.sh 
  echo "ping \-c 2 anode \>> \/tmp\/test\.out" >> /tmp/test.sh
  echo "ping \-c 2 bnode \>> \/tmp\/test\.out" >> /tmp/test.sh
  echo "ping \-c 2 cnode \>> \/tmp\/test\.out" >> /tmp/test.sh
  echo "ping \-c 2 dnode \>> \/tmp\/test\.out" >> /tmp/test.sh
  echo "ping \-c 2 enode \>> \/tmp\/test\.out" >> /tmp/test.sh
  echo "echo INTERNET TEST \>> \/tmp\/test\.out" >> /tmp/test.sh
  echo "ping \-c 2 google.com \>> \/tmp\/test\.out" >> /tmp/test.sh
  echo "echo END TESTS \>> \/tmp\/test\.out" >> /tmp/test.sh

  chmod +x /tmp/test.sh
  
SCRIPTTEST


$config_script_A=<<SCRIPTA
  yum install traceroute -y
######TEST 1
  ifconfig eth1 192.168.10.101 netmask 255.255.255.0 up && echo "OK IFCONFIG"
  echo "nameserver 192.168.10.2" > /etc/resolv.conf
  ip route replace default via 192.168.10.2 dev eth1  
###### 
######TEST 2
#  ifconfig eth1 192.168.11.101 netmask 255.255.255.0 up && echo "OK IFCONFIG"
#  echo "nameserver 192.168.11.2" > /etc/resolv.conf
#  ip route replace default via 192.168.11.2 dev eth1
###### 
SCRIPTA

$config_script_B=<<SCRIPTB
  yum install traceroute -y

  ifconfig eth1 192.168.11.102 netmask 255.255.255.0 up && echo "OK IFCONFIG"

  echo "nameserver 192.168.11.2" > /etc/resolv.conf
 
  ip route replace default via 192.168.11.2 dev eth1
  
SCRIPTB


$config_script_C=<<SCRIPTC
  yum install traceroute dnsmasq -y

  ifconfig eth1 192.168.11.2 netmask 255.255.255.0 up && echo "OK IFCONFIG"

  echo "nameserver 8.8.8.8" >> /etc/resolv.conf

  echo "net.ipv4.conf.default.forwarding=1" > /etc/sysctl.conf
  echo 1 > /proc/sys/net/ipv4/ip_forward
  sysctl -p && echo "OK SYSCTL"
  service dnsmasq restart

  service iptables stop
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
  iptables -A FORWARD -d 192.168.10.0/24 -j DROP
  service iptables save
  service iptables start
 	 
SCRIPTC

$config_script_D=<<SCRIPTD
  yum install traceroute -y

  ifconfig eth1 192.168.11.103 netmask 255.255.255.0 up && echo "OK IFCONFIG"

  echo "nameserver 192.168.11.2" > /etc/resolv.conf
 
  ip route replace default via 192.168.11.2 dev eth1
  
SCRIPTD


$config_script_E=<<SCRIPTE
  yum install traceroute dnsmasq -y

  ifconfig eth1 192.168.10.2 netmask 255.255.255.0 up && echo "OK IFCONFIG"

  echo "nameserver 8.8.8.8" >> /etc/resolv.conf

  echo "net.ipv4.conf.default.forwarding=1" > /etc/sysctl.conf
  echo 1 > /proc/sys/net/ipv4/ip_forward
  sysctl -p && echo "OK SYSCTL"
  service dnsmasq restart

  service iptables stop
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
  iptables -A FORWARD -d 192.168.11.0/24 -j DROP
  service iptables save
  service iptables start
 	 
SCRIPTE


### END SCRIPTS  ####


### NODES ###
# "A" node
  
  config.vm.define "anode" do |anode|
    anode.vm.hostname = "anode"
	anode.vm.box = "puphpet/centos65-x64"
    anode.vm.network "private_network",ip: "192.168.10.101"

  	anode.vm.provision "shell", inline: $config_test	
  	anode.vm.provision "shell", inline: $config_script_A  
  	anode.vm.provision "shell", inline: $config_hosts_file  
	
  end
  
  # "B" node

  config.vm.define "bnode" do |bnode|
    bnode.vm.hostname = "bnode"
	bnode.vm.box = "puphpet/centos65-x64"
	
    bnode.vm.network "private_network", ip: "192.168.11.102"
	
    bnode.vm.provision "shell", inline: $config_script_B  
    bnode.vm.provision "shell", inline: $config_hosts_file  

  end
  
  # "C" node (Intranet Gateway)

  config.vm.define "cnode" do |cnode|
    cnode.vm.hostname = "cnode"
	cnode.vm.box = "puphpet/centos65-x64"
	
    cnode.vm.network "private_network", ip: "192.168.11.2"
	
    cnode.vm.provision "shell", inline: $config_script_C    
    cnode.vm.provision "shell", inline: $config_hosts_file    

  end
  
  # "D" node

  config.vm.define "dnode" do |dnode|
    dnode.vm.hostname = "dnode"
	dnode.vm.box = "puphpet/centos65-x64"
	
    dnode.vm.network "private_network", ip: "192.168.11.103"
	
    dnode.vm.provision "shell", inline: $config_script_D 
    dnode.vm.provision "shell", inline: $config_hosts_file    

  end
  
  # "E" node (Guest Gateway)

  config.vm.define "enode", primary: true do |enode|
    enode.vm.hostname = "enode"
	enode.vm.box = "puphpet/centos65-x64"
	
    enode.vm.network "private_network", ip: "192.168.10.2"
    
	enode.vm.provision "shell", inline: $config_script_E
	enode.vm.provision "shell", inline: $config_hosts_file
	
	
  end
  
end
