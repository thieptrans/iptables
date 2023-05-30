# Detect iptables
## 1. Install and setting virtual machine
## Download and install virtual machines 
>[click here](https://drive.google.com/drive/folders/1kk28EXvbF1JrZGTdLqixROhHEiMK5j8A)
<p>

### We use linux centos as iptables, Win7 for LAN, windows server 2012 for DMZ.

### Win7:
* Ipv4: 172.16.1.10
* Gateway: 172.16.1.1

### Windows server:
* Ipv4: 10.0.0.20
* Gateway: 10.0.0.1

### Centos:
* Adapter 1 is NAT
* Adapter 2 is vmnet2
* Adapter 1 is vmnet3
## Config eth1 and eth2 on iptables
* Setting ipv4 of eth2: 
    * Address: 172.16.1.1
    * Netmask: 255.255.255.0
* Setting ipv4 of eth3:
    * Address: 10.0.0.1
    * Netmask: 255.255.255.0
<p>

### Restart network:
>Service network restart
### check ping: ping google.com, ping LAN, Ping server
>ping google.com

>ping 172.16.1.10

>ping 10.0.0.20

