# Basic Network Topolgy Configurations
<img width="769" height="459" alt="image" src="https://github.com/user-attachments/assets/86937932-3a65-4d50-b5ac-0b65b7d2c402" />
</br>

## "Network-Automation-1" Configurations:

</br>Run this command to access the network configuration file:
```
nano /etc/network/interfaces
```
Then uncomment "auto eth0" and "iface eth0 inet dhcp". _This allows internet access for the machine and to obtain an IP address from a DHCP server._
</br><img width="544" height="298" alt="image" src="https://github.com/user-attachments/assets/2df61cfc-6b9a-4c40-b27a-cbd187af3608" />
</br>
<u>The following commands are subject to change<u>

## "Cisco-Switch" Configurations:
</br> These commands set up the switch to be able to handle automated configuration changes by the "Network-Automation1" machine. The second set of commands enables all modes of remote access to the switch and then saves the configurations with "wr"; if you do not save your configurations, then will erase everytime you turn off GNS3.
</br>_These commands can either entered in manually or copied and pasted all together._
```
enable
conf t
host SWITCH
int vlan1
ip address 192.168.122.75 255.255.255.0 
no shut
end

conf t
username msfadmin password msfadmin
enable password msfadmin
line vty 0 4
login local
transport input all
end
wr
```
These commands configure the switch for ssh:
```
enable
conf t
username msfadmin pass msfadmin
username msfadmin priv 15

line vty 0 4
login local
transport input all

ip domain-name example.com
crypto key generate rsa
1024

end
wr
```
</br>

## "Cisco-Router" Configurations:

</br>These commands assign an ip address to the cisco router and allow it to communicate with other devices. The second set of commands configure the router for all modes of remote access.
```
conf t
host ROUTER
int g0/0
ip address 192.168.122.80  255.255.255.0
no shut
end

conf t
username msfadmin password msfadmin
line vty 0 4
login local
transport input all
end
wr
```
To configure a loopback interface:
```
enable
conf t
int loop 0
ip address 1.1.1.1 255.255.255.255
end
wr
```
These commands configure the router for ssh:
```
conf t
username msfadmin pass msfadmin
username msfadmin priv 15

line vty 0 4
login local
transport input all

ip domain-name example.com
crypto key generate rsa
1024

end
wr
```
# Python Script
</br>This script automates the creations or vlans on the switch and disables all remote access on the router except for SSH:
```
from netmiko import ConnectHandler

iosv_l2 = {
    'device_type': 'cisco_ios',
    'ip': '192.168.122.72',
    'username': 'msfadmin',
    'password': 'msfadmin'
}

iosv = {
    'device_type': 'cisco_ios',
    'ip': '192.168.122.71',
    'username': 'msfadmin',
    'password': 'msfadmin'
}

net_connect = ConnectHandler(**iosv_l2)
#output = net_connect.send_command('show ip int brief')
#print (output)

config_commands = ['int loop 0', 'ip address 1.1.1.1 255.255.255.0']
output = net_connect.send_config_set(config_commands)
print (output)

for n in range (2,4):
    print ("Creating VLAN " + str(n))
    config_commands = ['vlan ' + str(n), 'name Python_VLAN ' + str(n)]
    output = net_connect.send_config_set(config_commands)
    print (output) 

# Router commands
net_connect = ConnectHandler(**iosv)
config_commands = ['line vty 0 4', 'login local', 'transport input ssh']
output = net_connect.send_config_set(config_commands)
print (output)
```
