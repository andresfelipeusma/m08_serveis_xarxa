# Practica de classe.

## **1.-**  
a) ip addr add 172.16.0.20/16 dev enp5s0  
   ip addr add 172.16.0.19/16 dev enp5s0  
b) ping 172.16.0.19 per comprovar que es veuen  
## **2.-**  
Primer fer un arp -a per coneixer la ip de l'altre dispositiu  
[root@i20 ~]# arp -a   
? (172.16.0.19) at 40:8d:5c:e4:37:17 [ether] on enp5s0  

gandhi.informatica.escoladeltreball.org (192.168.0.10) at 60:a4:4c:b1:f4:ed [ether] on enp5s0  

gw.informatica.escoladeltreball.org (192.168.0.1) at 60:a4:4c:b1:f4:ed [ether] on enp5s0  

madiba.informatica.escoladeltreball.org (192.168.0.11) at 60:a4:4c:b1:f4:ed [ether] on enp5s0  

Després fer un arp amb la ip del dispositiu del que volem saber la MAC  

[root@i20 ~]# arp 172.16.0.19  
Address                  HWtype  HWaddress           Flags Mask            Iface  
172.16.0.19              ether   40:8d:5c:e4:37:17   C                     enp5s0  

## **3.-**  
Per poder cambiar el nom de la interficie utilitzarem les comandes:  
[root@i20 ~]# ip link set enp5s0 down  
[root@i20 ~]# ip link set name MYNETWORK enp5s0  
[root@i20 ~]# ip link set MYNETWORK up  

## **4.-**  
91.35 inbound  
91.43 outbound  

## **5.-**  
[root@i19 tmp]# ip link set enp5s0 mtu 9000  
483.21 inbound  
483.24 outbound  
La velocitat ha augmentat considerablement.  

## **6.-**  
[root@i20 ~]# nmcli con add con-name Practica ifname enp5s0 type ethernet ip4 172.16.0.20 gw4 192.168.1.10  
Connection 'enp5s0' (c4dad2a5-4fdd-4908-bf40-d3fb307262cf) successfully added.  
[root@localhost network-scripts]# nmcli conn up Practica  
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/1)  

## **7.-**  
[root@localhost tmp]# cat /etc/resolv.conf  
Generated by NetworkManager  
search informatica.escoladeltreball.org  
nameserver 192.168.0.10  
nameserver 10.1.1.200  
nameserver 193.152.63.197  
 
## **8.-**  
Fitxer /etc/sysconfig/network-scripts/ifcfg-enp5s0  
TYPE=Ethernet  
BOOTPROTO=none  
IPADDR=172.16.0.20  
PREFIX=32  
GATEWAY=192.168.1.10  
DEFROUTE=yes  
IPV4_FAILURE_FATAL=no  
IPV6INIT=yes  
IPV6_AUTOCONF=yes  
IPV6_DEFROUTE=yes  
IPV6_PEERDNS=yes  
IPV6_PEERROUTES=yes  
IPV6_FAILURE_FATAL=no  
IPV6_ADDR_GEN_MODE=stable-privacy  
NAME=enp5s0  
UUID=33076ac0-4858-492d-8ad5-638a23789603  
DEVICE=eth0  
ONBOOT=yes  

Amb l'ordre:  
[root@localhost network-scripts]# nmcli conn up Practica  

## **9.-**  
Creem y posem la seguent linea al fitxer:  
/etc/udev/rules.d/70-persistent-net.rules  
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="c4dad2a5-4fdd-4908-bf40-d3fb307262cf", NAME="enp5s0"  

## **10.-**  
Trobar ip a partir de un nom de domini:  
[root@localhost ~]# nslookup gandhi  
Server:		192.168.0.10  
Address:	192.168.0.10#53  

Name:	gandhi.informatica.escoladeltreball.org  
Address: 192.168.0.10  

Trobar nom de domini a partir de una adreça ip:  
[root@localhost ~]# nslookup 192.168.0.10  
Server:		192.168.0.10  
Address:	192.168.0.10#53  

10.0.168.192.in-addr.arpa	name = gandhi.informatica.escoladeltreball.org.  
