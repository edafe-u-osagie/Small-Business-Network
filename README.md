## Small Business Network: VLANs & Inter-VLAN Routing (Router-on-a-Stick)

📌 **Project Overview**  
This project demonstrates how to design and implement a secure, efficient, and scalable small business network using VLANs and Router-on-a-Stick for inter-VLAN communication. It includes DHCP pools for automatic IP assignment and ACLs for access control between VLANs.

✔ Segmentation with VLANs (Management, Sales, Accounts, Operations, IT/Server)  
✔ Inter-VLAN communication using router sub-interfaces  
✔ DHCP configuration for dynamic IP allocation  
✔ Access control via ACLs to protect sensitive data  

---

🖥️ **Topology (Logical)**  

     Switch2                         Switch0                       Switch1
(Accounts & Ops) (Trunk Gi0/1) --> (Mgmt & Sales) <-- (Trunk Gi0/2) (IT)
                                    (Fa0/24)          
                                       |             
                                       |            
                                       |           
                                 Router (G0/1)

**VLAN Plan**
VLAN-ID	     Department
5         	Management	
10	           IT	
15	        Operations	
20	         Accounts	
25	          Sales	


## ⚙️ **Configuration Summary**

 **Switch 1 (IT)**

!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport access vlan 10
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/2
 switchport access vlan 10
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/3
 switchport access vlan 10
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 10
 switchport mode trunk
!
!
end

**Switch 2 (Accounts & Operations)**
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport access vlan 20
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/2
 switchport access vlan 20
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/3
 switchport access vlan 20
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/4
 switchport access vlan 15
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/5
 switchport access vlan 15
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/6
 switchport access vlan 15
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
interface GigabitEthernet0/1
 switchport trunk allowed vlan 15,20
 switchport mode trunk
!
!
!
end

**Switch 0 (Management & Sales)**

!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
spanning-tree vlan 5,10,15,20,25 priority 0
!
interface FastEthernet0/1
 switchport access vlan 5
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/2
 switchport access vlan 5
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/3
 switchport access vlan 5
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/4
 switchport access vlan 25
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/5
 switchport access vlan 25
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/6
 switchport access vlan 25
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/7
 switchport access vlan 25
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/24
 switchport trunk allowed vlan 5,10,15,20,25
 switchport mode trunk
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 15,20
 switchport mode trunk
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 10
 switchport mode trunk
!
!
!
!
end

**Router**

!
!
ip dhcp pool IT
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
ip dhcp pool MGT
 network 192.168.5.0 255.255.255.0
 default-router 192.168.5.1
ip dhcp pool Operations
 network 192.168.15.0 255.255.255.0
 default-router 192.168.15.1
ip dhcp pool Accounts
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
ip dhcp pool Sales
 network 192.168.25.0 255.255.255.0
 default-router 192.168.25.1
!
!
interface GigabitEthernet0/1
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/1.5
 encapsulation dot1Q 5
 ip address 192.168.5.1 255.255.255.0
!
interface GigabitEthernet0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/1.15
 encapsulation dot1Q 15
 ip address 192.168.15.1 255.255.255.0
!
interface GigabitEthernet0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!
interface GigabitEthernet0/1.25
 encapsulation dot1Q 25
 ip address 192.168.25.1 255.255.255.0
 ip access-group 100 in

!
!
access-list 100 deny ip 192.168.25.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 100 permit ip any any
!
!
!
end

**ACL Rules Explained**
Deny: Sales (192.168.25.0) → Accounts (192.168.20.0)
Permit: All other traffic


## **Verification**
VLAN assignment: show vlan brief
Trunks verified: show interfaces trunk
DHCP working: PCs in each VLAN receive IPs automatically
Inter-VLAN communication: PCs ping across VLANs successfully
ACL verified: Sales cannot access Accounts

## **Video Attached**
Video Demo

⚡ By Edafe Urhukpeoghene Osagie
