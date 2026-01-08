# Basic Network Topolgy Configurations

<img width="1089" height="596" alt="image" src="https://github.com/user-attachments/assets/86937932-3a65-4d50-b5ac-0b65b7d2c402" />

The configurations for the "Network-Automation-1" machine are as follows:
```
cat /etc/network/interfaces
```
Then uncomment "auto eth0" and "iface eth0 inet dhcp". _This allows internet access for the machine and to obtain an IP address from a DHCP server._
