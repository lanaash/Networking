# Basic Cisco LNA, LAC and CPE config


* LAC terminates PPPoE from CPE
* Static or RADIUS L2TP tunnel from LAC to LNS
* LNS uses RADIUS to authenticate and provide network service to clients


## LNS

```
aaa new-model
!
!
aaa authentication ppp default group radius local
aaa authorization network default group radius local
!
vpdn enable
!
vpdn-group LNS
 accept-dialin
  protocol l2tp
  virtual-template 1
 terminate-from hostname BRAS
 ! Renegotiates LCP so MSS between LNS and LAC may differ to MSS between CPE and LAC
 lcp renegotiation always
 l2tp tunnel password 0 changeme
!
interface Virtual-Template1
 ip unnumbered GigabitEthernet0/2
 ppp authentication chap callin
 ! Might be needed to avoid MSS mismatches from bringing up PPP
 ppp mtu adaptive
!
ip radius source-interface GigabitEthernet0/1
!
radius-server host 10.104.2.1 auth-port 1812 acct-port 1813
radius-server key testing123
```


## LAC

```
aaa new-model
!
vpdn enable
vpdn multihop
! Needed for per-suscriber profiles not needed for realm based forwarding
vpdn authen-before-forward
vpdn search-order domain
!
! VPDN group not needed if RADIUS provides the tunnel config e.g.
!
!----------------------------------------------------------------
!
! testuser@testdomain  Cleartext-Password := "adsl"
!         Service-Type = Outbound-User,
!         Tunnel-Client-Auth-Id:1 = "BRAS",
!         Tunnel-Type:1 = L2TP,
!         Cisco-AVPair = "vpdn:l2tp-tunnel-password=changeme",
!         Tunnel-Server-Endpoint:1 = "10.104.0.253",
!         Tunnel-Preference:1 = 10
!----------------------------------------------------------------
!
!vpdn-group LAC
! request-dialin
!  protocol l2tp
!  domain rmplc.co.uk
! initiate-to ip 10.104.0.253
! local name BRAS
! l2tp tunnel password 0 changeme
!
bba-group pppoe global
 virtual-template 1
!
interface Virtual-Template1
 ip unnumbered GigabitEthernet0/0
 ip mtu 1492
 ppp authentication chap
!
interface GigabitEthernet0/0
 description *** WAN ***
 ip address 10.104.0.254 255.255.255.0
 media-type rj45
 speed auto
 duplex auto
 negotiation auto
 pppoe enable group global
!
```

## CPE

```
!
interface GigabitEthernet0/0
 no ip address
 duplex auto
 speed auto
 pppoe enable
 pppoe-client dial-pool-number 1
!
!
interface Dialer0
 ip address negotiated
 encapsulation ppp
 ip tcp adjust-mss 1452
 dialer pool 1
 dialer-group 1
 ppp authentication chap callin
 ppp chap hostname testuser@testdomain
 ppp chap password 7 1416161800
!
ip route 0.0.0.0 0.0.0.0 Dialer0
!
```
