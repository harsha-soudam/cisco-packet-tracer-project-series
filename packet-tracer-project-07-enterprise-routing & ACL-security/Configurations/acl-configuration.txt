============================================================
Cisco Packet Tracer Project 07
Enterprise Routing & ACL Security
ACL Configuration Reference
============================================================

R1 INTERFACES
-------------

enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
exit


R1 STATIC ROUTE
---------------

ip route 192.168.20.0 255.255.255.0 10.0.0.2


R1 EXTENDED NAMED ACL
---------------------

ip access-list extended LAN1_TO_LAN2
deny ip host 192.168.10.10 host 192.168.20.10
permit ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
exit


APPLY ACL TO R1
---------------

interface gigabitEthernet 0/0
ip access-group LAN1_TO_LAN2 in
exit

end


R2 INTERFACES
-------------

enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.20.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.0.0.2 255.255.255.252
no shutdown
exit


R2 STATIC ROUTE
---------------

ip route 192.168.10.0 255.255.255.0 10.0.0.1

end


VERIFICATION COMMANDS
---------------------

show ip interface brief
show ip route
show access-lists LAN1_TO_LAN2
show ip interface gigabitEthernet 0/0
show running-config


TESTING
-------

From PC1:

ping 192.168.20.10
Expected: BLOCKED after ACL is applied.

ping 192.168.20.1
Expected: SUCCESSFUL.

tracert 192.168.20.10


ACL POLICY
----------

DENY:
192.168.10.10 -> 192.168.20.10

PERMIT:
192.168.10.0/24 -> 192.168.20.0/24

ACL is applied to:
R1 Gi0/0 INBOUND


IMPORTANT
---------

ACLs are processed from top to bottom.

The specific deny rule is placed before the broader
permit rule.

Traffic that matches no explicit rule is subject to
the implicit deny.
