# Basic Network Topolgy Configurations

<img width="1089" height="596" alt="image" src="https://github.com/user-attachments/assets/86937932-3a65-4d50-b5ac-0b65b7d2c402" />

</br>## The configurations for the "Network-Automation-1" machine are as follows:
```
cat /etc/network/interfaces
```
Then uncomment "auto eth0" and "iface eth0 inet dhcp". _This allows internet access for the machine and to obtain an IP address from a DHCP server._
</br><img width="544" height="298" alt="image" src="https://github.com/user-attachments/assets/2df61cfc-6b9a-4c40-b27a-cbd187af3608" />

</br>## The configurations for the "Cisco-Switch" are as follows:
</br>_This commands can either entered in manually or copied and pasted all together._
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
</br>## The configurations for the "Cisco-Router" are as follows:
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
