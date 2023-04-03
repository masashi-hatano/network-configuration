# Configuring a Static IP Address: Step-by-Step Guide for Linux Users

## 1. Copy the network configuration file
To set a static IP address without editing the 01-network-manager-all.yaml network configuration file, copy it to another file and add the static IP address configuration to the new file.

This is because there is a risk that the  01-network-manager-all.yaml file may be overwritten, although this is not usually the case.

Therefore, it is generally recommended to create a separate yaml file by copying the original file. However, it is important to pay attention to the name of the new yaml file that you create.

Netplan has the following features:

- It reads all files in /etc/netplan/*.yaml
- The files are read in alphabetical order
- The settings are overwritten as they are read

In other words, the settings are read in alphabetical order and the last settings read take precedence.

Therefore, name the copied yaml file so that it comes after 01-network-manager-all.yaml in alphabetical order.

Here is an example command for copying the configuration file. In this example, the file is named 02-network-manager-all.yaml.
```
cd /etc/netplan
cp 01-network-manager-all.yaml 54-network-manager-all.yaml
```

## 2. Change the IP address to a static IP

To change the IP address to a static IP address, add the static IP address configuration to the copied network configuration file.

We will explain how to do this for both wired LAN and wireless LAN (Wi-Fi).

Static IP address for wired LAN
For wired LAN, set the static IP address as follows:
```
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    interface name:
      dhcp4: false
      dhcp6: false
      addresses: [IP address]
      gateway4: Default gateway IP address
      nameservers:
        addresses: [DNS address]
```
Enter the interface name confirmed with "ip a" for "interface name".
Specify the subnet mask in CIDR notation for the IP address to be entered in "addresses".

To set a static IP address, set the DHCP function to false.

### Check the interface name
```
ip a
```

### Check default gateway IP address
```
ip route show
```

### Check dns server address
```
nslookup www.grant.ne.jp
```

## 3. Verify if the IP is static
```
ip r
```
or 
```
nmtui
```

## References
- [Commands to Verify TCP/IP Configuration On Linux (Ubuntu)](https://www.n-study.com/en/tcp-ip/linux-tcpip-configuration/)
- [nslookup command in Linux with Examples](https://www.geeksforgeeks.org/nslookup-command-in-linux-with-examples/)
- [[Ubuntu] Setting a Static IP Address (Wireless LAN, Wi-Fi Compatible): netplan](https://office54.net/iot/linux/ubuntu-ipaddress-netplan#section2-4)
- [How to Know if IP Address is Static or Dynamic in Linux](https://linuxhint.com/know-if-ip-address-is-static-or-dynamic-in-linux/)
