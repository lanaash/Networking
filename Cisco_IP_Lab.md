# Basic Cisco configs for config development

## Cisco BGP server
```
!
interface Loopback0
 description "Interface for dummy default route"
 ip address 10.10.10.10 255.255.255.0
!
!
ip route 0.0.0.0 0.0.0.0 10.10.10.10
!
interface GigabitEthernet0/0
 description "Interface to test CPE"
 mtu 1600
 ip address 192.168.0.6 255.255.255.248
 ip mtu 1500
 duplex auto
 speed auto
!
router bgp 1
 no synchronization
 bgp router-id 192.168.0.6
 bgp log-neighbor-changes
 timers bgp 10 30 30
 redistribute connected
 neighbor 192.168.0.1 remote-as 2
 neighbor 192.168.0.1 password changeme
 neighbor 192.168.0.1 default-originate
 no auto-summary
 !
```

## Cisco PPPoE server
```
!
! Basic PPPoE server for testing
!
bba-group pppoe global
 virtual-template 1
!
interface GigabitEthernet0/0
 description *** CLIENTS ***
 ip address 10.104.0.254 255.255.255.0
 media-type rj45
 pppoe enable group global
 no shut
!
interface GigabitEthernet0/1
 description *** ISP Backbone ***
 ip address 172.16.0.1 255.255.255.0
 media-type rj45
 no shut
!
interface Virtual-Template1
 ip unnumbered GigabitEthernet0/0
 ip mtu 1500
 peer default ip address pool CLIENTS
 ppp authentication chap
!
!
username testuser@testdomain password 0 adsl
!
ip local pool CLIENTS 10.104.0.1 10.104.0.10
!
```

