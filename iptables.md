# **Detect iptables**
## **1. Install and setting virtual machine**
## Download and install virtual machines 
>[click here](https://drive.google.com/drive/folders/1kk28EXvbF1JrZGTdLqixROhHEiMK5j8A)

### We use linux centos as iptables, Win7 for LAN, windows server 2012 for DMZ.

### Win7:
* Ipv4: 172.16.1.10
* Gateway: 172.16.1.1

### Windows server:
* Ipv4: 10.0.0.20
* Gateway: 10.0.0.1
* DNS: 8.8.8.8

### Centos:
* Adapter 1 is NAT
* Adapter 2 is vmnet2
* Adapter 1 is vmnet3
### Config eth1 and eth2 on iptables
* Setting ipv4 of eth2: 
    * Address: 172.16.1.1
    * Netmask: 255.255.255.0
* Setting ipv4 of eth3:
    * Address: 10.0.0.1
    * Netmask: 255.255.255.0

### Restart network:
>Service network restart
### Check ping: ping google.com, ping LAN, Ping DMZ
>ping google.com<br>
>ping 172.16.1.10<br>
>ping 10.0.0.20

--> Ping success

Check iptables status
>service iptables status

Drop rules of iptables
>iptables -F <br>
>iptables -t nat -F<br>
>iptables -P  INPUT DROP <br>
>iptables -P  OUTPUT DROP <br>
>iptables -P  FORWARD DROP <br>

Ping google.com, LAN, DMZ

--> ping failure

## **Case1: Allow LAN Ping to internet**
Setting rules to allow LAN ping to internet
>iptables -A FORWARD -i eth2 -o eth1  -s 172.16.1.0/24 -p icmp --icmp-type any -j ACCEPT <br>
>iptables -A FORWARD -i eth1 -o eth2  -d 172.16.1.0/24 -p icmp --icmp-type any -j ACCEPT<br>
>iptables -t nat -A POSTROUTING -o eth1 -s 172.16.1.0/24 -j SNAT --to-source 192.168.123.456 <br>

IP 192.168.123.456 is ipv4 of iptables. You can check it by 'ifconfig'

## **Case 2: Allow LAN access internet**
Centos: Allow Ping
>iptables -A OUTPUT -o eth1 -p icmp -j ACCEPT
>
>iptables -A INPUT -i eth1 -p icmp -j ACCEPT

LAN: Allow Ping
>iptables -A OUTPUT -o eth2 -p icmp -j ACCEPT
>
>iptables -A INPUT -i eth2 -p icmp -j ACCEPT

Windows server:
>iptables -A OUTPUT -o eth3 -p icmp -j ACCEPT
>
>iptables -A INPUT -i eth3 -p icmp -j ACCEPT
